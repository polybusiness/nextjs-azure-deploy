🚀 Next.js Deployment on Azure VM

Deployed a Next.js app on an Azure Virtual Machine (Ubuntu 20.04 LTS) using PM2 and Nginx for production.

📌 Overview
This project covers:
✅ Setting up an Azure VM
✅ Installing Next.js & running it with PM2
✅ Configuring Nginx as a reverse proxy
✅ Securing SSH access & enabling a firewall

📌 1️⃣ Steps to Set Up the Azure VM

Create an Azure Virtual Machine

OS: Ubuntu 20.04 LTS

Instance Size: B1s

Allow HTTP (80) & SSH (22)

Connect to the VM using SSH: 
    ssh -i "your-key.pem" azureuser@your-vm-ip

Install Node.js & Git: 
    sudo apt update && sudo apt install -y nodejs npm git

📌 2️⃣ Deploy Next.js App

Clone or Create a Next.js App: 
    npx create-next-app my-app
    cd my-app
    npm install
    npm run build
    

Run the App in the Background with PM2:
    sudo npm install -g pm2
    pm2 start npm --name "nextjs-app" -- start
    pm2 save
    pm2 startup

📌 3️⃣ Configure Nginx Reverse Proxy

Install Nginx:
    sudo apt install nginx -y

Edit Nginx Config:
    sudo nano /etc/nginx/sites-available/default
  Paste this:
    server {
      listen 80;
      location / {
          proxy_pass http://localhost:3000;
          proxy_set_header Host $host;
          proxy_set_header X-Real-IP $remote_addr;
      }
    }

Restart Nginx:
    sudo systemctl restart nginx

📌 4️⃣ Secure the Server

Create a New User for SSH:
    sudo adduser devopsuser
    sudo usermod -aG sudo devopsuser

Disable Root Login:
    sudo nano /etc/ssh/sshd_config
    Change:
      PermitRootLogin no
      PasswordAuthentication no

Restart SSH:
    sudo systemctl restart sshd

Enable Firewall:
    sudo ufw allow OpenSSH
    sudo ufw allow 80/tcp
    sudo ufw allow 443/tcp
    sudo ufw enable

📌 5️⃣ Testing the App

Run:
  curl http://localhost:3000

Visit http://your-vm-public-ip in a browser


SCREENSHOTS:

✅ Azure VM Configuration
![image](https://github.com/user-attachments/assets/f8b4916a-83e0-419c-abcd-0ebd24ef025d)


✅ Next.js Running on the VM
![image](https://github.com/user-attachments/assets/7e41a2b7-3745-43ca-825b-0221daafc1ea)


✅ Firewall & Security Setup
![image](https://github.com/user-attachments/assets/a00f8750-45bd-4853-83bf-40f8ba656647)
![image](https://github.com/user-attachments/assets/4aecb4ab-3c7b-4445-80bf-30ae181f7720)


✅ Final Deployed App in Browser
![image](https://github.com/user-attachments/assets/5146680b-3b2d-492f-ad8e-f105c16edb2c)
