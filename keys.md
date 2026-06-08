# Andmebaasi võtmed (Keys)

## 1. Primary Key
Rea peamine unikaalne identifikaator. Seda kasutatakse tabelite linkimiseks ja duplikaatide vältimiseks. Tabeli kohta saab olla ainult üks selline võti ja see keelab rangelt NULL-väärtused.
```sql
create table opilased(
id INT PRIMARY KEY, 
nimi VARCHAR(50));
```
<img width="283" height="92" alt="image" src="https://github.com/user-attachments/assets/2c0c60e5-8129-4ec6-b5da-c3b539257b78" />


## 2. Foreign Key
See on veerg, mis viitab teise tabeli primaarvõtmele. Seda kasutatakse tabelite vaheliste seoste loomiseks.
```sql
