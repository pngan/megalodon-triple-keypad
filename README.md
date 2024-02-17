# megalodon-triple-keypad

Via keycodes for Megalodon Triple Knob Keypad from drop.com

To program the keyboard:
1. Plug keyboard into computer
2. Open Browser and navigate to https://usevia.app
3. Click on Authorize tab
4. Click file tab (file icon bottom left icon bar)
5. `Load Saved Layout` and load the file contained in this repository `megadolon.layout.json`
6. Assign keys using the via web app
7. Click file tab and choose `Save Current Layout`

Extra notes for Ubuntu:
By default the usevia web app is unable to authorize the keypad. You will give error like:
```
Failed to open the device.
Device: DOIO DOIO
Vid: 0xD010
Pid: 0x1601
```


If you go here `chrome://device-log/` You will notice the following line when trying to use the VIA Web App Something like this Failed to open `'/dev/hidraw5': FILE_ERROR_ACCESS_DENIED`

You need to permit access as a UDEV rule. In a terminal

Type `lsusb`

Look for a line like this `Bus 001 Device 004: ID d010:1601 DOIO DOIO` in the output.

To create a rule file type this:
```
sudo touch /etc/udev/rules.d/99-vial.rules
```
Now to populate the file enter below but edit this portion the required numbers are in the LSUSB output retrieved earlier. (where first part is vendor and second part is product) ATTRS{idVendor}=="d010", ATTRS{idProduct}=="1601"
```
echo 'KERNEL=="hidraw*", SUBSYSTEM=="hidraw", ATTRS{idVendor}=="d010", ATTRS{idProduct}=="1601", MODE="0660", GROUP="users", TAG+="uaccess", TAG+="udev-acl"' | sudo tee -a /etc/udev/rules.d/99-vial.rules > /dev/null
```
Lastly load the UDEV Rules some examples

```
sudo udevadm control --reload-rules && sudo udevadm trigger
sudo chmod 777 /dev/hidraw5  # the file is shown in chrome://device-log/
```
