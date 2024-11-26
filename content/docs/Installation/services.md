---
title: Glympse Services
type: docs
prev: /
next: docs/install_glympse
---

Glympse is split into multiple services so you can split the workload between multiple machines. 

The available services are as follows:
* Celery Beat\
You should only have one of these running across all your containers. This is responsible for managing the scheduled tasks. Without this, the scheduled tasks will not run. For ease, I usually run this in the same container as the webui. 

* WebUI\
  This is the webui front end. The reverse proxy needs to point to the machine that is running this service. It is possible to run more than one frontend for load balancing purposes. 

* Processing\
  This service is responsible for scanning the rushes drives and inserting the rushes into the Glympse database. It is also responsible for other background tasks such as sending emails etc. This worker will run 16 simultaneous tasks. 

* Transcoding - **Prefers Nvidia GPU**\
This is the transcoding engine. It takes the raw rushes and transcodes them to lower resolution proxy files that can be streamed. The transcoder will try use cuda hardware acceleration but will fall back to software if it is unavailable. This worker will run 4 simultaneous tasks. 

* Long Transcoding\
This was set up to create the thumbnail preview for the video player when you hover over the video play bar to allow for scrubbing. This worker will run 16 simultaneous tasks. 

* Transcribing - **Requires Nvidia GPU**\
  This is responsible for transcribing clips. A modern Nvidia GPU is required for this to work. It is possible to select a different model that may better suit the available hardware. A GPU with more VRAM can support a larger model. This worker will run a single task at a time. 

It is possible to run one or more services in a single docker container. the running services are managed through the environment settings in the compose file. 