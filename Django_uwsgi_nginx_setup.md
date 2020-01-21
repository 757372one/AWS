## Set up Django with uWSGI and nginx
Launch a Django Project on Amazon Web Services (AWS) EC2

1. Activate your virtual enviroment 
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
