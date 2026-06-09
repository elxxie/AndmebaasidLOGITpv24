
## 1. Taabelid
```sql
--Taabel kasutaja
create table kasutaja(
kasutaja_id int primary key identity (1,1),
eesnimi varchar(50),
perenimi varchar(50),
email varchar(50));

--Taabel kategooria
create table kategooria(
kategooria_id int primary key identity(1,1),
kategooria_nimi varchar(50));

--Taabel retsept
create table retsept(
retsept_id int primary key identity(1,1),
retsepti_nimi varchar(100),
kirjeldus varchar(200),
juhend varchar(500),
sisestatud_kp date,
kasutaja_id int,
foreign key(kasutaja_id) references kasutaja(kasutaja_id),
kategooria_id int,
foreign key(kategooria_id) references kategooria(kategooria_id));

--Taabel tehtud
create table tehtud(
tehtud_id int primary key identity(1,1),
tehtud_kp date,
retsept_id int,
foreign key(retsept_id) references  retsept(retsept_id));

--Taabel toiduaine
create table toiduaine(
toiduaine_id int primary key identity(1,1),
toiduaine_nimi varchar(100));

--Taabel yhik
create table yhik(
yhik_id int primary key identity(1,1),
yhik_nimi varchar(100));

--Taabel koostis
create table koostis(
koostis_id int primary key identity(1,1),
kogus int,
retsept_retsept_id int,
foreign key(retsept_retsept_id) references retsept(retsept_id),
toiduaine_id int,
foreign key(toiduaine_id) references toiduaine(toiduaine_id),
yhik_id int,
foreign key(yhik_id) references yhik(yhik_id));
```



