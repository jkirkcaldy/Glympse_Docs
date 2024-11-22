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

```yaml {filename="compose.yml"}
services:
  Glympse:
    image: git.themainframe.co.uk/josh/glympse
    container_name: Glympse
    restart: unless-stopped
    ports:
      - 80:80
    environment:
      TZ: Europe/London
      ENABLE_UWSGI: yes
      ENABLE_NGINX: yes
      ENABLE_CELERY_BEAT: yes
      ENABLE_PROCESSING: yes
      ENABLE_MIGRATIONS: yes
      ENABLE_TRANSCRIBER: yes
      ENABLE_TRANSCODER: yes
    volumes:
      - /opt/glympse/logs:/Glympse/logs
      - /opt/glympse/.env:/Glympse/config/.env
      - <Glympse media location>:/media
      - <rushes location>:/rushes:ro
    deploy:
      resources:
        reservations:
          devices:
            - driver: nvidia
              device_ids:
                - 00000000:01:00.0
              capabilities:
                - gpu     
  memcached:
    image: memcached:latest
    container_name: memcached
    entrypoint: memcached -m 256
    restart: unless-stopped                 
networks:
  default:
    external: true
    name: glympse_net
```


The following env file is required. You will need to change some of the options to what you specified in the database section of the install. 

Every line must be in the env file for every container you run regardless of what services are enabled. 

```env {filename=".env"}
DJANGO_SETTINGS_MODULE="Glympse.settings.production"
SECRET_KEY="<generate a long random string>"
SECRET_ADMIN_URL=<Generate a short random string>
CSRF_TRUSTED_ORIGINS=https://"<Glympse domain>"
ALLOWED_HOSTS="<Glympse Domain>"
ADMINS='<admin email address>'
DOMAIN='<Glympse domain>'
MAX_FIELDS=5000
SQL_ENGINE="django.db.backends.mysql"
SQL_DATABASE="glympse"
SQL_USER="glympse"
SQL_PASSWORD="<your glympse mysql password>"
SQL_HOST="mysql"
SQL_PORT="3306"
EMAIL_HOST="<mail server>"
EMAIL_PORT="<mail port>"
EMAIL_USE_TLS="<mail tls>"
EMAIL_HOST_USER="<email username>"
EMAIL_HOST_PASSWORD="<email password>"
DEFAULT_FROM_EMAIL="Glympse<glympse@example.com>"
ALLOWED_EMAIL_DOMAINS="<allowed domains for user sign up>"
REDIS_HOST="redis://redis:6379"
RABBITMQ_URL="amqp://glympse:<rabbitmq password>@rabbitmq:5672/glympse"
ADMIN_USERNAME="<admin username>"
ADMIN_EMAIL="<admin email address>"
ADMIN_PASSWORD="<default admin password>"
LOG_LEVEL="INFO"
TIMEZONE="Europe/London"
MICROSOFT_AUTH_CLIENT_ID='<client ID for SSO>'
MICROSOFT_AUTH_CLIENT_SECRET='<client secret for SSO>'
MICROSOFT_AUTH_TENANT_ID='<tenant ID for SSO>'
MEMCACHED='memcached:11211'
CUDA_DEVICE='cuda:0'
DEBUG=False
```