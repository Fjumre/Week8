# Week8

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
