---
title: Install Glympse
type: docs
prev: docs/services
next: docs/
---
To install glympse we first need to create a docker compose file and then an env file. These two files will allow you to configure most out of the box settings. Once you're set these up, you will be able to change more setting from the webui. 

Services default to enabled, to disable a service, change yes to no in the environment section of the compose file. To enable a service, either change the environment variable to yes or delete/comment the line.

UWSGI and NGINX are required for the webui service

The following compose file will enable all services in a single container. This will work but it is possible that the transcoding and transcribing tasks can slow down the webui if you're running on a slower machine. 

If you are going to be running the transcoder or the transcription engine, you will need an Nvidia GPU. To find the device ID run `nvidia_smi` on the system you will be installing the container. 
This will give you an output that looks like this:
```
+-----------------------------------------------------------------------------------------+
| NVIDIA-SMI 565.57.02              Driver Version: 566.03         CUDA Version: 12.7     |
|-----------------------------------------+------------------------+----------------------+
| GPU  Name                 Persistence-M | Bus-Id          Disp.A | Volatile Uncorr. ECC |
| Fan  Temp   Perf          Pwr:Usage/Cap |           Memory-Usage | GPU-Util  Compute M. |
|                                         |                        |               MIG M. |
|=========================================+========================+======================|
|   0  NVIDIA GeForce GTX 1080        On  |   00000000:01:00.0  On |                  N/A |
| 27%   38C    P8              8W /  180W |     893MiB /   8192MiB |      1%      Default |
|                                         |                        |                  N/A |
+-----------------------------------------+------------------------+----------------------+
                                                                                         
+-----------------------------------------------------------------------------------------+
| Processes:                                                                              |
|  GPU   GI   CI        PID   Type   Process name                              GPU Memory |
|        ID   ID                                                               Usage      |
|=========================================================================================|
|    0   N/A  N/A        26      G   /Xwayland                                   N/A      |
+-----------------------------------------------------------------------------------------+
```
The device id is in the third row, just above the memory:
```
+========================+
|   00000000:01:00.0  On |
|     893MiB /   8192MiB |
|                        |
+------------------------+
```
In this case the device id is: `00000000:01:00.0`

Add this to your compose file. A file that runs all services with a gpu looks like:

```yaml {filename="compose.yml"}
services:
  Glympse_net:
    image: git.themainframe.co.uk/josh/glympse
    container_name: Glympse
    restart: unless-stopped
    ports:
      - 8080:80
    environment:
      TZ: Europe/London
      ENABLE_UWSGI: yes
      ENABLE_NGINX: yes
      ENABLE_CELERY_BEAT: yes
      ENABLE_PROCESSING: yes
      ENABLE_MIGRATIONS: yes
      ENABLE_TRANSCRIBER: yes
      ENABLE_TRANSCODER: yes
      DJANGO_SETTINGS_MODULE: Glympse.settings.production
      SECRET_KEY: "super_secret_key"
      SECRET_ADMIN_URL: random_string
      CSRF_TRUSTED_ORIGINS: http://127.0.0.1
      ALLOWED_HOSTS: 127.0.0.1
      ADMINS: admin@glympsevideo.com
      DOMAIN: glympse.glympsevideo.com
      SQL_ENGINE: django.db.backends.mysql
      SQL_DATABASE: glympse
      SQL_USER: glympse
      SQL_PASSWORD: insecure_password
      SQL_HOST: mysql
      SQL_PORT: 3306
      EMAIL_HOST: mail.themainframe.co.uk
      EMAIL_PORT: 587
      EMAIL_USE_TLS: true
      EMAIL_HOST_USER: noreply@glympsevideo.com
      EMAIL_HOST_PASSWORD: insecure_password
      DEFAULT_FROM_EMAIL: Glympse<glympse@glympsevideo.com>
      ALLOWED_EMAIL_DOMAINS: glympsevideo.com
      REDIS_HOST: redis://redis:6379
      RABBITMQ_URL: amqp://glympse:insecure_password@rabbitmq:5672/glympse
      ADMIN_USERNAME: hub
      ADMIN_EMAIL: admin@glympsevideo.com
      ADMIN_PASSWORD: insecure_password
      LOG_LEVEL: DEBUG
      TIMEZONE: Europe/London
      MICROSOFT_AUTH_CLIENT_ID: None
      MICROSOFT_AUTH_CLIENT_SECRET: None
      MICROSOFT_AUTH_TENANT_ID: None
      MEMCACHED: memcached:11211
      CUDA_DEVICE: cuda:0
      DEBUG: "True"
      REMOTE_WORKER: "False"
      REMOTE_PRODUCTION: None
    volumes:
      - /opt/glympse/logs:/Glympse/logs
      - /media:/media
      - /rushes:/rushes:ro
    deploy:
      resources:
        reservations:
          devices:
            - driver: nvidia
              device_ids:
                - 00000000:01:00.0
              capabilities:
                - gpu
    networks:
      glympse_net: null
  memcached:
    image: memcached:latest
    container_name: memcached
    entrypoint: memcached -m 256
    restart: unless-stopped
    networks:
      glympse_net: null
networks:
  glympse_net:
    external: true
```

If you are install Glympse on a second system or a system other than where the raw camera files are stored you can use the following to mount the volumes as smb shares:
 
```yaml 
volumes:
  rushes:
    driver: local
    driver_opts:
      type: cifs
      o: username=<sbm_username>,password=<smb_password>,ro,domain=localhost
      device: \\smb\share\path
```

Alternatively you can use a nfs share:
```yaml
volumes:
  rushes:
    driver_opts:
      type: nfs
      o: "addr=<nfs-server-ip>,nolock,soft,rw"
      device: ":/full/share/path"
```

You will need to make sure that if you are mounting the raw rushes volume that you mount this as `ro` or read only so Glympse can not make any changes to the raw rushes. 

If you are mounting the glympse mediafiles or logs, you will need to mount this as `rw` or read write so Glympse can create the files. 