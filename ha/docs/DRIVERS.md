# Driver & Module Guide

Complete list of pre-installed kernel modules and supported USB peripherals.

All modules are cross-compiled for **Linux 3.10.104 ARMv7** (Rockchip RK3128) and auto-loaded at boot via Android init services.

---

## USB Serial Adapters

These modules enable plug-and-play support for USB-to-serial adapters. Once plugged in, the device appears as `/dev/ttyUSB0` (or `/dev/ttyUSB1`, etc.).

| Module | Chipset | Common Devices |
|---|---|---|
| `ftdi_sio.ko` | FTDI FT232R, FT232H, FT2232, FT4232 | Most industrial USB-RS232/RS485 converters, JTAG adapters |
| `ch341.ko` | WCH CH340, CH341 | Arduino Nano clones, budget serial adapters |
| `cp210x.ko` | Silicon Labs CP2102, CP2104, CP2108 | NodeMCU, Pololu boards, industrial adapters |
| `pl2303.ko` | Prolific PL2303 | Older USB-serial cables, console cables |

### Typical Serial Use Cases

- **Modbus RTU** — Connect RS-485 adapter → talk to industrial sensors, energy meters, PLCs
- **3D Printer Control** — Serial link to Marlin/Klipper firmware boards
- **CNC Communication** — G-code streaming to GRBL controllers
- **Console Access** — Serial console to routers, switches, embedded systems
- **Sensor Reading** — Direct UART connection to environmental sensors

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

## USB LTE / 3G / 5G Modems

Full cellular connectivity stack for USB modems:

### Serial/AT Command Modules

| Module | Function | Typical Modems |
|---|---|---|
| `option.ko` | Generic modem serial ports | Huawei E3372, ZTE MF823, Quectel EC25, Sierra MC7455 |
| `qcserial.ko` | Qualcomm modem serial | Sierra Wireless, Quectel with Qualcomm chipset |
| `sierra.ko` | Sierra Wireless modems | MC73xx, MC74xx, EM74xx series |
| `qcaux.ko` | Qualcomm auxiliary serial | Additional diagnostic/GPS serial ports |
| `zte_ev.ko` | ZTE EVDO modems | ZTE AC2726, AC8710 |
| `usb_wwan.ko` | Base WWAN serial | Shared dependency for above modules |
| `cdc-acm.ko` | USB ACM (modem AT port) | Standard AT command interface |

### Data/Networking Modules

| Module | Protocol | Use Case |
|---|---|---|
| `usbnet.ko` | Base USB networking | Required dependency for all below |
| `cdc_ether.ko` | CDC Ethernet | Standard USB modem data interface |
| `cdc_ncm.ko` | CDC NCM | High-speed aggregated data |
| `cdc_mbim.ko` | CDC MBIM | Mobile broadband (Windows-compatible) |
| `qmi_wwan.ko` | QMI (Qualcomm) | Native Qualcomm data interface (fastest) |
| `rndis_host.ko` | RNDIS | Windows-compatible USB networking |
| `sierra_net.ko` | Sierra DirectIP | Sierra Wireless proprietary data |
| `hso.ko` | Option HSO | Legacy 3G/HSDPA modems |

### Module Load Order

For USB modems, dependencies must be loaded in order:

```
usbnet.ko → cdc_ether.ko → cdc_ncm.ko → cdc_mbim.ko
                                       → qmi_wwan.ko
                                       → rndis_host.ko
                                       → sierra_net.ko

usb_wwan.ko → option.ko
            → qcserial.ko
            → sierra.ko
```

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

---

## Building Custom Modules

If you need a kernel module not included in the pre-built set, refer to the Kernel Module Build Guide for cross-compilation instructions. Key points:

- Kernel version: `3.10.104`
- Architecture: `arm` (ARMv7)
- Cross-compiler: `arm-linux-gnueabihf-gcc`
- Critical flag: `-fno-pic -fno-pie` (required for GCC 14+)
- Vermagic must match: `3.10.104 SMP preempt mod_unload ARMv7 p2v8`
