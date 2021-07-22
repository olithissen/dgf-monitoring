# DGF Monitoring

*Wir basteln uns unsere eigene Verfügbarkeitsprüfung für das Netz der Deutschen Glasfaser \o/*

## Start

### Shell

```bash
docker run \
  -e LAT=51.0 \
  -e LON=6.0 \
  -e USER=demo \
  -e INFLUXTOKEN=<dein_token> \
  -v ./telegraf.conf:/etc/telegraf/telegraf.conf:ro \
  telegraf
```

### docker-compose.yaml

```yaml
version: "3"

services:
  dgfmonitor:
    restart: always
    image: "telegraf"
    volumes:
      - "./telegraf.conf:/etc/telegraf/telegraf.conf:ro"
    environment:
        LAT: 51.0
        LON: 6.0
        USER: demo
        INFLUXTOKEN: <dein_token>  
```
