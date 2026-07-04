# Nordic Thingy:53 — System Overview

## Project Overview

The Nordic Thingy:53 is Nordic Semiconductor's flagship multi-protocol IoT prototyping
platform. It uses the nRF5340 SoC — a dual-core chip with an application core (128 MHz
Cortex-M33) and a network core (64 MHz Cortex-M33) that runs the Bluetooth, Thread, or
Matter radio stack. The two cores communicate over IPC (inter-processor communication).

The Thingy:53 ships ready-to-use with sensors (temperature/humidity/pressure via BME688,
motion via BMI270), a microphone, RGB LED, and a LiPo battery charger. Firmware is built
with NCS (nRF Connect SDK) + Zephyr RTOS using **sysbuild** (mandatory for dual-core).
Flashing is via USB DFU or an external J-Link.

## Build Commands

```bat
REM Activate NCS toolchain
%NCS_ROOT%\toolchains\v3.2.1\opt\bin\activate.bat

REM Build — sysbuild is required for dual-core images
cd %NCS_ROOT%\thingy53-app
west build -b thingy53/nrf5340/cpuapp --sysbuild

REM Flash via USB DFU (board in DFU mode — see notes.md)
west flash --runner nrfdl
REM Or with J-Link:
west flash --runner jlink
```

> **Board qualifier:** Always `thingy53/nrf5340/cpuapp`. The `/cpuapp` suffix selects
> the application core; omitting it causes a build error.

> **Sysbuild:** Without `--sysbuild`, only the app core image is built. The net core
> image (radio firmware) will be missing and Bluetooth/Thread won't start.

## Viewing UART Logs

The Thingy:53 exposes a USB CDC-ACM UART when connected via USB.

1. Connect via the USB-C port.
2. Open a serial terminal at **115200 baud** on the enumerated port (`COM5` on Windows,
   `/dev/ttyACM0` on Linux).
3. With `CONFIG_SHELL=y`, the prompt is `uart:~$`.
4. All Zephyr log output streams here automatically when `CONFIG_UART_CONSOLE=y`.

> If no CDC port appears after connection, verify `CONFIG_USB_CDC_ACM=y` and
> `CONFIG_USB_DEVICE_STACK=y` are in `prj.conf`. Thingy:53 does NOT have a built-in
> UART-to-USB chip — it relies solely on USB CDC.

## Cloud LLM Setup

Go to **Settings** in the agent web UI → add a Groq or Gemini API key → Save.
Free keys: console.groq.com (Groq), aistudio.google.com (Gemini).

## Toolchain Install Path

```
NCS_ROOT = C:\ncs   (Windows)
NCS_ROOT = ~/ncs    (Linux/macOS)
```
