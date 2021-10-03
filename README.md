# odroid_go_super_setup

- Get the OS installation from here: https://wiki.odroid.com/odroid_go_super/start 
- `sudo systemctl disable emulationstation`
- `sudo systemctl enable getty@tty1`
- `sudo apt remove emulationstation-go2 emulators-64bit-go2 emulators-32bit-go2`
- `sudo apt install sddm lxqt openbox`
- Add /etc/X11/xorg.conf
```
Section "Device"
	Identifier	"ODROID-GO"
	Driver		"modesetting"
	Option		"AccelMethod" "none"
	Option		"PageFlip" "off"
EndSection

Section "Screen"
	Identifier	"Default Screen"
	Device		"ODROID-GO"
	Monitor		"Default Monitor"
EndSection

Section "Monitor"
	Identifier	"Default Monitor"
	Option		"Rotate" "left"
EndSection
```
- Add /etc/sddm.conf
```
[Autologin]
Session=lxqt.desktop
User=odroid
```
- Remove what we don't need
```
sudo apt remove thunderbird firefox "libreoffice-*"
```
## To boot directly into a GUI application without desktop environment
- Create a user defined session with a custom `.desktop` file in `/usr/share/xsessions` as follows:
```
[Desktop Entry]
Name=My Custom Session
Comment=Run my custom GUI 
Exec=/path/to/custom/gui
TryExec=
Icon=
Type=Application
```
- 
## References
- ubuntu desktop: https://forum.odroid.com/viewtopic.php?t=42196
- Booting into a GUI directly: https://askubuntu.com/questions/23932/how-do-i-replace-the-desktop-by-an-application
