# Upgrade Ubuntu OS Using Terminal
## 1. Check current OS version:
```bash
cat /etc/os-release
````
or
```bash
cat /proc/version
```
or
```bash
hostnamectl
```
or
```bash
lsb_release –a
```
or
```bash
uname –a
```
## 2. Upgrading Ubuntu OS to Latest Release
### 2.1.	Install ubuntu-release-upgrader-core if it is not already installed:
```bash
sudo apt-get install ubuntu-release-upgrader-core
```
### 2.2.	Edit /etc/update-manager/release-upgrades and set Prompt=normal (originally it is Prompt=lts)
```bash
sudo nano /etc/update-manager/release-upgrades
```
### 2.3.	Launch the upgrade tool:
```bash
do-release-upgrade
```
### 2.4.	Follow the on-screen instructions.
