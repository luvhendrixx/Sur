<div align="centre">

## Sur

_A minimal setup focused on a balance of performance, aesthetics, productivity and a niche balance of keyboard and mouse driven workflows_

</div>

<p align="centre">
<img src="Assets/Sur.png" alt="Sur preview" width="100%" />
</p>

## TL;DR

- Arch + Hyprland (lua config) + Limine bootloader

- Waybar/Top bar (Quickshell)- Mac-OS like on with the file, edit, view etc stuff you see on an actual Mac,
  and yes, it also 'adapts' to what window your focused on

- Shell - zsh + starship

- Reminders — `ALT + R` opens a Liquid Glass reminder popover (name + duration or clock time, AM/PM or 24-hour), with a menu-bar icon, Control Center tile, due alerts (snooze / done) and a Settings → Reminders pane. Saved to `~/.config/macbar/reminders.json`. Future reminders survive reboot; overdue unacknowledged ones re-alert once after login.
- Lock-screen notifications — MacLock mirrors the latest notifications as normal glass cards (app, icon, time; previews stay hidden like macOS) and shows the next reminder as a live countdown, with Snooze / Done actions when one is due.

- Btfs file system

- For your Wallpapers, drag and drop them into the `~/Pictures/Wallpapers` directory (created automatically), SUPER + D to open and select your wallpaper
  (one is already provided by default since you'll boot into a black screen once the install is complete)

- Symlinks for direct and back-to-back reflection(s) if you want to change stuff.

- etc...

## Installation

```bash
git clone git@github.com:luvhendrixx/Sur.git ~/dotfiles
cd ~/dotfiles
chmod +x install.sh
./install.sh
```

(we recommend keeping this dir in your home directory since the custom update manager is hardcoded to "serve" from this directory. Same as keeping the dotfiles directory name, else update the update.sh file to match your preferences)

This, just to name a few...

- Installs pkgs
- Links every config
- setups up stuff like keyd and so on
- auto-detects your ESP mnt point and root UUID, sets up the bootloader, registers it in firmware and generates a boot entry for the installed kernel (tho you'll be instructed whether to run the install-limine.sh script)
- follow the small instructions once its done

From there **Reboot** and you ought to be good to go.

## Keymap (The Essentials)

| Keys                                  | Action                                                      |
| ------------------------------------- | ----------------------------------------------------------- |
| `SUPER + T` / `SUPER + Return/Enter`  | Terminal                                                    |
| `SUPER + Q` / `SUPER + W`             | Close window                                                |
| `SUPER + E` / `SUPER + SHIFT + F`     | Files (Nautilus)                                            |
| `SUPER + B`                           | Browser                                                     |
| `SUPER + Space`                       | App launcher (rofi)                                         |
| `SUPER + S` / `SUPER + ALT + S`       | Scratchpad toggle / send window                             |
| `SUPER + .`                           | Emoji picker (copies directly to focused window)            |
| `SUPER + C` / `SUPER + V`             | Universal copy / paste                                      |
| `SUPER + I`                           | Settings                                                    |
| `SUPER + N`                           | Notifications                                               |
| `ALT + R`                             | Reminders (view / add / snooze)                             |
| `SUPER + F`                           | Fullscreen                                                  |
| `SUPER + O`                           | Opacity                                                     |
| `SUPER + H J K L` / arrows            | Focus window                                                |
| `SUPER + SHIFT + H J K L` / arrows    | Move window                                                 |
| `SUPER + CTRL + H J K L`              | Resize window                                               |
| `SUPER + 1…0` / `SUPER + SHIFT + 1…0` | Switch / move to workspace                                  |
| `SUPER + /` / `SUPER + ALT + /`       | Zoom in / out (or you can use SUPER + scroll wheel up/down) |
| `SUPER + SHIFT + X`                   | Quickshell wheel launcher                                   |
| `SUPER + SHIFT + E`                   | Exit session                                                |
| `SUPER + L`                           | Lock screen (MacLock)                                       |
| `SUPER + SHIFT + GRAVE(backtick)`     | Power menu                                                  |
| `PRINT`                               | screenshot overlay                                          |
| `Volume / Brightness keys`            | wpctl / brightnessctl (+ OSD)                               |
| `SUPER + ALT + L`                     | 43PR engine for light/dark toggles                          |
| `SUPER + D`                           | Toggle wallpaper picker                                     |

Althogh you can click on the **help** button on the waybar to pull up the full cheatsheet

## BTW

Rule of thumb:

If it's a symlink, **touch at your own risk**

So...

> **DO NOT TRY TO DELETE, MOVE OR HIDE** the `~/dotfiles` directory after installation!!

you'll **break symlinks!!**

## Updates

As an addon, it has its own interactive script, **sur** that does stuff (see below).

```bash
Usage:
sur {flag}

Flags:
-u, --update Full suite (yay + fetch)

-f, --fetch Dotfiles fetch only

-F, --firmware Firmware (LVFS/fwupd) update check: refresh metadata, list updates, ask before installing

-p, --pull Dotfiles pull only

-y, --yay yay/AUR updates only

-s, --status Show repo status (branch, ahead/behind etc)

-d, --diff Dry run: commits/patch that would come in from the pull

-l, --log Show last 5 local & remote commits

-h, --help show this help your reading right now
```

```bash
Examples:

sur -u

sur update

sur -s

sur -F
```

Sometimes an update requires a reboot to apply, so keep that in mind.

### Firmware updates

The setup also keeps an eye on firmware (BIOS/SSD/dock updates from LVFS) so
it never silently rots behind your package upgrades:

- a **pacman hook** (`95-fwupd-check.hook`, installed by `install.sh`) runs after
  every `sudo pacman -Syu` / `sur -u` transaction, tells you right in the
  terminal (and with a desktop notification) if firmware updates exist —
  it **never installs anything**, it only reports;
- `fwupd-refresh.timer` keeps LVFS metadata fresh in the background (disabled by default tho);
- `sur -F` is the manual lever: refresh, show what's available, and only
  install when you answer `y`. Firmware updates often want a reboot and a
  decent battery, so the choice stays yours.

Read-only by design: `sudo pacman -Syu && ...` stays a normal upgrade flow,
whether or not there's a firmware update waiting.

Other useful flags:

`./install.sh --dry-run` (preview changes),

`./install.sh --link-only` (do symlinks only).

## All hell broke loose!?

Probably relax, the setup includes a pretty neat **limine x btrfs x snapper x pacman hook** 'pipeline' that takes a snapshot of your system on every update you make

So if something breaks, reboot and choose one of the snapshots

It's also 'reasonably' configured in that the snapshots don't "hog" your storage, tho you can use the **btrfs assistant** app that ought to be downloaded for a 'less scary' config incase you want to tweak stuff to your preferences

## Recommendations

- So far, we've tested the setup on almost all linux kernels, and we found the best experience using the cachyOS kernel, so give it a shot. And yes, since we use limine as the bootloader, no need to edit your limine.conf files at all, its all handled automatically unless you cherry pick your system.

- We also recommend using the normal blur over liquid glass for aesthetic reasons as its not quite possible (or we haven't found a solution) to automatically have the liquid glass effect on the macbar and spotlight to automatically use your current workspace background. As in, when you open something from the macbar with the liquid glass effect on, the liquid glass works but try switching workspaces with the macbar content still open. So we recommend just using the normal blur which is quite neat to be honest too.

## Extras

This setup is based off
[43PR's dots](https://github.com/43PR/dotfiles) and [O-ss](https://github.com/0-ss/MacShell) on Github. And thank you to both for the incredible and easy to use setup and xp

## Licence

MIT
