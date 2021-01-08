# install home assistant
`docker run --init -d -p 8123:8123 --name="hass" -v "T:\etc\hass":/config -e "TZ=Europe/Sofia" homeassistant/home-assistant`

# install mqtt for home assistant
`docker run --restart=always -d -e RABBITMQ_NODENAME=ggh --name ggh-rabbitmq -p 1883:1883 -p 8883:8883 -p 15672:15672 -p 5672:5672 gghome/rabbitmq-mqtt`
