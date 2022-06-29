---
title: How to Host a STORJ Node
date: 2022-06-29 10:55:00 +07:00
modified: 2022-06-29 10:55:00 +07:00
tags: [unix/linux, host]
description: Shell adalah sebuah command-line interpreter; program yang berperan sebagai penerjemah perintah yang diinputkan oleh User yang melalui terminal, sehingga perintah tersebut bisa dimengerti oleh si Kernel.
image: "https://rtd.ditatompel.com/content/images/size/w2000/2022/01/storj-node-operator-1.png"
---

Storj *(pronounced as "storage"),* is an open-source cloud storage platform, *S3-compatible* platform and suite of decentralized applications that allows you to store data in a secure and decentralized manner. Basically, it uses a decentralized network of nodes to host user data. The platform also secures hosted data using advanced encryption.

In a **white paper** published in December,2014, Storj was first introduced to the world as a concept. It was to be a decentralized peer-to-peer encrypted cloud storage platform.

To host a Node, your hardware and bandwidth will need to meet a few requirements:
- One processor core
- Minimum 550GB of available disk space
- Minimum of 2TB of available bandwidth a month
- Minimum upstream bandwidth of 5 Mbps
- Minimum download bandwidth of 25 Mbps
- Keep your node online 24/7
- Read and agree to the ***Storj Storage Node Operator Terms and Conditions***

If you can follow above requirement, you are good to go. In this article, I use Ubuntu 18.04 running on VPS with public IP address, 4 cores processor, 2GB RAM and 600GB additional disk partition for Storj data.

## Request authorization token
Sign up and request authorization token. Make sure you have received your personal single-use authorization token. It looks like this:

![image1](https://ap1.ditatompel.com/ghost/rtd/img/storj-node-auth-token.png)

The entire string, including your email is your auth token.

## Prepare the system
Make sure your system is up to date by running **apt update** and **apt upgrade.**

## UDP config
UDP transfers on high-bandwidth connections can be limited by the size of UDP recieve buffer.

Storj sofware attemps to increase the UDP recieve buffer size. However, on Linux, an application is only allowed to increase the buffer size up to a max value set in the kernel, and the the default maximum value is too small for high bandwidth UDP transfers.

Its is recommended to increase the max buffer size by running the following command to increase it to 2.5MB.

```sh
echo "net.core.rmem_max=2500000" >> /etc/sysctl.conf
sysctl -w net.core.rmem_max=2500000
```

## Generate node identity

Every node is required to have a unique identifier on the network.
You need to download Storj identity binary to generate your node identity (as regular user, not as root):

```sh
curl -L https://github.com/storj/storj/releases/latest/download/identity_linux_amd64.zip -o identity_linux_amd64.zip
unzip -o identity_linux_amd64.zip
chmod +x identity
sudo mv identity /usr/local/bin/identity
```
Run command below to create an identity:

```sh
identity create storagenode
```
This process can take several minutes or hours or days, depending on your machines processing power and luck.

This process will continue until it reaches a difficulty of at least 36. On completion, it will look something like this:

```sh
Generated 5186393 keys; best difficulty so far: 36
Found a key with difficulty 36!
Unsigned identity is located in "/home/USERNAME/.local/share/storj/identity/storagenode"
Please *move* CA key to secure storage - it is only needed for identity management and isn't needed to run a storage node!
        /home/USERNAME/.local/share/storj/identity/storagenode/ca.key
```

## Authorize the identity

Authorize your Storage Node identity using your single-use authorization token from "Request authorization token" section above. (replace the placeholder ***<email:characterstring>*** to your actual authorization token):

```sh
identity authorize storagenode <email:characterstring>
```

When it's done and success, you will get result similar like this:

```sh
2021/10/26 02:07:22 proto: duplicate proto type registered: node.SigningRequest
2021/10/26 02:07:22 proto: duplicate proto type registered: node.SigningResponse
Identity successfully authorized using single use authorization token.
Please back-up "/home/USERNAME/.local/share/storj/identity/storagenode" to a safe location.
```
To make sure the authorizing identity is successful, run these 2 following commands:

```sh
grep -c BEGIN ~/.local/share/storj/identity/storagenode/ca.cert
grep -c BEGIN ~/.local/share/storj/identity/storagenode/identity.cert
```
