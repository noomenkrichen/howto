# How to copy ssh public key from a windows machine to a VPS?

To copy your SSH public key from a Windows machine to a VPS, you can use the ssh-copy-id command if you are using Git Bash or WSL, or the manual method using scp and ssh commands in PowerShell or Command Prompt. 

## Prerequisites:
* An SSH key pair generated on your Windows machine (files id_rsa and id_rsa.pub in the ~/.ssh directory).
* The IP address and login credentials (username and password) for your VPS.
* OpenSSH client installed on your Windows machine (usually included in modern Windows 10/11 or via Git Bash/WSL). 

## Method 1: Using ssh-copy-id (Recommended for Git Bash/WSL users) 
ssh-copy-id automates the process of appending your public key to the remote server's authorized_keys file. 
Open your terminal (Git Bash or WSL).
Run the ssh-copy-id command, replacing username with your VPS username and server_ip with your VPS's IP address:
```bash
ssh-copy-id username@server_ip
```
If your public key file has a name other than the default (id_rsa.pub), specify it with the -i flag:
```bash
ssh-copy-id -i ~/.ssh/your_key.pub username@server_ip
```
If the VPS uses a non-default SSH port, you can specify it like this (note the quotes):
```bash
ssh-copy-id "-p <port_number> username@server_ip"
```
Enter your VPS password when prompted. The command will then copy the key and set the correct permissions.

Test the connection by attempting to log in via SSH:
```bash
ssh username@server_ip
```
You should now be logged in without being prompted for a password (though you may be prompted for your key's passphrase if you set one). 

## Method 2: Manual Method (Using scp and ssh in PowerShell)
This method involves two steps: copying the public key file to a temporary location on the server, and then appending its content to the authorized_keys file. 
Open PowerShell on your Windows machine.
Copy the public key file to a temporary location on the VPS using scp:
```powershell
scp C:\Users\YourUser\.ssh\id_rsa.pub username@server_ip:/tmp/id_rsa.pub
```
Replace C:\Users\YourUser\.ssh\id_rsa.pub with the actual path to your public key file, and username@server_ip with your VPS credentials. Enter your VPS password when prompted.

Append the key to the authorized_keys file on the VPS and set proper permissions using ssh:
```powershell
ssh username@server_ip "mkdir -p ~/.ssh && cat /tmp/id_rsa.pub >> ~/.ssh/authorized_keys && chmod 700 ~/.ssh && chmod 600 ~/.ssh/authorized_keys && rm /tmp/id_rsa.pub"
```
This command creates the ~/.ssh directory if it doesn't exist, appends the key, sets secure permissions, and removes the temporary file. Enter your VPS password when prompted.

Test the connection by logging in via SSH:
```powershell
ssh username@server_ip
```
 
After either method is successful, it is a good security practice to optionally disable password authentication on the VPS by editing the sshd_config file. 
