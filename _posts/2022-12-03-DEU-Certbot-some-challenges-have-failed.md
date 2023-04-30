---
title: "[DEU] Certbot: Some Challenges Have Failed (NGINX)"
date: 2022-12-03T05:14:00+01:00
draft: false
tags: [deutsch,certbot,nginx,ipv6]
---

```
Requesting a certificate for subdomain1.domain.de and subdomain2.domain.de

Certbot failed to authenticate some domains (authenticator: nginx). The Certificate Authority reported these problems:
  Domain: subdomain1.domain.de
  Type:   unauthorized
  Detail: 2602:fd50:100:807:254a:6edf:efb8:c63b: Invalid response from http://subdomain1.domain.de/.well-known/acme-challenge/xxxxx: 404

  Domain: subdomain2.domain.de
  Type:   unauthorized
  Detail: 2602:fd50:100:807:254a:6edf:efb8:c63b: Invalid response from http://subdomain2.domain.de/.well-known/acme-challenge/xxxxx: 404

Hint: The Certificate Authority failed to verify the temporary nginx configuration changes made by Certbot. Ensure the listed domains point to this nginx server and that it is accessible from the internet.

Some challenges have failed.
Ask for help or search for solutions at https://community.letsencrypt.org. See the logfile /var/log/letsencrypt/letsencrypt.log or re-run Certbot with -v for more details.```
```

**FIX**: `listen [::]:80;` im server block adden, da mit `listen 80;` nginx nur auf IPv4 aber nicht auf IPv6 listent.