# Andmebaasi võtmed (Keys)

## 1. Primary Key
Rea peamine unikaalne identifikaator. Seda kasutatakse tabelite linkimiseks ja duplikaatide vältimiseks. Tabeli kohta saab olla ainult üks selline võti ja see keelab rangelt NULL-väärtused.
```sql
create table opilased(
opilase_id INT PRIMARY KEY, 
nimi VARCHAR(50));
```
<img width="283" height="92" alt="image" src="https://github.com/user-attachments/assets/2c0c60e5-8129-4ec6-b5da-c3b539257b78" />
"opilase_id" on peavõti.
Primaarvõti opilase_id identifitseerib õpilase üheselt.

## 2. Foreign Key
See on veerg, mis viitab teise tabeli primaarvõtmele. Seda kasutatakse tabelite vaheliste seoste loomiseks.
```sql
create table arved(
arve_id int primary key,
summa int,
opilase_id int,
foreign key(opilase_id) references opilased(opilase_id));
```
<img width="282" height="121" alt="image" src="https://github.com/user-attachments/assets/4517e6a6-4367-44cf-8f2f-1174e7af6b8c" />
Võõrvõti osakond_id seob töötaja osakonnaga.

## 3. Unique Key
See on piirang, mis hoiab ära veerus duplikaatväärtuste esinemise.
```sql
create table auto(
auto_id int identity(1,1) primary key,
number varchar(50) unique);
```
<img width="323" height="133" alt="image" src="https://github.com/user-attachments/assets/0db5c956-6c96-4337-9432-7295185dcf55" />
Unikaalne võti klassi_number välistab identsed kontorinumbrid.

## 4. Simple Key
See on mis tahes võti (primary, foreign, unique), mis koosneb täpselt ühest veerust.
```sql
create table opetajad(
opetaja_id int primary key, 
eesnimi varchar(50));
```
<img width="347" height="136" alt="image" src="https://github.com/user-attachments/assets/803db29a-e191-4111-aada-4fa920c9cedf" />
Lihtne võti, opetaja_id, garanteerib unikaalsuse ühe veeruga.

## 5. Composite Key
See on võti, mis koosneb kahest või enamast veerust, mis ainult koos pakuvad unikaalsust.
```sql
create table tunnid(
opilase_id int,
opetaja_id int,
kuupaev date,
primary key (opilase_id, opetaja_id, kuupaev)
);
```
<img width="340" height="161" alt="image" src="https://github.com/user-attachments/assets/319cdd85-ec8a-4100-a621-4f18bcc7defa" />
Liitvõti (opilase_id, õpetaja_id, kuupaev) välistab õppetunni samaaegse kordamise.

## 6. Compound Key
See on liitvõti, milles iga veerg peab olema Foreign Key. Seda kasutatakse palju-mitmele-seoste tabelites. Kõik selle elemendid viitavad rangelt teistele tabelitele.
```sql
create table grupid(
opilase_id int,
opetaja_id int,
primary key (opilase_id, opetaja_id));
```
<img width="323" height="138" alt="image" src="https://github.com/user-attachments/assets/577068ef-07f0-4d19-8c21-24e0bd1b93ed" />
Kombineeritud võti (opilase_id, opetaja_id) seob tabeli kahe välisvõtme kaudu.

## 7. Superkey
See on mis tahes veergude kogum, mis identifitseerib tabeli rea unikaalselt. Seda kasutatakse unikaalsete väljade esmaseks otsinguks.
```sql
create table klass(
klass_id int identity(1,1) primary key,
klassi_number varchar(20),
kohtade_arv int);
```
<img width="317" height="132" alt="image" src="https://github.com/user-attachments/assets/b142b789-3b1c-4b0d-9425-a80182c16779" />
Super võti (kursus_id, kursuse_kood) tuvastab rea protsessite andmetega (ülivõti).

## 8. Candidate Key
See on minimaalne supervõti ilma üleliigsete väljadeta. Seda kasutatakse peavõtme kandidaadina.
```sql
create table filiaal(
filiaal_id int identity(1,1) primary key,
filiaali_kood varchar(50) unique,
litsentsi_number varchar(100) unique);
```
<img width="347" height="206" alt="image" src="https://github.com/user-attachments/assets/13049655-0227-48dd-a720-7fb1f818c898" />
Kandidaadi võti filiaali_kood ja litsentsi_number sobivad peaosatäitja rolli võrdselt.

## 9. Alternate Key
See on kandidaatvõti, millest pole veel primaarvõtit saanud. Seda kasutatakse andmete unikaalsuse täiendavaks kontrollimiseks.
```sql
create table pilet(
pileti_id int identity(1,1) primary key,
pileti_kood varchar(50) unique);
```
<img width="337" height="162" alt="image" src="https://github.com/user-attachments/assets/5e73f457-07b3-4810-a3e4-46a2b6b73352" />
Alternatiivne võti pileti_kood jäi pärast peavõtme valimist varuvõtmeks.

