# SumiBoy

E-Ink Game Boy emulator for the Xteink X4 (ESP32-C3, 4.26" e-ink display).

## Web Flasher

Visit the [SumiBoy Flasher](https://snugglelamb.github.io/sumi-emulator/) to install via browser (Chrome/Edge/Opera).

## Manual Flash

```
python -m esptool --chip esp32c3 write_flash 0x0 firmware.bin
```

## Updating Firmware

Replace `docs/firmware/firmware.bin` with the new build. No other files need to change.

## Controls

| Button | Action |
|--------|--------|
| D-Pad | Movement |
| A | Select / interact |
| B | Cancel / run |
| PWR tap | Full screen refresh |
| PWR hold ~2s | Save + open menu |
| PWR hold ~3s | Save + deep sleep |
| PWR (asleep) | Wake up |
