# Docker-Image with Nginx + PHP-FPM optimized & configured for Roundcube

[![Build Status](https://travis-ci.org/ras-martin/nginx-phpfpm-4-roundcube.svg?branch=master)](https://travis-ci.org/ras-martin/nginx-phpfpm-4-roundcube)

* [Docker Hub](https://hub.docker.com/r/rasmartin/nginx-phpfpm-4-roundcube)
* [GitHub](https://github.com/ras-martin/nginx-phpfpm-4-roundcube)

This is a custom Docker-Image based on [Nginx](https://www.nginx.com/) and [PHP-FPM](https://www.php.net/) to host your [Roundcube](https://roundcube.net/)-Instance.

Roundcube itself is not included in this image!

## Distributions

At the moment the following distributions are support:
* [Alpine Linux](https://alpinelinux.org/) amd64

The included `build.sh` can be used to build the required Docker-Image.

## FAQ's

### Where do I have to mount the docroot to?

The Roundcube docroot must be mounted to `/application/src`.

### How to overwrite PHP settings?

If it is required to overwrite PHP settings, create a custom configuration file ([php.ini](https://www.php.net/manual/en/ini.list.php) style) which contains the required configurations and mount it with `docker run` to `/etc/php7/conf.d/zzz-override.ini`.

