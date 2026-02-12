# VPS Disk Usage Check
## Overall Disk Usage
```bash
df -h
```
* df: Displays disk space usage
* -h: Human-readable format (GB, MB, etc.)
## Check Usage of a Specific Directory
```bash
du -sh /path/to/directory
```
* du: Displays disk usage of files and directories
* -s: Summarizes total size
* -h: Human-readable format
## List Disk Usage of All Directories
```bash
du -h --max-depth=1 /
```
* Shows the size of all directories under / (root)
## Find Large Files
```bash
find / -type f -size +1G
```
* Finds files larger than 1GB
## Graphical Representation (ncdu)
If you want a more interactive way:
```bash
sudo apt update && sudo apt install ncdu -y
ncdu /
```
* This provides a navigable interface to analyze disk usage.


## 1. Build Files Accumulating
Since you're running a MERN stack with a frontend build process (vite build), every build generates files in /var/www/kwstn/frontend/dist.
### 🔍 Check:
```bash
du -sh /var/www/kwstn/frontend/dist
```
### 🛠️ Fix:
If it’s large, clear old builds before running a new one:
```bash
rm -rf /var/www/kwstn/frontend/dist/*
```
## 2. Node Modules Taking Too Much Space
node_modules/ can become huge, especially with multiple project dependencies.
###🔍 Check:
```bash
du -sh /var/www/kwstn/frontend/node_modules
```
### 🛠️ Fix:
Delete and reinstall dependencies:
```bash
rm -rf node_modules
npm cache clean --force
npm install
```
## 3. Logs and Temporary Files Growing Uncontrolled
System logs (/var/log directory)


Build caches (/tmp and ~/.cache)


Nginx logs (/var/log/nginx/)

### 🔍 Check Largest Files in /var/log and /tmp:
```bash
du -ah /var/log | sort -rh | head -20
du -ah /tmp | sort -rh | head -20
```
### 🛠️ Fix:
```bash
sudo rm -rf /var/log/*.gz /var/log/*.1 /var/log/*-????????
sudo rm -rf /tmp/*
```
## 4. Database Logs or Overgrown Database
If you have MongoDB and MariaDB, they store logs and data that can grow unexpectedly.
### 🔍 Check:
```bash
du -sh /var/lib/mongodb
du -sh /var/lib/mysql
du -sh /var/lib/docker
```
### 🛠️ Fix:
For MongoDB:
```bash
sudo systemctl stop mongod
sudo rm -rf /var/lib/mongodb/journal/*
sudo systemctl start mongod
```
For MariaDB:
```bash
sudo journalctl --vacuum-time=3d
```
## 5. Old Kernel Files Taking Space (Common on Ubuntu VPS)
Ubuntu doesn’t always delete old kernels, and they take up space in /boot.
### 🔍 Check:
```bash
dpkg --list | grep linux-image
```
### 🛠️ Fix:
```bash
sudo apt autoremove --purge
```
## 6. Forgotten Docker Containers and Images
Since you use Node.js and possibly containers, Docker images can pile up.
### 🔍 Check:
```bash
docker system df
docker images --format "{{.Repository}}: {{.Size}}"
```
### 🛠️ Fix:
```bash
docker system prune -a
```
## 7. Too Many Small Files Consuming Inodes
If df -h shows free space, but df -i shows 100% inode usage, then small files are the problem.
### 🔍 Check:
```bash
find / -xdev -type f | wc -l
```
### 🛠️ Fix:
```bash
find /var/log -type f -delete
find /tmp -type f -delete
```
### Next Steps
Run df -h and df -i to check space and inodes.


Find largest files using:
```bash
du -ah / | sort -rh | head -20
```
Clear unnecessary files.


Reboot and retry your build.
