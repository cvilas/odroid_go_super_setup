# odroid_go_super_setup

- Get the OS installation from here: https://wiki.odroid.com/odroid_go_super/start 
- `sudo systemctl disable emulationstation`
- `sudo systemctl enable getty@tty1`
- `sudo apt remove emulationstation-go2 emulators-64bit-go2 emulators-32bit-go2`
- `sudo apt install lightdm ubuntu-mate-desktop`
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
- Add /etc/lightdm/lightdm.conf
```
[SeatDefaults]
user-session=mate
greeter-session=slick-greeter
```
- Disable compositing to improve desktop performance
```
gsettings set org.mate.Marco.general compositing-manager false
```
- Enable remote desktop sharing with XRDP
```
sudo apt-get install xrdp
sudo apt-get install mate-core mate-desktop-environment mate-notification-daemon
sudo sed -i.bak '/fi/a #xrdp multiple users configuration \n mate-session \n' /etc/xrdp/startwm.sh
sudo adduser xrdp ssl-cert  
sudo ufw allow 3389/tcp
sudo /etc/init.d/xrdp restart
```
- Install KRDC client on my desktop to access the desktop
- Remove what we don't need
```
sudo apt remove thunderbird firefox "libreoffice-*"
```

## References
- ubuntu desktop: https://forum.odroid.com/viewtopic.php?t=42196
- xrdp: https://www.e2enetworks.com/help/knowledge-base/how-to-install-remote-desktop-xrdp-on-ubuntu-18-04/
