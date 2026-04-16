# Changelog

All notable changes to the Industrial Touch Panel project.

## [1.0.0] — 2026-04-16

### Initial Release

- Home Assistant Companion pre-installed with auto-start
- Chrome 119 WebView (upgraded from AOSP v52)
- Custom boot animation and splash screen
- Pre-compiled kernel modules:
  - USB serial: FTDI, CH341, CP210x, PL2303
  - USB WiFi: RTL8188EU, RTL8192CU, RTL8812AU, RTL8822BU, and more
  - USB Bluetooth: btusb, ath3k, btbcm203x
  - USB modems: option, qmi_wwan, cdc_ether, cdc_ncm, cdc_mbim, rndis_host
- Automated setup script (`setup_tablet.sh`)
- On-device HA auto-updater
- Performance optimizations for 24/7 kiosk operation
