
This document provides a step-by-step process of setting up a virtual Cybersecurity lab using Kali Linux and Metasploitable. By the end of this guide, we will have a functional lab environment for penetration testing for learning purposes.

Here are the steps I did, I didn't encounter any problem since I have been doing this for a long time:

---
### Step 1: Installing the ISOs and a Virtual Machine Launcher

- Download ISOs (Kali Linux and Metasploitable2) from their official sites, (https://www.kali.org/get-kali/#kali-installer-images), (https://docs.rapid7.com/metasploit/metasploitable-2/)

![[screenshot_2025-01-16_09-30-25.png]]
![[screenshot_2025-01-16_09-35-09.png]]



- For ease of use, I downloaded the pre-configured VMs.

- Since I already had VMWare installed, I launched it


### Step 2: Launching VMware and Configs

1) Here is the Screenshot before setup.

![[screenshot_2025-01-16_09-36-39.png]]

2) Here's my Kali VMs configurations:

![[screenshot_2025-01-16_09-38-37.png]]

3) And here is my Metasploitable configurations:

![[screenshot_2025-01-16_09-41-54.png]]

**NOTE: the default login credentials were provided.**

**For Metasploitable:**

username: msfadmin
password: msfadmin

**For Kali:**

username: kali
password: kali


### Step 3 : Updating the System

1) Since Kali Linux is a Debian-based Distro, It uses the apt packages so , we can use the command: 
   
   ```shell
   sudo apt update && sudo apt upgrade -y
```

NOTE: This command not only updates but also upgrades the packages if there is the need of any and the -y argument confirms the execution at compile-time.


![[screenshot_2025-01-16_09-47-29.png]]

2) **Why Metasploitable cannot be updated?**
	
	Metasploitable is a a purposely outdated and vulnerable environment to simulate real-world scenarios. Updating it would defeat its purpose.


---

## Conclusion

With the successful setup of Kali Linux and Metasploitable virtual machines, the Cybersecurity lab is now ready for penetration testing and vulnerability assessment. This isolated environment ensures a safe space to practice ethical hacking techniques without impacting real-world systems. The next steps involve performing reconnaissance, identifying vulnerabilities, and documenting findings to enhance Cybersecurity skills further.



