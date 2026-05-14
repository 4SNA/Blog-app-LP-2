# Full-Stack Blog Application Deployment on AWS EC2 (Ubuntu)

This project demonstrates deployment of a Full-Stack Blog Application using:

- React.js (Frontend)
- Node.js + Express.js (Backend)
- MongoDB (Database)
- AWS EC2 Ubuntu Instance

The application allows users to:
- Create blog posts
- View blog posts
- Update blog posts
- Delete blog posts

---

# Problem Statement

Deploy a full-stack blog application (frontend, backend, and database) on a cloud platform. Configure the server environment so that users can create, view, and manage blog posts through a web interface.

---

# Architecture

```text
User → Browser → AWS EC2
                    ├── React Frontend
                    ├── Express Backend API
                    └── MongoDB Database
```

---

# Requirements

- AWS Account
- Ubuntu EC2 Instance
- `.pem` key file
- Internet Connection
- GitHub Repository

Repository:

```text
https://github.com/4SNA/Blog-app-LP-2
```

---

# Step 1 — Launch AWS EC2 Instance

## Open AWS Console

Go to:

```text
EC2 → Launch Instance
```

---

## Configure Instance

### AMI
```text
Ubuntu
```

### Instance Type
```text
t2.micro
```

### Key Pair
Create/download:

```text
your-key.pem
```

---

# Configure Security Group

Add these inbound rules:

| Type | Port | Source |
|------|------|---------|
| SSH | 22 | My IP |
| HTTP | 80 | 0.0.0.0/0 |
| Custom TCP | 5000 | 0.0.0.0/0 |

---

# Step 2 — Connect to EC2

Open PowerShell / Terminal.

Go to folder containing `.pem` file.

Example:

```powershell
cd C:\Users\hp\Downloads
```

---

## Connect Using SSH

```bash
chmod 400 your-key.pem
```

```bash
ssh -i your-key.pem ubuntu@<public-ip>
```

Example:

```bash
ssh -i blog.pem ubuntu@13.126.xxx.xxx
```

---

# Step 3 — Update Ubuntu Packages

```bash
sudo apt update
sudo apt upgrade -y
```

---

# Step 4 — Install Required Dependencies

Install:
- Node.js
- npm
- Git

```bash
sudo apt install nodejs npm git -y
```

Verify installations:

```bash
node -v
npm -v
git --version
```

---

# Step 5 — Install PM2

PM2 is a Node.js process manager.

Install globally:

```bash
sudo npm install -g pm2
```

Verify:

```bash
pm2 --version
```

---

# Step 6 — Clone GitHub Repository

```bash
git clone https://github.com/4SNA/Blog-app-LP-2.git
```

Enter project folder:

```bash
cd Blog-app-LP-2
```

Check files:

```bash
ls
```

Expected folders:

```text
backend
frontend
README.md
```

---

# Step 7 — Setup Backend

Go to backend:

```bash
cd backend
```

Install backend dependencies:

```bash
npm install
```

---

# Create `.env` File

Create environment file:

```bash
nano .env
```

Paste:

```env
MONGO_URI=mongodb://127.0.0.1:27017/blogdb
JWT_SECRET=sarthak123
PORT=5000
```

Save:

```text
CTRL + X → Y → Enter
```

---

# Explanation of `.env`

## MONGO_URI

```env
mongodb://127.0.0.1:27017/blogdb
```

- MongoDB running locally
- Database name = `blogdb`

---

## JWT_SECRET

Used for:
- login authentication
- token generation

Can be any random string.

---

## PORT

```env
PORT=5000
```

Backend runs on:

```text
localhost:5000
```

---

# Step 8 — Install MongoDB (If Needed)

Check MongoDB:

```bash
mongod --version
```

If not installed:

```bash
sudo apt install mongodb -y
```

Start MongoDB:

```bash
sudo systemctl start mongodb
```

Enable MongoDB:

```bash
sudo systemctl enable mongodb
```

Check MongoDB status:

```bash
sudo systemctl status mongodb
```

---

# Step 9 — Setup Frontend

Go to frontend folder:

```bash
cd ../frontend
```

Install frontend dependencies:

```bash
npm install
```

Build frontend:

```bash
npm run build
```

This creates production build files.

---

# Step 10 — Run Backend Application

Go to backend:

```bash
cd ../backend
```

Start backend:

```bash
pm2 start server.js
```

Save PM2 process:

```bash
pm2 save
```

Check running applications:

```bash
pm2 list
```

---

# Step 11 — Verify Application Locally

Inside EC2:

```bash
curl http://localhost:5000
```

If HTML output appears:

```text
Application is running correctly
```

---

# Step 12 — Access Application Publicly

Open browser:

```text
http://<public-ip>:5000
```

Example:

```text
http://13.126.xxx.xxx:5000
```

---

# Common Errors and Solutions

---

## Error 1 — `pm2: command not found`

### Solution

Install PM2:

```bash
sudo npm install -g pm2
```

---

## Error 2 — `ERR_CONNECTION_TIMED_OUT`

### Cause

Port 5000 blocked.

### Solution

Open Security Group inbound rule:

| Type | Port |
|------|------|
| Custom TCP | 5000 |

Source:

```text
0.0.0.0/0
```

---

## Error 3 — `Cannot find module`

### Solution

Run:

```bash
npm install
```

inside:
- backend
- frontend

---

## Error 4 — `MongoDB connection failed`

### Cause

Wrong MongoDB URI or MongoDB service not running.

### Solution

Check:

```bash
sudo systemctl status mongodb
```

Start MongoDB:

```bash
sudo systemctl start mongodb
```

---

## Error 5 — `server.js not found`

### Solution

Check backend files:

```bash
ls
```

Maybe project uses:
- app.js
- index.js

Run correct file:

```bash
pm2 start app.js
```

or

```bash
pm2 start index.js
```

---

## Error 6 — `scp not working`

### Cause

`.pem` file or source file not found.

### Solution

Run SCP from local machine:

```bash
scp -i your-key.pem file.txt ubuntu@public-ip:/home/ubuntu/
```

---

# Useful Commands

## View Files

```bash
ls
```

---

## View Hidden Files

```bash
ls -la
```

---

## Check Current Folder

```bash
pwd
```

---

## PM2 Logs

```bash
pm2 logs
```

---

## Restart Application

```bash
pm2 restart all
```

---

## Stop Application

```bash
pm2 stop all
```

---

## Delete PM2 Process

```bash
pm2 delete all
```

---

# Remote Updates

## Update Backend

```bash
cd backend
git pull
pm2 restart all
```

---

## Update Frontend

```bash
cd frontend
git pull
npm run build
pm2 restart all
```

---

# Features

- Create Blog Posts
- View Blog Posts
- Update Blog Posts
- Delete Blog Posts
- Cloud Deployment
- Public Accessibility
- Remote Server Management

---

# Viva Questions

## What is EC2?

Elastic Compute Cloud virtual machine service provided by AWS.

---

## What is PM2?

Node.js process manager used to run backend continuously.

---

## Why Port 5000?

Node.js backend application runs on port 5000.

---

## What is SSH?

Secure Shell protocol used for remote Linux access.

---

## What is Security Group?

Cloud firewall controlling network traffic.

---

## Why `.env` file?

Stores environment variables securely.

---

## What is MongoDB?

NoSQL database used to store blog data.

---

# Output

The Full-Stack Blog Application is successfully deployed on AWS EC2 Ubuntu instance and is publicly accessible using the EC2 public IP address.

---

# Conclusion

The Blog Application was successfully deployed on AWS EC2 cloud instance using React frontend, Node.js/Express backend, and MongoDB database. The application supports CRUD operations and can be accessed remotely through the internet.
