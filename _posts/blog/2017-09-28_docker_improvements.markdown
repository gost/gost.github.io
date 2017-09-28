---
layout: post
title: "GOST Docker improvements"
date: 2017-09-28 12:30:00
author: Bert
categories: 
- blog
- Docker
---

The default deployment option for GOST is using Docker images. At the moment, we have three Docker images:

- <a href="https://hub.docker.com/r/geodan/gost/">geodan/gost</a>: Image containing the server

- <a href= "https://hub.docker.com/r/geodan/gost-db/">geodan/gost-db</a>: Image containing the database

- <a href="https://hub.docker.com/r/geodan/gost-dashboard">geodan/gost-dashboard</a>: Image containing front-end proxy (NGINX) and dashboard

### Small, Smaller, Smallest

We try to keep the Docker images as small as possible. Small images means really fast deployment and operation. In the last year, the GOST server image is reduced in size from 400MB to 10MB :-) We were able to create really small using two techniques: 

1] Alpine 

The GOST images are now based on Alpine images. Alpine is minimal Docker image based on Alpine Linux with a complete package index and only 5 MB in size!

2] Use multi-stage builds

Multi-stage builds is a new feature in Docker tooling. This techique allows to only deploy the resulting binaries in the image. More information about Docker multi-stage builds: <a href="https://docs.docker.com/engine/userguide/eng-image/multistage-build/">https://docs.docker.com/engine/userguide/eng-image/multistage-build/</a>




