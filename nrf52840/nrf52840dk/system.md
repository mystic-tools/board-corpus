# nRF52840 DK + Sidewalk — System Overview

[hypothesis — from Nordic NCS Sidewalk documentation; not DTS-parsed]

## Board
- **SoC:** nRF52840, ARM Cortex-M4F, 64 MHz, 1 MB Flash, 256 KB RAM
- **Zephyr board:** `nrf52840dk/nrf52840` (upstream NCS)
- **Debug:** Onboard J-Link OB — no external programmer needed
- **USB:** USB CDC-ACM shell (uart:~$)

## Amazon Sidewalk Stack
- **Primary link:** Sidewalk BLE (nRF52840 internal radio, 2.4 GHz) — no extra hardware
- **Secondary link:** Sidewalk LoRa sub-GHz via SX1262 shield on Arduino headers (hypothesis)
- **Provisioning:** AWS IoT + Sidewalk console; device registered before sid_start
- **SDK:** Amazon Sidewalk SDK in NCS (CONFIG_SIDEWALK=y)
- **NCS examples:** `samples/sidewalk/` target nRF52840 DK directly

## Flash / Debug
- J-Link (onboard) — reliable NCS/Sidewalk bring-up path
- jlink_device: nRF52840_xxAA

## Data flow
BLE Sidewalk (primary) + SX1262 sub-GHz (optional shield); shell via USB CDC-ACM.
