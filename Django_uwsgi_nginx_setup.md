# Set up Django with uWSGI and nginx
1. Activate your virtual enviroment 
	```
	source venv/bin/activate
	```
2. Install uwsgi
	```
	pip install uwsgi
	```
3. Test your django project
  ```
  python manage.py runserver 0.0.0.0:8000
  ```
4. if it works, run using uwsgi
  ```
  uwsgi --http :8000 --module {django app}.wsgi
  ```

## Basic nginx
1. Install nginx
  ```
  sudo apt-get install nginx
  sudo /etc/init.d/nginx start # start nginx
  ```
2. Configure nginx, grab a uwsgi_params file from: https://github.com/nginx/nginx/blob/master/conf/uwsgi_params
   and copy it into the project directory
3. create a new file called {my project}_nginx.conf, and tell nginx to serve up media and static files from the fielsystem
4. Symlink to this file from /etc/nginx/sites-enabled so nginx can see it:
	```
	sudo ln -s ~/path/to/your/mysite/mysite_nginx.conf /etc/nginx/sites-enabled/
	```
