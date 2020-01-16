# Django On a linux server
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
  underneath the 127.0.0.1 localhost, enter the IP address of your server and you host name
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
