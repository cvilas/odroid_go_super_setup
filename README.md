# odroid_go_super_setup

## Instructions to boot directly into a GUI application without desktop environment

- Get the OS installation from here: https://wiki.odroid.com/odroid_go_super/start 
- `sudo systemctl disable emulationstation`
- `sudo systemctl enable getty@tty1`
- `sudo apt remove emulationstation-go2 emulators-64bit-go2 emulators-32bit-go2`
- `sudo apt install sddm openbox unclutter network-manager ip-tools`

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
- Restrict log file sizes
```
journalctl --vacuum-time=2d
```
- Disable WiFi power saving. Open `/etc/NetworkManager/conf.d/default-wifi-powersave-on.conf` and
ensure powersave option is set to `3`.
```
[connection]
wifi.powersave = 3
```
- Disable screen blanking. Create `/home/odroid/noblank.sh` as follows:
```
#!/bin/bash
xset dpms 0 0 0 && xset -dpms  && xset s off && xset s noblank
```
- Make it executable: `chmod +x /home/odroid/noblank.sh`.
- To quit xscreensaver on joystick events, download joystickwake
```
cd ~
git clone https://github.com/foresto/joystickwake.git
```
- Create script to run in graphical session, called `/home/odroid/my_application_xsession.sh`. This 
script wait for a network connection and then start your application
```
#!/bin/bash

# wait for network to come online
until [[ $(nm-online -q) -eq 0 ]]
do
        echo "Waiting for network"
done

# start a window manager so that our application can run full screen
openbox&

# stop screen blanking
/home/odroid/noblank.sh&

# stop screensaver from coming up
/home/odroid/joystickwake/joystickwake&

# hide mouse cursor
unclutter -idle 2&

# run your application
/home/odroid/path/to/my_application
```
- Mark the above file executable with `chmod +x /home/odroid/my_application_xsession.sh`.
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
