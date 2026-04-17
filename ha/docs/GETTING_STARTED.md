# Getting Started

First-time setup guide for the 7" Industrial Touch Panel.

---

## What's in the Box

- 1× 7" Industrial Touch Panel (Home Assistant pre-configured)
- 1× 24V DC cable with matching 2-pin connector
- 1× External power button
- 1× WiFi antenna
- 4× Mounting screws

---

## Quick Start (5 Minutes)

### 1. Power On

Connect the **24V DC adapter** to the barrel jack, or plug a **5V/2A USB charger** into the Micro-USB OTG port.

The tablet will boot automatically and launch Home Assistant Companion within ~60 seconds.

### 2. Connect to WiFi

If your WiFi is not pre-configured:

1. Swipe down from the top to open the notification shade
2. Tap the **WiFi** icon → select your network → enter password
3. Home Assistant will reconnect automatically

### 3. Configure Home Assistant

On first launch, the HA Companion app will ask you to connect to your Home Assistant instance:

1. Enter your HA server URL (e.g., `http://192.168.1.100:8123`)
2. Log in with your HA credentials
3. Choose which dashboard to display

The panel will remember this configuration and auto-connect on every boot.

---

## Mounting

### Wall Mount

1. Mark four screw positions using the tablet's mounting holes as a template
2. Drill pilot holes and insert wall anchors (for drywall) or drill directly (for wood/metal)
3. Secure the tablet using the 4× M3 screws
4. Route the power cable through the wall or use a surface-mount cable channel

### DIN Rail Mount

Use a standard DIN rail adapter plate with M3 holes. Secure the tablet to the plate, then clip onto DIN rail inside your electrical cabinet.

### Flush Panel Mount

1. Cut a rectangular opening in the panel (check dimensions for your specific unit)
2. Insert the tablet from the front
3. Secure from behind using the M3 mounting screws and optional brackets

---

## Power Options

### 24V DC (Recommended for Industrial)

- Connect to your cabinet's 24V DC rail
- Shares power bus with PLCs, sensors, and other industrial equipment
- Most reliable for permanent installations

### 5V USB

- Use any quality USB phone charger (minimum 2A recommended)
- Ideal for residential or office installations
- The Micro-USB OTG port #1 serves as both power input and data port

### Battery Backup

The internal Li-ion battery provides a brief buffer during power transitions. It is not designed for extended mobile use — this is a panel device.

---

## Connecting Serial Devices

### Hardware UART (Built-in)

The tablet's two UART pins provide direct 3.3V TTL serial:

1. Connect your device's TX to the tablet's RX, and vice versa
2. Connect grounds together
3. Access via `/dev/ttyS0` (UART0) or `/dev/ttyS1` (UART1)

### USB Serial Adapter

1. Plug a USB-to-serial adapter into one of the USB-A host ports
2. The appropriate driver loads automatically (FTDI, CH341, CP210x, or PL2303)
3. The device appears as `/dev/ttyUSB0`

### RS-485 / Modbus

For Modbus RTU communication with industrial devices:

1. Connect a USB-RS485 adapter (FTDI or CH341 recommended)
2. Wire the A/B lines to your Modbus devices
3. Configure your HA Modbus integration with the serial device path

---

## Connecting USB Peripherals

### USB WiFi Dongle

Plug a supported Realtek WiFi dongle into a USB-A port. The driver loads automatically. Use Android WiFi settings to connect to a network via the dongle.

### USB LTE/3G Modem

1. Insert a SIM card into your USB modem
2. Plug the modem into a USB-A host port
3. The modem's networking driver loads automatically
4. Configure APN settings through Android network settings

### USB Bluetooth Dongle

Plug a BT 4.0/5.0 USB dongle for extended Bluetooth range or if the built-in BT is insufficient.

---

## ADB Access

The tablet has ADB enabled for development and maintenance:

### USB Connection

```bash
adb devices              # verify connection
adb shell                # open shell
```

### WiFi Connection (Frees USB Ports)

```bash
adb tcpip 5555
adb connect <TABLET_IP>:5555
adb shell
```

### Useful Commands

```bash
# Check system status
adb shell getprop ro.build.display.id     # firmware version
adb shell cat /proc/version               # kernel version
adb shell cat /proc/modules               # loaded kernel modules

# Restart Home Assistant Companion
adb shell am force-stop io.homeassistant.companion.android.minimal
adb shell am start io.homeassistant.companion.android.minimal/.launch.LaunchActivity

# Adjust screen brightness (0-255)
adb shell settings put system screen_brightness 200

# Keep screen always on
adb shell settings put global stay_on_while_plugged_in 3
```

---

## Re-Provisioning

If you need to re-run the full setup (e.g., after a factory reset):

```bash
git clone https://github.com/plotter-doctor/industrial_tablet.git
cd industrial_tablet
./setup_tablet.sh
```

This restores: HA Companion, Chrome WebView, boot branding, kiosk mode, kernel modules, WiFi config, and auto-updater.

---

## Troubleshooting

| Problem | Solution |
|---|---|
| **Black screen on boot** | Wait 60 seconds. If still black, try a different power source (24V DC or USB). |
| **HA doesn't auto-start** | Run `./setup_tablet.sh --status` to check. Re-run setup if needed. |
| **WiFi won't connect** | Ensure 2.4 GHz network. 5 GHz is not supported by built-in WiFi. Use USB dongle for 5 GHz. |
| **USB device not recognized** | Try the other USB-A port. Check `adb shell dmesg` for driver messages. |
| **Serial port not working** | Verify wiring (TX↔RX crossover). Check baud rate matches your device. |
| **Screen too bright/dim** | Adjust via `adb shell settings put system screen_brightness <0-255>` |
| **Touch not responding** | Reboot the tablet. If persistent, check for screen damage. |
