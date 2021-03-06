---
layout: post
title: "GOST and Raspberry Pi"
date: 2017-09-28 12:31:00
author: Bert
categories: 
- blog
- Raspberry Pi
- docker
---

On the Raspberry Pi, GOST is running in a  Docker environment. Pretty crazy at first thought but it works really well. The recommended option is to install <a href="https://blog.hypriot.com/">Hypriot</a> on the Raspberry Pi, it has the Docker tools preinstalled. There are separate GOST Docker images for Raspberry Pi, they have the prefix 'rpi':

- <a href="https://hub.docker.com/r/geodan/gost/">geodan/rpi-gost</a>: Raspberry Pi image containing the GOST server;

- <a href= "https://hub.docker.com/r/geodan/gost-db/">geodan/rpi-gost-db</a>: Raspberry Pi image containing the GOST database;

- <a href="https://hub.docker.com/r/geodan/gost-dashboard">geodan/rpi-gost-dashboard</a>: Raspberry Pi image containing front-end proxy (NGINX) and GOST dashboard.

The docker-compose file for Raspberry has the postfix 'rpi' and can be found <a href="https://github.com/gost/docker-compose/blob/master/docker-compose-rpi.yml">here</a>.

For a complete description of how to run GOST on Docker on Raspberry Pi see <a href="https://github.com/gost/docs/blob/master/gost_raspberrypi.md">https://github.com/gost/docs/blob/master/gost_raspberrypi.md<a/>.
