# tiredofit/otrs

# Introduction

This will build a container for [OTRS](http://www.otrs.com) - An Incident tracking and ITSM Solution.

* This Container uses a [customized Alpine Linux base](https://hub.docker.com/r/tiredofit/alpine) which includes [s6 overlay](https://github.com/just-containers/s6-overlay) enabled for PID 1 Init capabilities, [zabbix-agent](https://zabbix.org) for individual container monitoring, Cron also installed along with other tools (bash,curl, less, logrotate, nano, vim) for easier management. It also supports sending to external SMTP servers

Additional Components Inside are Nginx, fcgiwrap, mysql-client.


[Changelog](CHANGELOG.md)

# Authors

- [Dave Conroy](https://github.com/tiredofit)

# Table of Contents

- [Introduction](#introduction)
    - [Changelog](CHANGELOG.md)
- [Prerequisites](#prerequisites)
- [Installation](#installation)
- [Quick Start](#quick-start)
- [Configuration](#configuration)
    - [Database](#database)
    - [Data Volumes](#data-volumes)
    - [Environment Variables](#environmentvariables)   
    - [Networking](#networking)
- [Maintenance](#maintenance)
    - [Shell Access](#shell-access)
   - [References](#references)

# Prerequisites

# Prerequisites

This image assumes that you are using a reverse proxy such as 
[jwilder/nginx-proxy](https://github.com/jwilder/nginx-proxy) and optionally the [Let's Encrypt Proxy 
Companion @ 
https://github.com/JrCs/docker-letsencrypt-nginx-proxy-companion](https://github.com/JrCs/docker-letsencrypt-nginx-proxy-companion) 
in order to serve your pages. However, it will run just fine on it's own if you map appropriate ports.

You will also need an external MySQL/MariaDB Container. The following settings must be set in the MySQL Configuration:

```bash
[mysqld]
max_allowed_packet=24M
innodb_log_file_size=512M 
```


# Installation

Automated builds of the image are available on [Docker Hub](https://hub.docker.com/tiredofit/otrs) and is the 
recommended method of installation.


```bash
docker pull tiredofit/otrs
```

The following image tags are available:

* `5` - OTRS 5 Latest Release w/Alpine Edge
* `6` - OTRS 6 Beta w/Alpine Edge
* `latest` - Most recent stable release of OTRS w/Alpine Edge

# Quick Start

* The quickest way to get started is using [docker-compose](https://docs.docker.com/compose/). See the examples folder for a working [docker-compose.yml](examples/docker-compose.yml) that can be modified for development or production use.

* Set various [environment variables](#environment-variables) to understand the capabilities of this image.
* Map [persistent storage](#data-volumes) for access to configuration and data files for backup.

Further edit your /var/lib/otrs/Kernel/Config.pm with any additional settings.


# Configuration

### Persistent Storage

In order for data to persist between start and stops the following folders are available to be mapped for persistent storage:

| Directory | Description |
|-----------|-------------|
| `/addons` | Put .OPM files here to be automatically installed upon container start |
| `/var/lib/otrs/Kernel` | Configuration Data |
| `/www/logs` | Nginx and OTRS Logs |
      

### Environment Variables

Along with the Environment Variables from the [Base image](https://hub.docker.com/r/tiredofit/alpine), and [Nginx](https://hub.docker.com/r/tiredofit/nginx) below is the complete list of available options that can be used to customize your installation.


| Parameter | Description |
|-----------|-------------|
| `HOSTNAME` | What the hostname of the helpdesk is (e.g. `helpdesk.example.com`)
| `SYSTEM_ID` | System ID - Numeric or Alphabetical (e.g. `EX`)
| `ADMIN_EMAIL` | Administrator's Email (e.g. `admin@example.com` )
| `ORGANIZATION` | Organization Name (e.g. `My Company`)
| `FETCH_TIME` | How often to check for new mails (e.g. 5)
| `LANGUAGE` | Default Language (e.g. en)
| `TICKET_NUMBER` | Ticker Number Start - e.g. `1` |
| `NUMBER_GENERATOR` | How to define Ticket Numbers (e.g. `AutoIncrement`) |
| `ROOT_PASS` | root@localhost password - Default `password` |
| `DB_HOST` | DB Container Hostname e.g. `rootpassword` |
| `DB_NAME` | DB Name e.g. `otrs` |
| `DB_USER` | DB User e.g. `otrs` |
| `DB_PASS` | DB Password e.g. `password` |

### Networking

The following ports are exposed.

| Port      | Description |
|-----------|-------------|
| `80` | HTTP |


# Maintenance
#### Shell Access

For debugging and maintenance purposes you may want access the containers shell. 

```bash
docker exec -it (whatever your container name is e.g. otrs) bash
```

# References

* https://www.otrs.com



