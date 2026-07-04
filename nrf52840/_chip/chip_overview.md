# nRF52840 — Shared Chip Reference

Generic silicon reference for all nRF52840-based boards in the MYSTIC board corpus.
Board-specific wiring, partition maps, and bring-up live under `<chip>/<board>/`.

| Item | Detail |
|------|--------|
| Core | ARM Cortex-M4F @ 64 MHz |
| Flash | 1 MB internal |
| RAM | 256 KB SRAM |
| Radio | Bluetooth 5.x, 802.15.4, NFC (device-dependent) |
| Debug | SWD; many boards ship J-Link OB |

Provenance: `community_verified` — creator-reviewed getting-started context, not
device-measured on your hardware.
