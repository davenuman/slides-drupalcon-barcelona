background-image: url(images/1_wide.png)
count: false

???
Notes:
- These are presenter notes
- For reference, see: https://github.com/gnab/remark/wiki

---
name: default
layout: true
background-image: url(images/bg.png)
class: middle

---
name: title
class: center

# Docker powered team <br/> and deployment

## David (daven) Numan

http://davenuman.github.io/

???
Self intro. CA 4yrs, Drupal Dev, SysAdm.
Interest in Drupal+DevOps started in DrupalCon Amsterdam

---
name: sandboxes
class: inverse
background-image: url(images/sandbox.jpg)

## Sandbox

<br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/>

???
This session is mostly about developer sandboxes. (And some automated testing.)
- Parts of a sandbox:
- Code
- Database
- Services (LAMP stack)
- Developer Tools (Compass Sass)

---
class: inverse
background-image: url(images/sandbox-outside.jpg)

## Usage
## It can get messy.

???
 -
Never know how sandboxes might be used ...or what mess they might become.

Raise hand if you support your dev team's sandboxes.
Many ways to do it.
LAMP
MAMP
Vagrant
Aquia dev
AWS


---
## Introducing Docker

### Elegant new _cross-platform\*_ development and deployment tool.

\* I'll get back to this in a bit.

???
 +

---
class: center
## Docker Containers

<img src="./images/virtual-machines.png"
 style="width: 300px; float: left; margin-right: 2em;" />

<img src="./images/docker-containers.png" style="width: 300px" />

???
 +

---
background-image: url(images/Container-ship.jpg)

???
 +
 Docker is quite efficient at running many containers in one host machine.

---
## The Docker Components

- Docker Engine - Hosts the containers
- **`docker`**  - "A self-sufficient runtime for containers."
- **`docker-compose`** - "Fast, isolated development environments using Docker."
- **`docker-machine`** - "Create and manage machines running Docker."
- **`boot2docker`** - vagrant machine to run the docker host (engine)
- kitematic - OSX gui


???
 +
- Engine
- docker - command to build, run, stop, list, ... containers
- compose - yaml config files
- machine -

---
class: center
![cottage](images/Container-Cottage.jpg)
### Put anything in a container

???
 +
 Can be complex and large, similar to a virtual machine.
 Much better to be a simple service, even as small as a single process.

---
class: center
![cottage](images/Container-city.jpg)
### Container city

???
 +
 Create many and connect them together.

---
class: full
background-image: url()
### Example docker-compose.yml file

```yaml
db:
  image: mariadb:10.0
  environment:
    - MYSQL_ROOT_PASSWORD=root
    - MYSQL_USER=dbuser
  expose:
    - "3306"
php:
  image: php:5.6-fpm
  command: php-fpm --allow-to-run-as-root
  volumes:
    - .:/var/www/docroot
  links:
    - db
web:
  image: nginx:1.9
  links:
    - php
  volumes_from:
    - php
```

---
background-image: url(images/docker-friends.png)

???
 +
 I took this image from the docker website. I really don't know what's going on in this picture. I recognize the penguin is likely the Linux tux.

 By the way, how many here run Linux on their workstation? Cheers.
 When I started working with Docker was excited. So many possibilities. Eager to get the dev team using this new flexible tool. Put together a few things and was ready to support docker based dev sandboxes.

 I saw the docker site had a full list of OS listed in their installation documentation. Just install docker and a few commands. So easy.

 -
 So we're good to go right? Then I got my Mac co-workers to use our new development sandbox...
 lol

---
background-image: url(images/run-aground.jpg)


???
 -!
This is my perception of Mac user's experience of Docker.
The official boot2docker is very slow for Drupal.
vboxfs

---
class: center
## Vagrant Boot2Docker
### (Thanks Leonid!)

https://github.com/blinkreaction/boot2docker-vagrant

???
 +
Hope for OSX and Windows users.
We use this for all our non-Linux users.

"Those who don't run Linux are doomed to virtualize it."

---
class: center
## Introducing Bowline

![knot](./images/knot.jpg)

https://github.com/davenuman/bowline

???
 +
Project I started with simple bash scripts which attempt to ease Drupal development on Docker.
Inspired by the DrupalCI testbot code, but diverged with a project sandbox focus.

---
## Bowline: command highlights

- check
- build
- import
- backup
- run

Overridden commands:
- drush
- composer
- phpcs

---
background-image: url()
class: full

## Install Bowline into your project

#### bash <(curl -s https://raw.githubusercontent.com/davenuman/bowline/master/lib/bowline/install.sh)

```bash
[...]
 create mode 100644 lib/rigging/drupal-core-dev/drupal-core-dev.hoist
 create mode 100755 lib/rigging/phpunit/bin/phpunit
 create mode 100644 lib/rigging/phpunit/phpunit.hoist
 create mode 100644 sandbox.md
Bowline is now installed into your project repository.

Would you like to build the docker containers now? [Y/n]

# STARTING CONTAINERS
Creating existing_db_1...
Creating existing_php_1...
Creating existing_web_1...

Containers:
db   ~  172.17.0.6
php  ~  172.17.0.7
web  ~  172.17.0.8  http://172.17.0.8/

Would you like to initialize the Drupal settings file? [Y/n]
```

???
This script adds the bowline repository as a remote.
It then checks out the bowline code into your project and commits it. (Push it if you like it.)
At this point bowline is installed.

The script then (optionally) builds and starts the containers.
Once running, outputs IP addresses.
Prompts for initializing the Drupal settings.php file
Optionally runs drush site install for a new project.

---
## So, What does Bowline provide?

- Infrastructure as code. (via docker-compose)
- Tools built into repo.


---
## Drupal 8

```bash
hoist drupal-core-dev
```

???
 clones the drupal repo into docroot. ready to contribute.

---
## Behat
```bash
hoist behat
```

---
## Jenkins

### Example build step: Full test suite

```bash
cd $WORKSPACE/foobar
. bin/activate
build
sleep 2
run fullsuite
```


---
## Some key challenges

- File permissions
- Docker api versions
- File sync


---
# Drupal on Docker: The Alternatives

- Bowline plus vagrant boot2docker
- Drude plus vagrant boot2docker
- Official Drupal docker image
?:
- Kalabox (?)
- Docker-Machine, docker-machine-nfs (?)


---
# Ideas

Avoid the resource strain of a Vagrant VM:  Cloud dev
Local headless device on your network instead of vagrant.

Docker hosting in production... TODO
~ Immutable infrastructure--

???
 We are currently working on a server hosted bowline-docker dev environment. Will require a decent network connection -- cafe might not work well.
 Main challenge is the file syncing.

---
## Final Thoughts

Git has revolutionized code management and allowed for new work-flows

Docker is revolutionizing the was we do infrastructure and deployment

???
 +
GitHub fork and Pull Request a prime example.

Docker perhaps easing Immutable Infrastructure? Perhaps in ways we haven't discovered yet.

---
class: center

# Questions?


---
name: final
background-image: url(images/last_wide.png)

