<p align="center">
  <img src="imgs/logo.png" alt="PWN — VCU firmware for scooters" width="240">
</p>

<p align="center">
  🇬🇧 <strong>English</strong>
  &nbsp;·&nbsp;
  <a href="README.ru.md">🇷🇺 Русский</a>
</p>

# PWN Firmware for Segway GT3 Pro
PWN is custom **VCU** (scooter control module) firmware for Segway **GT3 Pro**. It is a complete firmware written from scratch, not another stock patch and **NOT an AI-generated modification**. It replaces the stock VCU application on the controller and stays compatible with the original TFT display, BLE module, motor controllers, and battery.

The firmware is built for the GT3 Pro VCU. It talks to the other scooter modules in Segway’s native protocol, so the rest of the scooter keeps working as a normal GT3 Pro.

## Features beyond stock

These settings are not in the stock firmware. They are configured in the **PowerNine** app over BLE and stored on the VCU.

- Emergency mode (police mod): default on/off and a speed cap of 3–50 km/h
- Infinite boost
- Motor current limit (Iq) separately for Eco / Sport / Race: Stock or 1–200 A
- Battery current limit separately for Eco / Sport / Race: Stock or 1–150 A
- Beeper volume 0–100%
- Toggle orientation: light+ / light− and mode+ / mode−
- Assignable hold actions for keys: left and right turn signal, light− / light+, mode− / mode+, left brake + BOOST, right brake + CRUISE
- Available options: hazard lights, high-beam strobe, enable / disable / toggle emergency, toggle position light
- Strobe frequency setting 1–25 Hz

Stock means the factory limit for that mode, not zero current. Current values are the maximum threshold sent to the controller, not the current actually applied.

The official Segway-Mobility app can still be used for everyday pairing and stock functions.

## Bug reports and feature requests

If you find a bug or want to request a feature, please open an issue: [github.com/PWN-Firmware/GT3-Pro/issues](https://github.com/PWN-Firmware/GT3-Pro/issues)

## Disclaimer

Custom firmware can damage the scooter, void the warranty, and is used **at your own risk**.