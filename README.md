# RPI-Mobile-Bluetooth
The aim of this project is sending and receiving data from a Raspberry Pi to a mobile device through bluetooth. I will only use the terminal for simplification and more support over all the different versions of the RPI (Raspberry Pi).
## Raspberry Pi
### Set up bluetooth
In the beginning I have to change some lines in the bluetooth library of the RPI. Therefor I open the required file with `nano` and `sudo` so I can edit it.
```
sudo nano /etc/systemd/system/dbus-org.bluez.service
```
Now I add the compatibility flag with `-C` or `--compat` to the following line.
```
ExecStart=/usr/lib/bluetooth/bluetoothd --compat
```
To make the changes effective I restart the bluetooth service.
```
sudo systemctl daemon-reload
sudo systemctl restart bluetooth.service
```
----
### Pair a device
To pair a device I need to go into the bluetooth mode. After executing this command the terminal should print `Agent registered` and the user should change to `[bluetooth]#`.
```
sudo bluetoothctl
```
Next I turn on the Agent and select the default one. Afterwords I can scan the available bluetooth devices. To stop he scan just type `scan off`.
```
agent on
default-agent
scan on
```
I choose one device from the list. For me this is `[NEW] Device D0:49:7C:A3:B6:B6 OnePlus Nord2 5G`, where `D0:49:7C:A3:B6:B6` is most important for us. To pair to the choosen device type the following. The mobile device has to be turned on.
```
pair D0:49:7C:A3:B6:B6
```
My phone asks me if i want to pair. Meanwhile the RPI should print `Attempting to pair` and then I have to type yes when the RPI asks for it. Finally I trust this device and make it discoverable.
```
trust D0:49:7C:A3:B6:B6
discoverable on
```
----
### Sending data
To send data from the RPI to the mobile device I return to the normal terminal with `exit` and write the following.
```
echo "My text!" > /dev/rfcomm0
```
## Mobile Device (Android)
To see the output I have to install the [Serial Bluetooth Terminal](https://play.google.com/store/apps/details?id=de.kai_morich.serial_bluetooth_terminal) app on my mobile device. Then I open it and select the RPI. A terminal opens and shows the printed text.
