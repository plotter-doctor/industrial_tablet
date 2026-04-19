# Driver & Module Guide

Complete list of pre-installed kernel modules and supported USB peripherals for the GRBL CNC Controller.

All modules are cross-compiled for **Linux 3.10.104 ARMv7** (Rockchip RK3128) and auto-loaded at boot via Android init services.

---

## USB Serial Adapters

These modules enable plug-and-play support for USB-to-serial adapters — essential for GRBL communication. Once plugged in, the device appears as `/dev/ttyUSB0` (or `/dev/ttyUSB1`, etc.) and the ser2net bridge begins forwarding to TCP port 8023.

| Module | Chipset | Common Devices |
|---|---|---|
| `ftdi_sio.ko` | FTDI FT232R, FT232H, FT2232, FT4232 | Industrial serial adapters, premium CNC controllers |
| `ch341.ko` | WCH CH340, CH341 | Arduino-based GRBL shields (CNC Shield v3), clone boards |
| `cp210x.ko` | Silicon Labs CP2102, CP2104, CP2108 | grblHAL boards, Teensy, specialty CNC controllers |
| `pl2303.ko` | Prolific PL2303 | Older serial cables, legacy adapters |

### Typical CNC Serial Use Cases

- **G-code Streaming** — Real-time G-code commands to GRBL firmware via USB serial
- **Machine Control** — Jog, home, probe, and run jobs from the GRBL Controller app
- **Serial Debug** — Raw GRBL command console for `$$` settings, alarms, and diagnostics
- **TCP Bridge** — Web apps in Chrome connect via `127.0.0.1:8023` → ser2net → USB serial
- **Firmware Configuration** — Read and write GRBL `$` settings for axis calibration

---

## Hardware Serial Ports (Built-in)

The tablet has a hardware UART directly on the SoC, exposed via pin header:

| Port | Address | Device | Signal Level |
|---|---|---|---|
| UART0 | `0x20060000` | `/dev/ttyS0` | 3.3V TTL |

The port supports RTS/CTS hardware flow control.

> **Note:** UART2 (`0x20068000`) is typically used as the kernel debug console.

---

## USB WiFi Dongles

Extend range or add a second WiFi interface using USB dongles:

| Module | Chipset | Band | Typical Devices |
|---|---|---|---|
| `8188eu.ko` | RTL8188EU | 2.4 GHz | TP-Link TL-WN725N, generic nano adapters |
| `8192cu.ko` | RTL8192CU | 2.4 GHz | TP-Link TL-WN823N, Edimax adapters |
| `8192du.ko` | RTL8192DU | 2.4 GHz | D-Link DWA-131 |
| `8723au.ko` | RTL8723AU | 2.4 GHz | Combo WiFi+BT adapters |
| `8723bs.ko` | RTL8723BS | 2.4 GHz | SDIO WiFi+BT combo |
| `8723bu.ko` | RTL8723BU | 2.4 GHz | USB WiFi+BT combo |
| `8812au.ko` | RTL8812AU | 2.4/5 GHz | **Dual-band** high-gain adapters |
| `8188fu.ko` | RTL8188FU | 2.4 GHz | Ultra-compact nano adapters |
| `8822bu.ko` | RTL8822BU | 2.4/5 GHz | **Dual-band** USB 3.0 adapters |

---

## USB Bluetooth Dongles

| Module | Supported Chipsets | Notes |
|---|---|---|
| `btusb.ko` | CSR8510, Intel, Broadcom, Realtek | Generic HCI USB — works with most BT 4.0/5.0 dongles |
| `ath3k.ko` | Atheros AR3011, AR3012 | Atheros-specific firmware loader |
| `btbcm203x.ko` | Broadcom BCM203x series | Broadcom firmware loader |

---

## GPIO

Two general-purpose I/O pins accessible via the Android `sysfs` GPIO interface:

```bash
# Export a GPIO pin
echo <PIN_NUMBER> > /sys/class/gpio/export

# Set direction
echo out > /sys/class/gpio/gpio<PIN>/direction

# Write value
echo 1 > /sys/class/gpio/gpio<PIN>/value

# Read value
cat /sys/class/gpio/gpio<PIN>/value
```

**Signal level:** 3.3V logic. Use a level shifter for 5V devices.

### CNC GPIO Use Cases

- **Job completion indicator** — LED or buzzer triggered when G-code finishes
- **Spindle control** — GPIO-driven relay for spindle on/off (simple setups)
- **Door interlock** — detect enclosure door open/close for safety
- **Emergency stop** — hardware E-stop button via GPIO input
