---
title: "GNOME installation"
date: 2025-07-17
draft: false
---

## GNOME installation on Arch Linux

**Step1:** Basic step is to **update your system:**Updates all packages and the system to the latest versions.

```
sudo pacman -Syu
```

**Step 2 - Installs Xorg(display server):** Installs the X window System(Xorg),which handles graphic rendering and input devices.
**Why it is required:**GNOME is a graphical desktop environment - without Xorg, the system can't display windows or GUI elements.
**Essentials from Xorg:**xorg-server, xorg-apps, xorg-fonts, input drivers(usually installed), and video drivers(depends on GPU).

```
sudo pacman -S xorg xorg-server
```

**Step3 - Install GNOME desktop environment:** Installs the full GNOME desktop manager, system tools, apps, and libraries.

```
sudo pacman -S gnome
```

**Step4 - Install GDM(GNOME Display Manager):** Installs the GNOME Display Manager(GDM),the login screen and session manager.
**Why it is required:**Without GDM, you'd need to start GNOME manually from the terminal.

**Step5 - Enable GDM to auto-start at boot:** Without this have to manually start the GUI.

```
sudo systemctl enable gdm
sudo systemctl start gdm
```

This finally completes GNOME installation. Now we can use desktop environment.

Still more to go.....
Thanks for reading!
