# Getting Started

First-time setup guide for the 7" Industrial Touch Panel — Arduino Workbench.

---

## What's in the Box

- 1× 7" Industrial Touch Panel (Arduino Workbench pre-configured)
- 1× 24V DC cable with matching 2-pin connector
- 1× External power button
- 1× WiFi antenna
- 4× Mounting screws

---

## Quick Start (5 Minutes)

### 1. Power On

Connect the **24V DC cable** to the 2-pin connector, or plug a **5V/2A USB charger** into the Micro-USB OTG port.

The tablet will boot automatically and launch your workbench app within ~60 seconds.

### 2. Connect to WiFi

If your WiFi is not pre-configured:

1. Swipe down from the top to open the notification shade
2. Tap the **WiFi** icon → select your workshop network → enter password
3. Apps will reconnect automatically

### 3. Connect Your Arduino Board

#### USB Serial (Most Common)

1. Plug your Arduino/ESP board into a USB Host pin header
2. Open **Serial USB Terminal** from the home screen
3. Select the device (`/dev/ttyUSB0`) and set the baud rate
4. Serial data flows in real-time

#### Bluetooth Serial

1. Plug a USB Bluetooth dongle into a USB Host pin header
2. Pair with your HC-05/HC-06/BLE module in Android Bluetooth settings
3. Open **BT Serial Monitor** → select your paired device
4. Wireless serial — perfect for mobile robots and remote projects

#### WiFi Remote Control

1. Upload a RoboRemo or RemoteXY compatible sketch to your ESP8266/ESP32
2. Open **RoboRemo** or **RemoteXY** on the tablet
3. Connect to your board's IP address over WiFi
4. Control your project with custom buttons, joysticks, and sliders

#### Online Simulator

1. Tap the **Wokwi Simulator** shortcut on the home screen
2. Create or open an Arduino/ESP32 project
3. Run the simulation with virtual serial monitor — no hardware needed

---

## Connecting Your Board

### USB Host Pin Headers

The tablet has 2× USB Host pin headers. To connect a standard Arduino board:

1. Use a USB-A female breakout or adapter cable wired to the pin header
2. Plug your Arduino's USB cable (USB-B or micro-USB) into the adapter
3. The CH341/FTDI/CP210x driver loads automatically
4. Device appears as `/dev/ttyUSB0`

### Direct UART (Advanced)

1. Wire UART0 TX → Board RX, UART0 RX → Board TX, GND → GND
2. **Signals are 3.3V TTL** — matches 3.3V boards (ESP32, Arduino Due) directly
3. Use a level shifter for 5V boards (Arduino Uno, Mega)
4. Access via `/dev/ttyS0`

---

## Mounting

### Workbench Mount

1. Use a small stand or bracket with M3 holes
2. Position beside your breadboard or soldering station
3. Angle for easy viewing during development

### Shelf / Wall Mount

1. Mark four screw positions using the tablet's mounting holes as a template
2. Drill pilot holes
3. Secure the tablet using the 4× M3 screws

---

## Power Options

### 24V DC

- Use your bench power supply set to 24V
- Convenient for permanent workshop installations

### 5V USB (Simplest)

- Use any quality USB phone charger (minimum 2A recommended)
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

# Restart Serial USB Terminal
adb shell am force-stop de.kai_morich.serial_usb_terminal
adb shell am start de.kai_morich.serial_usb_terminal/.MainActivity

# Adjust screen brightness (0-255)
adb shell settings put system screen_brightness 200
```

---

## Re-Provisioning

If you need to re-run the full setup (e.g., after a factory reset):

```bash
git clone https://github.com/plotter-doctor/industrial_tablet.git
cd industrial_tablet
./setup_tablet.sh --profile arduino
```

This restores: Arduino apps, Chrome WebView, boot branding, kiosk mode, kernel modules, WiFi config, and auto-start.

---

## Troubleshooting

| Problem | Solution |
|---|---|
| **Black screen on boot** | Wait 60 seconds. If still black, try a different power source (24V DC or USB). |
| **App doesn't auto-start** | Run `./setup_tablet.sh --profile arduino --status` to check. Re-run setup if needed. |
| **WiFi won't connect** | Ensure 2.4 GHz network. 5 GHz is not supported by built-in WiFi. Use USB dongle for 5 GHz. |
| **Arduino not detected** | Check USB connection. Try `adb shell dmesg` for driver messages. Verify `/dev/ttyUSB0` exists. |
| **Serial data garbled** | Baud rate mismatch — ensure tablet and Arduino sketch use the same rate (9600, 115200, etc.). |
| **Bluetooth won't pair** | Ensure BT dongle is plugged in. Check `adb shell hciconfig` for adapter status. |
| **Screen too bright/dim** | Adjust via `adb shell settings put system screen_brightness <0-255>` |
