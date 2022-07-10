# RED VS BLUE PROJECT #2


### Day 1 Pen Test
## **EXPLOITATION**

### **Discover target IP:**

To discover the target ip:

I first had to run an nmap scan


![nmap scan](https://user-images.githubusercontent.com/96896057/176231038-50e01034-7818-4772-9f70-7a10cb988e0e.png)


Eventually the scan returned which ports were vulnerable

![image](https://user-images.githubusercontent.com/97201701/178120628-cb37157a-623a-4eeb-8c19-0805cd6f13c8.png)


Now that we know port 80 is vulnerable we open our Browser and navigate to the IP 192.168.1.105

![image](https://user-images.githubusercontent.com/97201701/178120675-e116f086-bdee-4c64-959e-32c63bf7376d.png)

Now once we are in the folder it gave us the instructions to connect to the server.


![image](https://user-images.githubusercontent.com/97201701/178120723-303d7295-d6cd-4aa4-9d14-fe2bde3dbab4.png)

Then when you are in the webdav connection the passwd.dav file contains the password to ryans account

![image](https://user-images.githubusercontent.com/97201701/178120752-ff16960b-ea77-4ab8-9bbd-24d87d5d74f3.png)


The hashed md5 password was instantly cracked using Crackstation, revealing the password linux4u

![image](https://user-images.githubusercontent.com/97201701/178120792-bf9aa2d9-a595-4a9a-b029-fa77534687f8.png)

We could then login to webdav using Ryan&#39;s credentials.

![alt-text](https://github.com/SamGeron/Red-Team-vs-Blue-Team/blob/main/images/Picture24.png)

![alt-text](https://github.com/SamGeron/Red-Team-vs-Blue-Team/blob/main/images/Picture25.png)




### Day 2 Blue Team

**Splunk Alert**

This alert created with splunk alerts you anytime a IP address reciever more than 10, 404 status codes.

![project2SSalert](https://user-images.githubusercontent.com/97201701/178120860-1251f5a0-2fc3-4130-a052-08c797cbde09.png)



### Day 3 Mitigation

**Blocking future port scans :**

You can set an alarm to detect the number of requests that come through per second.
The alarm would be triggered whenever a specific IP address sends more than 10 requests per second.
You can whitelist specific IPs to harden systems and mitigate port scans.

**Preventing Brute Force Attacks:**

You can set an alarm that detects the number of login attempts per second.
The alarm would be triggered whenever there are multiple 401 error codes that occur after five login attempts within a second.

You can lock identified users and IP addresses for a specific amount of time to block brute force attacks.


**Detecting the WebDAV connection:**

Monitor all access to webdav and trigger and alarm whenever a file in webdav is opened or read.
The alarm will trigger any time the webdav is accessed.

set a configuration to Whitelist specific machines that have granted access.

**Identifying Reverse Shell Uploads**

Set an alarm to detect whenever a .php file is uploaded or attempted to upload.
The alarm triggers anytime this occurs.



