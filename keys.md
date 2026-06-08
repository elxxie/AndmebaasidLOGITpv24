# Andmebaasi võtmed (Keys)

## 1. Primary Key
Rea peamine unikaalne identifikaator. Seda kasutatakse tabelite linkimiseks ja duplikaatide vältimiseks. Tabeli kohta saab olla ainult üks selline võti ja see keelab rangelt NULL-väärtused.
```sql
create table opilased(
id INT PRIMARY KEY, 
nimi VARCHAR(50));
```
