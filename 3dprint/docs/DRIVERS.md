# Driver & Module Guide

Complete list of pre-installed kernel modules and supported USB peripherals for the 3D Printer Dashboard.

All modules are cross-compiled for **Linux 3.10.104 ARMv7** (Rockchip RK3128) and auto-loaded at boot via Android init services.

---

## USB Serial Adapters

These modules enable plug-and-play support for USB-to-serial adapters — essential for direct communication with 3D printer boards. Once plugged in, the device appears as `/dev/ttyUSB0` (or `/dev/ttyUSB1`, etc.).

| Module | Chipset | Common Devices |
|---|---|---|
| `ftdi_sio.ko` | FTDI FT232R, FT232H, FT2232, FT4232 | Industrial USB-RS232/RS485, premium serial adapters |
| `ch341.ko` | WCH CH340, CH341 | Arduino Mega (RAMPS), MKS Gen, SKR boards, Ender 3 |
| `cp210x.ko` | Silicon Labs CP2102, CP2104, CP2108 | Prusa boards, Duet, NodeMCU, Pololu |
| `pl2303.ko` | Prolific PL2303 | Older serial cables, legacy adapters |

### Typical 3D Printing Serial Use Cases

- **Direct Printer Control** — Serial link to Marlin/Klipper via Octo4a or OctoPrint
- **G-code Streaming** — Real-time command stream to printer firmware
- **CNC/Laser Control** — Serial to GRBL controllers for CNC routers and laser cutters
- **Multi-Printer** — Multiple USB serial adapters for controlling several printers
- **Firmware Upload** — Flash printer firmware via serial bootloader

---

## Hardware Serial Ports (Built-in)

The tablet has two hardware UARTs directly on the SoC, exposed via pin headers:

| Port | Address | Device | Signal Level |
|---|---|---|---|
| UART0 | `0x20060000` | `/dev/ttyS0` | 3.3V TTL |
| UART1 | `0x20064000` | `/dev/ttyS1` | 3.3V TTL |

Both ports support RTS/CTS hardware flow control.

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

### 3D Printing GPIO Use Cases

- **Print completion indicator** — LED or buzzer triggered when print finishes
- **Enclosure fan control** — GPIO-driven relay for enclosure ventilation
- **Door sensor** — detect enclosure door open/close
- **Emergency stop** — hardware E-stop button via GPIO input
