---
title: "CTF: Arson Wireshark "
description: >- 
  The purpose of the lab is to analyze Wireshark packets to extract a script   
  
author: youssef
date: 2025-09-29 18:00:00 +0100
categories: [CTF Writeups,cybertalents]
tags: [CTF Writeups,cybersecurity,Analyze,network,Network Forensics,Tools]
image:
  path: assets/img/CTF/Arson/image copy 4.png
---


The purpose of the lab is to analyze **Wireshark** packets to extract a script.
The difficulty of this lab is **medium**

1. Download the `.pcapng` file.

2. Open **Wireshark** and load the downloaded file.
![Desktop View](assets/img/CTF/Arson/image.png){: width="972" height="589" }
 
## **Let’s start**

Based on the challenge description, it can be assumed that the user might have downloaded the script from an **HTTP** link sent by his friend.

So Let’s filter the traffic to display only **HTTP** packets.
![Desktop View](assets/img/CTF/Arson/image copy.png){: width="972" height="589" }

So As you can see, there is **HTTP** communication between 2 hosts: **192.168.1.4** and **192.168.1.11**
maybe exchange data

After searching through these requests, I found a **PowerShell** script that was being downloaded. 


I was right 😎
![Desktop View](assets/img/CTF/Arson/Screenshot from 2025-09-29 20-43-46.png){: width="972" height="589" }

Now let's follow the **TCP stream** to see and download the PowerShell script.

![Desktop View](assets/img/CTF/Arson/image copy 2.png){: width="972" height="589" }

When reading the script, I noticed it has 2 functions: one for **encryption** and one for **decryption**. Both use **AES-CBC** to encrypt the key, IV, and the data

> uses the `AES` algorithm with a secret key to encrypt data in blocks.
{: .prompt-info }
![Desktop View](assets/img/CTF/Arson/Screenshot from 2025-09-29 22-28-21.png){: width="972" height="589" }
![Desktop View](assets/img/CTF/Arson/Screenshot from 2025-09-29 22-24-29.png){: width="972" height="589" }

This is the **Key**
![Desktop View](assets/img/CTF/Arson/Screenshot from 2025-09-29 23-08-31.png){: width="972" height="589" }


And I noticed that there is some data transferred to the friend over **HTTP**

![Desktop View](assets/img/CTF/Arson/Screenshot from 2025-09-29 22-37-13.png){: width="972" height="589" }

So let's go back to **Wireshark** and search for that Data
![Desktop View](assets/img/CTF/Arson/Screenshot from 2025-09-29 22-42-54.png){: width="972" height="589" }

Now follow the **TCP stream**

![Desktop View](assets/img/CTF/Arson/Screenshot from 2025-09-29 22-47-04.png){: width="972" height="589" }
![Desktop View](assets/img/CTF/Arson/image copy 3.png){: width="972" height="589" }

So I found the data.
Now it’s time to decrypt it.

There is 2 options :
  1. use **CyberChef**
  2. Write a script to decrypt it (the script includes a decryption function — reverse it).
 
Going with **CyberChef**.

First, I decoded that string in **result variable**:
  > `irbYP4XxfwuTlCbMxv4CE9KdquYNczFCMziT5VTG6aS++MDZiChw3YJbtbrvt4FKO2WmdKwVBqjdX4xDguV7slrxsNNLqVbSOCceAURzkhNDvaMOIg8a0tPx3G7U+PUH`


After decrypting it, I found:
  > `Machine_Name(t3st3r)Username(SEC401-Student)LocalIPs(192.168.1.4)`

This wasn’t the flag. 


So went back to Wireshark and continued searching until I found the correct string.


![Desktop View](assets/img/CTF/Arson/Screenshot from 2025-09-30 18-53-46.png){: width="972" height="589" }

![Desktop View](assets/img/CTF/Arson/Screenshot from 2025-09-30 18-54-38.png){: width="972" height="589" }


I decoded and decrypted the string, which revealed the flag.

> The flag is **flag{2C_p0w3r_Chi11}**
{: .prompt-info }
