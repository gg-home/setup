## Transmission
### Windows
Download and install Transmission. Configure the following:

1. Stop the Transmission service and exit the QT client app
2. Wait couple of seconds...
3. Open and edit the following file: C:\Windows\ServiceProfiles\LocalService\AppData\Local\transmission-daemon\settings.json
=> `"rpc-whitelist-enabled": false`  
=> `"rpc-host-whitelist-enabled": false`  
5. Start the service
6. Verify that the settings are the still the same as you have entered them a moment ago. If not go to 1.

Configure paths:  
"download-dir": "T:\\etc\\transmission",  
"watch-dir": "T:\\Downloads",  
"incomplete-dir": "T:\\etc\\transmission-downloading",  
"incomplete-dir-enabled": true,  

https://github.com/transmission/transmission/issues/226#issuecomment-832206400

# Docker containers

This setup is based on Windows10 and Docker for Windows. 

By default all apps use the drive `T:\etc` as base directory for volumes or any other persistent data.

## install home assistant
`docker run --init -d -p 8123:8123 --name="hass" -v "T:\etc\hass":/config -e "TZ=Europe/Sofia" homeassistant/home-assistant`

## install mqtt for home assistant
`docker run --restart=always -d -e RABBITMQ_NODENAME=ggh --name ggh-rabbitmq -p 1883:1883 -p 8883:8883 -p 15672:15672 -p 5672:5672 gghome/rabbitmq-mqtt`

## install transmission
`docker run -d --name=transmission -e PUID=1000 -e PGID=1000 -e TZ=Europe/Sofia -p 9091:9091 -p 51413:51413 -p 51413:51413/udp -v "T:\etc\transmission2\config":/config -v "T:\etc\transmission2\downloads":/downloads -v "T:\etc\transmission2\watch":/watch --restart unless-stopped linuxserver/transmission:latest`

## install couchpotato
`docker run -d --name=couchpotato -e PUID=1000 -e PGID=1000 -e TZ=Europe/Sofia -p 5050:5050 -v "T:\etc\couchpotato":/config -v "T:\etc\transmission2\watch":/downloads -v "D:\Media":/media --restart unless-stopped ghcr.io/linuxserver/couchpotato`
