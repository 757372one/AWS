# create a new virutual enviroment,  
virtualenv venv

# activate the virtual enviroment
source venv/bin/activate

# install django, packages etc
pip install django==2.1.1

# create a django application
django-admin startproject ebdjango

# configure your site for Elastic Beanstalk
# run `pip freeze` and save the output to `requirements.txt`. 
pip freeze > requirements.txt

# install awsebcli
pip install awsebcli

# and check the version installed
eb --version

# Create a folder named .ebextensions. 
mkdir .ebextensions

# In the .ebextensions folder, add a config file `django.config` with the following text:
{{{
option_settings:
  aws:elasticbeanstalk:container:python:
    WSGIPath: [path to wsgi gile]/wsgi.py
}}}
# Deactivate your virtual environment with the deactivate command, reactivate to run application locally or to add packages
deactivate

# check python version and install awscli
pip3 install awscli


# create an environment and deploy your Django app
# Initialize your EB CLI repository with the `eb init` command. 
eb init -p python-3.6 [your django app name]

# it eb command not found run:\
sudo pip3 install awsebcli --force-reinstall --upgrade
which eb
export PATH=/usr/local/bin:$PATH # use path from which eb output
eb --version

# initialize your EB CLI repository with the eb init command. 
eb init -p python-3.6 [your-django-name]

# run eb init again to configure a default key pair 
# so that you can use SSH to connect to the EC2 instance running your application. 







