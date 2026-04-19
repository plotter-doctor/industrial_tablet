# Getting Started

First-time setup guide for the 7" Industrial Touch Panel — GRBL CNC Controller.

---

## What's in the Box

- 1× 7" Industrial Touch Panel (GRBL CNC Controller pre-configured)
- 1× 24V DC cable with matching 2-pin connector
- 1× External power button
- 1× WiFi antenna
- 4× Mounting screws

---

## Quick Start (5 Minutes)

### 1. Power On

Connect the **24V DC cable** to the 2-pin connector (share your CNC machine's 24V PSU), or plug a **5V/2A USB charger** into the Micro-USB OTG port.

The tablet will boot automatically and launch the GRBL Controller app within ~60 seconds. The ser2net bridge starts automatically in the background.

### 2. Connect to WiFi

If your WiFi is not pre-configured:

1. Swipe down from the top to open the notification shade
2. Tap the **WiFi** icon → select your workshop network → enter password

### 3. Connect to Your CNC Machine

#### Using GRBL Controller App (Recommended)

1. Plug a USB-to-serial adapter into a USB Host pin header
2. Connect the adapter to your GRBL board (CNC Shield, Arduino Mega + GRBL, grblHAL board)
3. GRBL Controller auto-detects the serial port
4. Jog, home, zero, and run G-code files immediately

#### Using Grbl Controller Web (via TCP Bridge)

1. Connect your GRBL board via USB serial (as above)
2. The ser2net bridge automatically exposes it on `127.0.0.1:8023`
3. Tap the **Grbl Controller** Chrome shortcut on the home screen
4. Connect to `ws://127.0.0.1:8023` in the web interface

#### Using jscut CAM

1. Tap the **jscut** Chrome shortcut
2. Import an SVG file
3. Configure toolpaths (pocket, inside, outside, engrave)
4. Generate G-code and copy to GRBL Controller

#### Using Serial USB Terminal (Debug)

1. Open Serial USB Terminal from the home screen
2. Select `/dev/ttyUSB0` and set baud rate to 115200
3. Type GRBL commands directly: `$$`, `$H`, `G0 X10 Y10`, etc.

---

## Connecting Your GRBL Board

### USB Serial Adapter (Recommended)

1. Plug a USB-to-serial adapter into one of the USB Host pin headers
   - **CH341** adapters: Arduino-based GRBL shields (CNC Shield v3, most common)
   - **FTDI FT232** adapters: industrial-grade, widest compatibility
   - **CP210x** adapters: grblHAL boards, Teensy controllers
2. Connect the adapter to your GRBL board's USB or serial header
3. The appropriate driver loads automatically
4. The device appears as `/dev/ttyUSB0`
5. ser2net bridge activates on TCP port 8023

### Direct UART (Advanced)

1. Wire UART0 TX → Board RX, UART0 RX → Board TX, GND → GND
2. **Signals are 3.3V TTL** — use a level shifter for 5V boards
3. Access via `/dev/ttyS0`
4. Note: ser2net bridge is configured for `/dev/ttyUSB0` by default — modify config for UART0

---

## TCP-to-Serial Bridge (ser2net)

The ser2net daemon starts automatically on boot:

| Setting | Value |
|---|---|
| **Listen** | `127.0.0.1:8023` (localhost only) |
| **Serial Device** | `/dev/ttyUSB0` |
| **Baud Rate** | 115200 |
| **Flow Control** | None |

Web apps in Chrome connect to `127.0.0.1:8023` via WebSocket or raw TCP. This replaces the WebSerial API that Chrome on Android 7 doesn't support.

To verify the bridge is running:

```bash
adb shell netstat -tlnp | grep 8023
```

---

## Mounting

### CNC Machine Mount

1. Mark four screw positions on your machine enclosure using the tablet's mounting holes as a template
2. Drill pilot holes
3. Secure the tablet using the 4× M3 screws
4. Route the power cable through your enclosure

### Wall / Desk Mount

Use a small stand or wall bracket with M3 holes. Position at a comfortable height for machine operation.

---

## Power Options

### 24V DC from CNC PSU (Recommended)

- Tap into your CNC machine's 24V DC rail
- Panel powers on/off with the machine
- Most convenient for permanent installations

### 5V USB

- Use any quality USB phone charger (minimum 2A recommended)
- Independent power — stays on when CNC machine is off
- The Micro-USB OTG port serves as both power input and data port

---

## ADB Access

The tablet has ADB enabled for development and maintenance:

### USB Connection

```bash
adb devices              # verify connection
adb shell                # open shell
```

### WiFi Connection

```bash
adb tcpip 5555
adb connect <TABLET_IP>:5555
adb shell
```

### Useful Commands

```bash
# Check system status
adb shell getprop ro.build.display.id     # firmware version
adb shell cat /proc/modules               # loaded kernel modules

# List serial devices
adb shell ls -la /dev/ttyUSB* /dev/ttyS*

# Check ser2net bridge
adb shell netstat -tlnp | grep 8023

# Restart GRBL Controller
adb shell am force-stop in.co.gorest.grblcontroller
adb shell am start in.co.gorest.grblcontroller/.MainActivity

# Adjust screen brightness (0-255)
adb shell settings put system screen_brightness 200
```

---

## Re-Provisioning

If you need to re-run the full setup (e.g., after a factory reset):

```bash
git clone https://github.com/plotter-doctor/industrial_tablet.git
cd industrial_tablet
./setup_tablet.sh --profile grbl
```

This restores: CNC apps, ser2net bridge, Chrome WebView, boot branding, kiosk mode, kernel modules, WiFi config, and auto-start.

---

## Troubleshooting

| Problem | Solution |
|---|---|
| **Black screen on boot** | Wait 60 seconds. If still black, try a different power source (24V DC or USB). |
| **App doesn't auto-start** | Run `./setup_tablet.sh --profile grbl --status` to check. Re-run setup if needed. |
| **WiFi won't connect** | Ensure 2.4 GHz network. 5 GHz is not supported by built-in WiFi. Use USB dongle for 5 GHz. |
| **GRBL board not detected** | Check USB serial adapter connection. Try `adb shell dmesg` for driver messages. Verify `/dev/ttyUSB0` exists. |
| **ser2net not running** | Check with `adb shell netstat -tlnp \| grep 8023`. Reboot tablet or re-run setup. |
| **Web sender can't connect** | Verify ser2net is running on port 8023. Try `adb shell nc 127.0.0.1 8023` to test. |
| **GRBL alarm state** | Send `$X` to clear alarm, or `$H` to home. Check limit switches and wiring. |
| **Screen too bright/dim** | Adjust via `adb shell settings put system screen_brightness <0-255>` |
