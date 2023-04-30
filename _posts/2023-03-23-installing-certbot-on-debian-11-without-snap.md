---
title: "Installing Certbot on Debian 11 Without Snap"
date: 2023-03-23T20:30:00+01:00
tags: [lets-encrypt,certbot,ssl,nginx]
draft: false
---

1. Install python3
```bash
apt install python3 python3-venv libaugeas0
```
2. Set up a virtual env in Python
```bash
python3 -m venv /opt/certbot/
```
Install pip in the virtual env
```bash
/opt/certbot/bin/pip install --upgrade pip
```
Install certbot
```bash
/opt/certbot/bin/pip install certbot certbot-nginx
```
Create Symlink for certbot
```bash
ln -s /opt/certbot/bin/certbot /usr/bin/certbot
```

3. Run Certbot
```bash
certbot --nginx --register-unsafely-without-email --agree-tos -d yourdomain.com
```
