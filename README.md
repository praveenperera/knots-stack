This is a docker compose stack for running a knots node with electrs and mempool. You can use this as a template to run your own.

## Prerequisites

- docker
- docker-compose

## Installation

1. Clone this repository
2. Run `docker compose up -d` to start the stack

## Systemd service

You can also run the stack as a systemd service. To do so, run `cp knots-stack.service /etc/systemd/system/` and then `systemctl enable knots-stack.service` to enable the service.

But before you do updaate the following lines with the actual location of your `docker-compose.yaml` file:

```
WorkingDirectory=/opt/stack
ExecStart=/usr/bin/docker compose -f /opt/stack/docker-compose.yaml up -d
ExecStop=/usr/bin/docker compose -f /opt/stack/docker-compose.yaml down
```

## Notes

- I am exposing the the RPC ports to the world, but thats because I'm running this already locked down to my local network
  if you are not going to use bitcoin RPC directly remove these lines from the docker-compose.yml

```yaml
ports:
  - "8333:8333" # P2P (optional to expose externally)
  - "8332:8332" # RPC (firewall/Tailscale restrict externally)
```

- A Caddy reverse proxy is included to expose the mempool frontend on port 80. It assumes that mDNS (i'm using avahi) is setup as `bitcoin.local`.
  This is completely optional, I just want to see my mempool.space instance when I go to bitcoin.local on my local network. If you don't have avahi installed, you can just use the IP address
  of your bitcoin node instead of bitcoin.local. To remove caddy remove the following lines from the `docker-compose.yml`.

```yaml
caddy:
  image: caddy:2-alpine
  container_name: caddy
  ports:
    - "80:80"
  volumes:
    - ./Caddyfile:/etc/caddy/Caddyfile:ro
```

## Usage

The stack exposes the following ports:

- Knots RPC: 127.0.0.1:8332
- Electrs: 127.0.0.1:50001
- Mempool: 127.0.0.1:8080
