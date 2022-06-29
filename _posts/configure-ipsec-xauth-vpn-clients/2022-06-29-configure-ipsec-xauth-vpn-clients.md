---
title: configure-ipsec-xauth-vpn-clients
date: 2020-01-25 11:58:47 +07:00
modified: 2020-02-02 16:49:47 +07:00
tags: [snippets]
description: IPsec/XAuth mode is also called "Cisco IPsec". This mode is generally faster than IPsec/L2TP with less overhead. IPsec/XAuth ("Cisco IPsec")
image: "https://rtd.ditatompel.com/content/images/size/w2000/2019/06/tyler-franta-iusJ25iYu1c-unsplash-cisco.jpg"
---

IPsec/XAuth mode is also called "Cisco IPsec". This mode is generally faster than IPsec/L2TP with less overhead. IPsec/XAuth ("Cisco IPsec") is natively supported by Android, iOS, and MacOS. There is no additional software to install for them.

```bash
Notes : You should upgrade Libreswan to the latest version due to IKEv1 informational exchange packets not integrity checked (CVE-2019-10155)
```
