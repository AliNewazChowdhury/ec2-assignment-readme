# Next.js Application Deployment on AWS EC2

## 📋 Project Information

**Project Name:** Next Japan  
**Deployment Date:** November 10, 2025  
**Deployed By:** Ali Newaz Chowdhury  
**Public IP:** http://3.19.72.136:3000  
**Server:** AWS EC2 (Ubuntu 24.04 LTS)

---

## 🚀 Deployment Overview

This document outlines the complete deployment process of a Next.js application on AWS EC2 using PM2 for process management.


## 🖥️ EC2 Instance Setup

### Instance Configuration

- **Instance Type:** t2.micro (Free tier eligible)
- **AMI:** Ubuntu 24.04 LTS
- **Storage:** 8 GB gp3
- **Public IPv4:** 3.19.72.136
- **Private IP:** 172.31.44.146

### Security Group Configuration

| Type  | Protocol | Port Range | Source    | Description           |
|-------|----------|------------|-----------|-----------------------|
| SSH   | TCP      | 22         | My IP     | SSH access            |
| HTTP  | TCP      | 80         | 0.0.0.0/0 | HTTP web traffic      |
| HTTPS | TCP      | 443        | 0.0.0.0/0 | HTTPS web traffic     |
| Custom| TCP      | 3000       | 0.0.0.0/0 | Next.js application   |

### Initial Connection

```bash
# Set key permissions
chmod 400 ~/.ssh/nextjs-server-key.pem

# Connect to EC2
ssh -i ~/.ssh/nextjs-server-key.pem ubuntu@3.19.72.136
```

---

## ⚙️ Environment Configuration

### 1. System Update

```bash
sudo apt update && sudo apt upgrade -y
```

### 2. Node.js Installation (v24.11.0 LTS)

```bash
# Install NVM
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.7/install.sh | bash

# Load NVM
source ~/.bashrc

# Install Node.js LTS
nvm install --lts

# Verify installation
node --version  # v24.11.0
npm --version   # 10.9.0
```

### 3. PM2 Installation

```bash
# Install PM2 globally
npm install -g pm2

# Verify installation
pm2 --version
```

### 4. Git Installation

```bash
sudo apt install git -y
git --version
```

---

## Application Deployment

### 1. Clone Repository

```bash
cd ~
git clone https://github.com/mdarifahammedreza/next-japan
cd next-japan
```

### 2. Install Dependencies

```bash
npm install
```


### 3. Build Application

```bash
npm run build
```


### 4. Start with PM2

```bash
pm2 start npm --name "nextjs-app" -- start
```

**Result:**
```
┌────┬───────────────┬─────────────┬─────────┬─────────┬──────────┬────────┬──────┬───────────┬──────────┬──────────┬──────────┬──────────┐
│ id │ name          │ namespace   │ version │ mode    │ pid      │ uptime │ ↺    │ status    │ cpu      │ mem      │ user     │ watching │
├────┼───────────────┼─────────────┼─────────┼─────────┼──────────┼────────┼──────┼───────────┼──────────┼──────────┼──────────┼──────────┤
│ 0  │ nextjs-app    │ default     │ 0.39.7  │ fork    │ 6675     │ 0s     │ 0    │ online    │ 0%       │ 36.8mb   │ ubuntu   │ disabled │
└────┴───────────────┴─────────────┴─────────┴─────────┴──────────┴────────┴──────┴───────────┴──────────┴──────────┴──────────┴──────────┘
```


Challenges Faced & Solutions
1. Kernel Update Requirement
Challenge: After system upgrade, the server required a reboot to load the new kernel version (6.14.0-1016-aws) but was still running the old version (6.14.0-1015-aws).

Solution: Documented the need for reboot but continued deployment since it wasn't critical for application functionality.

2. NVM Installation
Challenge: Needed to source the bashrc file after NVM installation to make it available in the current session.

Solution: Ran source ~/.bashrc after NVM installation.

3. PM2 Daemonization
Challenge: First PM2 command automatically spawned the daemon, which is normal but required understanding the process.

Solution: This is expected PM2 behavior - it automatically starts the daemon on first use.

4. Node.js Version Management
Challenge: Ensuring consistent Node.js version across deployments.

Solution: Used NVM to install and manage the LTS version of Node.js (v24.11.0).
