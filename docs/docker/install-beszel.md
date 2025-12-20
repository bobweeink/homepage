# Install beszel using docker

## About beszel

[beszel](https://beszel.dev/) is a simple, lightweight server monitoring solution.

## Installation steps

SSH to your Linux docker host.

I use [KiTTY](https://www.9bis.net/kitty/index.html#!index.md) to ssh to my Linux docker host.

### Create a directory for beszel

```bash
mkdir beszel
```

### Move to directory beszel

```bash
cd beszel
```

### Create docker compose file and open it with nano

```bash
sudo nano docker-compose.yaml
```

### Configure the docker compose file

Paste the following yaml code into nano:

```yaml
services:
  beszel:
    image: henrygd/beszel
    container_name: beszel
    restart: unless-stopped
    ports:
      - 8090:8090
    volumes:
      - ./beszel_data:/beszel_data
```

### Build, create and start with docker compose

```bash
sudo docker compose up -d
```

The -d option makes the container run in the background.

When the beszel container is created and started, you can login on its webinterface  

## beszel homepage

Go to the IP address of the Linux docker host with port number 8090.  
>My host has IP 192.168.1.30 so that translates to <http://192.168.1.30:8090>  
