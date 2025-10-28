# esp-fingerprint-doorbell
ESP32 WROOM + Grow R503 fingerprint sensor integration with Home Assistant and Nuki 3 Pro for secure door unlocking with color feedback.

# ESP Fingerprint Door — ESPHome + Home Assistant + Nuki 3 Pro

**Short:** Short touch = ring the doorbell. Hold longer = scan for fingerprint → Home Assistant decides and unlocks the Nuki 3 Pro.

This project is a local-first, auditable, hacker-friendly way to add fingerprint entry to a Nuki smartlock using:
- ESP32 WROOM running **ESPHome**,
- Grow **R503** fingerprint sensor (RGB/aura ring variant recommended),
- **Home Assistant** to hold lock credentials and run the unlock policy.

Why this design?
- Keep credentials & policies in one trusted place (Home Assistant).  
- Keep the edge device (ESP) simple: sense + feedback only.  
- UX: short press = bell (no unlock), long press = explicit unlock intent (safer, deliberate).

---

## What you get in this repo
- `esphome/fpr_nuki.yaml` — ESPHome firmware ready to flash (edit the placeholders).  
- `home_assistant/automations.yaml` — example automations to unlock and ring.  
- `docs/wiring.svg` — place your wiring diagram here.  
- `.gitignore`, `CONTRIBUTING.md`, `LICENSE` (MIT).

---

## Quick start (bench checklist)

1. Clone repo and open `esphome/fpr_nuki.yaml`. Replace:
   - `YOUR_WIFI_SSID`, `YOUR_WIFI_PASS`
   - optional: `api_key`, `ota_password`
   - `uart.rx_pin` / `uart.tx_pin` if needed
   - `sensing_pin`, `button_pin`
   - `tag_map` (finger id → username)
2. Flash the ESP via USB:
   ```bash
   esphome run esphome/fpr_nuki.yaml
   ```

3. Check logs in the ESPHome dashboard. Expect `fingerprint_grow` to initialize.
4. Add the device to Home Assistant via ESPHome integration.
5. Import / copy `home_assistant/automations.yaml` into HA and replace `lock.front_door_nuki3pro` with your Nuki lock entity.
6. Test:

   * Short press → check HA Developer Tools → Events for `doorbell_pressed`.
   * Long press → a scan window opens (10s). If finger matches, HA receives `fpr_unlock_request` event and unlocks (if automation allows).

---

## Wiring summary (short)

* R503 VCC → **3.3V** (ESP32 3V3)
* R503 GND → **GND** (common)
* R503 TXD → **ESP32 RX** (uart rx_pin)
* R503 RXD → **ESP32 TX** (uart tx_pin)
* R503 3.3VT → **3.3V** or `sensor_power_pin` (if present)
* Optional: R503 WAKEUP → `sensing_pin` (GPIO)

If the R503 aura LED is flaky on your module, wire an external NeoPixel ring (see comments in YAML).

---

## Security notes

* Keep Nuki credentials/tokens in Home Assistant only.
* Fingerprint templates remain on the sensor — do not publish them.
* Add conditions in HA automations (time, presence) for added security if desired.

---

## Troubleshooting tips

* `fingerprint_grow` marked FAILED → check UART pins & 3.3V/3.3VT wiring.
* LED not responding → try the loading animation switch in ESPHome UI, or add an external NeoPixel fallback.
* If an ESPHome upgrade causes instability, test on the bench and pin a working ESPHome version.

---

## Sources & inspiration

ESPHome docs (`fingerprint_grow`), FingerprintDoorbell community projects, SmartHomeYourself R503 guides, Home Assistant Nuki integration docs.
