
# Docker epics

This document describes the EPIC for the Dockerization of the development and production
environment for eZ Platform. This will be a set of scripts, helpers, images and documentation
to support best practice use of container services for development, test and production environments.

## Docerize the Development environment

As a developer I would like to initialize my project with a best practice boiler plate.

The tools should:

* Create or provide a skeleton for GitHub
* Allow me to choose:
    * A minimum simple or complete/advanced set of composer requirements
    * Database instance to use (MySQL, MariaDB, NoSQL)
    * If a memcache instance should be used
    * If Varnish or disk cache should be used
    * File storage S3 or NFS

### Instantiate a new data dump

As a developer I need to create an environment with content that is the same as in production.
I need tools that simply enable me to fetch a dump of SQL + binary files from production and have.

The command should be as simple as:

~~~
./dk dump-prod
~~~

This would take a copy of the SQL data and re-initialize the development database as well as the
binary files.

## Test environment

As a developer I need to create test environments for different purposes including:
* Internal QA demos for product owners
* Client preview test
* Client acceptance testing
* System demonstration, typically for eZ sales or marketing demos

When initiating a test I should select a desired branch of the project and what configuration
set to use. You typically want to have a simplified setup when testing. It might be an optimized
single instance vs a full built out environment.

** How would this work with Platform.sh? Just commit a live branch? **

The test environment should then be created in:
* Local Docker environment
* AWS or other cloud hosting environments

## Production environment

As a developer I would like to easily create a new production environment in a cloud hosting setup.

It should be plugin based and support different deployment strategies including:
* Amazon AWS
* Platform.sh (how would this work in this context?)
* Google

The production environment should typically first be created with a Docker command like:

~~~
 docker-machine create --driver amazonec2 --amazonec2-region eu-central-1 --amazonec2-instance-type t2.micro aws-sandbox-sportsjentene1
~~~


## Upgrading

As a developer I would like to be able to upgrade my eZ Platform environment from
