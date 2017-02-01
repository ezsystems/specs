
# Docker epics

This document describes the EPIC for the Dockerization of the development and production
environment for eZ Platform. This will be a set of scripts, helpers, images and documentation
to support best practice use of container services for development, test and production environments.

## Dockerize the Development environment

As a developer, I would like to initialize my project with a best practice boilerplate.

The tools should:

* Create or provide a skeleton for GitHub
* Allow me to choose:
    * A minimum simple or complete/advanced set of composer requirements
    * Database instance to use (MySQL, MariaDB)
    * If a Memcached/Redis instance should be used
    * If Varnish or disk cache should be used
    * File storage S3 or NFS should be used
    
Besides the architecture, the basics configuration should be handled in this boilerplate.
We miss what we have with the Legacy Wizard, we need something relatively similar.
> it could be a composer script that would ask more questions to setup a clean and clear eZ Platform configuration.

As a developer, I would love to have a built-in way to static analyze my project and cs-fix my code according to 
the community coding standard (or mine)

### Configuration

begin-todo:
-
    just notes for now:
   - wizard etc.
   - ezplatform.yml
   - config.yml
   - cleanup
   - siteaccess name
     
Note about the Symfony ENV
Should we use multiple "Symfony ENV". Only use "dev", "prod", and "test" shoud be enough.

It is possible to have many others "Symfony ENV", it is "complex" to manage.
Idea:

- dev (it is for dev, no cache)
- prod (QA, preprod, prod, full cache enable)

Those are "Application ENV"
In the meantime there are "Host ENV" related to where the project is hosted". Those have also specific 
configuration like password.

In a way, it is important to be able to switch between "Application ENV" on the same "Host ENV". 
Creating multiple "Application env" does not simplify at all because it would mean that you should have 
too many "Application ENV"

- docker_dev
- docker_prod
- qa_dev
- qa_prod
- preprod_dev
- preprod_prod
- prod_dev
- prod_prod

It is better to have

- dev
- prod

And then to have a way to change the configuration of the variables that are different for the "Host ENV".

There is plenty of ways to do that
- an include of a specific file per "Host ENV" (symlink)
- a tool to switch variables inside the files directly. 
- others


end-todo
-


### Architecture

Docker will provide one container by service. The objective is to manage simple and complex 
architecture in the same way.
As a developer I could work on a different project at the same time, then I need to be able to instantiate multiple 
Docker stack on my local machine.
As a Mac OS developer, I want to have good performances, we should then use the NFS sharing concept of "d4m". (link)
As a developer I want a simple solution to test emails that are sent.

Here is a draft of what could be the "simple architecture" as Memcache is always a good idea over standard I/O.

#### Draft basic architecture
![Architecture](exportpuml/architecture.png)

Notes:
* As it is really easy to add new services, we could think about providing the configuration to get
    * a php control control panel (container) with phpinfo (as it is not in eZ Platform anymore), 
      apc, php-opcache etc.
    * [adminer](https://www.adminer.org/) for the DB
    * something else?


### Docker Installation

We should assume that Docker is installed on the developer environment, eZ would not provide
 anything about that installation.
```bash
$ which|whereis docker-machine
$ which|whereis docker-compose
$ which|whereis docker
$ docker ps
```

All those commands should return something.

### Stack Usage and Configurations

Everything would be managed through *docker-compose*.

Because:
- Docker is slightly different on Mac OSX and on Linux
- We need to set up somewhere the "PORT PREFIX" (XX in the diagram) 
- We need to set up somewhere the network name of the Docker stack 

We would need a configuration file, let's call it **Docker.conf** for now.

#### Configuration 

A simple Docker.conf could be:

```
# Prefix of your container's names and of the intern network ([a-zA-Z0-9])
CONTAINER_PREFIX_NAME="mysuperprojectone"
CONTAINER_PREFIX_PORT="16"
```

> Then for instance here
> - I would reach eZ Platform on http://localhost:16080 in "dev" mode
> - I would reach eZ Platform on http://localhost:16081 in "prod" mode
> - My engine container would be named: mysuperprojectone_engine_1
> - My db container would be named: mysuperprojectone_db_1

To manage this architecture we are going to need a docker-compose.yml

Simple incomplete example for the specification/description purposes only:
```yml
version: '2'
services:
    nginx:
        image: xxx
        ports:
            - "${PROJECTPORTPREFIX}080:80"
            - "${PROJECTPORTPREFIX}081:81"

    engine:
        build: ./xxx/
        volumes:
            - "${PROJECTCOMPOSEPATH}:/var/www/html/project:rw"
    db:
        image: xxx
        environment:
            - "MYSQL_ROOT_PASSWORD=ezplatform"
        ports:
            - "${PROJECTPORTPREFIX}306:3306"

    memcache:
        image: xxxx

    mailcatcher:
        image: xxxx
        ports:
            - "${PROJECTPORTPREFIX}180:1080"

    memcachedadmin:
        image: xxx
        environment:
            - "MEMCACHE_HOST=memcache"
        ports:
            - "${PROJECTPORTPREFIX}083:9083"
```

> note that this file would be "generic" then *PROJECTPORTPREFIX* will have to be sent to the Docker commands.

> $PROJECTCOMPOSEPATH would be the path of the project as docker is relative to the docker-compose by default. 

##### Directory structure

A new folder before the eZ Platform application should part of the package, something like that 
(non-exhaustive but interesting for the concept)

```
$ git clone myproject .; ls
Docker.conf.dist
README.md
conf (a bunch of configuration of your project, not only eZ stuff)
data (a bunch of data, dump, or other files not only eZ stuff)
provisioning (the Docker stuff)
scripts (a bunch of scripts usefull for the project)
ezplatform (the eZ Platform application)
```

Not the time to enter into the details but the *provisioning* folder would contain (again nonexhaustive here):
```
cd provisioning; ls
dev
   engine
   nginx
   xxxxx
   docker-compose.yml
prod
   xx
   xx
```

#### Mac OS Specificity 

@todo: explain d4m

#### Usage

Docker has only 3 main commands and 2 that we are going to use: *docker* and *docker-compose*.

Using what was described below and with no usage of Docker.conf, to instantiate our stack the command would be:

```bash
PROJECTCOMPOSEPATH="../../" PROJECTPORTPREFIX=$CONTAINER_PREFIX_PORT docker-compose -p $CONTAINER_PREFIX_NAME -f provisioning/dev/docker-compose.yml $ACTUAL_COMMAND_THAT_YOU_NEED
```
> More details on the complexity: (https://github.com/ezsystems/ezplatform/blob/master/doc/docker-compose/README.md)

But for instance:
```
PROJECTCOMPOSEPATH="../../" PROJECTPORTPREFIX=$CONTAINER_PREFIX_PORT docker-compose -p $CONTAINER_PREFIX_NAME -f provisioning/dev/docker-compose.yml $ACTUAL_COMMAND_THAT_YOU_NEED up
PROJECTCOMPOSEPATH="../../" PROJECTPORTPREFIX=$CONTAINER_PREFIX_PORT docker-compose -p $CONTAINER_PREFIX_NAME -f provisioning/dev/docker-compose.yml $ACTUAL_COMMAND_THAT_YOU_NEED start enging
```

It is too complex we should provide a **wrapper** in charge to simplify those arguments.
This wrapper should not do anything else, it would *ONLY* wrap the docker commands.

#### The Wrapper (let's call it 'dk' for now)

./dk would wrap up the command to let you pass the arguments that matters for what you want to achieve.
As a developer I do not care that I have to pass the docker-compose.yml file as an argument, I do not care that I have
to provide the networkname or the port.
But as a developer I care that I want to restart a container or up another.

Here are what would be the available wrapper help

```
clean: Remove the containers.
create: Create the services and install the project from a DUMP.
start: Start the existing services.
buildup: Build + Create, start the services.
up: Create, start the services.
ps|info|infos|status: List the containers for the project.
logs: Displays log output from services.
stop: Stop the services.
enter: Enter as www-data into a container (default: engine).
root: Enter as root into a container (default: engine).
sfrun: Run a Symfony Command as www-data into a container (engine).
scrun: Run a Script Command as www-data into a container (engine).
release-dev: Run the release script into the engine (env=dev). (alias of ./dk sc release.bash dev)
release-prod: Run the release script into the engine (env=prod). (alias of ./dk sc release.bash prod)
codechecker: Run the codechecker script into the enin
create-dump: Dumping the database & storage to push a new database model into the db.
```

Again, it is important to avoid the "wrapper" to become obsolete, it MUST be devoid of intelligence.

I could be only 3 main methods

```
docker_compose()
{
    PROJECTPORTPREFIX=$CONTAINER_PREFIX_PORT PROJECTCOMPOSEPATH=$DOCKER_HOST_SOURCES_PROJECT docker-compose $@
}

docker_exec_root()
{
    docker exec -it -u root $@
}

docker_exec()
{
    docker exec -it -u www-data $@
}
```

And then, just obvious wrapping, for instance:

```bash
    'start')
        echoTitle "Start the existing containers"
        docker_compose $DOCKER_COMPOSE_ARGS start
    ;;
    'logs')
        echoTitle "Displays the logs of all the services"
        docker_compose $DOCKER_COMPOSE_ARGS logs -f --tail 100
    ;;
    
    # a more complex but still a wrapper
    # allow you to enter into all the container even selecting the bash: ex: ../dk enter networkname_nginx_1 /bin/ash
    'enter')
        CONTAINER_DEST=$ENGINE_CONTAINER
        CONTAINER_COMMAND="/bin/bash"
        if [ "$2" != '' ]; then
            CONTAINER_DEST="$2"
        fi
        if [ "$3" != '' ]; then
            CONTAINER_COMMAND="$3"
        fi
        echoTitle "Entering into $CONTAINER_DEST as www-data"
        docker_exec $CONTAINER_DEST $CONTAINER_COMMAND
    ;;
    'sfrun')
        echoTitle "Run Symfony command in ENGINE_CONTAINER"
        docker_exec $ENGINE_CONTAINER bash $CONTAINER_PROJECT_MOUNT_DEST/scripts/sfrun.bash $2 $3 $4 $5 $6 $7 $8 $9
    ;;
    'release-dev')
        echoTitle "Run release.bash in the project env=DEV"
        docker_exec $ENGINE_CONTAINER bash $CONTAINER_PROJECT_MOUNT_DEST/scripts/release.bash dev
    ;;
```

> IMO this simplicity can not become "obsolete"


### Scripts 

Because we are all doing the same thing, scripts can be provided too.
For instance:

```
# scripts/release.bash
 if [ "$1" == "prod" ]; then
        COMP_ARGS=" --optimize-autoloader --no-dev"
    fi
    SYMFONY_ENV=$1 $PHP composer.phar install --no-interaction $COMP_ARGS
    $PHP bin/console doctrine:schema:update --dump-sql --force --env=$1
    if [ $1 == "prod" ];then
        $PHP bin/console assetic:dump --env=$1
    fi
```


#### Database and Storage

As a developer, I need to create an environment with content that is the same as in production.
I need tools that simply enable me to fetch a dump of SQL + binary files from production and have.

There are two parts here

1/ dumping the current database to provide a full dump to the coworkers

```bash
./dk create-dump
```
2/ importing the full dump to the local database 

```bash
./dk resetdb-dump
```

3/ getting the production database and content
```bash
./dk resetdb-fromremotedump
```
> here a lot of network and performance "issues" are at stack, a valid process would be a script that could get a 
  daily dump stored somewhere. 
> This would take a copy of the SQL data and re-initialize the development database as well as the binary files.


#### Coding standard script included !
```bash
if [ -z "$1" ]; then
    SRC="src/"
else
    SRC="$1"
fi
echo "******** Code Fixer ******** \n"
$PHP ./vendor/bin/php-cs-fixer fix --config=../conf/.php_cs.php
echo "******** Mess Detector ********"
$PHP ./vendor/bin/phpmd $SRC text ../conf/md_ruleset.xml
echo "******** Copy/Paste Detector ********"
$PHP ./vendor/bin/phpcpd $SRC
echo "******** CodeSniffer ********"
$PHP ./vendor/bin/phpcs -n $SRC --standard=../conf/cs_ruleset.xml

```

Would be so great, nothing is specific to the project here. 



## Test environment

As a developer, I need to create test environments for different purposes including:
* Internal QA demos for product owners
* Client preview test
* Client acceptance testing
* System demonstration, typically for eZ sales or marketing demos

When initiating a test I should select a desired branch of the project and what configuration
set to use. You typically want to have a simplified setup when testing. It might be an optimized
single instance vs a full built out environment.

The process with Docker would be to use the "development mode" docker architecture for that purpose.

The process would be simple
```
$ git clone myproject .
$ cp Docker.conf.dist Docker.conf
# set the CONTAINER_PREFIX_PORT to empty (to get the usual ports)
$ git checkout desired-branch
$ ./dk create
$ ./dk release-prod
```
Done!

The test environment should then be created in:
* Local Docker environment
* AWS or other cloud hosting environments


begin-todo:
-
** How would this work with Platform.sh? Just commit a live branch? **

end-todo
-



begin-todo:
-
## Production environment

As a developer, I would like to easily create a new production environment in a cloud hosting setup.

It should be plugin based and support different deployment strategies including:
* Amazon AWS
* Platform.sh (how would this work in this context?)
* Google

The production environment should typically first be created with a Docker command like:

~~~
 docker-machine create --driver amazonec2 --amazonec2-region eu-central-1 --amazonec2-instance-type t2.micro aws-sandbox-sportsjentene1
~~~
end-todo
-

## Upgrading

As a developer I would like to be able to upgrade my eZ Platform environment.

A lot of possibilities here.

- (https://github.com/kaliop-uk/ezmigrationbundle): a really good candidate

- also, another approach less "complete", but simpler

Actually, most of the time, only the Content Type are changing because when you want to create new content 
you will probably create a command.

A script hat synchronizes the Content Types and "something" would simplify. 
Ex a Google Doc document
![Content Type Google Doc document example](contenttypesgoogledocexample1.png)

With this approach, we just have to provide the XLSX file in git repo, or just running the command to fetch Google Doc.
 
> It is really convenient for the Content Type, and does not require a lot of configuration

BUT it does not manage the rollback, also we could ask ourselves if we are to rollback at this point, 
would not be simpler to reimport the dump just made before to start a deploy?
> just a thought

For the content, we could use kind of the same approach. Doing a script that 
manages create/update (via remoteid) contents/tree.
> It could be enough in plenty of projects






