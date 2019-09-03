# Linux Server Configuration
**Udacity Fullstack Nanodegree**

Linux Server Configuration Challenge from the Udacity Fullstack Nanodegree.

## IP Address and SSH port 

 | Ip Address  | SSH port |
 |------------ |----------|
 |3.123.69.175 | 2200     |

## Webapplication Url 


# Installed software

a. Packages updated and upgraded

 ```
 $ sudo apt-get update 
 $ sudo apt-get upgrade
 ```
  

  b. Additionally installed packages 
  

 Finger for user inspection
 ```
 $ sudo apt-get install finger
 ```
 Git
 ```
 $ sudo apt-get install git
 ```
 
## Configuration changes 

-------------------------------------
 1. Changed ssh port from 22 to 2200
-------------------------------------

- Added a custom port 2200 to the Lightsail management console 
- Edited the port 22 to 2200 in sshd_config (folder: /etc/ssh/)
- Added port 2200 to the ufw firewall and enabled the ufw firewall 
- Restarted sshd with: `sudo service sshd restart`
- Deleted port ssh port 22 from Lightsail management console
   
------------------------------------   
2. Configured firewall
------------------------------------

- Default incoming requests: Denied 
    ```
    $ sudo ufw default deny incoming
    ```
- Default outgoing requests: Allowed
    ```
    $ sudo ufw default allow outgoing
    ```
- Allow port 2200 
    ```
    $ sudo ufw allow 2200
    ```
- Allow port 80 
    ```
    $ sudo ufw allow 80
    ```
- Allow port 123
    ```
    $ sudo ufw allow 123 
    ```
- Enable firewall
    ```
    $ sudo ufw enable
    ```
- Request status
    ```
    $ sudo ufw status
    ```
--------------------------------  
3. Added another user account
-------------------------------

- Added user grader:  
    ```
    $ sudo adduser grader
    ```  
- Gave sudo rights to grader:  

Copied the following permissions file from the user ubuntu  
```
$ sudo cp /etc/sudoers.d/ubuntu /etc/sudoers.d/grader
```  
Changed name ubuntu in file to grader with nano  
```
$ sudo nano /etc/sudoers.d/grader
```  
- Enabled keybased authentication:
   > Created a key pair locally with `ssh-keygen`  
    Opened the .pub file and copied public key  
    Created /.ssh directory on server  
    Created a authorized_key file in /.ssh directory   
    Opened authorized_key file with nano and pasted in public key     
    Set permissions on /.ssh directory to 700 with `chmod 700 .ssh`  
    Set permissions on .ssh/authorized_keys to 644 with chmod `chmod 644 .ssh`  

Login with: 
```
$ ssh grader@3.123.69.175 -p 2200 -i ~/.ssh/linuxxCourse
```

---------------------------------
4. Denied remote login as root
---------------------------------

Changed etc/ssh/sshd_config: 

```
PermitRootLogin no
```

Restarted sshd service:

```
$ sudo service sshd restart
```

### Third-pary resources used to complete project

[1] [Putty](https://www.putty.org/) - SSH and telnet client






















