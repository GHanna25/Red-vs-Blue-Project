# RED VS BLUE PROJECT #2


### Day 1 Pen Test
## **EXPLOITATION**

### **Discover target IP:**

To discover the target ip:
```
netdiscover -r <ip subnet>
```

![alt-text](https://github.com/SamGeron/Red-Team-vs-Blue-Team/blob/main/images/Picture1.png)

![alt-text](https://github.com/SamGeron/Red-Team-vs-Blue-Team/blob/main/images/Picture2.png)

| IP | Machine |
|:-------------:|:-------------:|
| 192.168.1.1 | Gateway IP, Hyper-V |
| 192.168.1.100 | ELK server |
| 192.168.1.105 | Capstone, target machine |

### **Service and version scan:**

```
nmap -sV -v 192.168.1.105
```
| Port | Service | Version |
|:--:|:--:|:--:|
| Port 22 | SSH | OpenSSH 7.6p1 |
| Port 80 | HTTP | Apache httpd 2.4.29 |

![alt-text](https://github.com/SamGeron/Red-Team-vs-Blue-Team/blob/main/images/Picture3.png)

![alt-text](https://github.com/SamGeron/Red-Team-vs-Blue-Team/blob/main/images/Picture4.png)

### **Aggressive scan:**

```
nmap -A -vvv 192.168.1.105
```

A simple aggressive scan reveals a webserver directory structure on tcp port 80, which is a http port, and two potential usernames of employees – ashton and hannah (which will be more relevant for bruteforcing later):

![alt-text](https://github.com/SamGeron/Red-Team-vs-Blue-Team/blob/main/images/Picture5.png)

![alt-text](https://github.com/SamGeron/Red-Team-vs-Blue-Team/blob/main/images/Picture6.png)

### **Navigating the Webserver:**

As this is a webserver, we can investigate further from a browser in the attacker machine:

![alt-text](https://github.com/SamGeron/Red-Team-vs-Blue-Team/blob/main/images/Picture7.png)

In a text document the blog directory we can see a 3rd potential username – Ryan, who would potentially have the highest level access as CEO:

![alt-text](https://github.com/SamGeron/Red-Team-vs-Blue-Team/blob/main/images/Picture8.png)

In the _company folders_ directory, we can see reference to a &quot;_secret\_folder_&quot; in ALL documents within this directory, which is now a target for this Penetration Test.

![alt-text](https://github.com/SamGeron/Red-Team-vs-Blue-Team/blob/main/images/Picture9.png)

The _meet\_our\_team_ folder confirms the three potential users, and each document references the _secret\_folder:_

![alt-text](https://github.com/SamGeron/Red-Team-vs-Blue-Team/blob/main/images/Picture10.png)

As we can see below, we will need Ashton&#39;s password to gain access to the secure hidden folder.

![alt-text](https://github.com/SamGeron/Red-Team-vs-Blue-Team/blob/main/images/Picture11.png)

### **Vulnerability scan:**

```
nmap -A --script=vuln -vvv 192.168.1.105
```

Returning to scanning for further recon.

Aggressive scan with a vulnerability script reveals:

- Webdav vulnerability
- SQL Injection vulnerability across all directories on the webserver
- CVE-2017-15710 – Apache httpd vulnerability

![alt-text](https://github.com/SamGeron/Red-Team-vs-Blue-Team/blob/main/images/Picture12.png)

![alt-text](https://github.com/SamGeron/Red-Team-vs-Blue-Team/blob/main/images/Picture13a.png)

![alt-text](https://github.com/SamGeron/Red-Team-vs-Blue-Team/blob/main/images/Picture14.png)

![alt-text](https://github.com/SamGeron/Red-Team-vs-Blue-Team/blob/main/images/Picture15a.png)

### **Bruteforce:**

Now that we have some usernames and a main target - Ashton, using hydra we can attempt to bruteforce the login for the _secret\_folder_.

Ashton, the CEO, had a common password within our password list. Using the following command, we could get Ashton&#39;s password.

```
hydra -l ashton -P /opt/rockyou.txt -s 80 -f -vV 192.168.1.105 http-get "/company_folders/secret_folder"
```

![alt-text](https://github.com/SamGeron/Red-Team-vs-Blue-Team/blob/main/images/Picture16.png)

### **SSH:**

```
ssh ashton@192.168.1.105
```

Using Ashton's credentials we could gain ssh entry into the server.

![alt-text](https://github.com/SamGeron/Red-Team-vs-Blue-Team/blob/main/images/Picture17.png)

![alt-text](https://github.com/SamGeron/Red-Team-vs-Blue-Team/blob/main/images/Picture18.png)

#### **Flag 1**

In the root home directory we could pickup a flag.

![alt-text](https://github.com/SamGeron/Red-Team-vs-Blue-Team/blob/main/images/Picture19.png)

Using the same credentials, we could access the protected hidden folder.

![alt-text](https://github.com/SamGeron/Red-Team-vs-Blue-Team/blob/main/images/Picture20.png)

### **Password hash:**

Within this folder was a document with instructions to connect to a _corp\_server_. Included in the document are Ryan&#39;s hashed credentials and reference to a webdav directory

![alt-text](https://github.com/SamGeron/Red-Team-vs-Blue-Team/blob/main/images/Picture21.png)

![alt-text](https://github.com/SamGeron/Red-Team-vs-Blue-Team/blob/main/images/Picture22.png)

Th hashed md5 password was instantly cracked using Crackstation, revealing the password _linux4u_

![alt-text](https://github.com/SamGeron/Red-Team-vs-Blue-Team/blob/main/images/Picture23.png)

