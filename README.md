# Assignment 9 - Online Blog Application Deployment

## Problem Statement

Deploy a full-stack blog application frontend, backend, and database on a cloud platform. Configure the server environment so that users can create, view, and manage blog posts through a web interface.

---

# Repository

```text
https://github.com/4SNA/Blog-app-LP-2
```

---

# Technologies Used

- AWS EC2 Ubuntu Instance
- React.js + Vite Frontend
- Node.js + Express.js Backend
- MongoDB Atlas Database
- Git
- npm
- PM2 Process Manager

---

# Architecture

```text
User Browser
     |
     v
AWS EC2 Instance
     |
     |-- React + Vite Frontend (Port 5173)
     |-- Express Backend API (Port 5000)
     |
     v
MongoDB Atlas Database
```

---

# Step 1 - Create AWS EC2 Instance

1. Open AWS Console.
2. Go to EC2.
3. Click Launch Instance.
4. Select Ubuntu.
5. Select t2.micro.
6. Create/select key pair.
7. Download `.pem` file.
8. Configure Security Group.

---

# Security Group Inbound Rules

| Type | Port | Source |
|---|---|---|
| SSH | 22 | My IP |
| HTTP | 80 | 0.0.0.0/0 |
| Custom TCP | 5000 | 0.0.0.0/0 |
| Custom TCP | 5173 | 0.0.0.0/0 |

---

# Step 2 - Connect to EC2

Open terminal or PowerShell where `.pem` file exists.

```bash
chmod 400 your-key.pem
```

```bash
ssh -i your-key.pem ubuntu@YOUR_PUBLIC_IP
```

Example:

```bash
ssh -i blog.pem ubuntu@13.126.xxx.xxx
```

---

# Step 3 - Update Ubuntu

```bash
sudo apt update
sudo apt upgrade -y
```

---

# Step 4 - Install Required Software

Install Node.js, npm and Git:

```bash
sudo apt install nodejs npm git -y
```

Verify:

```bash
node -v
npm -v
git --version
```

Install PM2:

```bash
sudo npm install -g pm2
```

Verify:

```bash
pm2 --version
```

---

# Step 5 - MongoDB Atlas Setup

Use MongoDB Atlas cloud database instead of local MongoDB.

---

# MongoDB Atlas Details

```text
Username: lp2_user
Password: lp2password123
Database Name: blogdb
Cluster Name: Cluster0
```

---

# Atlas Setup Steps

1. Open MongoDB Atlas.
2. Create free M0 cluster.
3. Create database user:
   ```text
   lp2_user
   ```
4. Set password:
   ```text
   lp2password123
   ```
5. Go to:
   ```text
   Network Access
   ```
6. Click:
   ```text
   Add IP Address
   ```
7. Click:
   ```text
   Allow Access From Anywhere
   ```
8. Confirm:
   ```text
   0.0.0.0/0
   ```

MongoDB Atlas automatically creates database `blogdb` after first insertion.

---

# Step 6 - Clone Repository

```bash
git clone https://github.com/4SNA/Blog-app-LP-2.git
```

```bash
cd Blog-app-LP-2
```

Check files:

```bash
ls
```

Expected:

```text
backend
frontend
README.md
```

---

# Step 7 - Backend Setup

Go to backend:

```bash
cd backend
```

Install dependencies:

```bash
npm install
```

Create `.env`:

```bash
nano .env
```

Paste:

```env
MONGO_URI=mongodb+srv://lp2_user:lp2password123@cluster0.tki1kaj.mongodb.net/blogdb?retryWrites=true&w=majority&appName=Cluster0
JWT_SECRET=sarthak123
PORT=5000
```

Save:

```text
CTRL + X → Y → Enter
```

Check `.env`:

```bash
cat .env
```

---

# Step 8 - Frontend Setup

Go to frontend:

```bash
cd ../frontend
```

Install frontend dependencies:

```bash
npm install
```

---

# Step 8.1 - Fix localhost API Issue

Search localhost references:

```bash
grep -R "localhost" .
```

If you find:

```text
http://localhost:5000
```

replace with your EC2 public IP.

Example:

```text
http://15.206.xxx.xxx:5000
```

Quick replace:

```bash
grep -rl "localhost:5000" . | xargs sed -i 's|http://localhost:5000|http://15.206.xxx.xxx:5000|g'
```

---

# Step 8.2 - Configure Vite Public Hosting

Open Vite config:

```bash
nano vite.config.js
```

OR

```bash
nano vite.config.ts
```

Add:

```js
server: {
  host: "0.0.0.0",
  port: 5173
}
```

Example:

```js
import { defineConfig } from 'vite'
import react from '@vitejs/plugin-react'

export default defineConfig({
  plugins: [react()],
  server: {
    host: "0.0.0.0",
    port: 5173
  }
})
```

---

# Step 8.3 - Run Frontend

```bash
npm run dev -- --host 0.0.0.0
```

Expected:

```text
Local:   http://localhost:5173
Network: http://172.xxx.xxx.xxx:5173
```

---

# Step 9 - Run Backend Using PM2

Open new terminal or reconnect SSH.

Go to backend:

```bash
cd ~/Blog-app-LP-2/backend
```

Check files:

```bash
ls
```

If `server.js` exists:

```bash
pm2 start server.js
```

Save PM2 process:

```bash
pm2 save
```

Check status:

```bash
pm2 list
```

---

# Step 10 - Test Backend Locally

```bash
curl http://localhost:5000
```

If HTML or JSON appears:
backend is working.

---

# Step 11 - Open Application Publicly

## Backend

```text
http://YOUR_PUBLIC_IP:5000
```

---

## Frontend

```text
http://YOUR_PUBLIC_IP:5173
```

Example:

```text
http://15.206.xxx.xxx:5173
```

---

# Step 12 - Verify MongoDB Atlas

1. Open MongoDB Atlas.
2. Go to Database.
3. Click Browse Collections.
4. Create one blog post.
5. Database `blogdb` should appear.
6. Collections/documents should be visible.

---

# Useful Commands

## Check PM2

```bash
pm2 list
```

---

## View Logs

```bash
pm2 logs
```

---

## Restart Backend

```bash
pm2 restart all
```

---

## Stop Backend

```bash
pm2 stop all
```

---

## Delete PM2 Processes

```bash
pm2 delete all
```

---

## Test Backend

```bash
curl http://localhost:5000
```

---

# Common Errors and Fixes

---

## 1. pm2 command not found

```bash
sudo npm install -g pm2
```

---

## 2. Website not opening publicly

Check Security Group.

Required:

| Type | Port |
|---|---|
| Custom TCP | 5000 |
| Custom TCP | 5173 |

Source:

```text
0.0.0.0/0
```

---

## 3. MongoDB Atlas connection failed

Check:

```text
Network Access → 0.0.0.0/0
```

Check `.env`:

```env
MONGO_URI=mongodb+srv://lp2_user:lp2password123@cluster0.tki1kaj.mongodb.net/blogdb?retryWrites=true&w=majority&appName=Cluster0
```

Restart:

```bash
pm2 restart all
pm2 logs
```

---

## 4. Frontend works only on localhost

Fix Vite config:

```js
server: {
  host: "0.0.0.0",
  port: 5173
}
```

Run:

```bash
npm run dev -- --host 0.0.0.0
```

---

## 5. localhost API issue

Replace:

```text
http://localhost:5000
```

with:

```text
http://YOUR_PUBLIC_IP:5000
```

---

## 6. EADDRINUSE port 5000 already in use

Fix:

```bash
pm2 delete all
pm2 start server.js
```

---

## 7. Cannot find module

Run:

```bash
npm install
```

inside frontend/backend.

---

## 8. server.js not found

Check:

```bash
ls
```

If app.js exists:

```bash
pm2 start app.js
```

If index.js exists:

```bash
pm2 start index.js
```

---

# Features

- Create blog posts
- View blog posts
- Update blog posts
- Delete blog posts
- MongoDB Atlas integration
- Cloud deployment
- Public access using EC2

---

# Viva Questions

## What is EC2?

AWS virtual machine service.

---

## What is MongoDB Atlas?

Cloud-based MongoDB database service.

---

## What is PM2?

Node.js process manager.

---

## Why ports 5000 and 5173?

- 5000 → Backend API
- 5173 → React + Vite frontend

---

## What is Security Group?

AWS firewall controlling traffic.

---

## Why `.env` file?

Stores secret/environment variables.

---

# Output

The Blog Application is successfully deployed and publicly accessible using:

```text
http://YOUR_PUBLIC_IP:5173
```

---

# Conclusion

The full-stack Blog Application was successfully deployed on AWS EC2 Ubuntu using React + Vite frontend, Node.js/Express backend, and MongoDB Atlas database. The application supports CRUD operations and is publicly accessible through EC2 public IP.
