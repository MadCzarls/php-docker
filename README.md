# Description

Template project for PHP with Docker with basic composer setup. By default, includes xdebug extension and PHP_CodeSniffer for easy development. Includes instruction for setting it in PhpStorm. 

- https://www.docker.com/
- https://docs.docker.com/compose/
- https://www.php.net/
- https://xdebug.org/
- https://github.com/squizlabs/PHP_CodeSniffer
- https://www.jetbrains.com/phpstorm/

Clone and tweak it to your needs. Tested on Linux (Ubuntu):

1. Docker version 20.10.8, build 3967b7d
1. docker-compose version 1.29.2, build 5becea4c

# Usage

Clone repository, `cd` inside, take a look at `docker-compose.yml` file, change it according if needed. Afterwards run:

<pre>
docker-compose build
docker-compose up
</pre>

After that log into container with `docker exec -it php_docker bash`, where `php.docker` is the default container name from `docker-compose.yml` file.
To execute script type:
<pre>
php src/run.php
</pre>

# Overview

All PHP extensions can be installed via `docker-php-ext-install` command in docker/php/Dockerfile. Examples and usage:
`https://gist.github.com/giansalex/2776a4206666d940d014792ab4700d80`.

## PhpStorm configuration
_Based on PhpStorm version: 2021.1.4_

Open directory including cloned repository as directory in PhpStorm.

### Interpreter

1. `Settings` -> `PHP` -> `Servers`: create server with name `docker` (the same as in ENV variable `PHP_IDE_CONFIG`), host `localhost`, port `8050` (default from `docker-compose.yml` file).
1. Tick `Use path mappings` -> set `File/Directory` <-> `Absolute path on the server` as: `</absolute/path>/php_docker_cli/app` <-> `/var/www/app` (default from `docker-compose.yml`).
1. `Settings` -> `PHP`: three dots next to the field `CLI interpreter` -> `+` button -> `From Docker, Vagrant(...)` -> from `docker-compose`, from service `php`, server `Docker`, configuration files `./docker-compose`. After creating in `Lifecycle` section ensure to pick `Always start a new container (...)`, in `General` refresh interpreter data.

### xdebug

1. `Settings` -> `PHP` -> `Debug`  -> `Xdebug` -> `Debug port`: `9003` (set by default) and check `Can accept external connections`.
1. Click `Start Listening for PHP Debug connections` -> `+` button, set breakpoints and refresh website.

### PHPCS

1. Copy `app/phpcs.xml.dist` and name it `phpcs.xml`. Tweak it to your needs.
1. `Settings` -> `PHP` -> `Quality Tools` -> `PHP_CodeSniffer` -> `Configuration`: three dots, add interpreter with `+` and validate paths. By default, there should be correct path mappings and paths already set to `/var/www/app/vendor/bin/phpcs` and `/var/www/app/vendor/bin/phpcbf`.
1. `Settings` -> `Editor` -> `Inspections` -> `PHP` -> `Quality tools` -> tick `PHP_CodeSniffer validation` -> tick `Show sniff name` -> set coding standard to `Custom` -> three dots and type `/var/www/app/phpcs.xml` (path in container).

### PHPUnit

1. Copy `phpunit.xml.dist` into `phpunit.xml`.
1. Login into `app.php` container where `app.php` is the default container name from `docker-compose.yml.dist`, and run `./bin/phpunit`.
1. `Settings` -> `PHP` -> `Test frameworks`. Click `+` and `PHPUnit by Remote Intepreter` -> pick interpreter. In `PHPUnit library` tick `Path to phpunit.phar` and type `bin/phpunit`. Click refresh icon. In `Test runner` section set `Default configuration file` to `phpunit.xml` and `Default bootstrap file` to `tests/bootstrap.php`.
