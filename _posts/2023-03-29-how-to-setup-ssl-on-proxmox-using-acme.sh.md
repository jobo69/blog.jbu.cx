---
title: "How to setup SSL on Proxmox using acme.sh"
date: 2023-03-29T02:30:00+02:00
draft: false
tags: [lets-encrypt,acme.sh,ssl,proxmox,zerossl]
image: /static/media/posts/how-to-setup-ssl-on-proxmox-using-acme.sh/franck-DoWZMPZ-M9s-unsplash.jpg
---

## Introduction
This tutorial shows how to set up SSL on Proxmox using the acme.sh script. I will be using a manual DNS Challenge for verification, since my Proxmox host is not publicly accessible and my Authoritative DNS Server does not currently support `acme.sh` (as far as I know).

## Downloading and Installing acme.sh
1. Download acme.sh
```bash
wget https://raw.githubusercontent.com/acmesh-official/acme.sh/master/acme.sh
```
2. Make acme.sh executable
```bash
chmod +x acme.sh
```
3. Install acme.sh
```bash
./acme.sh --install
```
4. Register on the acme server
```bash
./acme.sh --register-account -m your@email.com
```

## Issuing a Certificate
1. Issue a certificate using DNS Challenge
```bash
./acme.sh --issue --dns --yes-I-know-dns-manual-mode-enough-go-ahead-please -d your.domain.com
```
2. You should now see a console output that looks something like this:
```bash
[Wed 29 Mar 2023 09:53:58 PM CEST] Add the following TXT record:
[Wed 29 Mar 2023 09:53:58 PM CEST] Domain: '_acme-challenge.your.domain.com'
[Wed 29 Mar 2023 09:53:58 PM CEST] TXT value: 'SUzVFknn1NS8SddmQ9x4Z9TSXltuFgu8nNC6WLTFEwg'
[Wed 29 Mar 2023 09:53:58 PM CEST] Please be aware that you prepend _acme-challenge. before your domain
[Wed 29 Mar 2023 09:53:58 PM CEST] so the resulting subdomain will be: _acme-challenge.your.domain.com
[Wed 29 Mar 2023 09:53:58 PM CEST] Please add the TXT records to the domains, and re-run with --renew.
```
5. Now add the DNS Records shown in the console to your domain.
4. Run this command to renew (issue) the Certificate
```bash
./acme.sh --renew --dns --yes-I-know-dns-manual-mode-enough-go-ahead-please -d your.domain.com
```

## Installing the Certificate on the host
1. To install the Certificate, we run the following command:
```bash
./acme.sh --installcert -d your.domain.com --certpath /etc/pve/local/pveproxy-ssl.pem --keypath /etc/pve/local/pveproxy-ssl.key --capath  /etc/pve/local/pveproxy-ssl.pem --reloadcmd "systemctl restart pveproxy"
```

## Conclusion
The Certificate should now be installed on your Proxmox host.
![](/static/media/posts/how-to-setup-ssl-on-proxmox-using-acmesh/installed-ssl-cert.png) 
