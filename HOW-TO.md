## Github SSH
# https://stackoverflow.com/questions/2643502/how-to-solve-permission-denied-publickey-error-when-using-git#:~:text=This%20error%20can%20happen%20when,write%20access%20to%20that%20repo.&text=An%20equivalent%20case%20is%20when,write%20access%20with%20SSH%20URL.

# Make a github repo, enble gitignore, readme and licence
# 
# These are the steps I followed in windows 10
# Open Git Bash.

# Generate Public Key:

# ssh-keygen -t rsa -b 4096 -C "youremailaddress@xyz.com"
# Copy generated key to the clipboard (works like CTRL+C)

# clip < ~/.ssh/id_rsa.pub
# Browser, go to Github => Profile=> Settings => SSH and GPG keys => # Add Key
## Provide the key name and paste clipboard (CTRL+V).
## Finally, test your connection (Git bash)

# ssh -T git@github.com


## Make a github repo
# Make a Docker file *d
# Make a requirements.txt file *r
# Make a /app folder
# COMMAND: docker build .
# Make a docker-compose.yml *dc
# COMMAND: docker-compose build
# COMMAND: docker-compose run app sh -c "django-admin.py startproject app ."

## Travis CI Setup
# Activate all repos on http://travis-ci.org 
# Create ./.travis.yml *tci

## Flake8
# Create ./app/.flake8 *f8

// **** //

# *f8:
[flake8]
exclude =
  migrations,
  __pychache__,
  manage.py,
  settings.py

# *tci:
language: python
python:
  - "3.6"

services:
  - docker

before_script: pip install docker-compose

script:
  - docker-compose run app sh -c "python manage.py test && flake8"

# *r:
Django>=2.1.3,<2.2.0
djangorestframework>=3.9.0,<3.10.0
psycopg2>=2.7.5,<2.8.0
flake8>=3.6.0,<3.7.0

# *d:
FROM python:3.7-alpine
MAINTAINER Stephanie Hallberg

ENV PYTHONUNBUFFERED 1

COPY ./requirements.txt /requirements.txt
RUN pip install -r /requirements.txt

RUN mkdir /app
WORKDIR /app
COPY ./app /app

RUN adduser -D user
USER user

# *dc: 
version: "3"

services:
  app:
    build:
      context: .
    ports:
      - "8000:8000"
    volumes:
      - ./app:/app
    command: >
      sh -c "python manage.py runserver 0.0.0.0:8000"