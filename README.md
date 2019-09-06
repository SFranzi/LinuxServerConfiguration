# Linux Server Configuration
**Udacity Fullstack Nanodegree**

To access the CatalogApp type **http://3.123.69.175/** in your browser.

## IP Address and SSH port 

 | IP Address  | SSH port |
 |------------ |----------|
 |3.123.69.175 | 2200     |

## Installed software

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
 
 Apache 
 ```
 sudo apt-get install apache2
 ```
 
 Mod_wsgi module 
 ```
 $ sudo apt-get install libapache2-mod-wsgi-py3
 # enable wsgi
 $ sudo a2enmod wsgi
 ```
 
 
 Python and flask packages
 ```
 $ sudo apt-get install python3 ipython3 python3-flask curl python3-venv python3-dev
 ```
 
 SQLite3
 ```
 $ sudo apt-get install sqlite3
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
1. Created a key pair locally with `ssh-keygen`  
2. Opened the .pub file and copied public key  
3. Created /.ssh directory on server  
4. Created a authorized_key file in /.ssh directory   
5. Opened authorized_key file with nano and pasted in public key     
6. Set permissions on /.ssh directory to 700 with `chmod 700 .ssh`  
7. Set permissions on .ssh/authorized_keys to 644 with chmod `chmod 644 .ssh`  

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

-------------------------------------------------------------
5. Apache Server Configuration and Web Application deployment 
-------------------------------------------------------------

5.1 Created a directory in `/var/www/` named catalog
5.2. Cloned Git repository CatalogApp into `/var/www/catalog` and named it catalog. 
So the new directory has the following structure `/var/www/catalog/catalog`
5.3. Changed user and group to grader of catalog directory 
```
$ sudo chown -R $USER:$USER /var/www/catalog
```
5.4 Set permissions of /var/www/catalog/catalog directory to 666
```
$ sudo chmod 666 catalog
```
5.5. Created virtual environment with 
```
$ python3 -m venv venv 
```
5.6. Activated virtual environement 
```
$ source venv/bin/activate
```
5.7 Installed package 'wheel'
```
$ pip install wheel`
```

5.8. Installed all required packages
```
$ pip install -r requirements.txt
```

5.9. Created a catalog.wsgi file in /var/www directory

```

#!/usr/bin/python
import sys
import logging
logging.basicConfig(stream=sys.stderr)
sys.path.insert(0, "/var/www/catalog/catalog")

from catalog import app as application

application.secret_key = 'your_secret_key'

```

5.9. Created a catalog.conf file in the /etc/apache2/sites-enabled directory

```
## /etc/apache2/sites-available/catalog.conf

<VirtualHost *:80>
    ServerName 3.123.69.175
   # ServerAlias ip-172-26-1-146
   # ServerAdmin admin@3.123.69.175
    #The WSGIDaemoProcess needs to contain the path to the application and the path to the site packages when using virtual env
    WSGIDaemonProcess catalog python-path=/var/www/catalog/catalog/venv/lib/python3.6/site-packages
    # The WSGIProcessGroup tells the virtual host to use the WSGI daemon created before
    WSGIProcessGroup catalog
    WSGIScriptAlias / /var/www/catalog/catalog.wsgi
    <Directory /var/www/catalog/catalog/>
       # Order allow,deny
       # Allow from all
        Options FollowSymLinks
        AllowOverride None
        Require all granted
    </Directory>
    ErrorLog ${APACHE_LOG_DIR}/error.log
    LogLevel warn
    CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
```

5.10. Enabled the catalog.conf file and restarted Apache

```
$ sudo a2ensite CatalogAppp.conf
$ sudo service apache2 restart
```
--------------------------------------------
6. Settig up database server
--------------------------------------------

Changed file permissions of app.db to 666

```
$ sudo chmod 666 app.db
```

----------------------------
7. Further helpful commands 
----------------------------

- Restart Apache 

```
$ sudo service apache2 restart
```

- Check Apache Status 

```
$ sudo systemctl status apache2
```

- Apache Error Log 

```
$ nano /var/log/apache2/error.log
```

- Check if package is installed

```
$ dpkg -s packagename | grep Status
```

### Third-pary resources used to complete project

[1] [Putty](https://www.putty.org/) - SSH and telnet client
[2] [The Flask Mega-Tutorial Part XVII: Deployment on Linux](https://blog.miguelgrinberg.com/post/the-flask-mega-tutorial-part-xvii-deployment-on-linux)
