# Bluetooth issues

```
bluetoothctl
show
No default controller available

src/plugin.c:init_plugin() System does not support ccp plugin
```
You need to run :

```bash
sudo modprobe -r btusb
sudo modprobe btusb
```
