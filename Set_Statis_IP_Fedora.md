# Setting Up Static IP address for Fedora Server VM
## 1. Open the Network Configuration File
```bash
sudo vi /etc/systemd/network/99-static.network
```
## 2. Modify the Configuration
Replace gateway4 with routes. Update your file as follows:
```yaml
[Match]
Name=eth0

[Network]
Address=192.168.1.100/24
Gateway=192.168.1.1
DNS=8.8.8.8
DNS=8.8.4.4

[DHCP]
ClientIdentifier=mac
```
## 3. Apply the New Configuration
Save the file (:wq, then ENTER), then restart systemd-networkd to apply the changes:
```bash
sudo systemctl restart systemd-networkd
```
## 4. Verify the Configuration
Check if the IP has been set correctly:
```bash
ip a
```
