# How to install Retro Arch

## on Arch Linux

### 1 - Install basic package

```bash
sudo pacman -S retroarch retroarch-assets-glui retroarch-assets-ozone retroarch-assets-xmb
```

### 2 - Install some emulator cores

```bash
sudo pacman -S libretro-mame libretro-play libretro-snes9x libretro-picodrive libretro-mgba libretro-desmume
```

### 3 - Edit retroarch config

```bash
cd &&
vim .config/retroarch/retroarch.cfg
```

on this file, you must:
- set `core_updater_show_experimental_cores` to `true`
- set `menu_show_core_updater` to `true`

WIP (cuz is easier if i put my config file on github)
