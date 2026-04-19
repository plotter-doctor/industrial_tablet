# Getting Started

First-time setup guide for the 7" Industrial Touch Panel — HMI Pro.

---

## What's in the Box

- 1× 7" Industrial Touch Panel (HMI Pro pre-configured)
- 1× 24V DC cable with matching 2-pin connector
- 1× External power button
- 1× WiFi antenna
- 4× Mounting screws

---

## Quick Start (5 Minutes)

### 1. Power On

Connect the **24V DC cable** to the 2-pin connector (share your DIN rail 24V PSU), or plug a **5V/2A USB charger** into the Micro-USB OTG port.

The tablet will boot automatically and launch your HMI dashboard within ~60 seconds.

### 2. Connect to WiFi

If your WiFi is not pre-configured:

1. Swipe down from the top to open the notification shade
2. Tap the **WiFi** icon → select your plant/office network → enter password
3. HMI apps will reconnect to your Modbus TCP / MQTT devices automatically

### 3. Connect to Your Equipment

#### Using TeslaModbusSCADA (Modbus TCP)

1. Open TeslaModbusSCADA from the home screen
2. Create a new project → add a Modbus TCP device
3. Enter your PLC's IP address and port (default: 502)
4. Add register reads/writes and build your dashboard

#### Using TeslaModbusSCADA (Modbus RTU)

1. Plug a USB RS-485 adapter into a USB Host pin header
2. Open TeslaModbusSCADA → select serial port `/dev/ttyUSB0`
3. Configure baud rate, parity, and slave address
4. Start polling registers

#### Using Virtuino 6 (Custom Dashboard)

1. Open Virtuino 6 from the home screen
2. Create a new project with drag-and-drop widgets (gauges, charts, switches)
3. Connect to your device via WiFi, Bluetooth, or serial
4. Design your custom HMI screen

#### Using Virtuino MQTT

1. Open Virtuino MQTT from the home screen
2. Configure your MQTT broker (IP, port, credentials)
3. Subscribe to topics and create dashboard widgets
4. Monitor and control MQTT-connected devices

#### Using Node-RED / Grafana (Web Dashboards)

1. Tap the Node-RED or Grafana shortcut on the home screen
2. Or open Chrome and enter your server URL
3. Full web interface with Chrome 119 rendering

---

## Connecting Modbus RTU Devices

### USB RS-485 Adapter (Recommended)

1. Plug a USB-to-serial adapter into one of the USB Host pin headers
   - **FTDI FT232** adapters: industrial-grade, widest compatibility
   - **CH341** adapters: budget-friendly option
   - **CP210x** adapters: Silicon Labs industrial adapters
2. Connect the adapter's RS-485 A/B terminals to your Modbus bus
3. The appropriate driver loads automatically
4. The device appears as `/dev/ttyUSB0`

### Direct UART (Advanced)

1. Wire UART0 TX → Device RX, UART0 RX → Device TX, GND → GND
2. **Signals are 3.3V TTL** — use a level shifter for RS-232 or RS-485
3. Access via `/dev/ttyS0`

---

## Mounting

### Control Cabinet Door

1. Mark four screw positions on the cabinet door panel
2. Drill pilot holes
3. Secure the tablet using the 4× M3 screws
4. Route the power cable inside the cabinet

### DIN Rail (with adapter)

Use a VESA-to-DIN-rail bracket to mount the panel alongside your PLCs and I/O modules.

### Wall / Enclosure Mount

1. Mark four screw positions on the wall or enclosure
2. Drill pilot holes and insert wall anchors (for drywall)
3. Secure the tablet and route power cable

---

## Power Options

### 24V DC from DIN Rail PSU (Recommended)

- Connect to the same 24V DC rail as your PLCs
- Panel powers on/off with the automation system
- Most convenient for permanent installations

### 5V USB

- Use any quality USB phone charger (minimum 2A recommended)
- Independent power — stays on when automation system is off
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

# Restart Virtuino
adb shell am force-stop com.virtuino_automations.virtuino
adb shell am start com.virtuino_automations.virtuino/.MainActivity

# Adjust screen brightness (0-255)
adb shell settings put system screen_brightness 200
```

---

## Re-Provisioning

If you need to re-run the full setup (e.g., after a factory reset):

```bash
git clone https://github.com/plotter-doctor/industrial_tablet.git
cd industrial_tablet
./setup_tablet.sh --profile hmi
```

This restores: HMI apps, Chrome WebView, boot branding, kiosk mode, kernel modules, WiFi config, and auto-start.

---

## Troubleshooting

| Problem | Solution |
|---|---|
| **Black screen on boot** | Wait 60 seconds. If still black, try a different power source (24V DC or USB). |
| **App doesn't auto-start** | Run `./setup_tablet.sh --profile hmi --status` to check. Re-run setup if needed. |
| **WiFi won't connect** | Ensure 2.4 GHz network. 5 GHz is not supported by built-in WiFi. Use USB dongle for 5 GHz. |
| **Modbus RTU not working** | Check USB serial adapter connection. Try `adb shell dmesg` for driver messages. Verify `/dev/ttyUSB0` exists. |
| **Modbus TCP timeout** | Verify PLC IP address, port (502), and that the tablet is on the same network/VLAN. |
| **MQTT connection failed** | Check broker IP, port (1883), and credentials. Ensure the broker allows connections from the tablet's IP. |
| **Screen too bright/dim** | Adjust via `adb shell settings put system screen_brightness <0-255>` |
