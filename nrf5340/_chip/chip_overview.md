# nRF5340 — Shared Chip Reference

Generic silicon reference for all nRF5340-based boards in the MYSTIC board corpus.
Board-specific wiring and dual-core bring-up live under `<chip>/<board>/`.

| Item | Detail |
|------|--------|
| App core | Cortex-M33 @ 128 MHz, 1 MB flash |
| Net core | Cortex-M33 @ 64 MHz, 256 KB flash |
| Shared RAM | 512 KB (app + net) |
| Radio | Bluetooth 5.4, Thread, Matter (net core) |
| Build | sysbuild mandatory for dual-core images |

Provenance: `community_verified` — creator-reviewed getting-started context, not
device-measured on your hardware.
