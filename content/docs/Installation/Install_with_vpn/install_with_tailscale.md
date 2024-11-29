---
title: Install With Tailscale
type: docs
prev: /docs
weight: 1
---
It is possible to use Tailscale as a private vpn proxy between all your Glympse containers. This connect nodes to nodes on other networks. 

The reasons for doing this are mainly for security purposes. It gives you a couple of options. First you can create a tunnel to your Glympse webUI from a cheap VPS. This hides your IP address and does not require you to open any ports in your firewall. You can also limit the traffic to only entering your Glympse WebUI contianer. 

Another option which is even more secure but has an additional cost, is you can require your users be on your tailnet. This would mean that there is zero access to Glympse from the internet, but your users who have tailscale installed can access Glympse through the tailscale VPN. 

## Install sidecar container
Connecting your containers to tailscale uses the tailscale sidecar containers. To read more about how this works click [here](https://tailscale.com/kb/1282/docker).

Adding a sidecar container to your Glympse compose file is pretty simple. first add the Tailscale container to the top of your Glympse compose file: 

```yaml {filename="compose.yml"}
...

  glympse-ts:
    image: tailscale/tailscale
    container_name: glympse-ts
    cap_add:
      - net_admin
      - sys_module
    volumes:
      - /dev/net/tun:/dev/net/tun
    environment:
      - TS_AUTHKEY=<tailscale auth key>
      - TS_STATE_DIR=/var/lib/tailscale
    hostname: glympse-<machine name>
    restart: unless-stopped

...

```

You then need to make a small tweak to your Glympse container's networking settings:

```yaml {filename="compose.yml"}
...
    network_mode: service:glympse-ts
...

```
This will tell your Glympse container to route it's traffic through the Tailscale Container. 