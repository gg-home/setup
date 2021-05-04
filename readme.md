# Docker containers

This setup is based on Windows10 and Docker for Windows. 

By default all apps use the drive `T:\etc` as base directory for volumes or any other persistent data.

## install home assistant
`docker run --init -d -p 8123:8123 --name="hass" -v "T:\etc\hass":/config -e "TZ=Europe/Sofia" homeassistant/home-assistant`

## install mqtt for home assistant
`docker run --restart=always -d -e RABBITMQ_NODENAME=ggh --name ggh-rabbitmq -p 1883:1883 -p 8883:8883 -p 15672:15672 -p 5672:5672 gghome/rabbitmq-mqtt`

## install couchpotato
`docker run -d --name=couchpotato -e PUID=1000 -e PGID=1000 -e TZ=Europe/Sofia -p 5050:5050 -v "T:\etc\couchpotato":/config -v "T:\etc\couchpotato\downloads":/downloads -v "D:\Media\movies":/movies --restart unless-stopped ghcr.io/linuxserver/couchpotato`
