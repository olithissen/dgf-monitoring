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

* todo ...

## Manuelle Datenlieferung über InfluxDB line protocol

Falls du eine andere Möglichkeit hast, um Daten aus `ping` rauszuholen, kann die Übertragung auch per HTTP POST im *InfluxDB line protocol* Format erfolgen:
 
```bash
curl -i --request POST "https://ifx.tonick.net/api/v2/write?org=dgf&bucket=dgf_ping_manual" \ 
    --header 'Authorization: Token <influxtoken>' \
    --data-raw 'ping,lat=<lat>,lon=<lon>,host=<hostname>,url=8.8.8.8,user=<user> average_response_ms=1.0,maximum_response_ms=1.0,minimum_response_ms=1.0,packets_received=1,packets_transmitted=1,percent_packet_loss=0,result_code=0,standard_deviation_ms=1.0,ttl=123'
```

Die Einhaltung des Formats ist wichtig:

```
ping,tag_key=tag_value,... field_key=field_value,...
  |  ---------------------|-------------------------
  |             |         |       |   
  |             |         |       |   
+-----------+-------+-------+---------+
|measurement|tag_set| SPACE |field_set|
+-----------+-------+-------+---------+
```
