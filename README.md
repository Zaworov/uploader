## REQUIREMENTS: 

 - [x] Make possible to upload more files parallelly 
 - [ ] Each file has name that HTTP header contains, ex. "XUpload-File: \[file.zip]"
 - [ ] POST request: /api/v1/upload HTTP/1.1
 - [ ] Host to be: localhost:9090 
 - [ ] Answear to post query: X-Upload-File: test.zip Content-Length: 1024
 - [ ] Each upload process should calculate number of uploaded files
 - [ ] Upload details are available via REST api (GET /api/v1/upload/progress HTTP/1.1 Host: localhost:9090). 
 Finished upload isn't available there. Data is available in json format (see below).
 - [ ] Each upload should count time in ms. This data is available and saved during app working time, available on (GET /api/v1/upload/duration Host: localhost:9090), available in format: upload_duration{id=”test.zip-”} 1567.0
 - [ ] Max number of threads is 8
 - [ ] Servis should serve 100 parallel uploads of files up to 50 MB 
 - [ ] Write a test to ensure above requirement
 - [ ] server timeout podesiti u skladu s prethodnim zahtjevom (not sure...?)
 - [ ] at the same time app can't upload more files with the same name, but same name file loaded previously can be replaced with new one

 ## ORIGINAL SPECIFICATION:
 ### Vacation CalendarFile Upload Service

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

 ### Zahtjevi:
- Spring Boot, JDK/JRE 1.8+, Maven
- Rest API implementirati pomoću spring rest controllera
- server smije koristiti maksimalno 8 tredova za obradu zahtjeva
- servis mora podržavati 100 paralelnih uploada u veličini od 50 MB (napisati integracijski test koji to potvrđuje)
- server timeout podesiti u skladu s prethodnim zahtjevom
- paralelno se NE može uploadati više datoteka s istim imenom dok sekvencijalno se može s time da se prethodni file s istim nazivom pregazi
