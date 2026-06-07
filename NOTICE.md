# NOTICE — Acknowledgements & third-party references

ChipZapper is a **clean-room project**: it imports **no third-party code or design files**.
The work below is acknowledged as inspiration / reference only. ChipZapper is licensed MIT
(see `LICENSE`); these acknowledgements are courtesy, not a license obligation.

## Inspiration (not incorporated)

### PicoEMP — Colin O'Flynn / NewAE Technology (CC-BY-SA-3.0)
- Source: https://github.com/newaetech/chipshouter-picoemp
- Conceptual inspiration for a low-cost EMFI tool. ChipZapper does **not** copy its code or design
  files; any firmware/hardware here is written independently. We still credit it as good practice.
- ChipZapper is designed to **interface with / add onto** a PicoEMP device (using the product is not
  a derivative of its files).

## Studied for understanding (NOT copied / NOT redistributed)

### ChipShouter (CW520) — © NewAE Technology, "reference only"
- Source: https://github.com/newaetech/ChipSHOUTER
- The published schematic is marked "for reference and informational purposes only" and is
  copyright NewAE. We **do not** copy or redistribute its files. Factual information learned from it
  (e.g. that the output stage uses a UCC21520 gate driver) is reported, not reproduced. The
  reference PDF is excluded from this repository (see `.gitignore`).

## Conceptual references

- **glitch3d** — Baldanos — https://github.com/Baldanos/glitch3d — **no license** (all rights
  reserved). **Not forked.** The small scan-grid + 3D-printer transport layer will be written
  independently for OctoPrint.
- **"EM-Fault It Yourself"** — Kühnapfel et al., TU Berlin / Fraunhofer SIT —
  arXiv:2209.09835 — methodology and 3D-printer-stage reference.
- **bio-rad-psu-controller** — BitBangingBytes —
  https://github.com/BitBangingBytes/bio-rad-psu-controller — cheap programmable HV-source reference.

## Trademarks
"ChipSHOUTER" and "PicoEMP" are registered trademarks of NewAE Technology Inc. ChipZapper is an
independent, **non-commercial** project, not affiliated with or endorsed by NewAE. Trademarks are
used nominatively only.
