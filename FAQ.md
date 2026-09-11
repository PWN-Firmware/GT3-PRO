<p align="center">
  🇬🇧 <strong>English</strong>
  &nbsp;·&nbsp;
  <a href="FAQ.ru.md">🇷🇺 Русский</a>
</p>

# How to dump the Segway GT3 Pro VCU

A full VCU Flash dump is a safety copy of **your** controller’s stock firmware. Make it **before** writing anything to the chip. Dump size: **128 KB (131072 bytes)**.

Tool: [x3utils](https://github.com/ztakis/x3utils). It talks to the VCU over ST-LINK / SWD.

> **Caution.** A bad connection, wrong wiring, or an interrupted flash can leave the VCU unusable until it is recovered. Always take a full dump first and keep the file somewhere safe. You do this at your own risk.

This guide covers **read** operations only (connection check and Backup). Do **not** use Backup + Flash, Flash Only, SHU compatible, or Unlock / Rescue just to dump. **Flash SHU Compatible is not supported on GT3 / GT3 Pro at any VCU version.**

Walkthrough video (x3utils web UI): [https://youtu.be/xceug-i_RRA](https://youtu.be/xceug-i_RRA)

---

## What you need

- GT3 Pro VCU board (MCU **AT32F415CBT7**) with access to the SWD test pads
- An **ST-LINK** programmer (genuine / Nucleo, or a common clone) and a USB cable that carries data, not charge-only
- Wires for **GND**, **3.3V**, **DIO (SWDIO)**, **CLK (SWCLK)**; for a genuine ST-LINK also **RST / NRST** to C45
- Power for the VCU (scooter / main connector). Do not power the board from the scooter and from the programmer’s 3.3V output at the same time
- A computer or Android phone: Chrome or Edge. Firefox, Safari, and iPhone / iPad browsers **do not** support WebUSB

---

## 1. Wire the ST-LINK to the board

Connect the wires **exactly as in the photo**. Left to right on the pad row: **GND**, **3.3v**, **DIO**, **CLK**. Reset is separate, on **GND**.

<p align="center">
  <img src="imgs/gt3_vcu.jpeg" alt="ST-LINK pad map on the GT3 Pro VCU" width="720">
</p>

| Pad on the board | ST-LINK signal | Role |
| ---------------- | -------------- | ---- |
| **GND** | `GND` | Common ground. SWD will not work without it |
| **3.3v** | `3.3V` / `VAPP` / `VTarget` | Target 3.3V reference. On a genuine ST-LINK this is usually **sense**, not board power |
| **DIO** | `SWDIO` / `DIO` | SWD data |
| **CLK** (labelled ClK on the photo) | `SWCLK` / `CLK` | SWD clock |
| **RST\RESET (C45)** | `GND` | MCU reset. Required for **C45 Genuine**. The arrow points at the via next to capacitor C45, by the `RESET` silkscreen |

Practical rules:

- Keep the wires short and mechanically stable.
- Do not swap **DIO** and **CLK**.
- Power the VCU from **one** source: either the main connector / scooter, or (separately) ST-LINK 3.3V if you are deliberately powering the board from the programmer only. **Never both at once.**
- **C45 Genuine:** leave the programmer `GND` connected to **RST\RESET (C45)** the whole time.
- **C45 Clone:** usually **do not** use `NRST` (clone reset pins are often dead). When the tool asks, short **C45 to GND**, then release.

---

## 2. Dump with x3utils

Pick one path. The Chrome web app is the shortest if the browser can see the ST-LINK.

Official x3utils guides:

- Windows: [x3utils_win/README.md](https://github.com/ztakis/x3utils/blob/main/x3utils_win/README.md)
- macOS: [x3utils_mac/README.md](https://github.com/ztakis/x3utils/blob/main/x3utils_mac/README.md)
- Linux: [x3utils_linux/README.md](https://github.com/ztakis/x3utils/blob/main/x3utils_linux/README.md)
- GUI / CLI / APK releases: [github.com/ztakis/x3utils/releases](https://github.com/ztakis/x3utils/releases)
- Wiki: [github.com/ztakis/x3utils/wiki](https://github.com/ztakis/x3utils/wiki)

### Chrome / Edge on a computer

1. Open [https://x3utils-web.pages.dev/](https://x3utils-web.pages.dev/) in **Chrome** or **Edge**.
2. Click in the order shown on the screenshot (1 → 2 → 3):

<p align="center">
  <img src="imgs/x3utils.jpeg" alt="x3utils-web click order: Check connection, C45 Genuine, Connect ST-Link" width="900">
</p>

| Step | Click | 
| ---- | ----- |
| **1** | **Check connection** under Actions |
| **2** | **C45 - Genuine** (Hardware nRST) under Advanced |
| **3** | **Connect ST-Link** |

3. When the probe succeeds, under Actions choose **Backup** (dump ~128 KB) and save the file.
4. Do not close the tab or move the wires until the dump finishes.

Labels can differ slightly in newer site versions; the flow is the same: probe, C45 mode, connect ST-LINK, then Backup. Click-by-click walkthrough: [video](https://youtu.be/xceug-i_RRA).

### Android (Chrome)

Same WebUSB flow, Chrome only:

- [https://x3utils-web.pages.dev/](https://x3utils-web.pages.dev/)
- phone layout: [https://x3utils-web.pages.dev/m/](https://x3utils-web.pages.dev/m/)

Same order: **Check connection** → **C45 Genuine** (or **C45 Clone**) → **Connect ST-Link** → **Backup**.

You can also sideload the arm64 APK from [x3utils releases](https://github.com/ztakis/x3utils/releases). Safari / Firefox / iPhone will not work with WebUSB.

### Windows

Two options: the GUI from the releases, or the CLI.

**CLI** ([guide](https://github.com/ztakis/x3utils/blob/main/x3utils_win/README.md)):

1. Download and unzip x3utils, open the `x3utils_win` folder.
2. Run `launcher.bat`.
3. Select mode **C** — C45 / Genuine ST-LINK, or **B** — C45 / Clone ST-LINK.
4. Option **1. Check Connection**.
5. Option **2. Backup Full Memory (128 KB)**.
6. Keep the resulting `.bin`.

If Windows does not show the ST-LINK in Device Manager (no yellow warning icon), the scripts cannot help yet: try another cable, another port, or the ST-LINK driver.

**GUI:** download a current desktop build from the [releases page](https://github.com/ztakis/x3utils/releases) and do the same actions: C45 mode → connection check → Backup.

### macOS

This is not a `.dmg` and there is no drag-to-Applications app. You need Homebrew and Terminal. Full guide: [x3utils_mac/README.md](https://github.com/ztakis/x3utils/blob/main/x3utils_mac/README.md).

```bash
cd x3utils_mac
chmod +x installer.sh
./installer.sh
./launcher.sh
```

In the launcher: mode **C** (Genuine) or **B** (Clone) → **1** Check Connection → **2** Backup Full Memory (128 KB).

Or use the GUI from the x3utils releases.

### Linux

Full guide: [x3utils_linux/README.md](https://github.com/ztakis/x3utils/blob/main/x3utils_linux/README.md).

```bash
cd x3utils_linux
chmod +x *.sh oocd/bin/openocd
./launcher.sh
```

Same launcher items: C45 mode → Check Connection → Backup 128 KB. If OpenOCD cannot open the ST-LINK as a normal user, install the udev rules from the x3utils guide and replug the adapter.

Or use the GUI / AppImage from the releases.

---

## 3. What a good dump looks like

- The file exists and is **exactly 131072 bytes**.
- It is not all `0xFF` and not a single repeated byte.
- Check connection already succeeded before the dump.
- Keep a copy off the Downloads folder: this is a snapshot of **your** controller and is the way back to stock.

If the dump fails, **do not** flash, and **do not** run Unlock / Rescue “just in case”: rescue rewrites protection and can mass-erase main Flash.

---

## If it will not connect

1. Recheck **GND / 3.3v / DIO / CLK** against the photo. For Genuine, also check the **C45** wire.
2. Confirm the board is powered and you are using **one** power source.
3. Clone → **C45 Clone** (hold C45 to GND when prompted). Genuine with `NRST` → **C45 Genuine**.
4. The computer must see the ST-LINK over USB.
5. In Clone mode, increase the countdown if you cannot short C45 in time.
6. Do not let the wires move during probe or dump.

OpenOCD and USB details: [x3utils Troubleshooting](https://github.com/ztakis/x3utils/wiki/31.-Troubleshooting).
