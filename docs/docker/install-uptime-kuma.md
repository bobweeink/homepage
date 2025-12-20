# Install uptime kuma

## About uptime kuma

[uptime kuma](https://uptimekuma.org/) is an awesome monitoring tool.

>Uptime Kuma is an open-source, free and easy-to-use self-hosted monitoring tool. Uptime Kuma is compatible with multiple platforms including Linux, Windows 10 (x64) and Windows Server.

## Installation steps

SSH to your Linux docker host.

I use [KiTTY](https://www.9bis.net/kitty/index.html#!index.md) to ssh to my Linux docker host.

### Create a directory for uptime kuma

```bash
mkdir uptimekuma
```

### Move to directory uptimekuma

```bash
cd uptimekuma
```

### Create docker compose file and open it with nano

```bash
sudo nano docker-compose.yaml
```

### Configure the docker compose file

Paste the following yaml code into nano:

```yaml
services:
  uptime-kuma:
    image: louislam/uptime-kuma:latest
    volumes:
      - ./data:/app/data
      - /var/run/docker.sock:/var/run/docker.sock
    ports:
      - 3001:3001
    restart: unless-stopped

```

I want to access uptime kuma on port 3001, so I put in the following ports configuration:

```yaml  
    ports:
      - 3001:3001
```

The port definition on the left side is your local host.  
The port definition on the right side is the port inside the docker container.

Save the file in nano with CTRL+O  
Hit Enter  
Exit nano with CTRL+X

### Build, create and start with docker compose

```bash
sudo docker compose up -d
```

The -d option makes the container run in the background.

When the uptime kuma container is created and started, you can login on its webinterface  

## uptime kuma homepage

Go to the IP address of the Linux docker host with port number 3001.  
>My host has IP 192.168.1.2 so that translates to <http://192.168.1.2:3001>  
