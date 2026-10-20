# Heathkit SB-200 Restoration Plan

A restoration plan for the Heathkit SB-200 HF linear amplifier (two 572B tubes in grounded grid, 1200 W PEP input). It covers which parts fail with age (HV filter bank, bleeders, bias supply, grid and parasitic resistors, RFC, tuning cap), a phased restoration and tune-up procedure, a symptom table, optional field mods (glitch resistor, soft-start, low-voltage keying) and maintenance.

**Status:** Rev. 1, built from field sources and checked against the original Heath assembly manual (595-682-01).

## Files

- [`sb200-restoration-plan.md`](sb200-restoration-plan.md) — source (pandoc markdown)
- [`sb200-restoration-plan.pdf`](sb200-restoration-plan.pdf) — rendered PDF

## Build

```sh
pandoc sb200-restoration-plan.md -o sb200-restoration-plan.pdf \
  --pdf-engine=xelatex -V mainfont="DejaVu Serif"
```

## Safety

**The plate supply is about 2400 V DC, which is lethal.** Unplug the amplifier and wait for the bleeders to drain the capacitors, then short each anode clip to the chassis with an insulated tool and confirm with a meter before touching anything. An open bleeder leaves the capacitor bank fully charged.

## Sources

- Heath SB-200 Assembly Manual 595-682-01
- AF6C, *Heathkit of the Month #33: SB-200* (w6ze.org)
- WA3DSP, WB3ANQ, Wireless-Girl and AE1S SB-200 restoration write-ups
- Heath mailing list; Heathkit-HFAmps and Heathkit-Radios groups on groups.io

See also: [heathkit-sb301-restoration](https://github.com/usa4148/heathkit-sb301-restoration) · [heathkit-hw101-restoration](https://github.com/usa4148/heathkit-hw101-restoration) · [heathkit-sb102-restoration](https://github.com/usa4148/heathkit-sb102-restoration) · [heathkit-sb110-restoration](https://github.com/usa4148/heathkit-sb110-restoration)
