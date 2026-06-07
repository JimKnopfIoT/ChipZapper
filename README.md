# ChipZapper

> # ⚠️ Safety & Scope — please read
> **Educational use only · Non‑commercial · Selfmade · Proof‑of‑Concept (POC) / Work‑in‑Progress (WIP) · provided AS‑IS, no warranty**
>
> This project works with **high voltage** (up to ~500 V, designed to 750 V; stored energy ~1 J),
> which carries a real **risk of injury / health hazard** (electric shock, burns).
> - ⚠️ **Don't reproduce this without proper high‑voltage experience — at your own risk.**
> - 📚 **For education and authorized security research only. No commercial intent.**
> - 🚧 **POC / WIP** — incomplete and unverified; may be incorrect in its current form.
> - 🚫 **No warranty, no liability** for any damage, injury, or misuse.
> - ✅ Only test hardware you **own** or are **authorized** to test.
>
> 👉 Details: [`DISCLAIMER.md`](DISCLAIMER.md)

> 🎯 **Goal progress: ~25 %** &nbsp;`███░░░░░░░`&nbsp; · updated 2026-06-09
>
> **Status:** Toolchain operational (full trigger→glitch→target chain + HV bring-up); EMP baseline measured
> (PicoEMP edge ~640 ns — the bottleneck Phase B must beat); XYZ control (OctoPrint/MK4S) verified incl. cameras.
> **Phase B output stage fully specified — BOM complete, parts ordered.** Next: build the SiC reservoir stage.
> Next: glitch position tuning + **Phase B** output stage (UCC21520 + SiC — parts being ordered).

**ChipZapper is an open electromagnetic fault injection (EMFI) pulse generator that targets
ChipShouter-class performance at PicoEMP-class cost.**

The guiding idea: build **only the high-voltage pulse head**, and reuse lab gear you already own
(programmable PSU, trigger/capture, oscilloscope, DMM) for everything the commercial tools bundle
into one expensive box. A ChipShouter (~€5 000) pays for the precision supply, the trigger engine,
the measurement and the safety logic all at once — we already have those on the bench, so the part
that's actually missing (a fast, stable, high-voltage pulse head) is cheap.

> ℹ️ See the safety warning at the top and [`DISCLAIMER.md`](DISCLAIMER.md) before doing anything.

---

## Targets (benchmark = NewAE ChipShouter CW520)

| Parameter | ChipShouter | PicoEMP | **ChipZapper goal** |
|-----------|-------------|---------|---------------------|
| Output voltage | 150–500 V | ~250 V | **0–500 V, adjustable** |
| Design safety margin | 630 V caps / 650–800 V parts | ~500 V | **safe to 750 V** |
| Pulse edge | ~20–30 ns | slow (IGBT) | **≤ 20 ns** |
| Trigger jitter | sub-ns (HW) | µs (SW) | **sub-ns (ChipWhisperer Husky)** |
| Amplitude stability | constant (reservoir) | droops | **constant (reservoir)** |
| Pulse-head BOM cost | (part of ~€5 000) | ~€13–50 | **~€60–140** |

---

## Key design decisions

- **Reservoir, not crowbar.** A fat reservoir capacitor (5–10 µF / 1 kV) is *tapped* for only a
  fraction (~0.1–1 %) of its energy per pulse — controlled by the switch on-time (= the Husky pulse
  width). This keeps amplitude constant across a burst and allows high repetition rates. (Unlike the
  RFID-zapper / NFCKill which dump the whole capacitor every shot.)
- **Output stage = `UCC21520` gate driver + fast high-voltage MOSFET** — the same driver the
  ChipShouter uses, paired with a 1000–1200 V **SiC** MOSFET (faster edge than the ChipShouter's Si
  part, plus the 750 V headroom). The PicoEMP's IGBT is exactly the bottleneck we replace.
- **PicoEMP add-on first.** The primary deliverable is a **non-invasive add-on** that upgrades any
  existing PicoEMP to ChipShouter class (PicoEMP = brain/control, add-on = HV muscle). A full
  standalone board is an optional later step.
- **Reuse the bench.** Programmable HV setpoint via EEZ BB3, trigger/pulse-width via ChipWhisperer
  Husky, fast transient via Siglent SDS2104X-HD + H-field probe, precise DC readback via Keithley
  DMM7510, bring-up pulse source via Siglent SDG6022X.

---

## Equipment used (reused from the bench — not bought for this)

This is the whole cost trick: the expensive functions a ChipShouter bundles into one box, we
already have on the bench. Only the **HV pulse head** is actually built (BOM ~€60–140).

| Tool | Role in ChipZapper |
|------|--------------------|
| **EEZ BB3** (DCP405 module) | Programmable, stable HV setpoint / input; SCPI / MicroPython control |
| **ChipWhisperer Husky** | Trigger & pulse-width engine (sub-ns, target-synchronous) |
| **Siglent SDS2104X-HD** + H-field / EMP probe | Fast transient: rise time, amplitude, jitter |
| **Keithley DMM7510** (7½-digit) | Precise DC high-voltage readback to 1000 V + digitizer |
| **Siglent SDG6022X** (arbitrary waveform generator) | Clean bench pulse source for output-stage bring-up |
| **NewAE PicoEMP** | Starting platform (Phase A) and host for the add-on (Phase B) |
| **Prusa MK4S** + OctoPrint (+ 3 cameras) | XYZ stage for autonomous, camera-guided chip-surface scanning (step 2); top-down probe-cam for CV chip detection |

## What gets published here

This repository holds the **finished, releasable outputs**. When the design is done, this will include:

- **`hardware/`** — final KiCad schematic, PCB layout and Gerbers
- **`BOM`** — final bill of materials
- firmware and build/usage docs as they finalize

**Intermediate work is not published.** Our iterative working set (concept drafts, phase notes,
experiments, measurement logs, design dead-ends) is kept private/local. This README is the public
English overview; the roadmap below tracks progress until the released artifacts land here.

---

## Roadmap & progress

| Phase | Deliverable | Status | % |
|-------|-------------|--------|---|
| Research & concept | design documents | ✅ done | 100 % |
| **A** — PicoEMP bring-up | toolchain + glitch baseline | 🟦 in progress | ~85 % |
| **B1** — output stage | ≤ 20 ns edge proven | 🟦 BOM done, parts ordered | ~5 % |
| **B2** — HV + reservoir | stable 500 V, low droop | ⬜ | 0 % |
| **B3** — PicoEMP integration | armed/triggered via PicoEMP | ⬜ | 0 % |
| **B4** — safety + mechanics | 750 V layout, enclosure | ⬜ | 0 % |
| **B5** — characterization | device vs ChipShouter baseline | ⬜ | 0 % |
| **Step 2** — autonomous scan | MK4S/OctoPrint + camera-guided auto-positioning + CV heatmap | 🟦 control + cameras verified | ~15 % |
| **Overall** | working ChipShouter-class pulse head | | **~25 %** |

> Done: toolchain operational, EMP baseline measured, XYZ control + cameras verified, **Phase B BOM complete and parts
> ordered**. Ahead: build/prove the SiC reservoir output stage (the core), and camera-guided automatic positioning
> for the scan stage (3 cameras incl. a top-down probe-cam for CV chip detection). First on-target fault still pending.

---

## Built on / acknowledgements & licensing of references

This project studies and builds on prior open work. Licenses checked and respected:

- **PicoEMP** — Colin O'Flynn / NewAE Technology (CC-BY-SA-3.0). Acknowledged as **conceptual
  inspiration only** — ChipZapper is a **clean-room rewrite** and imports **no PicoEMP code or
  design files**, so it carries no ShareAlike obligation. (We still credit it — good manners.)
- **ChipShouter** — NewAE Technology — schematic published **"for reference only", © NewAE**. We
  study it for design understanding but **do not copy or redistribute** its design files. The
  reference schematic is intentionally **not** included here (git-ignored); get it from the
  [upstream repo](https://github.com/newaetech/ChipSHOUTER).
- **glitch3d** — Baldanos — **no license published** (= all rights reserved). We therefore **do not
  fork it**; the small scan-grid + OctoPrint layer will be re-implemented independently (or used
  only after a license is added). Conceptual reference only.
- **"EM-Fault It Yourself"** — Kühnapfel et al., TU Berlin, [arXiv:2209.09835](https://arxiv.org/abs/2209.09835) — methodology reference.
- **bio-rad-psu-controller** — [BitBangingBytes](https://github.com/BitBangingBytes/bio-rad-psu-controller) — cheap HV-source reference.

See [`NOTICE.md`](NOTICE.md) for full attribution.

### Trademarks
**"ChipSHOUTER"** and **"PicoEMP"** are registered trademarks of **NewAE Technology Inc.**
ChipZapper is an **independent, non‑commercial, selfmade project, not affiliated with or endorsed by NewAE**.
Those names are used only nominatively (e.g. "an add-on for the PicoEMP").

---

## License

**[MIT](LICENSE)** — maximally permissive. Anyone may use, modify, redistribute, and build upon
this (including commercially), with no obligation other than keeping the copyright/permission
notice. Because ChipZapper is a clean-room work (no copyleft code imported), we are free to license
it this way. See [`LICENSE`](LICENSE). *(Provided AS-IS, no warranty — see the warning above.)*

---

## Disclaimer

See [`DISCLAIMER.md`](DISCLAIMER.md). In short: **educational, non-commercial, authorized research
only; POC/WIP; high voltage carries a risk of injury; don't reproduce without HV experience;
AS-IS, no warranty, no liability.**
