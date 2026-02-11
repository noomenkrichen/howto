# Deploying MERN Stack Project on VPS
- Preparing the VPS Environment
- Setting Up the MongoDB Database
- Deploying the Express and Node.js Backend
- Deploying the React Frontends
- Configuring Nginx as a Reverse Proxy
- Setting Up SSL Certificates
## 1. Preparing the VPS Environment
### 1.1 Log in to Your VPS in Terminal 
```bash
ssh root@vps_ip_address
```

### 1.2 Update and Upgrade Your System
```bash
sudo apt update
```
```bash
sudo apt upgrade -y
```

### 1.3 Create new user
```bash
sudo adduser new_username
```
#### 1.3.1 Add sudo privilege to new user by adding it to the sudo group
```bash
sudo usermod -aG sudo new_username
```
#### 1.3.2 Verify the user creation
```bash
cat /etc/passwd
```
#### 1.3.3 Test the new user
```bash
su - new_username
```

### 1.4 Create local ssh key on the machine you wish to use to connect to the VPS
You could create a key on every machine or computer you plan to use to connect

It is recommended to use at leasrt two different machine (for backup) in case you loose the key on one machine you will still have the backup machine to login using ssh

Because if you disable the password login and the root login, you may loose access permanently in the vent the ssh key is lost!

Simple way (default is ed25519)
```bash
ssh-keygen -C "your_email@example.com"
```
or specify rsa, older algorithm (1977); Security based on factoring large prime numbers; Needs large key sizes to be secure (2048 or 4096 bits)
```bash
ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
```
or specify ed25519, modern algorithm (2011); Based on elliptic curve cryptography; Very strong security with much smaller keys. Today, ED25519 is considered more modern and secure by design.
```bash
ssh-keygen -t ed25519 -C "your_email@example.com"
```

### 1.5 Copy ssh key to vps
```bash
ssh-copy-id -i ~/.ssh/id_rsa.pub user@your_vps_ip
```
#### 1.5.1 Elevate security acces by disabling password login and root login
```bash
sudo nano /etc/ssh/sshd_config
```
Find and modify the following line:
```bash
PubkeyAuthentication yes

PasswordAuthentication no

PermitRootLogin no
```
Restart the SSH service:
```bash
sudo systemctl restart sshd
```

### 1.6 Check who is trying to connect to your VPS (optional)
```bash
tail -n 10 -f /var/log/auth.log
```
### 1.7 Install Node.js and npm (if not pre-installed)
To get the latest version of Node.js Follow this Guide: [click here](https://nodejs.org/en)

The following command will install version 22.x of Node.js
```bash
curl -fsSL https://deb.nodesource.com/setup_22.x | sudo bash -
```
Or install LTS version of Node.js
```bash
curl -fsSL https://deb.nodesource.com/setup_lts.x | sudo bash -
```
```bash
sudo apt-get install -y nodejs
```

### 1.8 Install Latest version of Git (if not pre-installed)
You can follow this Guide from Git website: [click here](https://git-scm.com/downloads/linux)
```bash
add-apt-repository ppa:git-core/ppa
```
```bash
sudo apt update
```
```bash
sudo apt install -y git
```

### 1.9 Create a folder and changing its owner
Basic Syntax: 
#### 1.9.1 Run the following command to transfer ownership to a specific user.
```bash
sudo chown [new_owner] /path/to/folder
```
For example: to changes owner to user "ubuntu" run:
```bash
sudo chown ubuntu /home/myfolder
```
#### 1.9.2 For recursive Ownership, add -R flag to apply changes to the folder and all contents:
```bash
cd /var/www
mkdir folder
sudo chown -R new_owner:new_group /path/to/folder
```
#### 1.9.3 Use the following command to assign to your current user.
```bash
sudo chown -R $USER:$USER /path/to/folder
```
#### 1.9.4 Verify Changes
Check ownership with 
```bash
ls -l /path/to/folder
```
The first field after permissions shows owner:group. List users/groups via getent passwd or getent group.


##  2. Setting Up the MongoDB Database
If you want to setup MongoDB on VPS Follow this Guide: [click here](https://www.mongodb.com/docs/manual/tutorial/install-mongodb-on-ubuntu/)
### 2.1 Adding a User
First start mongoDB with the following command
```bash
mongosh
```
Use your database (i.e. kwstn)
```bash
use kwstn
```
Now add a new user
```bash
db.createUser({ user: "myUser", pwd: "mySecurePassword", roles: [{ role: "readWrite", db: "mydatabase" }]})
```
To add a new admin
```bash
db.createUser({ user: "adminUser", pwd: "mySecurePassword", roles: [{ role: "root", db: "admin" }]})
```
or
```bash
db.createUser({
  user: "myUser",
  pwd: "mySecurePassword",
  roles: [
    { role: "readWrite", db: "mydatabase" }, // Grants read & write access
    { role: "dbAdmin", db: "mydatabase" }   // Grants admin privileges for the database
  ]
})
```
Verify the user
```bash
show users
```
Enable authentication (optional)

If authentication is not enabled, edit your MongoDB configuration file /etc/mongod.conf
```yaml
security:
  authorization: enabled
```
Then restart MongoDB:
```bash
sudo systemctl restart mongod
```
Connect with new user
```bash
mongosh -u myUser -p mySecurePassword --authenticationDatabase mydatabase
```

## 3. Deploying the Express and Node.js Backend
### 3.1 Clone Your Backend Repository
```bash
mkdir /var/www
```
```bash
cd /var/www
```
```bash
mkdir app_name
```
```bash
cd app_name
```
```bash
git clone https://github.com/yourusername/server.git
```
```bash
cd server
```
### 3.2 Install Dependencies
```bash
npm install
```
### 3.3 Create .env file & configure Environment Variables
```bash
nano .env
```
add environment variables then save and exit (Ctrl + X, then Y and Enter).
### 3.4 Installing pm2 to Start Backend
```bash
npm install -g pm2
```
```bash
pm2 start server.js --name app_name-api
```
### 3.5 Start app_name-api on startup
```bash
pm2 startup
```
```bash
pm2 save
```
### 3.6 Allowing backend port in firewall 
```bash
sudo ufw status
```
If firewall is disable then enable it using 
```bash
sudo ufw enable
```
```bash
sudo ufw allow 'OpenSSH'
```
```bash
sudo ufw allow 4000
```
## 4. Deploying the React Frontends
### 4.1 Clone your frontend repository
```bash
cd ..
```
```bash
git clone https://github.com/yourusername/client.git
```
### 4.2 Install Dependencies
```bash
cd client
```
```bash
npm install
```
### 4.3 If you have ".env" file in your project
Create .env file and paste the variables
```bash
nano .env
```
### 4.4 Create build of project
```bash
npm run build
```
Repeat for the second or mulitiple React app.
### 4.5 Install Nginx
```bash
sudo apt install -y nginx
```
adding Nginx in firewall
```bash
sudo ufw status
```
```bash
sudo ufw allow 'Nginx Full'
```
### 4.6 Configure Nginx for React Frontends
```bash
nano /etc/nginx/sites-available/yourdomain1.com.conf
```
```bash
server {
    listen 80;
    server_name yourdomain1.com www.yourdomain1.com;

    location / {
        root /var/www/your-repo/frontend/dist;
        try_files $uri /index.html;
    }
}
```
Save and exit (Ctrl + X, then Y and Enter).
### 4.6 Create a similar file for the second or multiple React app.
```bash
nano /etc/nginx/sites-available/yourdomain2.com.conf
```
```bash
server {
    listen 80;
    server_name yourdomain2.com www.yourdomain2.com;

    location / {
        root /var/www/react-app-2/dist;
        try_files $uri /index.html;
    }
}
```
### 4.7 Create symbolic links to enable the sites.
```bash
ln -s /etc/nginx/sites-available/yourdomain1.com.conf /etc/nginx/sites-enabled/
```
```bash
ln -s /etc/nginx/sites-available/yourdomain2.com.conf /etc/nginx/sites-enabled/
```
### 4.8 Test the Nginx configuration for syntax errors.
```bash
nginx -t
```
```bash
systemctl restart nginx
```
## 5. Configuring Nginx as a Reverse Proxy for the backend
### 5.1 Update Backend Nginx Configuration
```bash
nano /etc/nginx/sites-available/api.yourdomain.com.conf
```
```bash
server {
    listen 80;
    server_name api.yourdomain.com;

    location / {
        proxy_pass http://localhost:4000;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}
```
### 5.2 Create symbolic links to enable the sites.
```bash
ln -s /etc/nginx/sites-available/api.yourdomain.com.conf /etc/nginx/sites-enabled/
```
### 5.3 Restart nginx
```bash
systemctl restart nginx
```
### 5.4 Connect Domain Name with Website
Point all your domain & sub-domain on VPS IP address by adding DNS records in your domain manager 


Now your website will be live on domain name
## 6. Setting Up SSL Certificates 
### 6.1 Install Certbot
```bash
sudo apt install -y certbot python3-certbot-nginx
```
### 6.2 Obtain SSL Certificates
```bash
certbot --nginx -d yourdomain1.com -d www.yourdomain1.com -d yourdomain2.com -d api.yourdomain.com
```
### 6.3 Verify Auto-Renewal
```bash
certbot renew --dry-run
```


## 7. Configuring Nginx for yourdomain.com and your api as yourdomain.com/server (or /api) [optional step]
### 7.1 Update Backend Nginx Configuration
```bash
nano /etc/nginx/sites-available/yourdomain.com.conf
```
```bash
server {
    listen 80;
    server_name yourdomain.com www.yourdomain.com;

    location /server/ {
        proxy_pass http://localhost:4000/; # Your todo API (running via PM2)
        proxy_set_header X-Real-IP $remote_addr;                     # optional, Useful for logs, rate limiting, auth
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for; # optional, Commonly used in proxies/load balancers
        proxy_set_header X-Forwarded-Proto $scheme;                  # optional, Needed if backend enforces HTTPS-only logic
        proxy_http_version 1.1;                    # Basic with WebSocket support
        proxy_set_header Upgrade $http_upgrade;    # Basic with WebSocket support
        proxy_set_header Connection 'upgrade';     # Basic with WebSocket support
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;          # Basic with WebSocket support
    }

    location / {
        root /var/www/react-app-2/dist;
        try_files $uri /index.html;
    }
}
```
or
```bash
    location /server/ {
         proxy_pass http://localhost:5000/;
         proxy_http_version 1.1;
         proxy_set_header Upgrade $http_upgrade;
         proxy_set_header Connection 'upgrade';
         proxy_set_header Host $host;
         proxy_cache_bypass $http_upgrade;
    }
```

### 7.2 Then reload Nginx:

```bash
sudo nginx -t
sudo systemctl reload nginx
```
