<p align="center">
  <img src="docs/preview.png" alt="SumiBoy" width="360">
</p>

<h1 align="center">SumiBoy</h1>
<p align="center"><strong>E-Ink Game Boy for the Xteink X4</strong></p>
<p align="center">
  <a href="https://psychoplath9450.github.io/sumi-emulator/">⚡ Flash Now</a> · 
  <a href="https://github.com/psychoplath9450/sumi-emulator/releases">Releases</a>
</p>

---

Play **Pokémon Red** on your SUMI's 4.26" e-ink display. Full Game Boy CPU emulation with per-pixel Bayer dithering, save states, deep sleep, and 12 ROM patches optimized for e-paper.

No build tools needed — flash directly from your browser in 60 seconds.

## → [Flash Now](https://psychoplath9450.github.io/sumi-emulator/)

Open in **Chrome** or **Edge**, plug in your SUMI via USB-C, click the button. That's it.

---

## Features

- **Full GB emulation** — Z80-style CPU, MBC3 banking, 32KB SRAM
- **E-ink dithering** — 4×4 Bayer matrix, 4 Game Boy shades → crisp 1-bit black/white
- **Save system** — Full state saves + SRAM persist, smart auto-save that skips idle writes
- **Deep sleep** — ~20µA with random Pokémon sleep art, instant wake
- **12 ROM patches** — Instant text, fast fades, faster walking, starting items, skipped intros
- **Adaptive performance** — Fewer CPU cycles when screen is static, preserves battery

## Controls

| Input | Action |
|-------|--------|
| D-Pad | Movement |
| A (Confirm) | Select / interact |
| B (Back) | Cancel / run |
| PWR tap | Full screen refresh |
| PWR hold ~2s | Save + open in-game menu |
| PWR hold ~3s | Save + deep sleep |
| PWR (asleep) | Wake up |

## Manual Flash

If you prefer not to use the web flasher, download the `.bin` from [Releases](../../releases) and flash with esptool:

```
python -m esptool --chip esp32c3 write_flash 0x0 SUMI-v2.6.0-full.bin
```

## Compatibility

**Hardware:** SUMI e-reader (Xteink X4) — ESP32-C3 + GDEQ0426T82 4.26" e-ink display

This firmware replaces SUMI entirely. To revert, re-flash the stock SUMI firmware the same way.

## FAQ

**Will this brick my device?**  
No. ESP32-C3 can always be recovered over USB. Hold the boot button while connecting to enter flash mode.

**Can I go back to stock?**  
Yes. Re-flash the original SUMI firmware using the web flasher or esptool.

**First boot slow?**  
Only once — it formats a small save partition. Subsequent boots are near-instant.

---

## Attributions

- [GxEPD2](https://github.com/ZinggJM/GxEPD2) by ZinggJM — e-ink display driver (GPL v3)
- [ESP Web Tools](https://esphome.github.io/esp-web-tools/) by Nabu Casa — browser-based flashing (Apache 2.0)
- [esptool-js](https://github.com/espressif/esptool-js) by Espressif — serial flash protocol (GPL v2)

---

<sub>Not affiliated with Nintendo, The Pokémon Company, or Xteink.</sub>
