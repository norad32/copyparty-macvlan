# copyparty Docker Compose on MacVLAN

This repository contains a Docker Compose setup for running [Copyparty](https://github.com/9001/copyparty) as a LAN-native service using a macvlan network with a static IP.

---

## Setup

### 1. Create the macvlan 

Example (adjust interface and subnet to your environment):

```bash
docker network create -d macvlan \
  --subnet=192.168.1.0/24 \
  --gateway=192.168.1.1 \
  -o parent=eth0 \
  lan_macvlan
```

### 2. Create the .env file

Create a file named `.env` next to `docker-compose.yaml`.

This file is required and exlcuded via gitignore.

```
###############################################################################
# Copyparty – environment configuration
#
# This file is read by Docker Compose via ${VAR} expansion.
# It MUST live in the same directory as docker-compose.yaml.
# Do NOT commit this file to git (it contains secrets).
###############################################################################

### ---------------------------------------------------------------------------
### Networking
### ---------------------------------------------------------------------------

# Static IPv4 address assigned to the container on the macvlan network.
# This IP must:be within the macvlan subnet
COPY_PARTY_IP=192.168.1.69


### ---------------------------------------------------------------------------
### Data paths
### ---------------------------------------------------------------------------

# Host directory that will be mounted to /w inside the container.
# This is where your shared files live.
#
# Example:
#   /srv/media
COPY_PARTY_FOLDER=/srv/media


### ---------------------------------------------------------------------------
### Timezone
### ---------------------------------------------------------------------------

# Timezone used by the container (affects log timestamps).
# List: https://en.wikipedia.org/wiki/List_of_tz_database_time_zones
COPY_PARTY_TZ=Europe/London
```

### 3. Configure copyparty

Edit `copyparty.conf` to your liking.

---

### 4. Start the service

```bash
docker compose up -d
```

Check status:

```bash
docker compose ps
docker compose logs copyparty
```
---

## Accessing Copyparty

From another machine on the LAN:

http://<COPY_PARTY_IP>:3923

---

## Credits

Copyparty is developed by [9001](https://github.com/9001)
Original repository: https://github.com/9001/copyparty
