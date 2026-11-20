# Topology ID (the LINK_CONFIG strap)

One hardware strap, the OAM's **LINK_CONFIG[4:0]** pins, decides whether a single MI250 on
this baseboard comes up usable. They tell the module how it's connected: whether its host link
is PCIe, and how the baseboard wires the modules to each other. The AOM-MCM-Q sets them with
resistors at each site, to a multi-module topology.

## What the strap is

- **Five pins on the module's second mezzanine connector (Conn1):** bits 0–4 are F54, F53,
  F50, F49 and F46.
- **The module supplies the pull-ups.** The baseboard either pulls a bit down (to 0) or
  leaves it open (to 1). A missing resistor isn't neutral: it reads as a 1. The specs say
  this in UBB v1.5 §6.1.6.2 and OAM v1.1/v1.5 §9.3. The bit meanings are in OAM v1.5 Table 7.
- **It's sampled only at the module's power-on reset.** Changing it on a live module does
  nothing until the next power-up.
- **The module reports it back.** After power-up, the value reads from the host as
  `TOPOLOGY_ID` (TID below) in the die's `SMUIO_MCM_CONFIG` register. That makes it the check
  that the strap you fitted is the one the module saw.

## What the stock AOM-MCM-Q sets

**`10000` (16).** In the OAM spec's table that's "Combined FC / 6-port HCM", a topology the
spec draws with eight modules. I measured it at GPU0 and GPU2. Bits 3–0 have 1 kΩ
pull-downs fitted, and bit 4's spot is empty. Each site has its own bank of five 0402
positions with its own test pads. The GPU0 bank:

| Bit | Mezz pin | GPU0 resistor | Test pad | Stock |
|---|---|---|---|---|
| 4 | F46 | R1050 | TP1069 | **empty** (reads ~100 kΩ to the module's pull-up when a module is seated) |
| 1 | F53 | R1053 | TP1066 | 1 kΩ fitted |
| 0, 2, 3 | F54, F50, F49 | R1051, R1052, R1055 | | 1 kΩ fitted |

R1050 = bit 4 and R1053 = bit 1 are confirmed by readback. Fitted bits read about 1000 Ω at
their test pads.

## What each value did

One module (POLLUX) on a bare baseboard, out of reset, with the host links as in
[sequencing](sequencing.md). "Finishes start-up" means the die completes its own
boot and the driver can bind it.

| Strap | TID | How to set it from stock | Result | Runs |
|---|---|---|---|---|
| `10000` | 16 | stock | **Neither die finishes start-up.** Both enumerate on PCIe, but the driver can't use them. | 2+ |
| `10010` | 18 | remove R1053 | Neither die finishes start-up. | 1 |
| `00000` | 0 | 1 kΩ across R1050, R1053 fitted | **Only one die (GCD-P)** finishes start-up. | 2 |
| **`00010`** | **2** | **1 kΩ across R1050, R1053 removed** | **Both dies finish start-up, enumerate at x16 and run work.** | 3+, and every boot since |

The spec defines neither of the values that work here. Apart from bit 0 = 0 meaning a PCIe
host link, `00010` isn't in its table, and `00000` is reserved for the module vendor's test use.

## As built

GPU0 is set to `00010`: R1053 removed, and a 1 kΩ axial resistor across R1050's pads under
Kapton (ugly, but I don't have decent surface-mount tools). Every boot reads back TID 2.
