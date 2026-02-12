# Changing Timezone in Linux
On Ubuntu, you can change the timezone using timedatectl (the recommended way). Here are the steps:

## 1. Check the current timezone
```bash
timedatectl
```
It will show something like:
```pgsql
Local time: Sat 2025-08-30 11:00:00 CET
Time zone: Africa/Tunis (CET, +0100)
```

## 2. List available timezones
```bash
timedatectl list-timezones
```
You can scroll through or filter, for example:
```bash
timedatectl list-timezones | grep Tunis
```

## 3. Set a new timezone
For example, to set the timezone to Europe/Paris:
```bash
sudo timedatectl set-timezone Africa/Tunis
```

## 4. Verify the change
```bash
timedatectl
```
Now it should display the new timezone.

## Alternative (manual way if needed)
If timedatectl isn’t available, you can manually reconfigure:
```bash
sudo dpkg-reconfigure tzdata
```
Then select the geographic area and city.

## ⚡ If you also want to enable automatic clock sync with NTP (recommended so the time stays accurate), run:
```bash
sudo timedatectl set-ntp true
```

### If NTP is inactive, even though you ran set-ntp true. This usually happens when Ubuntu doesn’t have a default time sync service installed/enabled.
1. Install systemd-timesyncd (lightweight, recommended):
```bash
sudo apt update
sudo apt install systemd-timesyncd -y
```

2. Enable and start it:
```bash
sudo systemctl enable systemd-timesyncd --now
```

3. Verify again:
```bash
timedatectl
```
You should now see:
```yaml
System clock synchronized: yes
NTP service: active
```

### 🔎 Alternative:
If you prefer, you can also use chrony or ntp packages, but for most cases, systemd-timesyncd is enough and integrates directly with timedatectl.
