 
# Our Fabulous <img src="https://upload.wikimedia.org/wikipedia/commons/thumb/4/4e/Docker_%28container_engine%29_logo.svg/320px-Docker_%28container_engine%29_logo.svg.png" alt="Docker" width="210px"/> Images

# creiwork

Image configured to work out of the box with [Creiwork](https://github.com/creios/creiwork).

### creios/creiwork:v4-dev
An Image for development and testing in CI context.

Baseimage: php:7.2-fpm-stretch

Additional Containment:
 - Xdebug
 - PHP Extensions: pdo_mysql, gmp, intl
 
# deployment
Image used for deploying projects to remote servers.

### creios/deploy
Baseimage: debian:stretch

Additional Containment:
- openssh-client
- rsync
- lftp
- wget
- curl
- deployer

_Usage of deployer:_

Help:
deployer deploy -h

Example:
deployer deploy --host 127.0.0.1 --port 22 --user root --exclude-from excludes.txt . /var/www
