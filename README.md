**OBS! Utvecklad för mitt eget behov och kanske inte fungerar exakt till det du önskar**

![Screenshot 2024-12-03 222320](https://github.com/user-attachments/assets/2c279108-b2d1-45c2-a18a-ef5b58086824)


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
          - "stop_deviations"
```

Hämta stations-id här:
https://transport.integration.sl.se/v1/sites?expand=true




Installation av kortet görs genom att lägga sl-departure-card.js i mappen [config]/www eller lämplig undermapp och sedan ange den mappen under resources i HA. [config]/www brukar definieras som /local och [config]/www/community som /hacsfiles . Eventuellt kan browserns cache och/eller data behöva rensas för att filen ska laddas in som resource.

Därefter behöver kortet läggas till i en vy manuellt.
Enklast är kanske att kopiera in koden nedan och redigera den till din setup
```
type: custom:sl-departure-card
show_cardname: true
name: Stockholm C
header: true
show_departures: true
show_cancelled: true
number_of_departures_to_show: 10
show_departure_deviations: true
number_of_departure_deviations_lines: 3
show_site_deviations_: true
number_of_site_deviations: 6
show_trains: true
show_trams: true
show_buses: true
show_metros: true
show_lines:
  - "14"
  - "18"
  - "40"
  - "7"
  - "69"
direction: 0
language: sv-SE
show_last_updated: true
entities:
  - sensor.rest_sl_stockholm_c
```


Parametrar som kan justeras:
|Parameter|Default|Förklaring|
|---|---|---|
|show_cardname|false|Visar rubrik på kortet|
|name|*namn på sensorn som används*|Det som skrivs ut på rubriken i kortet|
|header|false|Om rubriker ska visas för avgångar|
|show_departures|false|Om avgångar ska visas|
|show_cancelled|false|Om inställda avgångar ska visas|
|number_of_departures_to_show|Alla|Hur många avgångar som ska visas|
|show_departure_deviations|false|Om avvikelser för enskilda avgångar ska visas|
|number_of_departure_deviations_lines|Alla|Hur många rader avvikelser för enskilda avgångar som ska visas|
|show_site_deviations|false|Om avvikelser för stationen/platsen ska visas|
|number_of_site_deviations|Alla|Hur många avvikelser för stationen/platsen som ska visas|
|show_trains|false|Om avgångar av trafiktypen tåg ska visas|
|show_trams|false|Om avgångar av trafiktypen spårvagn ska visas|
|show_buses|false|Om avgångar av trafiktypen buss ska visas|
|show_metros|false|Om avgångar av trafiktypen tunnelbana ska visas|
|show_lines|Alla|Ger möjlighet att ange specifikt vilka linjer som avgångar ska visas för. OBS! Måste anges som en lista|
|direction|Alla|Vilken riktning avgångar ska visas för. Alternativen är 0 = Alla, 1 eller 2|
|language|Svenska|Anger språk för rubriker och infotext. Alternativen är sv-SE = Svenska, en-EN = Engelska. OBS! Texten från REST-sensorn är alltid på svenska|
|show_last_updated|false|Om datum/tid när sensorn senast uppdaterades ska visas|
|entities||Vilken REST-sensor som ska användas| 
  


