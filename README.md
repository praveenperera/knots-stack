This is a docker compose stack for running a knots node with electrs and mempool.

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

## Usage

The stack exposes the following ports:

- Knots RPC: 127.0.0.1:8332
- Electrs: 127.0.0.1:50001
- Mempool: 127.0.0.1:8080
