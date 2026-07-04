# Hardware Quick Reference — Nordic Thingy:53

## What This Device Does

The Thingy:53 is Nordic's professional IoT prototyping kit — it has everything built in:
sensors for temperature, humidity, pressure, and motion; a microphone; RGB LED; and a
LiPo battery with charger. It runs on Nordic's most powerful multi-protocol chip
(nRF5340), which has two processor cores — one for your application code and one
dedicated to running the radio (Bluetooth, Thread, or Matter). You build and flash it
with NCS/Zephyr and connect to it via USB or Bluetooth.

## Key Specs

| Item | Detail |
|------|--------|
| SoC | Nordic nRF5340 (dual-core Cortex-M33) |
| App core | 128 MHz, 1 MB flash, 512 KB RAM (shared) |
| Net core | 64 MHz, 256 KB flash, 64 KB RAM |
| Radio | Bluetooth 5.4, Thread, Matter, BLE Mesh, Zigbee |
| USB | USB-C Full Speed (CDC-ACM console, DFU) |
| Flashing | USB DFU or external J-Link |
| Battery | Rechargeable LiPo via USB-C |

## Onboard Sensors

| Sensor | Part | What It Measures | Interface |
|--------|------|-----------------|-----------|
| Environmental | BME688 | Temperature, humidity, pressure, air quality | I2C @ 0x76 |
| IMU | BMI270 | Accelerometer + gyroscope | SPI3 |
| Magnetometer | BMM150 | Compass heading | I2C @ 0x10 |
| Microphone | IM69D130 | Audio (PDM) | PDM |
| Light | Onboard ambient | Proximity + light (future boards) | I2C |

## LED Reference Table

| LED | Color | Meaning |
|-----|-------|---------|
| RGB LED | User-defined | Application status; common: blue = BLE advertising, green = connected |
| DFU mode | Blue pulse | Device in USB DFU bootloader (hold SW1 on power) |

RGB LED GPIOs: Red P0.08, Green P1.08, Blue P0.16 (active-high via GPIO expander on some HW revisions — check board DTS).

## What I Can Help You With

- Dual-core build setup (sysbuild, child images, IPC)
- Bluetooth, Thread, Matter, BLE Mesh Kconfig and connection issues
- USB DFU flash procedure and recovery
- Sensor driver configuration (BME688, BMI270, microphone)
- IPC buffer tuning between app and net cores
- Build errors: wrong board qualifier, missing sysbuild config
- Devicetree overlays for sensor pinmux
- Debug setup with external J-Link (nRF5340_xxAA_APP target)
- Kconfig search — which CONFIG_ key controls what

## What Requires an Engineer (Out of Agent Scope)

- Hardware damage assessment (battery, USB port, antenna)
- RF certification and Thread/Matter certification
- Custom sensor PCB integration
- nRF5340 secure boot / TrustZone partition setup
