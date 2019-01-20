---
layout: post
title: How to Setup L2TP/IPsec VPN on Ubuntu 16.04
date: 2018-09-02 22:29:00 +0300
description: How to Setup L2TP/IPsec VPN on Ubuntu 16.04 with NetworkManager.
img: vpn.png # Add image post (optional)
tags: [VPN, Ubuntu, Linux, L2TP, L2TP/IPsec,  NetworkManager ] # add tag
---

# How to Setup L2TP/IPsec VPN on Ubuntu 16.04 with NetworkManager

In this article, we will learn how to setup L2TP/IPsec VPN with NetworkManager on Ubuntu 16.04. Ubuntu has stopped its support on L2TP since almost forever but there are a few workarounds and alternatives to overcome this problem.

### 1) Install network-manager-l2tp Package

```network-manager-l2tp``` package can be installed with adding related PPA;

```bash
sudo add-apt-repository ppa:nm-l2tp/network-manager-l2tp  
sudo apt-get update  
sudo apt-get install network-manager-l2tp   
```

You should also install ```network-manager-l2tp-gnome```, if you are using gnome desktop environment.

```bash
sudo apt-get install network-manager-l2tp-gnome
```

### 2) Stop xl2tpd Service

```bash
sudo service xl2tpd stop
sudo update-rc.d xl2tpd disable
```

### 3) Create Your L2TP/IPsec VPN

In order to create L2TP/IPsec VPN, use the fallowing instructions;

* Navigate to Settings > Network > Click the + button > Select "Layer 2 Tunneling Protocol (L2TP)"
* Name the new VPN connection something
* Put the host name or address in the Gateway field.
* Put username in the Username field.
* Click the icon in the Password field and select your preference for how to supply the password.
* Click IPSec Settings...
* Click the box for "Enable IPsec tunnel to L2TP host"
* Enter the shared secret into the Pre-shared key field.
* Leave the Gateway ID field empty.
* Expand the Advanced options area
* Enter "3des-sha1-modp1024" into the Phase 1 Algorithms box.
* Enter "3des-sha1" into the Phase 2 Algorithms box.
* Leave the box checked for "Enforce UDP encapsulation".
* Click OK.
* Click Save.

And, thats it!

### 4) Use libreswan instead of strongswan (Optional)

If you still have problems to connect your L2TP/IPsec VPN, you can try to use libreswan instead of strongswan. For doing this, remove the phase 1 & 2 algorithms in the IPsec config dialog box and install libreswan by issuing:

```bash
sudo apt-get install libreswan
```

And that's how you can use L2TP/IPsec VPN on your Ubunut 16.04! You can use ```tail -f /var/log/syslog``` to identify problems about your connection.
