# Hardware Quick Reference — nRF52840 DK + Sidewalk

[hypothesis — from Nordic PCA10056 documentation; verify with west build DTS]

## SoC
- **Part:** nRF52840, ARM Cortex-M4F, 64 MHz, 1 MB Flash, 256 KB RAM

## Sidewalk Radio Links
- **BLE (primary):** nRF52840 internal, 2.4 GHz, BLE 5.0 — Sidewalk BLE link (no shield needed)
- **Sub-GHz (optional):** SX1262 via SPI shield on Arduino headers — Sidewalk LoRa link
- **SX1262 shield Zephyr flag:** `--shield lora_sx1262_shield` (hypothesis)

## UART / Console
- **UART0:** TX=P0.06, RX=P0.08 (hypothesis)
- **Preferred:** USB CDC-ACM (no cable needed)
- **Baud:** 115200, 8N1; shell prompt: `uart:~$`

## Debug
- **Method:** J-Link OB (SEGGER, onboard) — no external programmer
- **J-Link device:** nRF52840_xxAA
- **SWD:** P18 connector (10-pin ARM Cortex debug)

## AWS / Sidewalk
- Device registered in AWS IoT + Sidewalk console before deployment
- Sidewalk gateway: Amazon Echo (4th gen+) within BLE range (~10 m) or sub-GHz range (~1 km LOS)
