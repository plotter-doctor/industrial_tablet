# Getting Started

First-time setup guide for the 7" Industrial Touch Panel — 3D Printer Dashboard.

---

## What's in the Box

- 1× 7" Industrial Touch Panel (3D Printer Dashboard pre-configured)
- 1× 24V DC cable with matching 2-pin connector
- 1× External power button
- 1× WiFi antenna
- 4× Mounting screws

---

## Quick Start (5 Minutes)

### 1. Power On

Connect the **24V DC cable** to the 2-pin connector (share your printer's 24V PSU), or plug a **5V/2A USB charger** into the Micro-USB OTG port.

The tablet will boot automatically and launch your printer dashboard within ~60 seconds.

### 2. Connect to WiFi

If your WiFi is not pre-configured:

1. Swipe down from the top to open the notification shade
2. Tap the **WiFi** icon → select your workshop network → enter password
3. Printer apps will reconnect automatically

### 3. Connect to Your Printer

#### Using Mobileraker (Klipper/Moonraker)

1. Open Mobileraker from the home screen
2. Tap **Add Printer** → enter your Moonraker IP (e.g., `http://192.168.1.100:7125`)
3. Your printer status, temperatures, and controls appear immediately

#### Using OctoRemote (OctoPrint)

1. Open OctoRemote from the home screen
2. Enter your OctoPrint server URL and API key
3. Monitor prints, control temperatures, view webcam feed

#### Using Octo4a (OctoPrint on Tablet)

1. Open Octo4a — it runs OctoPrint directly on the tablet
2. Connect your printer via USB serial adapter to a USB Host pin header
3. Access OctoPrint web UI at `http://localhost:5000`

#### Using OctoPrint / Mainsail / Fluidd Web UI

1. Tap the OctoPrint shortcut on the home screen
2. Or open Chrome and enter your server URL (e.g., `http://octopi.local:5000`)
3. Full web interface with Chrome 119 rendering

---

## Connecting Your Printer

### USB Serial Adapter (Recommended)

1. Plug a USB-to-serial adapter into one of the USB Host pin headers
   - **CH341** adapters: most Arduino-based boards (RAMPS, MKS Gen, SKR)
   - **FTDI FT232** adapters: industrial-grade, widest compatibility
   - **CP210x** adapters: Prusa, Duet boards
2. Connect the adapter to your printer board's USB or serial header
3. The appropriate driver loads automatically
4. The device appears as `/dev/ttyUSB0`

### Direct UART (Advanced)

1. Wire UART0 TX → Printer RX, UART0 RX → Printer TX, GND → GND
2. **Signals are 3.3V TTL** — use a level shifter for 5V boards
3. Access via `/dev/ttyS0`

---

## Mounting

### Printer Enclosure Mount

1. Mark four screw positions on your enclosure panel using the tablet's mounting holes as a template
2. Drill pilot holes
3. Secure the tablet using the 4× M3 screws
4. Route the power cable inside the enclosure

### Shelf / Desk Mount

Use a small stand or bracket with M3 holes. Position beside or above your printer for easy viewing during prints.

### Wall Mount

1. Mark four screw positions on the wall
2. Drill pilot holes and insert wall anchors (for drywall)
3. Secure the tablet and route power cable

---

## Power Options

### 24V DC from Printer PSU (Recommended)

- Tap into your printer's 24V DC rail
- Panel powers on/off with the printer
- Most convenient for permanent installations

### 5V USB

- Use any quality USB phone charger (minimum 2A recommended)
- Independent power — stays on when printer is off
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

# Restart Mobileraker
adb shell am force-stop com.mobileraker.android
adb shell am start com.mobileraker.android/.MainActivity

# Adjust screen brightness (0-255)
adb shell settings put system screen_brightness 200
```

---

## Re-Provisioning

If you need to re-run the full setup (e.g., after a factory reset):

```bash
git clone https://github.com/plotter-doctor/industrial_tablet.git
cd industrial_tablet
./setup_tablet.sh --profile 3dprint
```

This restores: printer apps, Chrome WebView, boot branding, kiosk mode, kernel modules, WiFi config, and auto-start.

---

## Troubleshooting

| Problem | Solution |
|---|---|
| **Black screen on boot** | Wait 60 seconds. If still black, try a different power source (24V DC or USB). |
| **App doesn't auto-start** | Run `./setup_tablet.sh --profile 3dprint --status` to check. Re-run setup if needed. |
| **WiFi won't connect** | Ensure 2.4 GHz network. 5 GHz is not supported by built-in WiFi. Use USB dongle for 5 GHz. |
| **Printer not detected** | Check USB serial adapter connection. Try `adb shell dmesg` for driver messages. Verify `/dev/ttyUSB0` exists. |
| **OctoPrint can't connect** | Verify baud rate matches printer (usually 115200). Check TX/RX wiring if using direct UART. |
| **Mobileraker can't find printer** | Ensure Moonraker is running on your Klipper host. Verify IP address and port (default: 7125). |
| **Screen too bright/dim** | Adjust via `adb shell settings put system screen_brightness <0-255>` |
