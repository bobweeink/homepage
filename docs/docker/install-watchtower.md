# Install watchtower with docker, and get notifications with Discord

## About watchtower

[watchtower](https://containrrr.dev/watchtower/) is an awesome tool to update your docker containers automatically.

## About Discord

>Discord is an instant messaging and VoIP social platform which allows communication through voice calls, video calls, text messaging, and media. Communication can be private or take place in virtual communities called "servers"

I want watchtower to send me notifications on Discord.  
You can signup for free on [Discord](https://discord.com/)  
With your Discord account, you can create your own free Discord server with webhooks connected to tekst channels.  

## Installation steps

SSH to your Linux docker host.

I use [KiTTY](https://www.9bis.net/kitty/index.html#!index.md) to ssh to my Linux docker host.

### Create a directory for watchtower

```bash
mkdir watchtower
```

### Move to directory watchtower

```bash
cd watchtower
```

### Create docker compose file and open it with nano

```bash
sudo nano docker-compose.yaml
```

### Configure the docker compose file

Paste the following yaml code into nano:

```yaml
services:
  watchtower:
    image: containrrr/watchtower:latest
    container_name: watchtower
    environment:
      - PUID=1000
      - PGID=1000
      - TZ=Europe/Amsterdam
      - WATCHTOWER_CLEANUP=true
      - WATCHTOWER_INCLUDE_STOPPED=true
      - WATCHTOWER_REVIVE_STOPPED=false
      - WATCHTOWER_INCLUDE_RESTARTING=true
      - WATCHTOWER_SCHEDULE=0 0 4 * * *
      - WATCHTOWER_NOTIFICATIONS=shoutrrr
      - WATCHTOWER_NOTIFICATION_URL=discord://TOKEN@WEBHOOKID
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock
    restart: unless-stopped
```

Select the timezone that you live in:

[List of time zones](https://en.wikipedia.org/wiki/List_of_tz_database_time_zones)

```yaml  
      - TZ=Europe/Amsterdam
```

I want watchtower to run every night on 04:00:

```yaml  
      - WATCHTOWER_SCHEDULE=0 0 4 * * *
```

When you copy your own Discord server webhook URL you get the following:

```html
https://discord.com/api/webhooks/WEBHOOKID/TOKEN
```

The watchtower docker compose yaml code wants the TOKEN first followed by the @ and then the WEBHOOKID:

```yaml  
      - WATCHTOWER_NOTIFICATION_URL=discord://TOKEN@WEBHOOKID
```

Save the file in nano with CTRL+O  
Hit Enter  
Exit nano with CTRL+X

### Build, create and start with docker compose

```bash
sudo docker compose up -d
```

The -d option makes the container run in the background.

When the watchtower container is created and started, your Discord channel should receive a notification.

## Excluding docker containers

If you don't want a container to be updated by watchtower you can exclude it.  
You exclude it in the docker compose file of the container you want excluded, you don't put this code in the docker compose file of watchtower.

```yaml  
services:
  someimage:
    container_name: someimage
    labels:
      - "com.centurylinklabs.watchtower.enable=false"
```
