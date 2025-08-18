# Chromium Kiosk Installer (Raspberry Pi + labwc + DSI/DPI display)

This script sets up **Chromium Browser** to run automatically in **kiosk mode** on a Raspberry Pi running Raspberry Pi OS with the **labwc (Wayland)** compositor.  
It is designed for systems using the Raspberry Pi’s **DSI/DPI ribbon cable displays** (not HDMI).  

---

## Features
- Detects DSI/DPI output via `wlr-randr` (e.g., `DSI-2`).
- Creates or updates `~/.config/labwc/autostart`.
- Asks for the kiosk URL interactively.
- Configures resolution and rotation for the detected panel.
- Launches Chromium in **kiosk mode** using Wayland (`--ozone-platform=wayland`).
- Backs up any existing `~/.config/labwc/autostart`.
- Optional backlight control for the official Raspberry Pi DSI panel.

---

## Requirements
- Raspberry Pi OS (Bookworm or later) with **labwc** desktop.
- Chromium browser (`chromium-browser` or `chromium` package).
- `wlr-randr` utility (usually in `wlroots` package).

The script will install missing dependencies automatically if you agree.

---

## Installation
1. Download or copy the script:
   ```bash
   nano install-chromium-kiosk.sh
   ```
   Paste the script content, save, and exit.

2. Make it executable:
   ```bash
   chmod +x install-chromium-kiosk.sh
   ```

3. Run the installer:
   ```bash
   ./install-chromium-kiosk.sh
   ```

4. Follow the prompts:
   - Enter the kiosk URL (e.g., `https://example.com`).
   - Confirm display output, resolution, and rotation.
   - Optionally reboot at the end.

---

## Usage
After installation, on every boot or login:
- `wlr-randr` ensures the DSI panel is active at the right resolution/rotation.
- Chromium starts in kiosk mode with your chosen URL.
- To exit kiosk mode:
  - Switch to TTY (`Ctrl+Alt+F2`), log in, then run:
    ```bash
    pkill chromium
    ```

---

## Configuration
- To change the kiosk URL or rotation later, edit:
  ```bash
  nano ~/.config/labwc/autostart
  ```

- To restore a backup of your autostart:
  ```bash
  cp ~/.config/labwc/autostart.bak.<timestamp> ~/.config/labwc/autostart
  ```

---

## Notes
- For landscape mode, use transform `90` or `270` when prompted.
- On official Raspberry Pi DSI displays, the script sets brightness to `200` (out of `255`). Adjust in `~/.config/labwc/autostart` if needed.
- To prevent the screen from blanking, add `consoleblank=0` to `/boot/firmware/cmdline.txt`.

---

## Uninstall
Remove the autostart file:
```bash
rm ~/.config/labwc/autostart
```

Reboot, and Chromium will no longer start automatically.
