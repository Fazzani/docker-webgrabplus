[![linuxserver.io](https://raw.githubusercontent.com/linuxserver/docker-templates/master/linuxserver.io/img/linuxserver_medium.png)](https://linuxserver.io)

The [LinuxServer.io](https://linuxserver.io) team brings you another container release featuring :-

 * regular and timely application updates
 * easy user mappings (PGID, PUID)
 * custom base image with s6 overlay
 * weekly base OS updates with common layers across the entire LinuxServer.io ecosystem to minimise space usage, down time and bandwidth
 * regular security updates

Find us at:
* [Discord](https://discord.gg/YWrKVTn) - realtime support / chat with the community and the team.
* [IRC](https://irc.linuxserver.io) - on freenode at `#linuxserver.io`. Our primary support channel is Discord.
* [Blog](https://blog.linuxserver.io) - all the things you can do with our containers including How-To guides, opinions and much more!

# [linuxserver/webgrabplus](https://github.com/linuxserver/docker-webgrabplus)
[![](https://img.shields.io/discord/354974912613449730.svg?logo=discord&label=LSIO%20Discord&style=flat-square)](https://discord.gg/YWrKVTn)
[![](https://images.microbadger.com/badges/version/linuxserver/webgrabplus.svg)](https://microbadger.com/images/linuxserver/webgrabplus "Get your own version badge on microbadger.com")
[![](https://images.microbadger.com/badges/image/linuxserver/webgrabplus.svg)](https://microbadger.com/images/linuxserver/webgrabplus "Get your own version badge on microbadger.com")
![Docker Pulls](https://img.shields.io/docker/pulls/linuxserver/webgrabplus.svg)
![Docker Stars](https://img.shields.io/docker/stars/linuxserver/webgrabplus.svg)
[![Build Status](https://ci.linuxserver.io/buildStatus/icon?job=Docker-Pipeline-Builders/docker-webgrabplus/master)](https://ci.linuxserver.io/job/Docker-Pipeline-Builders/job/docker-webgrabplus/job/master/)
[![](https://lsio-ci.ams3.digitaloceanspaces.com/linuxserver/webgrabplus/latest/badge.svg)](https://lsio-ci.ams3.digitaloceanspaces.com/linuxserver/webgrabplus/latest/index.html)

[Webgrabplus](http://www.webgrabplus.com) is a multi-site incremental xmltv epg grabber. It collects tv-program guide data from selected tvguide sites for your favourite channels.

[![webgrabplus](http://www.webgrabplus.com/sites/default/themes/WgTheme/images/slideshows/EPG_fading.jpg)](http://www.webgrabplus.com)

## Supported Architectures

Our images support multiple architectures such as `x86-64`, `arm64` and `armhf`. We utilise the docker manifest for multi-platform awareness. More information is available from docker [here](https://github.com/docker/distribution/blob/master/docs/spec/manifest-v2-2.md#manifest-list) and our announcement [here](https://blog.linuxserver.io/2019/02/21/the-lsio-pipeline-project/). 

Simply pulling `linuxserver/webgrabplus` should retrieve the correct image for your arch, but you can also pull specific arch images via tags.

The architectures supported by this image are:

| Architecture | Tag |
| :----: | --- |
| x86-64 | amd64-latest |
| arm64 | arm64v8-latest |
| armhf | arm32v7-latest |


## Usage

Here are some example snippets to help you get started creating a container.

### docker

```
docker create \
  --name=webgrabplus \
  -e PUID=1000 \
  -e PGID=1000 \
  -e TZ=Europe/London \
  -v <path to config>:/config \
  -v <path to data>:/data \
  --restart unless-stopped \
  linuxserver/webgrabplus
```


### docker-compose

Compatible with docker-compose v2 schemas.

```
---
version: "2"
services:
  webgrabplus:
    image: linuxserver/webgrabplus
    container_name: webgrabplus
    environment:
      - PUID=1000
      - PGID=1000
      - TZ=Europe/London
    volumes:
      - <path to config>:/config
      - <path to data>:/data
    restart: unless-stopped
```

## Parameters

Container images are configured using parameters passed at runtime (such as those above). These parameters are separated by a colon and indicate `<external>:<internal>` respectively. For example, `-p 8080:80` would expose port `80` from inside the container to be accessible from the host's IP on port `8080` outside the container.

| Parameter | Function |
| :----: | --- |
| `-e PUID=1000` | for UserID - see below for explanation |
| `-e PGID=1000` | for GroupID - see below for explanation |
| `-e TZ=Europe/London` | Specify a timezone to use EG Europe/London |
| `-v /config` | Where webgrabplus should store it's config files. |
| `-v /data` | Where webgrabplus should store it's data files. |

## User / Group Identifiers

When using volumes (`-v` flags) permissions issues can arise between the host OS and the container, we avoid this issue by allowing you to specify the user `PUID` and group `PGID`.

Ensure any volume directories on the host are owned by the same user you specify and any permissions issues will vanish like magic.

In this instance `PUID=1000` and `PGID=1000`, to find yours use `id user` as below:

```
  $ id username
    uid=1000(dockeruser) gid=1000(dockergroup) groups=1000(dockergroup)
```


&nbsp;
## Application Setup

To configure WebGrab+Plus follow the [guide][guideurl]

Note that there are some things in the guide that does not apply to this container. Below you can find the changes.

**The configuration files are found where your config volume is mounted.**
**Do not change the filename tag in the configuration file!**

The /data volume mapping is where WebGrab+Plus outputs the xml file. To use the xml file in another program, you have to point it to the host path you mapped the /data volume to.

To adjust the scheduled cron job for grabbing, edit the wg-cron file found in the `/config` folder. After you have edited the the wg-cron file, restart the container to apply the new schedule.
Do not adjust the command!

Below is the syntax of the cron file.

```
 ┌───────────── minute (0 - 59)
 │ ┌───────────── hour (0 - 23)
 │ │ ┌───────────── day of month (1 - 31)
 │ │ │ ┌───────────── month (1 - 12)
 │ │ │ │ ┌───────────── day of week (0 - 6) (Sunday to Saturday;
 │ │ │ │ │                                       7 is also Sunday on some systems)
 │ │ │ │ │
 │ │ │ │ │
 * * * * *  s6-setuidgid abc /bin/bash /defaults/update.sh
```



## Support Info

* Shell access whilst the container is running: `docker exec -it webgrabplus /bin/bash`
* To monitor the logs of the container in realtime: `docker logs -f webgrabplus`
* container version number 
  * `docker inspect -f '{{ index .Config.Labels "build_version" }}' webgrabplus`
* image version number
  * `docker inspect -f '{{ index .Config.Labels "build_version" }}' linuxserver/webgrabplus`

## Updating Info

Most of our images are static, versioned, and require an image update and container recreation to update the app inside. With some exceptions (ie. nextcloud, plex), we do not recommend or support updating apps inside the container. Please consult the [Application Setup](#application-setup) section above to see if it is recommended for the image.  
  
Below are the instructions for updating containers:  
  
### Via Docker Run/Create
* Update the image: `docker pull linuxserver/webgrabplus`
* Stop the running container: `docker stop webgrabplus`
* Delete the container: `docker rm webgrabplus`
* Recreate a new container with the same docker create parameters as instructed above (if mapped correctly to a host folder, your `/config` folder and settings will be preserved)
* Start the new container: `docker start webgrabplus`
* You can also remove the old dangling images: `docker image prune`

### Via Taisun auto-updater (especially useful if you don't remember the original parameters)
* Pull the latest image at its tag and replace it with the same env variables in one shot:
  ```
  docker run --rm \
  -v /var/run/docker.sock:/var/run/docker.sock taisun/updater \
  --oneshot webgrabplus
  ```
* You can also remove the old dangling images: `docker image prune`

### Via Docker Compose
* Update all images: `docker-compose pull`
  * or update a single image: `docker-compose pull webgrabplus`
* Let compose update all containers as necessary: `docker-compose up -d`
  * or update a single container: `docker-compose up -d webgrabplus`
* You can also remove the old dangling images: `docker image prune`

## Versions

* **23.03.19:** - Switching to new Base images, shift to arm32v7 tag.
* **21.03.19:** - Update to beta 2.1.7.
* **19.02.19:** - Add pipeline logic and multi arch.
* **18.01.18:** - Initial Release.
