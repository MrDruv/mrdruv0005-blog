Gnome terminal needs dbus,vte3 and gnome-shell to run

sudo pacman -S vte3 gnome-shell dbus

sudo systemctl enable dbus
sudo systemctl start dbus

to view status
systemctl --user status

1. generate locale:
   sudo nano /etc/locale.gen

Uncommet:
en_US.UTF-8 UTF-8
then run: sudo locale-gen

2. set locale system-wide
   export LANG=en_US.UTF-8
   export LC_ALL=en_US.UTF-8

reboot
