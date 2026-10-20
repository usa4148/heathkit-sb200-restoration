---
title: "Heathkit SB-200 Linear Amplifier — Restoration Plan"
subtitle: "Rev. 1 — field sources and service bulletins, checked against the original Heath manual"
date: "October 2026"
geometry: margin=0.9in
fontsize: 10pt
colorlinks: true
toc: true
---

<!-- Build: pandoc sb200-restoration-plan.md -o sb200-restoration-plan.pdf --pdf-engine=xelatex -V mainfont="DejaVu Serif" -->

> **DANGER — LETHAL VOLTAGE.** The SB-200's plate supply is a voltage doubler that measures **about 2400 V DC unloaded** (manual p. 42). It delivers enough current to kill instantly. The manual: *"LETHAL VOLTAGES ARE PRESENT IN THIS UNIT. USE EXTREME CARE WHEN MAKING ANY TESTS."*
>
> - **Unplug** the line cord. The front-panel switch alone is not enough.
> - **Wait.** The six 30 kΩ bleeders (R5–R10) take tens of seconds to drain 21 µF from 2400 V, and **a bleeder that has gone open leaves full charge on the bank.**
> - **Short every tube anode clip to chassis** with an insulated-handle screwdriver or a grounded shorting stick. This is the manual's own discharge step (p. 42). Then short across each filter capacitor.
> - **Verify with a meter** on the HV scale before touching anything. Never trust the front-panel HV reading for safety.
> - Work **one hand in your pocket** when the unit is live. Never defeat the cover or top-plate interlock practices, and **never operate with the RF shield or cover off.**
> - The tube **caps are at plate voltage** whenever the amp is on.

# 1. Source documents

| Document | Where | Use |
|---|---|---|
| **Heath SB-200 Assembly Manual** 595-682-01 (©1964, printed 12-6-68), 52 pp. + fold-outs | Scan supplied by owner | **Primary reference.** Specs (p. 4), circuit description (pp. 5–7), parts list (pp. 8–10), initial test (p. 42), installation (pp. 43–46), operation (pp. 47–49), in case of difficulty (p. 50), schematic (fold-out p. 51). |
| AF6C, *Heathkit of the Month #33: SB-200* | w6ze.org | Circuit walk-through, versions, filter-cap and filament advice. |
| WA3DSP, *SB-200 Restoration and Modifications* | crompton.com | Full documented rebuild: failures, protection, keying. |
| WB3ANQ, *Heathkit SB-200 Modifications* | wb3anq.com | Measured cap/resistor degradation, glitch resistor, metering. |
| Wireless-Girl, *SB-200 Repair and Mods* | wireless-girl.com | Arc protection, parasitic-choke resistors, bias-cap drift. |
| AE1S, *Restoration of Heathkit SB-200* | blog.kotarak.net | Harbach board rebuild, glitch resistor, meter clamp, keying. |
| Heath mailing list | puck.nether.net/pipermail/heath | Field diagnosis (grid current path, cracked grid resistors, keying voltage). |
| **Heathkit-HFAmps** (groups.io) | groups.io/g/HeathKit-HFAmps | Heath amplifier group. Topic threads on SB-200 relay voltage, TX/RX bias, low power, low output on 10 m, input SWR network, HV, keying and tube life. *Pages could not be fetched for this revision (§11).* |
| Heathkit-Radios, heathkit (groups.io) | groups.io | General Heath field experience. |

**Service bulletins:** none for the SB-200 turned up in the Heath-list bulletin transcriptions for this revision. The plan relies on the manual plus field sources.

**Numbering:** the parts in this plan use the schematic designators (fold-out p. 51). Resistors are 1/2 W unless marked. The manual's voltages were taken with an 11 MΩ VTVM, ±10%, with no signal input.

# 2. Amplifier at a glance

## 2.1 Factory specifications (manual p. 4)

| Item | Spec |
|---|---|
| Bands | 80, 40, 20, 15, 10 m |
| Max input | **1200 W PEP SSB; 1000 W CW** |
| Drive required | **100 W** (40–60 W exciters won't reach full power, p. 43) |
| Duty cycle | SSB continuous voice; **CW 50%, key-down ≤ 5 minutes** |
| IMD | −30 dB or better at 1000 W PEP |
| Output | 50–75 Ω unbalanced pi-network; SWR ≤ 2:1 |
| Input | 52 Ω, broadband, factory pretuned (no adjustment) |
| Metering | Grid 0–100 mA (white area); plate 0–1000 mA; relative power 0–1000; SWR 1:1–3:1; HV 1500–3000 V |
| Tubes | **Two 572-B (or two T-160-L) in parallel**, grounded grid, class B |
| Power | **120 VAC @ 16 A max / 240 VAC @ 8 A max** |
| Size / weight | 14-7/8 × 6-5/8 × 13-3/8 in.; 35 lb |

Production ran 1964–1978. The **SB-201** (1978–83) removed 10 m for the FCC 10/11 m rule and added an input filter (AF6C).

## 2.2 How it works (manual pp. 5–7)

- **Power transformer T1:** dual 120 V primaries (parallel for 120 V, series for 240 V). The **blower fan runs across one primary half.** Protection is by **two 8 A reset-type circuit breakers, CB1 and CB2.**
- **Secondaries:**
  - **6.3 VAC** for the filaments and the **#47 meter lamp**.
  - **About 800 VAC**: the HV winding, feeding diodes **D1–D16** (16 × silicon 57-27) and electrolytics **C4–C9** (6 × 125 µF 450 V) in a **full-wave voltage doubler.**
  - **About 120 VAC**: the bias winding. Through **R1** (465 Ω 15 W, per parts list) and **R2** (10 kΩ 2 W), rectified by **D17** and filtered by **C3** (20 µF 150 V), it gives relay control voltage and **cut-off bias.**
- **Bleeders/equalizers:** **R5–R10, 30 kΩ 7 W**, one across each filter cap. **R11** at the bottom of the string (700 Ω 7 W) provides an **ALC threshold of about 10 V DC.** **R12** (1 Ω 5 W) is the **plate-current shunt** in the HV return.
- **Input:** cathode-driven grounded grid. A broadband network for each band (L1–L5 with capacitors), via **C16**, drives the cathodes. A **bifilar filament choke** isolates the filament winding from RF.
- **Grid return:** **R21/R22 (33 Ω 1 W each)** go from each tube's grid to ground through RFC2, R15 and the relay path.
- **Output:** HV reaches the plates through **RFC1** (50 µH) and **parasitic chokes PC1/PC2** (on the anode clips). It then runs through blocking cap **C24** (1000 pF 5 kV) to the pi network:
  - tuning cap **C25** (150 pF 3 kV); **C26** (100 pF 5 kV) added on 80 m;
  - tapped coils **L6/L7**;
  - loading cap **C28A/B** (2 × 437 pF); **C27** (500 pF 2 kV) added on 80 m;
  - **RFC3** (1.1 mH) across the output, a DC safety path: if C24 shorts, HV goes to ground and trips the breakers.
- **Antenna changeover and bias** (p. 6):
  - **Receive:** cut-off bias passes through relay coil **RL1**, R15 and RFC2 to the grids. The relay stays open, and RF passes straight through (exciter → antenna).
  - **Transmit:** the exciter's VOX relay **grounds the ANT RELAY jack.** RL1 pulls in and the grids drop to **about −2 V**, giving a **resting plate current of about 90 mA.**
  - **With the amp OFF** there's no bias voltage, so RL1 is released and the exciter feeds straight through. This allows barefoot operation without changing cables.
- **Metering** (pp. 6–7):
  - Grid: across **R15** (1.5 Ω).
  - Plate: across **R12** through **R14** (3600 Ω precision).
  - HV: divider **R3** (three 4.7 MΩ 1 W in series) and **R4** (15 kΩ).
  - REL PWR / SWR: bridge **L8–L10**, **CR1/CR2** (1N191 germanium), **C20/C23**, **R19/R20**; sensitivity control **R13** (100 kΩ).
- **ALC:** sampled from the grid via **C29** (5 pF), rectified by **D18** (1N458), out to the ALC jack.

## 2.3 Tubes and semiconductors

| Ref | Part | Function | Notes |
|---|---|---|---|
| V1, V2 | **572-B or T-160-L** (411-198) | Final, grounded grid, parallel | Thoriated-tungsten filament; 160 W plate dissipation each (AF6C). The T-160-L is the Cetron equivalent. Instant-heating: the amp is usable as soon as it's switched on (p. 6). |
| D1–D16 | Silicon 57-27 | HV doubler | 17 × 57-27 in the kit. |
| D17 | Silicon 57-27 | Bias rectifier | — |
| D18 | **1N458** (56-24) | ALC detector | — |
| CR1, CR2 | **1N191** (56-26) germanium | SWR/REL PWR bridge | — |
| Meter lamp | **#47** | Meter illumination | On the 6.3 V filament winding. |
| RL1 | DPDT relay (69-5) | Antenna changeover | The parts list calls it a "110 V AC relay". In circuit its coil runs from the bias supply through R18 (2000 Ω 7 W). |

## 2.4 Manual test and operating figures

| Item | Value (page) |
|---|---|
| Resistance, either anode clip → chassis | **≈ 180 kΩ** after the meter settles (p. 42) |
| Resistance, V1/V2 socket lug 3 → chassis | **5 kΩ – 15 kΩ** (p. 42) |
| HV, no signal | **≈ 2400 V** on the HV meter position (p. 42) |
| Idle plate current, keyed, no drive | **≈ 90 mA** (pp. 6, 48) |
| Bias, keyed / unkeyed | ≈ −2 V / cut-off. The manual gives no figure for the cut-off value; the Heath list gives ≈ −100 V and AF6C ≈ −100 to −120 V. |
| Tune-up | 200 mA plate first, then full drive (p. 48) |
| **Grid current limit** | **≤ 100 mA** (white area, midscale); reduce drive if higher (pp. 6, 48) |
| **Plate current limit** | **≤ 500 mA**; reduce loading (LOAD counter-clockwise) if higher (p. 48) |
| Voice peaks | Plate meter reads about ½ or less of the single-tone value (p. 49) |
| AC wiring | 120/240 V jumpers on terminal strip **S**; outlet fused and wired with **≥ #14** (p. 43) |

# 3. What fails, ranked

## 3.1 HV power supply (failure #1)

| Item | Likelihood | Symptom | Action |
|---|---|---|---|
| **C4–C9, 125 µF 450 V electrolytics (×6, in series)** | **Very high** | Low HV, hum on the signal, bulging or venting; one shorted cap overstresses the rest | **Replace all six as a set.** WB3ANQ measured the originals at **about 72 µF average.** Use 105 °C, ≥ 450 V (500 V preferred), long-life parts. |
| **R5–R10 bleeders, 30 kΩ 7 W** | High | Drifted values unequalize the cap voltages, so one cap sees too much; an open bleeder leaves the bank charged | Replace all six with matched parts. If you go to larger caps, recompute the bleeder current: higher-capacitance banks with mismatched leakage need adequate equalizing current (AE1S commenter's formula). |
| D1–D16 (57-27) | Medium | A shorted diode puts more reverse voltage on the rest of its string, causing a cascade failure | Replace all with 1N4007 or 1N5408. Some rebuilders use fewer P600M (1 kV 6 A) diodes, jumpering the unused positions (WB3ANQ). |
| Larger filter caps | — | **Not recommended without soft-start.** AF6C says 470 µF parts cause inrush surges that stress the transformer and diodes and trip the breakers. The Harbach board uses 180 µF (30 µF bank); AE1S went to 390 µF only after adding soft-start. | Stay near stock values, or add soft-start first (§4.9). |
| R11 700 Ω 7 W, R12 1 Ω 5 W | Medium | Wrong ALC threshold / wrong plate reading; an open R12 floats the HV return | Measure; replace. |

## 3.2 Bias, relay and keying circuit

| Item | Likelihood | Symptom | Action |
|---|---|---|---|
| **C3 (20 µF 150 V) and C19 (2 µF 150 V) electrolytics** | **High** | Bias drift, relay chatter, high idle current | Replace. Wireless-Girl replaced the 2 µF with a film cap and swapped the rest of the bias-supply caps. |
| R1 465 Ω 15 W, R2 10 kΩ 2 W, R18 2000 Ω 7 W | Medium | Wrong bias or relay voltage | Measure; replace. |
| D17 bias rectifier | Medium | **No bias** → the tubes conduct in receive (very dangerous for them), relay won't work | Replace (1N4007). |
| **RL1 contacts and coil** | Medium–high | Intermittent switching, hot switching, SWR jumps, no output | Clean or replace. If you change the relay, re-check the **−2 V transmit bias**: a different coil resistance changes it. AE1S changed the relay's parallel resistor, and Harbach adopted the same change. |
| **Keying voltage on the ANT RELAY jack** | **Compatibility issue** | Stock, the jack carries roughly **−100 to −120 V** of bias (Heath list; AF6C). That can **damage modern transceivers'** keying circuits. | Key through an external relay, or fit a low-voltage keying mod (§4.9). |

## 3.3 RF deck

| Item | Likelihood | Symptom | Action |
|---|---|---|---|
| **R21/R22 grid resistors (33 Ω 1 W)** | Medium–high | **Cracked or open after a tube flashover**; one tube carries less current, low plate current, discolored tube (Heath list) | Replace, **and find the cause first** (gassy tube, arc). |
| **Parasitic-choke resistors (PC1/PC2)** | High | Carbon resistors drift high; Wireless-Girl measured over 300 Ω on a 47 Ω part. Parasitics, instability. | Replace with **non-inductive** 1–2 W carbon-composition parts (Ohmite OX/OY type) of the original value. |
| **RFC1 plate choke (50 µH)** | Medium | Cracked form, burned windings, arcing | Replace (WA3DSP found one cracked at the base; AE1S found a burnt HV choke). |
| **C24 blocking cap (1000 pF 5 kV)** | Medium | If it shorts, HV reaches the output and RFC3 trips the breakers | Replace with a proper 5 kV+ doorknob or tubular. |
| **C25 tuning cap / bandswitch** | Medium | **Arc marks, pitting**; flashover on tune | Clean; replace damaged parts. RF Parts sells an exact tuning cap (Wireless-Girl). |
| Tube sockets | Medium | Loose pins → low filament voltage | Re-tension or replace; WA3DSP fitted ceramic sockets. |
| **Filament wiring / bifilar choke** | Medium | **Low filament voltage.** One restorer measured about 4.5 V instead of 6.3 V. At 8 A, a quarter-ohm of bad connection drops a lot. | Measure **6.3 ±0.3 V at the sockets, under load** (AF6C). Fix the joints; check choke continuity. |
| C14/C15 grid bypass (200 pF), C29 (5 pF ALC) | Low–medium | — | Check. Keep C14/C15 at the original value (Wireless-Girl). |
| Input network (L1–L5) | Low | High input SWR; Wireless-Girl traced one case to a bad solder joint on an input capacitor | Reflow; the manual says the input coils are factory set (p. 42). |

## 3.4 Metering and SWR bridge

| Item | Symptom | Action |
|---|---|---|
| **R3 HV divider (3 × 4.7 MΩ 1 W)** | HV reads low (WB3ANQ: meter 1800 V, actual about 2300 V). High-value carbon parts drift. | Replace with metal-film parts rated for the voltage (WB3ANQ used ten series 1 % metal films). |
| R14 (3600 Ω precision), R15 (1.5 Ω), R12 (1 Ω) | Wrong plate or grid readings | Measure; replace. |
| CR1/CR2 (1N191) | No REL PWR or SWR | Test; substitute 1N34A. |
| Meter movement | Pegging during flashover can bend the needle or open the movement | **Add back-to-back diodes across the meter** (several restorers). |

## 3.5 Tubes

- **572B condition:** a dull, brown or white getter means gas (Wireless-Girl). Check for a **dead filament half** (WB3ANQ) and an **unevenly lit filament** (WA3DSP).
- **Matching:** run a matched pair. With unbalanced tubes, one carries the load.
- **New-tube quality varies.** WA3DSP returned a new Chinese 572B with low emission.
- **Gassy tubes cause flashover**, which cracks R21/R22 and damages the meter.
- **Gettering:** run the tubes at dull-red plate under RF into a dummy load, cycling them, per the W8JI/W3JW procedure (WA3DSP). Do this before high power on stored tubes.
- **Idle current** is set by the fixed bias, about 90 mA. A healthy pair should land near that. WB3ANQ measured 65 mA idle; a weak or mismatched pair shows lower.

## 3.6 Mechanical and AC

- **Circuit breakers CB1/CB2 (8 A):** worn breakers nuisance-trip or fail to trip. Test them and replace if suspect.
- **Fan motor:** clean, oil the bearings and check rotation. The tubes depend on forced air.
- **Line cord:** heavy-gauge, three-wire, grounded. **#14 minimum** for the 120 V branch circuit (p. 43); heavier for the cord itself.
- **Grounds and RF shield:** all shield screws tight. The RF compartment's perforated shielding is part of TVI and stability control (p. 5).

# 4. Restoration plan

## 4.1 Phase 0: Assess cold (unplugged, discharged)

1. **Discharge** (safety box). Verify zero volts on the bank.
2. Photograph everything. Note mods: Harbach boards, soft-start, low-voltage keying, glitch resistor, larger caps, rewired metering.
3. Inspect the filter caps (bulging, leakage), bleeders (scorching), RFC1 (cracks, burns), tuning cap and bandswitch (arc pits), parasitic chokes, R21/R22 (cracks), tube getters, sockets, fan and relay.
4. **Manual resistance check (p. 42):**
   - **Anode clip → chassis ≈ 180 kΩ** (after the meter settles; this is the bleeder string).
   - **Socket lug 3 → chassis 5–15 kΩ.**

   If either is off, stop and find out why.
5. Check that the 120/240 V jumpers on terminal strip **S** match your mains.

## 4.2 Phase 1: HV supply rebuild

1. Replace **C4–C9** (six matched caps, ≥ 450 V, 105 °C) and **R5–R10** (six 30 kΩ 7 W, or a value recalculated for the new caps).
2. Replace **D1–D16**. Use a replacement HV board (Harbach) if the original board is damaged.
3. Check **R11** and **R12**.
4. Replace the bias-supply caps **C3, C19**; check **R1, R2, D17, R18**.
5. **First power-up of the supply, on a variac:**
   - Tubes **out**, or plate caps disconnected and insulated.
   - Raise the voltage slowly while watching the HV with an external HV probe/meter **and** the panel meter.
   - Expect about 2400 V unloaded at full line.
   - Confirm the bias is negative and the relay works when the ANT RELAY jack is grounded.

## 4.3 Phase 2: RF deck

1. Replace the **parasitic-choke resistors** (non-inductive) and inspect the choke windings.
2. Replace **R21/R22** if they're cracked or drifted.
3. Inspect and replace **RFC1**, **C24** and any arc-damaged parts. Clean the **tuning cap and bandswitch.**
4. Re-tension or replace the sockets. Reflow the **filament joints**; check the bifilar choke.
5. Replace the **R3 HV divider**; check R14/R15; test CR1/CR2.
6. Clean and oil the fan; clean or replace the relay.

## 4.4 Phase 3: Tubes and first full power-up

1. Test or inspect the 572Bs; fit a matched pair.
2. **Initial test (p. 42):**
   - All knobs fully CCW except TUNE at 9 o'clock.
   - Switch on: filaments, meter lamp and fan should come up.
   - Rotate the meter switch: **no reading except HV ≈ 2400 V.**
   - Off, unplug, and discharge (short an anode clip to chassis).
3. Measure **filament voltage at the sockets under load: 6.3 ±0.3 V.**
4. **Getter** stored tubes (§3.5) at low power before full output.

## 4.5 Phase 4: Tune-up and verification (manual pp. 47–49)

Connect a **50 Ω dummy load rated for 1 kW.**

1. Preset: LOAD **4**; BAND and TUNE to the segment; METER on **REL PWR**; REL PWR SENS **full CW**.
2. Tune the exciter for full CW output with the amp **OFF**. The REL PWR and SWR meters help tune the exciter through the amp.
3. Reduce the exciter drive to minimum. METER on **PLATE**; amp **ON**; key with no drive and check **idle ≈ 90 mA.**
4. Add drive for **200 mA** plate. Switch to REL PWR, then peak TUNE and LOAD.
5. Increase to **full drive**; re-peak TUNE and LOAD for maximum REL PWR.
6. **Check the limits:**
   - **GRID** ≤ midscale (100 mA); reduce drive if higher.
   - **PLATE** ≤ **500 mA**; turn LOAD counter-clockwise if higher.
7. Key-down only for seconds at a time while tuning. **CW key-down limit is 5 minutes, 50 % duty.**
8. Optional scope check against Figs. 2-5 to 2-7: sharp peaks with no flat-topping.
9. Record the LOAD settings per band for presetting.

**Benchmark:** at 1000 W input, expect about 600–700 W out on the lower bands. AF6C estimates about 60 % efficiency. WA3DSP measured over 600 W with 60 W drive on new tubes.

## 4.6 SWR measurement (manual pp. 44–45)

With the amp **OFF**, the exciter feeds through it. Set REL PWR SENS for full-scale SET, then read SWR. Readings are most accurate at 70–100 W. **Keep antenna SWR below 2:1.**

## 4.7 Exciter hookup (manual pp. 43–46)

| Exciter | Notes |
|---|---|
| SB-400/401 | Uses the transmitter's LINEAR RELAY and ALC jacks (Fig. 2-1). |
| SB-100/101 (and SB-102, HW-101 the same way) | ALC and ANT RELAY lines (Fig. 2-2). |
| Transmitters without an internal antenna relay (HX-10) | Need an external antenna relay (Fig. 2-3). |
| **Modern solid-state radios** | **Don't key the stock jack directly** (−100 V class). Use an external relay or a low-voltage keying mod. |

## 4.8 Restore stock first

Get the amp working **stock** before adding mods, so any problem can be traced. The Heath list advice for odd bias or grid readings on a modified amp is to restore it to stock and key it with a small external relay.

## 4.9 Phase 5: Optional modifications (field, not factory)

| Mod | Purpose | Notes (source) |
|---|---|---|
| **Glitch resistor** in the HV lead to RFC1 | Limits fault current in a flashover; protects the tubes, diodes, meter and caps | 10–100 Ω high-voltage-rated wirewound or HV resistor. Examples: 10 Ω (Wireless-Girl, Ameritron part), 15 Ω 10 W (AE1S), 24 Ω 30 W (WB3ANQ). Often paired with an **HV fuse**: 1.5 A fast (Wireless-Girl); WA3DSP found 1 A blew and settled on 2 A. Move the R3 HV-meter tap to the tube side of the fuse. |
| **Meter / R12 clamp diodes** | Protect the meter and plate-current circuit during a flashover | Back-to-back 1N5404/1N5408 across the meter; a diode string across R12 (AE1S). |
| **Soft-start (step-start)** | Limits inrush into the caps and filaments | Harbach kit, or an NTC thermistor in the primary (WB3ANQ). **Required before upsizing the filter caps.** |
| **Low-voltage keying** | Safe with modern transceivers | Harbach Soft-Key (optically isolated), a MOSFET keying stage (WA3DSP), or an external relay (Heath list). |
| ALC sample to the R21/R22 junction | Stop ALC loading unbalancing one tube | Move C29 (WB3ANQ, Wireless-Girl). |
| Adjustable or regulated bias | Set idle precisely | WA3DSP used an LM337 transmit bias. **Optional; the stock fixed bias is fine with good tubes.** |
| Arc gaps on the filament line | Divert flashover energy | Ameritron spark gaps (Wireless-Girl). |
| Grid bypass increase on 80 m | More gain on 80 m | Extra 470 pF across each 200 pF (WB3ANQ). Optional. |

Keep the factory parts removed by any mod, and document it, so the amp can go back to stock.

# 5. Symptom quick-reference

| Symptom | Look at |
|---|---|
| Breakers trip at turn-on | Shorted filter cap or diode, shorted C24 (HV through RFC3), arc in the tuning cap, larger caps without soft-start |
| HV low | Filter caps (low µF), a failed diode, low line voltage, **or just the meter**: check R3 |
| Hum on the signal | Filter caps |
| Tubes conduct in receive / hot standby | **No bias**: D17, C3, R1/R2, relay path |
| Relay won't pull in | Bias supply, R18, relay coil, keying line not grounded by the exciter |
| Idle current far from ~90 mA | Tubes (weak or mismatched), bias −2 V point, R21/R22, relay coil resistance changed |
| Low plate current, one tube discolored | Cracked R21/R22 after a flashover; bad tube (Heath list) |
| No grid current | R15 / RFC2 / grid return path; meter switch (Heath list) |
| High input SWR | Input network solder joints; input caps; tubes |
| Low output on 10 m | Tubes, parasitic chokes, tuning cap; SB-201 has no 10 m |
| Flashover / bang | Gassy tube, dirty tuning cap, RFC1. **Fix the cause, then add a glitch resistor.** |
| Parasitic oscillation, unstable | Parasitic-choke resistors drifted; sockets; shield screws |
| Short tube life | Low filament voltage from bad joints, or over-driving (grid > 100 mA) |

# 6. Operation notes

- Use **100 W drive max.** **Grid ≤ 100 mA, plate ≤ 500 mA** at tune-up.
- **CW: ≤ 5 min key-down, 50 % duty.** SSB is rated for continuous voice.
- Tune with brief key-downs; preset LOAD from your log.
- Use ALC from the amp to the exciter to prevent flat-topping.
- Barefoot operation: switch the amp **OFF** and it passes the exciter straight through.
- Keep the room ventilated and the cabinet vents clear (p. 43).

# 7. Tips and tricks

- Measure filament voltage at the sockets, not at the transformer.
- Log the per-band LOAD and TUNE settings into the dummy load.
- Keep a spare matched 572B pair. Getter new or stored tubes before full power.
- An external HV probe is worth having. Don't trust the panel HV reading until R3 is renewed.
- Keep the cover and RF shield on for all live tests.

# 8. Maintenance schedule

| Interval | Task |
|---|---|
| Each session | Check idle ≈ 90 mA; watch the grid and plate limits during tune-up. |
| 6 months | Clean the fan, check rotation; vacuum the dust; inspect the tuning cap for arc marks. |
| Yearly | Filament voltage at the sockets; HV with an external meter; relay contacts; SWR bridge check. |
| 2–3 years | Measure the bleeders; check the R21/R22 and parasitic resistors; check the breakers. |
| 5 years | Tube emission and gas check; capacitor ESR and leakage check. |
| ~15–20 years | Replace the HV filter bank. |

# 9. Parts sources

- **Harbach Electronics:** SB-200 HV board kit (180 µF caps, diodes, precision metering, bleeders), soft-start, Soft-Key low-voltage keying.
- **RF Parts:** 572B tubes, tuning caps, HV parts.
- **Ameritron:** glitch resistors, spark gaps (also used by restorers).
- **Mouser / Digi-Key:** 105 °C snap-in electrolytics, 7 W bleeders, 1N4007/1N5408, metal-film HV divider resistors.
- **Heathkit-HFAmps** and **Heathkit-Radios** (groups.io): parts units, RFCs, bandswitches, advice.

# 10. Sources

- Heath Co., *SB-200 Linear Amplifier Assembly Manual*, 595-682-01 (1964/1968).
- Eckweiler AF6C, "Heathkit of the Month #33: SB-200," w6ze.org.
- WA3DSP, *SB-200 Restoration and Modifications* (crompton.com/hamradio/heath/sb200).
- WB3ANQ, *Heathkit SB-200 Modifications* (wb3anq.com).
- Wireless-Girl, *Heathkit SB-200 Repair and Mods: Soft Start, ARC protection, 572B replacement*.
- AE1S, *Restoration of Heathkit SB-200 (Part 1)* (blog.kotarak.net).
- Heath mailing list: "Testing for grid current on SB-200" (Mar 2002, WB8JKR); "SB-200 plate current" (Jun 2003).
- **Heathkit-HFAmps** (groups.io/g/HeathKit-HFAmps): SB-200 topics including relay voltage, TX/RX relay and grid bias, low power, low output on 10 m, input SWR network, HV on arrival, keying and tube life.

# 11. What the manual confirmed and what it didn't

**Confirmed from the manual:**

- The specs, duty cycle and drive (p. 4).
- The power supply topology: doubler, 16 diodes, 6 × 125 µF 450 V, R5–R10 30 kΩ bleeders, R11 ALC tap, R12 shunt, 8 A breakers (pp. 5, 8, schematic).
- Bias: about 90 mA idle at −2 V keyed (p. 6).
- Metering circuits (pp. 6–7).
- Resistance checks: 180 kΩ, and 5–15 kΩ (p. 42).
- HV about 2400 V and the discharge procedure (p. 42).
- Grid ≤ 100 mA and plate ≤ 500 mA tune-up limits (p. 48).
- Line wiring: #14 minimum (p. 43).

**Not in the manual:**

- The unkeyed cut-off bias value. The manual says "cut-off"; the field sources give about −100 to −120 V.
- Anything about filter-cap upsizing, soft-start, glitch resistors or low-voltage keying. These are all field mods.
- 572B emission and matching criteria.
- Parasitic-choke resistor values. They aren't legible on the scan; check yours against the schematic or replacement parts list.
- The exact R1 wattage. The parts list says 15 W; the schematic label is small.

**Heathkit-HFAmps:** the group's topic pages wouldn't load for this revision. Its SB-200 topics are listed in §10. Thread content should be folded in during a later revision.

**SB-200 service bulletins:** none was found in the Heath-list transcriptions searched for this revision.
