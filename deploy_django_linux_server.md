## Django deployment on a linux server
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
## 13. Django deployment, you can simply use git clone, or scp
create a requirements.txt file
	```
	source bin/activate
	pip freeze > requirements.txt
	git clonde {git_url}
	scp -r project_folder {user}@server_address:~/
	```
	
14. create a virtual enviroment on the server
	```
	sudo apt-get install python3-pip
	sudo apt-get install python3-venv
	python3 -m venv {project_folder}/venv
	source {project_folder}/venv/bin activate
	pip install -r requirements.txt
	```
	
15. Change settings on django, settings.py, ALLOW_HOSTS,
	```
	ALLOW_HOSTS = ['your ip address']
	```
	
16. add above STATIC URL
	```
	STATIC_ROOT = os.path.join(BASE_DIR, 'static')
	```
	
17. run command to collect static files
	```
	python manage.py collectstatic
	```
	
18. 	Test it
	```
	python manage.py runserver 0.0.0.0:8000
	```
18a. 	open HTTP port in AWS instance menu.(all ports except ssh closed in AWS)
	Go to > EC2 > your instance > last menu item "security groups" > lauch wizard, 
	click on "Inbound" in bottom menu, then "edit", and add HTTP or any port what you want.
	
19. 	If there are no errors the application should be accessible on the public ip on port 8000

20.	Install Apache and WSGI (Web service getway interface)
	```
	sudo apt-get install apache2
	sudo apt-get install libapache2-mod-wsgi-py3
	```
21. 	Configure apache web server
	```
	cd /etc/apache2/sites-available
	sudo cp 000-default your_project.conf
	```
Add:
	```
	Alias /static /home/ubuntu/dash/static
	<Directory /home/ubuntu/dash/static>
		Require all granted
	</Directory>

	Alias /media /home/ubuntu/dash/media
        <Directory /home/ubuntu/dash/media>
                Require all granted
        </Directory>
		
	<Directory /home/ubuntu/dash/djangodash>
		<Files wsgi.py>
			Require all granted
		</Files>
	</Directory>
	WSGIScriptAlias / /home/ubuntu/dash/djangodash/wsgi.py
	WSGIDaemonProcess dash_app python-path=/home/ubuntu/dash python-home=/home/ubuntu/dash/venv
	WSGIProcessGroup dash_app
	```
22.	Enable site via apche, and disable the default
	```
	sudo a2ensite your_project.conf
	sudo a2dissite 000-default.conf
	```
23. 	Manage permissions
	```
	sudo chown :www-data ~/dash/db.sqlite3
	sudo chmod 664 ~/dash/db.sqlite3
	sudo chown :www-data ~/dash
	sudo chown -R :www-data ~/dash/media
	sudo chmod -R 775 ~/dash/media
	```
24.	Create a config file to store sensitive info, place your secret key
	```
	sudo touch /etc/config.json
	```
	```
	{
		"SECRET_KEY": "safasdfasdf",
		"EMAIL_USER": "",
		"EMAIL_PASS": ""
	}
	```
	in settings.py
	```
	import json
	with open('/etc/config.json') as config_file:
		config = json.load(config_file)
	SECERET_KEY = config['SECRET_KEY']
	```
