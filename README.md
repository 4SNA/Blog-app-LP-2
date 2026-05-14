# Assignment 9 - Online Blog Application Deployment

## Problem Statement

Deploy a full-stack blog application frontend, backend, and database on a cloud platform. Configure the server environment so that users can create, view, and manage blog posts through a web interface.

## Repository

```text
https://github.com/4SNA/Blog-app-LP-2
```

## Technologies Used

- AWS EC2 Ubuntu Instance
- React.js Frontend
- Node.js + Express.js Backend
- MongoDB Atlas Database
- Git
- npm
- PM2 Process Manager

---

# Step 1 - Create AWS EC2 Instance

1. Open AWS Console.
2. Go to EC2.
3. Click Launch Instance.
4. Select Ubuntu.
5. Select t2.micro.
6. Create or select key pair.
7. Download `.pem` file.
8. Configure Security Group.

## Security Group Inbound Rules

| Type | Port | Source |
|---|---|---|
| SSH | 22 | My IP |
| HTTP | 80 | 0.0.0.0/0 |
| Custom TCP | 5000 | 0.0.0.0/0 |

---

# Step 2 - Connect to EC2

Open terminal or PowerShell in folder where `.pem` file is present.

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

# Step 4 - Install Node.js, npm, Git and PM2

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

Verify PM2:

```bash
pm2 --version
```

---

# Step 5 - MongoDB Atlas Setup

Use MongoDB Atlas instead of installing MongoDB locally.

## Atlas Details

```text
Username: lp2_user
Password: lp2password123
Database Name: blogdb
Cluster: Cluster0
```

## Atlas Steps

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
5. Go to Network Access.
6. Click Add IP Address.
7. Click Allow Access From Anywhere.
8. Confirm:
   ```text
   0.0.0.0/0
   ```

MongoDB Atlas will automatically create `blogdb` after first data insertion.

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

Expected folders:

```text
backend
frontend
README.md
```

---

# Step 7 - Backend Setup

```bash
cd backend
```

Install backend dependencies:

```bash
npm install
```

Create `.env` file:

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

---

# Step 9 - Run Backend Using PM2

```bash
cd ../backend
```

Check backend file:

```bash
ls
```

If `server.js` exists, run:

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

If HTML or JSON output appears, backend is running.

---

# Step 11 - Access Application Publicly

Open browser:

```text
http://YOUR_PUBLIC_IP:5000
```

Example:

```text
http://13.126.xxx.xxx:5000
```

---

# Step 12 - Verify MongoDB Atlas

1. Open MongoDB Atlas.
2. Go to Database.
3. Click Browse Collections.
4. Create or insert one blog post from the application.
5. Database `blogdb` should appear.
6. Collection should contain blog post documents.

---

# Useful Commands

Check running apps:

```bash
pm2 list
```

View logs:

```bash
pm2 logs
```

Restart app:

```bash
pm2 restart all
```

Stop app:

```bash
pm2 stop all
```

Delete all PM2 processes:

```bash
pm2 delete all
```

Check port:

```bash
curl http://localhost:5000
```

---

# Common Errors and Fixes

## 1. pm2 command not found

```bash
sudo npm install -g pm2
```

---

## 2. Website not opening in browser

Check AWS Security Group.

Required rule:

```text
Custom TCP | 5000 | 0.0.0.0/0
```

---

## 3. MongoDB connection error

Check Atlas Network Access:

```text
0.0.0.0/0
```

Check `.env`:

```env
MONGO_URI=mongodb+srv://lp2_user:lp2password123@cluster0.tki1kaj.mongodb.net/blogdb?retryWrites=true&w=majority&appName=Cluster0
```

Restart backend:

```bash
pm2 restart all
pm2 logs
```

---

## 4. EADDRINUSE port 5000 already in use

This means backend is already running.

Fix:

```bash
pm2 delete all
pm2 start server.js
```

---

## 5. Cannot find module

Run:

```bash
npm install
```

inside backend or frontend folder.

---

## 6. server.js not found

Check backend files:

```bash
ls
```

If backend has `app.js`, run:

```bash
pm2 start app.js
```

If backend has `index.js`, run:

```bash
pm2 start index.js
```

---

# Features

- Create blog posts
- View blog posts
- Update blog posts
- Delete blog posts
- Cloud deployment
- MongoDB Atlas database
- Public access using EC2 public IP

---

# Output

The full-stack blog application is deployed successfully on AWS EC2 and is accessible publicly using:

```text
http://YOUR_PUBLIC_IP:5000
```

---

# Viva Questions

## What is EC2?

EC2 is Elastic Compute Cloud, a virtual machine service provided by AWS.

## What is PM2?

PM2 is a Node.js process manager used to keep backend applications running continuously.

## What is MongoDB Atlas?

MongoDB Atlas is a cloud-based MongoDB database service.

## What is Security Group?

Security Group is a virtual firewall that controls inbound and outbound traffic for EC2.

## Why port 5000?

The Node.js backend application runs on port 5000.

## What is `.env` file?

`.env` file stores configuration variables like database URL, port number, and secret key.

---

# Conclusion

The Blog Application was successfully deployed on AWS EC2 using React frontend, Node.js/Express backend, and MongoDB Atlas database. The application supports CRUD operations for blog posts and is publicly accessible through the EC2 public IP.
