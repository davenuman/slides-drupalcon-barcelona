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
Self intro

---
name: sandboxes
class: inverse
background-image: url(images/sandbox.jpg)

## Sandbox

<br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/>

???
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
Never know how sandboxes might be used... or what mess they might become.

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
class: center
## Docker Things

- cli
- engine
- docker-compose
- docker-machine
- boot2docker
- kitematic


???
 +

---
background-image: url()
## Example docker-compose.yml file

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
 By the way, how many here run Linux on their workstation? Cheers.

 -
 So we're good to go right? Then I got my Mac co-workers to use our new development sandbox...

---
background-image: url(images/.jpg)

 ... insert failed image of shipping containers

???
 -!
The official boot2docker is very slow for Drupal

---
class: center
## Vagrant Boot2Docker
### (Thanks Leonid!)

https://github.com/blinkreaction/boot2docker-vagrant

???
 +
Hope for OSX and Windows users.
We use this for all our non-Linux users.

---
class: center
## Introducing Bowline

![knot](./images/knot.jpg)

https://github.com/davenuman/bowline

???
 +
My project with simple bash scripts which attempt to ease Drupal development on Docker.

---
## Install Bowline into your project

#### bash <(curl -s https://raw.githubusercontent.com/davenuman/bowline/master/lib/bowline/install.sh)

```bash
...
 create mode 100644 lib/rigging/drupal-core-dev/drupal-core-dev.hoist
 create mode 100755 lib/rigging/phpunit/bin/phpunit
 create mode 100644 lib/rigging/phpunit/phpunit.hoist
 create mode 100644 sandbox.md
Bowline is now installed into your project repository.

Would you like to build the docker containers now? [Y/n]
```

???
This script adds the bowline repository as a remote. It then checks out the bowline code into your project and commits it. Push it if you like it.

---
## What does Bowline provide?

- Infrastructure as code.
- Tools built into repo.


---
## Drupal 8

```bash
hoist drupal-core-dev
```

---
## Behat
```bash
hoist behat
```

---
## Jenkins

Example build step

---
## Key challenges

- File permissions
- Docker api versions


---
## Final Thoughts

Git has disrupted code management work-flows

Docker is disrupting to infrastructure and deployment

---
name: final
background-image: url(images/last_wide.png)

