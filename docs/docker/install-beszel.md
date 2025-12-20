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

Save the file in nano with CTRL+O  
Hit Enter  
Exit nano with CTRL+X

### Build, create and start with docker compose

```bash
sudo docker compose up -d
```

The -d option makes the container run in the background.

When the beszel container is created and started, you can login on its webinterface.  

## beszel homepage

Go to the IP address of the Linux docker host with port number 8090.  
>My host has IP 192.168.1.30 so that translates to <http://192.168.1.30:8090>  

## beszel on raspberry pi not showing container statistics

I have beszel running on a raspberry pi, and noticed that it did not show the container statistics.  
To fix this issue I used the following guide [Resolving Missing Memory Stats in Docker Stats on Raspberry Pi](https://akashrajpurohit.com/blog/resolving-missing-memory-stats-in-docker-stats-on-raspberry-pi/)  

Open the file /boot/firmware/cmdline.txt with nano:

```bash
sudo nano /boot/firmware/cmdline.txt
```

add the following at the end of the line:

```bash
cgroup_enable=cpuset cgroup_enable=memory cgroup_memory=1  
```

Save the file in nano with CTRL+O  
Hit Enter  
Exit nano with CTRL+X

reboot the raspberry pi:

```bash
sudo reboot
```
