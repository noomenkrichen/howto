## 1. Connect to MongoDB
```bash
mongosh --eval 'db.adminCommand({listDatabases: 1})'
```

## 2. Create a User
To create a user for a specific database (e.g., mydatabase), run:
```bash
mongosh --eval 'use mydatabase; db.createUser({user: "myUser", pwd: "mySecurePassword", roles: [{role: "readWrite", db: "mydatabase"}]})'
```

If you want to create an admin user:
```bash
mongosh --eval 'use admin; db.createUser({user: "adminUser", pwd: "strongPassword", roles: [{role: "root", db: "admin"}]})'
```

## 3. Verify User Creation
To list users:
```bash
mongosh --eval 'use mydatabase; show users'
```

## 4. Enable Authentication (Optional)
If authentication is disabled, open the MongoDB config file (/etc/mongod.conf or /usr/local/etc/mongod.conf) and enable authentication:
```bash
sudo sed -i '/security:/a \  authorization: enabled' /etc/mongod.conf
sudo systemctl restart mongod
```
or
```bash
sudo nano /etc/mongod.conf
```
Find the security section in the file. If it doesn’t exist, add it under the main YAML structure:
```yaml
security:
  authorization: enabled
```
```bash
sudo systemctl restart mongod
```

## 5. Basic Connection Command
### If authentication is enabled in MongoDB, use the following command:
```bash
mongosh --username <adminUser> --password <adminPassword> --authenticationDatabase admin
```
Replace:
<adminUser> with your admin username.
<adminPassword> with your admin password.

## 6. Connect as the New User
```bash
mongosh -u myUser -p mySecurePassword --authenticationDatabase mydatabase
```

## 7. Connect to a Specific Host and Port
```bash
mongosh --host <hostname> --port <port> --username <adminUser> --password <adminPassword> --authenticationDatabase admin
```
For example:
```bash
mongosh --host 127.0.0.1 --port 27017 --username admin --password mysecurepassword --authenticationDatabase admin
```

## 8. If Password Prompt is Preferred
Instead of typing the password in the command (which can be insecure), omit the --password flag to be prompted:
```bash
mongosh --username <adminUser> --authenticationDatabase admin
```
Then, enter the password when prompted.

## 9. Connect to a Remote MongoDB Instance
If your MongoDB instance is running on a remote server:
```bash
mongosh "mongodb://<adminUser>:<adminPassword>@<remote-host>:27017/admin"
```
For example:
```bash
mongosh "mongodb://admin:mysecurepassword@51.178.169.201:27017/admin"
```

## 10. If Using a Replica Set or Cloud MongoDB
If connecting to a replica set or a cloud database (e.g., MongoDB Atlas), the URI format will be:
```bash
mongosh "mongodb+srv://<adminUser>:<adminPassword>@cluster0.mongodb.net/admin"
```
