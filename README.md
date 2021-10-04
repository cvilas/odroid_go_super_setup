# odroid_go_super_setup

## Instructions to boot directly into a GUI application without desktop environment

- Get the OS installation from here: https://wiki.odroid.com/odroid_go_super/start 
- `sudo systemctl disable emulationstation`
- `sudo systemctl enable getty@tty1`
- `sudo apt remove emulationstation-go2 emulators-64bit-go2 emulators-32bit-go2`
- `sudo apt install sddm openbox`
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
- Remove what we don't need
```
sudo apt remove thunderbird firefox "libreoffice-*"
```
- Create script to run in graphical session, called `/home/odroid/my_application_xsession.sh`. This 
script wait for a network connection and then start your application
```
#!/bin/bash

until [[ $(nm-online -q) -eq 0 ]]
do
        echo "Waiting for network"
done

/home/odroid/path/to/my_application
```
- Create a user defined X session with a custom `my_application.desktop` file in 
`/usr/share/xsessions` as follows, and set it up to run the above script in your session:
```
[Desktop Entry]
Name=My Custom Session
Comment=Run my custom GUI 
Exec=/home/odroid/my_application_xsession.sh
TryExec=
Icon=
Type=Application
```
- Add /etc/sddm.conf
```
[Autologin]
Session=my_application.desktop
User=odroid
```
## References
- ubuntu desktop: https://forum.odroid.com/viewtopic.php?t=42196
- Booting into a GUI directly: https://askubuntu.com/questions/23932/how-do-i-replace-the-desktop-by-an-application
