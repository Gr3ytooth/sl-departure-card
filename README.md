**OBS! Utvecklad för mitt eget behov och kanske inte fungerar exakt till det du önskar**

Bygger på HASL Departure Card (https://github.com/hasl-sensor/lovelace-hasl-departure-card) men är anpassat efter Trafiklabs SL Transport API (https://www.trafiklab.se/sv/api/trafiklab-apis/sl/transport/)

För att hämta data till HA så behöver man skapa en REST-sensor. 

Exempel på configuration.yaml för Stockholm C (id 1080).
```
rest:
  - resource: https://transport.integration.sl.se/v1/sites/1080/departures?forecast=120
    method: GET
    sensor:
      - name: rest_sl_stockholm_c
        force_update: true
        value_template: "OK"
        json_attributes:
          - "departures"
```

Hämta stations-id här:
https://transport.integration.sl.se/v1/sites?expand=true




Installation av kortet görs genom att lägga sl-departure-card.js i mappen [config]/www eller lämplig undermapp och sedan ange den mappen under resources i HA. [config]/www brukar definieras som /local och [config]/www/community som /hacsfiles .

Därefter behöver kortet läggas till i en vy manuellt.
Enklast är kanske att kopiera in koden nedan och redigera den till din setup
```
type: custom:sl-departure-card
header: true
show_cardname: true
name: Stockholm C
departures: true
deviations: true
max_deviations: 3
show_trains: true
show_trams: true
show_buses: true
show_metros: true
show_lines:
  - "40"
  - "18"
  - "744"
  - "10"
  - "19"
  - "13"
direction: 0
number_to_show: 20
language: sv-SE
entities:
  - sensor.rest_sl_stockholm_c
```



