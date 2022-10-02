# IoT_MQTT_Pi_ESP8266_MQ2

## Dijagram projekta

![This is a alt text.](/Slike/Screenshot_8.png "Dijagram")

## Opis projekta

### Raspberry Pi kao MQTT Broker

Pomocu Dockera lako je instalirati image svih potrebnih komponenti

Mosquitto omogucava RPi-u funkcionalnost MQTT Brokera

Telegraf je pretplacen kao klijent na odredjeni MQTT Topic i omogucava RPi-u da sve podatke koji stignu na taj odredjeni topic zapise u InfluxDB

Grafana se moze iskoristiti za vizualizaciju podataka u InfluxDB bazi

eKuiper kao stream procesing engine moze se iskoristiti za procesiranje i rad nad podacima kako bi se rasteretio cloud od veoma intezivnih radnji procesiranja

Jednostavno pokretanje eKuipera preko Dockera

```
docker run -p 9081:9081 -d --name kuiper -e MQTT_SOURCE__DEFAULT__SERVER="tcp://broker.emqx.io:1883" lfedge/ekuiper:$tag
 
 -- In host
# docker exec -it kuiper /bin/sh

-- In docker instance
# bin/kuiper create stream demo '(temperature float, humidity bigint) WITH (FORMAT="JSON", DATASOURCE="devices/+/messages")'
Connecting to 127.0.0.1:20498...
Stream demo is created.

# bin/kuiper query
Connecting to 127.0.0.1:20498...
kuiper > select * from demo where temperature > 30;
Query was submit successfully.

kuiper > select * from demo WHERE temperature > 30;
[{"temperature": 40, "humidity" : 20}]
```

### ESP8266 kao klijent sa MQ2 senzorom

ESP8266 se povezuje na mrezu preko WiFi-a i publishuje podatke koje konstantno ocitava sa MQ2 senzora na predefinisani topic

MQ2 senzor je rucno kalibrisan pomocu poneciomera koji se nalazi na njemu ali kalibraciju je moguce odratiti i softverski


### Android aplikacija

Android aplikacija sadrzi mogucnosti i subscribe i publish na odredjeni topic u nasem primeru koristimo samo subscribe funkcionalnost i osluskujemo odredjeni topic na odredjenom MQTT Brokeru.

Kada aplikacija primi poruku na tom topicu od MQTT Brokera prikazuje se i informacija korisniku u vidu SnackBar-a i Notifikacije

![This is a alt text.](/Slike/app.jpg "Aplikacija")




