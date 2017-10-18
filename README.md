# LegalServices Alpha Provider Management Platform [Last Update: Aug/2017] 

Developed with python 3.x, there might be some 3.x specific dependencies that can be flipped out for their 2.x counterparts.

For local development, you can use basic django operations, or the platform is also packaged with a Dockerfile deployment option.


# Getting Started Locally

## System

* You will need Python installed as well as Postgres

* Ensure you have pip installed. (comes with python >2.79, python3 >3.4)

* A VirtualEnvironment https://virtualenv.pypa.io/en/stable/ is recommended for managing dependencies.

Installing:

`[sudo] pip install virtualenv`

* Initiate a virtualenv:

Windows:

`virtualenv [environmentname]`

`[environmentname]\Scripts\activate`

Unix:

`virtualenv [environmentname]`

`source bin/activate`

* Ensure you have pip installed. (comes with python >2.79, python3 >3.4)
* navigate to the main directory and install dependencies into your virtualenv:

`pip install -r requirements.txt`

## Postgres
* The platform in configured to write to a database called 'openref' right now with the following config:

`'NAME': 'openref',
'USER': 'postgres',
'PASSWORD': 'postgres',
'HOST': '127.0.0.1',
'PORT': '5432',`

* if you'd like to use different settings they can be edited in the 'openref3\settings.py' file under the DATABASES section. You can see that there are some other strategies commented out for using things like environment variables to set database config programatically. 

## Django

So for a one-time django setup you'll need to run the following:

`python manage.py makemigrations`

`python manage.py migrate`

`python manage.py loaddata ./fixtures/counties.json`

`python manage.py loaddata ./fixtures/languages.json`

`python manage.py collectstatic`

* at this time you will want to create a django admin login:

`python manage.py createsuperuser`

and follow the prompts

* Once the user is created, you may start the platform using:

`python manage.py runserver`

**The platform should be running at http://127.0.0.1:8000**

* Visit the administrative console and login with your superuser login:
http://127.0.0.1:8000/admin/


# Using the Platform

## Administrative Console:

### Users:
* This page allows you to add new users who will be able to manage their Provider records within the system.

### Tokens:
* Whenever a user is created, they are assigned a key for the API that allows them to programatically read all provider records and push updates to their own records
* This page can be used to expire, rotate, or create new keys for specific users

### Providers:
* This page allows the administrator to create new provider records and assign their ownership to a user using the 'owner' field. Once a provider has been created, they should be managed through the update process as described below, but this page can be used for direct editing of the record as a workaround to any issues.

### Provider Update:
* This page contains all updates submitted by users.
* Admins may review unprocessed updates here in a side by side comparison and accept the changes to live or reject them

For all objects, audit trails can be accessed through the 'History' links at the top of the object's page. Previous versions of objects can be recovered using the 'Recover' links at the top of the object's page.


## User pages:

### Provider Update
* When a user who is assigned as an owner of a record logs in, they are presented with a page allowing them to update their record.

### My Updates
* They can also review their previous updates to check on progress or requests for follow-up.

### API Token
* Allows a user to view API authentication and instructions


# Docker Setup

* The enclosed Dockerfile sets up the required system elements
* There are also lines to assign Environmental Variables to set various configuration

* The openref3\settings.py script will then need to be edited to use these variables for database & email settings

* The Dockerfile sets up all required dependencies and runs a start.sh script to initialize django (you may add additional set-up here if needed)
* The platform is configured to run using nginx, uwsig, and supervisor to ensure a performant deployment
