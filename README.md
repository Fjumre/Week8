# Week8
Ved første gennemgang blev det tydeligt, at databasen og konfigurationen ikke var tilstrækkeligt sikret:
Alle kunne oprette, ændre, hente og slette data direkte i databasen – der var ingen login eller adgangskontrol.
config.py og etl_db_setup.py indeholdt hardcodede brugernavne og passwords.
docker-compose.yaml eksponerede brugernavn og adgangskode i klartekst og gjorde databasen offentligt tilgængelig via port 3306.
Data (fx fornavne, adresser, e-mails) kunne ses direkte i koden, hvilket er uacceptabelt, hvis repositoriet er offentligt.

I stedet for at gemme brugernavn og adgangskode direkte i docker-compose.yaml, skal de ligge i en separat secret-fil.

Eksempel:
mkdir secrets
echo "password" > secrets/mysql_root_password.txt

Og i docker-compose.yaml:
services:
  mysql:
    image: mysql:8.0
    container_name: test_integrated_db
    environment:
      MYSQL_ROOT_PASSWORD_FILE: /run/secrets/mysql_root_password
    secrets:
      - mysql_root_password

secrets:
  mysql_root_password:
    file: ./secrets/mysql_root_password.txt

Denne secret-fil bør aldrig uploades til GitHub.

Begræns eksponering af databasen
For at undgå angreb (f.eks. SQL Injection via åbne porte):
Fjern linjen:
ports:
  - "3306:3306"

Bind i stedet phpMyAdmin til localhost:
ports:
  - "127.0.0.1:8080:80"

Lad MySQL kun være tilgængelig internt i Docker-netværket.

Fjern data fra koden
Undgå at gemme personfølsomme data (PII) i koden eller GitHub.
Hvis data skal bruges til test, anvend syntetiske/fake data (f.eks. med Faker biblioteket).
Hvis du absolut skal have dem i repoet, skal de placeres i mapper, som er ignoreret af Git.


Problem	Løsning
Hardcodede credentials	Brug .env eller Docker secrets
Eksponeret databaseport	Fjern 3306:3306, brug interne netværk
Følsomme filer i repo	Tilføj secrets/ og .env til .gitignore
Ingen adgangskontrol	Brug MySQL-login og least-privilege-brugere
Persondata i kode	Fjern eller erstattet med fake-data


Ved at bruge Docker korrekt, .env-filer og secrets kan vi:
Fjerne credentials fra kodebasen
Minimere risikoen for SQL-injektion
Isolere skader til enkelte containere
Gøre koden sikker at dele på GitHub uden følsomme data
