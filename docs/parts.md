# Parts list

## The GPU side

| Name | Part | Notes |
|---|---|---|
| **POLLUX** | AMD Instinct MI250 OAM module, **102-D65206** | The module that runs. |
| — | AMD Instinct MI250 OAM module, **102-D65205** | The spare, still in its bag. |
| — | Heatsink SNK-P1043V, on each module | Passive vapor chamber with fins. |
| **SLEEPER** | Supermicro **AOM-MCM-Q** universal baseboard (UBB), rev 1.01 | 4 OAM sites (8 Mirror Mezz connectors, two per site), 8 MCIO-124 connectors, two 54 V input headers. Part of the AS-4124GQ-TNMI server (manual MNL-2507). |
| — | Supermicro **MCP-240-45801-0N** stiffener | Flat counter-support under the baseboard for the mezzanine clamp. |
| — | Wooden support box, 31.3 × 45.7 × 20.2 cm | Holds the baseboard. Not necessary: added after the setup was proven. |

## Host and PCIe path

| Name | Part | Notes |
|---|---|---|
| — | TYAN Transport SX **TN70E-B8026** (S8026 board), AMD EPYC **7352** (Rome, 24 cores) | 6 × 32 GB DDR4 ECC RDIMM, running at 2400 MT/s. |
| **OOG** | JiaWen PCIe 5.0 x16 → dual MCIO-8i host card (RTE162PM5B-2U) | For GCD-R. Hand-modified for the sideband signals. |
| **ZONK** | JiaWen PCIe 5.0 x16 → dual MCIO-8i host card (RTE162PM5B-2U) | For GCD-P. |
| **HYDRA** | MCIO cable **MCIO-2304-70**: one MCIO-124 to two MCIO-74, 700 mm | For GCD-R. 85 Ω twinax, rated PCIe 5.0. |
| **AMPHISBAENA** | MCIO cable **MCIO-2304-70**: one MCIO-124 to two MCIO-74, 700 mm | For GCD-P. |

## 54 V power

| Name | Part | Notes |
|---|---|---|
| — | Dell PowerConnect **MPS1000** | 54 V / 18.5 A (1 kW) backup supply for Dell network switches. |
| — | Half of a Supermicro **CBL-PWEX-1280**, cut in two | Feeds one of the baseboard's two 54 V headers, enough for one module. The plug is a 2 × 5 Molex Micro-Fit 3.0 with male terminals. Untested generic equivalent: 43020-1000 housing with 43031-series male crimps (the largest I found takes 18 AWG; this cable is 16 AWG). |
| — | 1x8 wood plank about 34 cm long, carrying copper bus bars, 15 A DC breaker, enable DIP switch, Fujitsu RY5W-K relay and WCS1800 Hall current sensor | Details in [54 V power](power.md#bars-breaker-terminations). |
| **MITTEN** | Raspberry Pi Pico W clone, INA219 current-sense breakout and ME6211 3.3 V LDO on breadboards, in a plastic pencil box with switches, USB-C and JST-XH connectors | Holds the module in reset, and feeds and monitors its 3.3 V standby rail. |
| — | Kasa KP115 smart plug on the PSU's power cord | Measures AC watts and doubles as a remote AC switch ([power-down](power-sequence.md#power-down)). |
| — | Kasa KP115 smart plug on the host's power cord | Measures AC watts. |

## Cooling

| Part | Notes |
|---|---|
| Delta **GFB0912EHG**, 92 × 92 × 50 mm dual-rotor 12 V fan | AliExpress, looked cool. I don't recommend it: very loud, and three wires per rotor means no PWM input. Runs at full speed. |
| Generic two-prong 12 V brick | Powers the fan. |
| Simply Frozen Wood Fired Pepperoni & Spicy Honey Pizza box | Cut to shape for the duct. Flammable. |
| Eggo Minis Cinnamon Toast waffle box | Duct over the module. Flammable. |

## Tools that mattered

| Part | Used for |
|---|---|
| Digital torque screwdriver, Torx and Phillips bits | Seating the OAM: 4 × Torx T30 at 11 kgf·cm (1.08 N·m). Heatsink to OAM: T10 at 3.5 kgf·cm (0.34 N·m). Values from the heatsink's service label. |
| Digilent Analog Discovery (1st gen) | Logic analyzer and I²C master |
| 0–60 V / 5 A current-limited bench supply | First energize of the baseboard at 54 V |
| Weller WTCP soldering station, lead-free tin/silver/copper solder, rosin paste flux | The hand-soldered leads and the strap |
| OAEBLLE 1000× handheld digital microscope (sold as a kids' toy), a generic mini-USB digital microscope, a cheap handheld LED magnifier | Fine solder joints |
| Wire | 38 AWG enamelled, 22 AWG solid telecom, 26 AWG silicone stranded, 10 AWG blue stranded copper |

## Software

Ubuntu 24.04 (kernel 6.8) with open-source **amdgpu + ROCm 6.4.4**. The non-secure MI250 is
Aldebaran (`1002:740c`, gfx90a) and is supported upstream. Running it outside its server
needed some driver changes.
