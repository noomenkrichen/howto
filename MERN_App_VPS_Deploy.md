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
### 1.3 Install Node.js and npm (if not pre-installed)
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
### 1.4 Install Latest version of Git (if not pre-installed)
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
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;                     # optional, Useful for logs, rate limiting, auth
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for; # optional, Commonly used in proxies/load balancers
        proxy_set_header X-Forwarded-Proto $scheme;                  # optional, Needed if backend enforces HTTPS-only logic
        proxy_http_version 1.1;                    # Basic with WebSocket support
        proxy_set_header Upgrade $http_upgrade;    # Basic with WebSocket support
        proxy_set_header Connection 'upgrade';     # Basic with WebSocket support
        proxy_cache_bypass $http_upgrade;          # Basic with WebSocket support
    }

    location / {
        root /var/www/react-app-2/dist;
        try_files $uri /index.html;
    }
}
```

### 7.2 Then reload Nginx:

```bash
sudo nginx -t
sudo systemctl reload nginx
```
