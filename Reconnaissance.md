
This report outlines the results of the initial reconnaissance phase conducted on the Metasploitable virtual machine within a controlled cybersecurity lab environment. Using Kali Linux as the attacking machine, various reconnaissance techniques were employed, including network scanning with Nmap, service enumeration, and vulnerability assessment. The purpose of this phase was to identify open ports, services, and potential vulnerabilities in the target system to guide further penetration testing efforts.

---

### Step 1 :

**In Metasploitable**

- Executed the command:

```shell
ifconfig
```

to Check the ip address of the machine.

```Shell
eth0      inet6 addr: 192.168.1.9
```

So, our Metasploitable's ip is `192.168.1.9`


**In Kali**

1) Executed the `ping` command to check if the machine was up.

```shell
ping 192.168.1.9
```

Output:

```shell
PING 192.168.1.9 (192.168.1.9) 56(84) bytes of data.
64 bytes from 192.168.1.9: icmp_seq=1 ttl=64 time=1.10 ms
64 bytes from 192.168.1.9: icmp_seq=2 ttl=64 time=1.58 ms
64 bytes from 192.168.1.9: icmp_seq=3 ttl=64 time=1.82 ms
64 bytes from 192.168.1.9: icmp_seq=4 ttl=64 time=1.63 ms
64 bytes from 192.168.1.9: icmp_seq=5 ttl=64 time=0.925 ms
```

Since the ip returned `icmp` packets, we now know that the machine is up.

2) Without any waste of time I performed a normal `nmap` SYN scan:

```shell
sudo nmap -sS 192.168.1.9
```

Output:

```shell
Starting Nmap 7.94SVN ( https://nmap.org ) at 2025-01-15 23:37 EST
Nmap scan report for 192.168.1.9 (192.168.1.9)
Host is up (0.0011s latency).
Not shown: 977 closed tcp ports (reset)
PORT     STATE SERVICE
21/tcp   open  ftp
22/tcp   open  ssh
23/tcp   open  telnet
25/tcp   open  smtp
53/tcp   open  domain
80/tcp   open  http
111/tcp  open  rpcbind
139/tcp  open  netbios-ssn
445/tcp  open  microsoft-ds
512/tcp  open  exec
513/tcp  open  login
514/tcp  open  shell
1099/tcp open  rmiregistry
1524/tcp open  ingreslock
2049/tcp open  nfs
2121/tcp open  ccproxy-ftp
3306/tcp open  mysql
5432/tcp open  postgresql
5900/tcp open  vnc
6000/tcp open  X11
6667/tcp open  irc
8009/tcp open  ajp13
8180/tcp open  unknown
MAC Address: 00:0C:29:CB:8C:2A (VMware)

Nmap done: 1 IP address (1 host up) scanned in 11.53 seconds
```

It's no surprise seeing a vulnerable machine having many open ports. Let's go through some common ports and how they can be exploited.

3) Before going through common ports, i would like to gather more information.

4) Performed another nmap scan, A Service Detection and Traceroute Scan:

```shell
sudo nmap -O 192.168.1.9
```

Output:
```shell
Device type: general purpose
Running: Linux 2.6.X
OS CPE: cpe:/o:linux:linux_kernel:2.6
OS details: Linux 2.6.9 - 2.6.33
Network Distance: 1 hop
```


Now we found out what operating system the machine uses.

---

### **Step 2: Nikto Scan Results**

A Nikto scan was conducted against the target Metasploitable web server running on `192.168.1.9`. The scan revealed several potential vulnerabilities and misconfigurations, summarized below:

#### **Key Findings:**

1. **Web Server Information:**
    
    - **Server:** Apache/2.2.8 (Ubuntu) DAV/2
    - The server version is outdated. Apache 2.2.34 is the end-of-life version for the 2.x branch, with the latest stable release being Apache 2.4.x.
    
2. **Headers and Configuration Weaknesses:**
    
    - **X-Frame-Options Header Missing:** The lack of this header makes the server vulnerable to clickjacking attacks.
    - **X-Content-Type-Options Header Missing:** This could allow MIME type mismatches, enabling attacks like drive-by downloads.
    - **HTTP TRACE Method Enabled:** This indicates the server may be vulnerable to Cross-Site Tracing (XST) attacks.
3. **Directory Indexing:**
    
    - **/doc/** and **/test/** directories are browsable, exposing potentially sensitive files or directories.
    - **/icons/** directory is accessible, revealing Apache default files like `README`.
4. **PHP Information Disclosure:**
    
    - A `phpinfo.php` file was found, exposing detailed system information that can assist attackers in identifying potential exploits.
5. **phpMyAdmin Misconfigurations:**
    
    - The **/phpMyAdmin/** directory and associated files like `changelog.php`, `README`, and `Documentation.html` are accessible, exposing sensitive information about database management.
6. **Sensitive Files Found:**
    
    - A `#wp-config.php#` file was detected. This file often contains credentials and configuration details for WordPress installations, which could lead to unauthorized access.
7. **Apache Misconfigurations:**
    
    - Apache mod_negotiation with MultiViews is enabled, potentially allowing brute-forcing of filenames.

#### **Exploitation Opportunities:**

- The accessible **phpinfo.php** and **phpMyAdmin** directories are prime targets for further exploitation.
- Outdated Apache and PHP versions could be checked against known vulnerabilities (e.g., CVEs) for exploitation.
- The `#wp-config.php#` file can be analyzed for credentials or sensitive configuration information.


### **Conclusion**

The reconnaissance phase conducted on the Metasploitable virtual machine provided valuable insights into the target system's configuration and vulnerabilities. Using tools such as Nmap and Nikto, several weaknesses were identified, including outdated software versions, misconfigured headers, directory indexing, and exposed sensitive files like `phpinfo.php` and `#wp-config.php#`. These findings highlight potential exploitation opportunities that can be leveraged in subsequent penetration testing phases.

The results of this initial assessment demonstrate the importance of securing systems against common misconfigurations and keeping software up to date. Moving forward, the identified vulnerabilities will serve as a foundation for deeper analysis and potential exploitation in a controlled environment to better understand the risks associated with these misconfigurations. This phase not only enhanced practical skills in reconnaissance but also laid the groundwork for a systematic approach to penetration testing.