[![Build Status](https://travis-ci.org/edonosotti/ci-cd-tutorial-sample-app.svg?branch=master)](https://travis-ci.org/edonosotti/ci-cd-tutorial-sample-app)
[![codebeat badge](https://codebeat.co/badges/0e006c74-a2f9-4f34-9cf4-2378fb7d995a)](https://codebeat.co/projects/github-com-edonosotti-ci-cd-tutorial-sample-app-master)
[![Maintainability](https://api.codeclimate.com/v1/badges/e14a2647843de209fd5e/maintainability)](https://codeclimate.com/github/edonosotti/ci-cd-tutorial-sample-app/maintainability)

# CD/CI Tutorial Sample Application

## Description

This sample Python REST API application was written for a tutorial on implementing Continuous Integration and Delivery pipelines.

It demonstrates how to:

 * Write a basic REST API using the [Flask](http://flask.pocoo.org) microframework
 * Basic database operations and migrations using the Flask wrappers around [Alembic](https://bitbucket.org/zzzeek/alembic) and [SQLAlchemy](https://www.sqlalchemy.org)
 * Write automated unit tests with [unittest](https://docs.python.org/2/library/unittest.html)

Also:

 * How to use [GitHub Actions](https://github.com/features/actions)

Related article: https://medium.com/rockedscience/docker-ci-cd-pipeline-with-github-actions-6d4cd1731030

## Requirements

 * `Python 3.8`
 * `Pip`
 * `virtualenv`, or `conda`, or `miniconda`

The `psycopg2` package does require `libpq-dev` and `gcc`.
To install them (with `apt`), run:

```sh
$ sudo apt-get install libpq-dev gcc
```

## Installation

With `virtualenv`:

```sh
$ python -m venv venv
$ source venv/bin/activate
$ pip install -r requirements.txt
```

With `conda` or `miniconda`:

```sh
$ conda env create -n ci-cd-tutorial-sample-app python=3.8
$ source activate ci-cd-tutorial-sample-app
$ pip install -r requirements.txt
```

Optional: set the `DATABASE_URL` environment variable to a valid SQLAlchemy connection string. Otherwise, a local SQLite database will be created.

Initalize and seed the database:

```sh
$ flask db upgrade
$ python seed.py
```

## Running tests

Run:

```sh
$ python -m unittest discover
```

## Running the application

### Running locally

Run the application using the built-in Flask server:

```sh
$ flask run
```

### Running on a production server

Run the application using `gunicorn`:

```sh
$ pip install -r requirements-server.txt
$ gunicorn app:app
```

To set the listening address and port, run:

```
$ gunicorn app:app -b 0.0.0.0:8000
```

## Running on Docker

Run:

```
$ docker build -t ci-cd-tutorial-sample-app:latest .
$ docker run -d -p 8000:8000 ci-cd-tutorial-sample-app:latest
```

## Deploying to Heroku

Run:

```sh
$ heroku create
$ git push heroku master
$ heroku run flask db upgrade
$ heroku run python seed.py
$ heroku open
```

or use the automated deploy feature:

[![Deploy](https://www.herokucdn.com/deploy/button.svg)](https://heroku.com/deploy)

For more information about using Python on Heroku, see these Dev Center articles:

 - [Python on Heroku](https://devcenter.heroku.com/categories/python)
 

## Info:
From Jenkins running on local machine, this repo provides pipeline to manage dev, staging, production environments

## Release notes:
- new feature in new branch -> staging -> main (production)

## Start containers 
```
docker compose up -d
```
1. This will start flask and jenkins ccontainers. 
2. From jenkins-cicd container try to run ```docker ps```. If permission denied chown to 1000:1000 to the file /var/run/docker.sock on the container executing as a root user.
3. Go to ```localhost:8000``` and see if you receive ```{"status":"ok"}```
4. Go '''localhost:8080''' and set up jenkins with multibranch pipeline - https://www.youtube.com/watch?v=aDmeeVDrp0o

## Seting up github webhook with jenkins running locally and ngrok 
https://www.youtube.com/watch?v=yMNJeWeE0qI
- TIP: adjust ngrok https tunnel address in github app created if your ngrok terminal was closed

## Pepare AWS ECR
1. authenticate yourself with ```aws configure```
2. login to ECR
```aws ecr get-login-password --region REGIONHERE!!!! | docker login --username AWS --password-stdin AWS_ACCOUNTID_HERE!!!!.dkr.ecr.REGIONHERE!!!.amazonaws.com```
3. create registries for storing previously created images and push images to aws ecr

## Build ECS Cluster
``` 
aws cloudformation create-stack \
--stack-name cicd-example-stack-name \
--template-body file://./aws-cf-template.yaml \ 
--capabilities CAPABILITY_NAMED_IAM \
--region eu-central-1 \
--parameters ParameterKey=SubnetID,ParameterValue=your-default-subnet-id ParameterKey=ImageName,ParameterValue=your-image-uri
```