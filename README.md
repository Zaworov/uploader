##REQUIREMENTS: 
[x] 1
[ ] 2
[ ] 3

##ORIGINAL SPECIFICATION:
###Vacation CalendarFile Upload Service
Zadatak: Potrebno je implementirati HTTP servis za upload proizvoljnih datoteka. Servis omogućava paralelan upload više datoteka. Svaka datoteka ima naziv koji se šalje u HTTP zaglavlju XUpload- File.

Opis zadatka:

POST /api/v1/upload HTTP/1.1
Host: localhost:9090
X-Upload-File: test.zip
Content-Length: 1024

//<bytes >

Za svaki od uploada potrebno je pratiti količinu prenesenih podataka. Podaci su dostupni preko
REST API-ja. Upload koji je završio ne prikazuje se u rezultatima.

GET /api/v1/upload/progress HTTP/1.1
Host: localhost:9090
```
{
	”uploads”: [
		{
				”id”: ”test.zip-<timestamp>”,
				”size”: 1024,
				”uploaded”: 534
		}
	]
}
```

Za svaki od uploada potrebno je mjeriti trajanje (ms). Podaci o trajanju uploada dostupni su
preko API-ja. Trajanje se ispisuje za sve uploade koji su se izvršili za vrijeme izvođenja servisa
(metrike je dovoljno čuvati u memoriji).

GET /api/v1/upload/duration
Host: localhost:9090

upload_duration{id=”test.zip-<timestamp>”} 1567.0

Zahtjevi:
- Spring Boot, JDK/JRE 1.8+, Maven
- Rest API implementirati pomoću spring rest controllera
- server smije koristiti maksimalno 8 tredova za obradu zahtjeva
- servis mora podržavati 100 paralelnih uploada u veličini od 50 MB (napisati integracijski test koji to potvrđuje)
- server timeout podesiti u skladu s prethodnim zahtjevom
- paralelno se NE može uploadati više datoteka s istim imenom dok sekvencijalno se može s time da se prethodni file s istim nazivom pregazi
