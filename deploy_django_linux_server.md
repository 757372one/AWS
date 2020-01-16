# Django deployment on a linux server
Launch a Django Project on Amazon Web Services (AWS) EC2

1. Lunch an instance to ssh into it, Im am going to use ubuntu, if you are using a different linux, map the differences
	```
	sudo apt-get update && apt-get upgrade
	```
  
2. Set the host name of the new machine
	```
	hostnamectl set-hostname django-server
  	hostname
	```
3. Set the host name in the host file
	```
	nano /etc/hosts
	``` 
3.a underneath the 127.0.0.1 localhost, enter the IP address of your server and you host name
	```
	127.0.0.1 localhost
	3.134.89.255 django-server
	```
	
4. Set up a limited user, best not to use root.
	```
	adduser {your_username}
	``` 
	
5. Enable user to run sudo commands, add user to sudo group
	```
	adduser {your_username} sudo
	``` 
	
6. log in as the user
	```
	ssh {user}@youserveraddress.com
	```
	
7. create a .ssh folder
	```
	mkdir -p ~/.ssh
	```
	
8. on your local machine, generate a two key files
	```
	ssh-keygen -b 4096
	```
	
9.  upload public key
	```
	scp ~/.ssh/{file}.pub {username}@server_ip:~/.ssh/authorized_keys
	```
	
10. update permissions
	```
	sudo chmod 700 ~/.ssh/
	sudo chmod 600 ~/.ssh/*
	```
	
11. block root login, change: PermitRootLogin No
	```
	sudo nano /etc/ssh/sshd_config
	sudo systemctl restart sshd
	```
	
12. Install firewall ufw, and do a basic config
	``` 
	sudo apt-get install ufw
	sudo ufw default allow outging
	sudo ufw default deny incoming
	sudo ufw allow ssh
	sudo ufw allow 8000
	```
	this will allow to view our application on port 8000, after everything is production ready, allow http
	```
	sudo ufw enable
	sudo ufw status
	```
# 13. Django deployment, you can simply use git clone
	create a requirements.txt file
	```
	source bin/activate
	pip freeze > requirements.txt
	```
	
