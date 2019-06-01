# rpi
rpi basic setup for qcInsecta screen

# /boot/wpa_supplicant.conf
```
ctrl_interface=DIR=/var/run/wpa_supplicant GROUP=netdev
update_config=1
country=«your_ISO-3166-1_two-letter_country_code»

network={
    ssid="«your_SSID»"
    psk="«your_PSK»"
    key_mgmt=WPA-PSK
}
```

# change pi user
1. Create root passwd (sudo passwd root)
2. Activate root ssh login /etc/ssh/sshd_config >> PermitRootLogin yes
3. Create new user: usermod -l newuser pi -md /home/newuser
4. Deactivate root ssh login
5. Delete root passwd (sudo passwd -l root)

If we are connected to X session: [Stackexchangue](https://unix.stackexchange.com/questions/287620/why-failing-to-delete-user-in-raspbian "Why failing to delete user in raspbian")

# 4.3" TFT Screen
git clone https://github.com/Elecrow-keen/Elecrow-LCD35.git
cd Elecrow-LCD35
./Elecrow-LCD35

# Basic kiosk mode
```
sudo apt-get install --no-install-recommends xserver-xorg x11-xserver-utils xinit openbox
sudo apt-get install --no-install-recommends chromium-browser
```
Into /etc/xdg/openbox/autostart
```
# Disable any form of screen saver / screen blanking / power management
xset s off
xset s noblank
xset -dpms

# Allow quitting the X server with CTRL-ATL-Backspace
setxkbmap -option terminate:ctrl_alt_bksp

# Start Chromium in kiosk mode
xset s off
xset s noblank
xset -dpms

# Allow quitting the X server with CTRL-ATL-Backspace
setxkbmap -option terminate:ctrl_alt_bksp

# Start Chromium in kiosk mode
sed -i 's/"exited_cleanly":false/"exited_cleanly":true/' ~/.config/chromium/'Local State'
sed -i 's/"exited_cleanly":false/"exited_cleanly":true/; s/"exit_type":"[^"]\+"/"exit_type":"Normal"/' ~/.config/chromium/Default/Preferences
chromium-browser --disable-infobars --noerrdialogs --kiosk 'http://192.168.1.190/dashboard/php/qcInsecta_show_basic.php' --incognito --disable-translate --window-size=480,320 --window-posit$
```
Into .bash_profile
```
[[ -z $DISPLAY && $XDG_VTNR -eq 1 ]] && startx -- -nocursor
```
