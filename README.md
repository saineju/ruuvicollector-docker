# Dockerized Ruuvi collector setup
This set of scripts aims to provice a really simple way of setting up a RuuviCollector
instance. While this obviously provides an easy way for beginners, I would encourage
beginners to first try to set up the environment manually according to the ruuvi blog,
just to get the learning experience of fiddling with linux a bit & getting the learning
experience.

It is assumed that you already have a Raspberry Pi (or some other linux) up and
running. These instructions have been tested on Raspberry Pi with debian wheezy,
but should work on any fairly recent linux with suitable bluetooth module (requires
bluetooth 4.0 LE)

This setup script is based on instructions from here:
* https://blog.ruuvi.com/rpi-gateway-6e4a5b676510

And the actual RuuviCollector is from Scrin:
* https://github.com/Scrin/RuuviCollector

Currently this is still a work in progress and mainly done for my own amusement, so there may be open
problems.

## Prerequisites
* If you have installed docker from raspberry pi debian repository, remove it before installing CE version

`sudo apt-get remove docker docker-engine docker.io containerd runc`

* Install docker CE (do not use the one from debian repository)

```
curl -fsSL https://get.docker.com -o get-docker.sh
sudo sh get-docker.sh
```

For more complete information refer to official instructions: https://docs.docker.com/install/linux/docker-ce/debian/

* Install python3, python3-pip & libffi-dev

`sudo apt-get install python3 python3-pip python3-dev libffi-dev`

* install docker-compose

`pip3 install docker-compose`

## Usage
When all prerequisites are met the setup should just work with command

`sudo docker-compose -d up`

When dockers are up and running (may take some time as it needs to build the ruuvicollector docker)
you should be able to browse to [your server private ip]:8080 and see grafana login page

log in with admin/admin, change admin password and you should see welcome screen with 
"Create datasource" and "Create Dashboard" both crossed out and Ruuvi Temperature -dashboard
in the lower right space. When clicking that you should see temperature graphs being drawn
from your ruuvi sensors.

## What does it do?
The docker compose should create persistent disks for influxdb and grafana
and a network in which the dockers can connect each other. The ruuvicollector
docker will be run in privileged mode ton make sure it can access bluetoot device.

## Todo
* Add authentication to fluxdb
* Add humidity & air pressure to default panels
* Add nginx frontend
* Add support for letsencrypt
* Create makefile or other script to make this whole thing run with one command only
