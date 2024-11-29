---
title: Install With Netbird
type: docs
prev: /docs
weight: 2
---
Insted of using Tailscale as your VPN options, it is possible to use Netbird istead. Netbird works in a very similar way to Tailscale however it can be completely selfhosted. This means you could run Netbird on a VPS for a few £ per month and have unlimited users accessing Glympse adn any other services you wish to include. 

The reasons for doing this are mainly for security purposes. It gives you a couple of options. First you can create a tunnel to your Glympse webUI from a cheap VPS. This hides your IP address and does not require you to open any ports in your firewall. You can also limit the traffic to only entering your Glympse WebUI contianer. 

Another option which is even more secure but has an additional cost, is you can require your users be on your tailnet. This would mean that there is zero access to Glympse from the internet, but your users who have the netbird client installed can access Glympse through the netbird VPN. 

Connecting your containers to Netbird uses the Netbird sidecar containers. To read more about how this works click [here](https://netbird.io/knowledge-hub).
## Install sidecar container
Adding a sidecar container to your Glympse compose file is pretty simple. first add the Netbird container to the top of your Glympse compose file: 

```yaml {filename="compose.yml"}
...

  glympse-nb:
    image: netbirdio/netbird
    container_name: glympse-nb
    cap_add:
      - net_admin
      - sys_module
    environment:
      - NB_SETUP_KEY=<your netbird key>
      - NB_MANAGEMENT_URL=<netbird management url>
    hostname: glympse-<machine name>
    restart: unless-stopped

...

```

You then need to make a small tweak to your Glympse container's networking settings:

```yaml {filename="compose.yml"}
...
    network_mode: service:glympse-nb
...

```
This will tell your Glympse container to route it's traffic through the Netbird Container. 