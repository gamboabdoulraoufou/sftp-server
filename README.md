## sftp-server

In this post, I show how to setup sftp server. You can found more details [here](https://memo-linux.com/mettre-en-place-un-serveur-sftp-sous-debian-8/)

> log as root  

```sh
sudo su
cd
``` 

> install openssh package  

```sh
apt-get install openssh-server
``` 

> create ftp group  

```sh
groupadd ftpaccess
``` 

> configure ssh  

```sh
# permit password login
PasswordAuthentication yes

# edit sshd_config file
nano /etc/ssh/sshd_config
 
# find "Subsystem sftp /usr/lib/openssh/sftp-server" and comment it

# Find X11Forwarding and set value to "no"

# Add the following lines at the end of file
Subsystem sftp internal-sftp
Match group ftpaccess
ChrootDirectory %h
X11Forwarding no
AllowTcpForwarding no
ForceCommand internal-sftp

# save and quit sshd_config
``` 

> restart ssh  

```sh
service ssh restart
``` 

> configure user who which access to sftp 

```sh
useradd -m orange_ic -g ftpaccess -s /usr/sbin/nologin

passwd orange_ic

chown root:root /home/orange_ic

chmod 755 /home/orange_ic 

mkdir /home/orange_ic/www

chown orange_ic:ftpaccess /home/orange_ic/www

sudo /etc/init.d/ssh restart
``` 

> test connexion  

```sh
sftp orange_ic@hostname
``` 

