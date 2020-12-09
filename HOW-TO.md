## Make a github repo
# Make a Docker file *d
# Make a requirements.txt file
# Make a /app folder
# COMMAND: docker build .

# Make a docker-compose.yml *dc
# COMMAND: docker-compose build

# COMMAND: docker-compose run app sh -c "django-admin.py startproject app ."

// **** //

*d:
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

*dc: 
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