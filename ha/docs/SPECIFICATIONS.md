# Technical Specifications

Detailed spec sheet for the 7" Industrial Touch Panel.

---

## System-on-Chip

| Parameter | Value |
|---|---|
| **SoC** | Rockchip RK3128 |
| **Architecture** | ARMv7-A (32-bit) |
| **CPU** | 4× ARM Cortex-A7 @ 1.2 GHz |
| **GPU** | ARM Mali-400 MP |
| **OpenGL** | ES 2.0 |
| **Process Node** | 28nm |

## Memory

| Parameter | Value |
|---|---|
| **RAM** | 1 GB DDR3 |
| **eMMC** | 8 GB (7.3 GB raw) |
| **System Partition** | 1.9 GB |
| **User Data** | ~3.6 GB available |
| **MicroSD** | Up to 64 GB (SDHC/SDXC) |
| **NTFS Support** | Yes (USB drives) |

## Display

| Parameter | Value |
|---|---|
| **Diagonal** | 7.0 inches |
| **Resolution** | 1024 × 600 px |
| **Pixel Density** | 160 DPI |
| **Type** | IPS LCD |
| **Refresh Rate** | 57 Hz |
| **Touch** | 5-point capacitive |
| **Touch Controller** | Integrated (jazzhand) |
| **Brightness Range** | 0–255 (software adjustable) |

## Power

| Parameter | Value |
|---|---|
| **Input Voltage (DC)** | 24V DC (2-pin connector) |
| **Input Voltage (USB)** | 5V DC |
| **Typical Consumption** | < 5W |
| **Operating Without Battery** | Supported (direct DC power) |

## Connectivity

| Interface | Details |
|---|---|
| **WiFi** | 802.11 b/g/n, 2.4 GHz, up to 72 Mbps |
| **Ethernet** | Via USB adapter (drivers included) |
| **3G/LTE** | Via USB modem (drivers included) |

## I/O Ports

| Port | Count | Specification |
|---|:---:|---|
| Micro-USB OTG | 1 | USB 2.0 OTG (doubles as power input) |
| USB OTG (pin header) | 1 | 4-pin connector, USB 2.0 OTG |
| USB Host (pin header) | 2 | 4-pin connectors, USB 2.0 |
| UART (Serial) | 1 | 3.3V TTL, UART0 |
| GPIO | 2 | 3.3V logic, software controllable |
| DC Power (2-pin) | 1 | 24V DC connector |
| Speaker Header | 1 | 8Ω speaker connection |
| Microphone Connector | 1 | Pin header for external microphone |
| MicroSD | 1 | Up to 64 GB |

## Sensors

| Sensor | Model | Range |
|---|---|---|
| **Accelerometer** | NXP MMA8451Q | ±2g/±4g/±8g, 100 Hz |

## Environmental

| Parameter | Value |
|---|---|
| **Operating Temperature** | 0°C to +50°C |
| **Storage Temperature** | -20°C to +60°C |
| **Cooling** | Passive (fanless) |
| **Humidity** | 10%–90% (non-condensing) |

## Mechanical

| Parameter | Value |
|---|---|
| **Mounting** | 4× M3 threaded holes |
| **Enclosure Material** | ABS / Polycarbonate |

## Software

| Parameter | Value |
|---|---|
| **OS** | Android 7.1.2 (API 25) |
| **Kernel** | Linux 3.10.104 |
| **Build Type** | userdebug (rooted) |
| **SELinux** | Permissive |
| **ADB** | Enabled (USB + TCP/IP) |
| **WebView** | Chrome 119 |
| **Home Assistant** | Companion (minimal) pre-installed |
| **Boot Mode** | Auto-start HA + kiosk mode |

## Certifications & Compliance

| Standard | Status |
|---|---|
| **CE** | Compliant |
| **RoHS** | Compliant |
| **FCC** | Part 15 Class B |
