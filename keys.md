# Andmebaasi võtmed (Keys)

## 1. Primary Key
Rea peamine unikaalne identifikaator. Seda kasutatakse tabelite linkimiseks ja duplikaatide vältimiseks. Tabeli kohta saab olla ainult üks selline võti ja see keelab rangelt NULL-väärtused.
```sql
create table opilased(
opilase_id INT PRIMARY KEY, 
nimi VARCHAR(50));
```
<img width="283" height="92" alt="image" src="https://github.com/user-attachments/assets/2c0c60e5-8129-4ec6-b5da-c3b539257b78" />

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

## 3. Unique Key
See on piirang, mis hoiab ära veerus duplikaatväärtuste esinemise.
```sql
create table auto(
auto_id int identity(1,1) primary key,
number varchar(50) unique);
```
<img width="320" height="160" alt="image" src="https://github.com/user-attachments/assets/dffc1450-7787-480a-99a4-3fec7b896b27" />

## 4. Simple Key
See on mis tahes võti (primary, foreign, unique), mis koosneb täpselt ühest veerust.
```sql
create table opetajad(
opetaja_id int primary key, 
eesnimi varchar(50));
```
<img width="302" height="101" alt="image" src="https://github.com/user-attachments/assets/fd510cd2-7152-44a6-b365-3c55ff48f2a8" />

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
<img width="317" height="122" alt="image" src="https://github.com/user-attachments/assets/a92312cd-83c8-4779-84fe-ea8c343a6e6f" />




