# MI250 at home

> **Personal project**, done on my own time with hardware I bought secondhand.
> I work at AMD, but this isn't affiliated with or endorsed by AMD, and it uses
> only public information. Opinions are my own, not my employer's.

> ⚠ **Don't treat this as instructions.** I'm an amateur doing this for fun, and I'm
> willing to brick my hardware poking at it. This is a record of what I did, not a guide
> to what you should do. See [Safety and liability](#safety-and-liability).

The AMD Instinct MI250 is a datacenter GPU, usually installed four to a server. Two fat slabs of silicon, 128 GB of
HBM2e. Nice. I bought two of the OAM modules on eBay and run one by itself as a single GPU,
on a bare Supermicro baseboard with a rack-server host, a 54 V switch power supply and
off-the-shelf MCIO cables. I used Claude, Kimi and DeepSeek a lot along the way.

An OAM (OCP Accelerator Module) is a GPU on a mezzanine card instead of a PCIe card. Other
terms are defined in [wiring](docs/wiring.md#terms-used-here).

Work in progress. Reference notes are below.

- [Parts list](docs/parts.md)
- [Wiring and pinout](docs/wiring.md)
- [54 V power](docs/power.md)
- [Power sequence](docs/power-sequence.md)
- [Topology ID](docs/topology-id.md)
- [Results](docs/results.md)

## What works today

As of November 2026:

- **Power-up is repeatable.** Full cycles (power up, load, power down) run clean.
- **It does real work.** It serves an LLM and runs image generation day to day. Basic
  benchmark numbers are in [results](docs/results.md).
- **No firmware is modified.** The hardware changes are in [topology ID](docs/topology-id.md) and
  [wiring](docs/wiring.md#whats-actually-wired-on-this-bench).

Still open:

- **One die occasionally drops off the bus.** The same die has dropped three times in
  service, once under load and twice idle. The last time, the other die halted too. The
  cause isn't known yet.
- **The graphics clock tops out at 1405 MHz** on both dies, under every load and setting
  tried. The cause isn't known yet.

## Safety and liability

Some of what's described here was worked out by trial and error, and some of it may be
wrong.

This project involves high-current DC power (a 1 kW, 54 V supply with its
output wired by hand onto bus bars), mains-powered equipment, hot components, and hardware that costs thousands
of dollars and can't easily be replaced. Mistakes can destroy that hardware, start
a fire, or hurt you.

If you try anything like this, you do it at your own risk. Only attempt it if you
know what you're doing and understand the hazards. I'm not responsible for damaged
equipment, injury, or anything else that results from following what I did.
Everything here is provided as-is, with no warranty of any kind.
