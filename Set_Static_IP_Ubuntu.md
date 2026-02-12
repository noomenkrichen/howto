# Setting Up Static IP address for Ubuntu Server VM
## 1. Open the Netplan Configuration File
```bash
sudo nano /etc/netplan/00-installer-config.yaml
```
## 2. Modify the Configuration
Replace gateway4 with routes. Update your file as follows:
```yaml
network:
  version: 2
  ethernets:
    eth0:  # Replace with your actual network interface name
      dhcp4: false
      addresses:
        - 192.168.1.100/24  # Your static IP address
      routes:
        - to: default
          via: 192.168.1.1  # Your router's IP
      nameservers:
        addresses: [1.1.1.1, 8.8.8.8, 8.8.4.4]
```
## 3. Apply the New Configuration
Save the file (CTRL + X, then Y, then ENTER), then apply the changes:
```bash
sudo netplan apply
```
If you want to test first:
```bash
sudo netplan try
```
## 4. Verify the Configuration
Check if the IP has been set correctly:
```bash
ip a
```
Check if routing is working:
```bash
ip route show
```
Test internet connectivity:
```bash
ping 8.8.8.8
```
