# DGF Monitoring

*Wir basteln uns unsere eigene Verfügbarkeitsprüfung für das Netz der Deutschen Glasfaser \o/*

## Seiten
* Landing Page: https://dgf.tonick.net (Öffentlich)
* Grafana: https://gfa.tonick.net (Gastzugang)
* InfluxDB: https://ifx.tonick.net (Nur für geladene Gäste)

## Setup

### Ein paar Hinweise vorab
* Setze für `LAT` und `LON` möglichst präzise die Koordinaten deines Anschlusses ein
* Verwende für `USER` einen möglichst eindeutigen und aussagekräftigen Namen

### Wie komme ich an mein Token?
* Mail an oli[at_]tonick.net

## Docker

### docker run

```bash
docker run \
  -e LAT=51.0 \
  -e LON=6.0 \
  -e USER=demo \
  -e INFLUXTOKEN=<dein_token> \
  -v ./configs/telegraf.conf:/etc/telegraf/telegraf.conf:ro \
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
      - "./configs/telegraf.conf:/etc/telegraf/telegraf.conf:ro"
    environment:
        LAT: 51.0
        LON: 6.0
        USER: demo
        INFLUXTOKEN: <dein_token>  
```

## Vorhandene Telegraf Installation

<todo>

## Manuelle Datenlieferung über InfluxDB line protocol
 
<todo>
