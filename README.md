# 🔐 Certbot DNS-01 Automation

This guide provides instructions to automatically issue and renew SSL certificates
using [Certbot](https://certbot.eff.org/) with a DNS-01 challenge on:

- ✅ Google Cloud DNS
- ✅ DigitalOcean DNS

---

## 📦 Prerequisites

- A Linux server (Ubuntu/Debian)
- Root or sudo access
- [Certbot](https://certbot.eff.org/instructions) installed
- Your domain is managed by Google Cloud DNS or DigitalOcean DNS
- Domain is already pointing to your server (optional for DNS-01, but required for using HTTPS)

---

## 🌐 Google Cloud DNS Setup

### Step 1: Install Certbot DNS Plugin for Google

```bash
sudo apt-get update
sudo apt-get install python3-certbot-dns-google
```

### Step 2: Create Google Cloud Service Account Key

1. Go to Google Cloud Console > IAM & Admin > Service Accounts
2. Create a new service account with the role: DNS Administrator
3. Generate and download the JSON key

### Step 3: Move and Secure the Credentials

```bash
sudo mv /path/to/your-key.json /etc/letsencrypt/gcloud-dns.json
sudo chmod 600 /etc/letsencrypt/gcloud-dns.json
```

### Step 4: Issue the SSL Certificate

```bash
sudo certbot certonly \
  --dns-google \
  --dns-google-credentials /etc/letsencrypt/gcloud-dns.json \
  -d "yourdomain.com" \
  -d "*.yourdomain.com"
```

## ☁️ DigitalOcean DNS Setup

### Step 1: Install Certbot DNS Plugin for DigitalOcean

```bash
sudo apt-get update
sudo apt-get install python3-certbot-dns-digitalocean
```

### Step 2: Create a DigitalOcean API Token

1. Visit: https://cloud.digitalocean.com/account/api/tokens
2. Generate a Personal Access Token with read/write access.

### Step 3: Store the API Token Securely

```bash
sudo mkdir -p /etc/letsencrypt
sudo nano /etc/letsencrypt/digitalocean.ini
```

Paste the following line into the file:

```int
dns_digitalocean_token = YOUR_DIGITALOCEAN_API_TOKEN
```

Secure the file:

```bash
sudo chmod 600 /etc/letsencrypt/digitalocean.ini
```

### Step 4: Issue the SSL Certificate

```bash
sudo certbot certonly \
  --dns-digitalocean \
  --dns-digitalocean-credentials /etc/letsencrypt/digitalocean.ini \
  -d "yourdomain.com" \
  -d "*.yourdomain.com"
```