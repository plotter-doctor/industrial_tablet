# Getting Started

First-time setup guide for the 7" Industrial Touch Panel — Weather & Calendar Dashboard.

---

## What's in the Box

- 1× 7" Industrial Touch Panel (Weather Dashboard pre-configured)
- 1× 24V DC cable with matching 2-pin connector
- 1× External power button
- 1× WiFi antenna
- 4× Mounting screws

---

## Quick Start (5 Minutes)

### 1. Power On

Connect the **24V DC cable** to the 2-pin connector, or plug a **5V/2A USB charger** into the Micro-USB OTG port.

The tablet will boot automatically and launch the Weawow weather dashboard within ~60 seconds.

### 2. Connect to WiFi

WiFi is required for weather data. If not pre-configured:

1. Swipe down from the top to open the notification shade
2. Tap the **WiFi** icon → select your network → enter password
3. Weather data begins updating automatically

### 3. Configure Weawow

On first launch:

1. **Allow location access** — Weawow uses GPS/network location for local weather
2. **Choose your location** — or manually search for your city
3. Weather data, forecasts, and radar load automatically

The display shows current conditions, hourly forecast, daily forecast, and beautiful weather photography.

### 4. Set Up Calendar (Optional)

1. Swipe to the home screen and open **Etar Calendar**
2. Calendar permissions are pre-granted
3. To sync with your calendar:
   - **Google Calendar** — add your Google account in Android Settings → Accounts
   - **CalDAV server** — use a CalDAV sync adapter (e.g., DAVx⁵)
   - **Local calendars** — create events directly in Etar

### 5. Set Up Tasks (Optional)

1. Open **Tasks.org** from the home screen
2. Create task lists, set due dates, and organize with tags
3. Sync with CalDAV, Google Tasks, or use locally

---

## Switching Weather Apps

Two weather apps are pre-installed:

### Weawow (Default)

- Beautiful weather photography backgrounds
- Detailed hourly and daily forecasts
- Weather radar and maps
- Sunrise/sunset, UV index, wind, humidity
- No ads

### Pearl Weather

- Elegant animated weather backgrounds
- Clean, minimal interface
- Widget-friendly layout
- Good for at-a-glance weather viewing

To switch the default auto-start app, use ADB:

```bash
# Switch to Pearl Weather auto-start
adb shell am force-stop com.weawow
adb shell am start com.macropinch.pearl/.MainActivity
```

---

## Mounting

### Wall Mount (Recommended)

1. Choose a wall location at **eye height** with good visibility from across the room
2. Mark four screw positions using the tablet's mounting holes as a template
3. Drill pilot holes and insert wall anchors (for drywall) or drill directly (for wood/metal)
4. Secure the tablet using the 4× M3 screws
5. Route the power cable through the wall or use a surface-mount cable channel

**Ideal locations:**
- Kitchen — check weather while cooking
- Hallway — glance at forecast before heading out
- Home office — weather + calendar at a glance
- Living room — ambient information display

### Desk / Shelf Stand

Use any tablet stand or 3D-printed bracket. Position where it's visible from your workspace or seating area.

---

## Power Options

### 5V USB (Simplest for Home Use)

- Use any quality USB phone charger (minimum 2A recommended)
- Plug into any wall outlet
- The Micro-USB OTG port serves as both power input and data port

### 24V DC (For Commercial Installations)

- Connect to a 24V DC power supply
- Clean, hidden-cable installation behind walls
- Ideal for offices, lobbies, and commercial spaces

---

## Screen Brightness

The dashboard ships with **maximum brightness** for visibility from across the room. To adjust:

```bash
# Set brightness (0-255)
adb shell settings put system screen_brightness 200

# Enable auto-brightness (if ambient light sensor is sufficient)
adb shell settings put system screen_brightness_mode 1
```

For nighttime dimming in bedrooms, consider reducing brightness via ADB or a scheduled automation.

---

## ADB Access

The tablet has ADB enabled for configuration and maintenance:

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

# Restart Weawow
adb shell am force-stop com.weawow
adb shell am start com.weawow/.MainActivity

# Switch to Pearl Weather
adb shell am start com.macropinch.pearl/.MainActivity

# Adjust screen brightness (0-255)
adb shell settings put system screen_brightness 200

# Check WiFi status
adb shell dumpsys wifi | grep "mNetworkInfo"
```

---

## Re-Provisioning

If you need to re-run the full setup (e.g., after a factory reset):

```bash
git clone https://github.com/plotter-doctor/industrial_tablet.git
cd industrial_tablet
./setup_tablet.sh --profile weather
```

This restores: weather apps, Chrome WebView, boot branding, kiosk mode, WiFi config, brightness settings, and auto-start.

---

## Troubleshooting

| Problem | Solution |
|---|---|
| **Black screen on boot** | Wait 60 seconds. If still black, try a different power source (24V DC or USB). |
| **Weather not updating** | Check WiFi connection. Ensure 2.4 GHz network (5 GHz not supported). |
| **Weawow shows wrong location** | Open Weawow → Settings → Location. Search for your city manually. |
| **Calendar not syncing** | Verify your CalDAV account is configured in Android Settings → Accounts. |
| **Screen too bright at night** | Use `adb shell settings put system screen_brightness 50` to dim. |
| **App doesn't auto-start** | Run `./setup_tablet.sh --profile weather --status` to check. Re-run setup if needed. |
| **WiFi keeps disconnecting** | Attach the external WiFi antenna. Move closer to your router. |
| **Touch not responding** | Reboot the tablet. If persistent, check for screen damage. |
