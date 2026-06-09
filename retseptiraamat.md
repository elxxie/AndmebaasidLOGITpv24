
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
<img width="203" height="252" alt="{D5B8E040-0375-44AB-8AB6-E8F6F11DDBA9}" src="https://github.com/user-attachments/assets/9416c2d0-60a4-4f9e-8eeb-2a9b921bd9ec" />

## Protseduur lisamine kasutaja
```sql
CREATE Procedure lisaKasutaja
@eesnimi varchar(50),
@perenimi varchar(50),
@email varchar(50)
AS
BEGIN
	INSERT INTO kasutaja
	VALUES (@eesnimi,@perenimi,@email);
	SELECT * FROM kasutaja;
END;
```
<img width="484" height="258" alt="{864F48E7-57A2-4715-8F2F-679AFA737D70}" src="https://github.com/user-attachments/assets/28d0806d-733a-4e89-9f94-744950662839" />

## Protseduur lisamine kategooria
```sql
CREATE Procedure lisaKategooria
@nimi varchar(30)
AS
BEGIN
	INSERT INTO kategooria(kategooria_nimi)
	VALUES (@nimi);
	SELECT * FROM kategooria;
END;
```
<img width="300" height="225" alt="{97CE98E0-03F5-4124-9A12-D496110A4AD2}" src="https://github.com/user-attachments/assets/52c8954a-95f3-4aaf-b3cc-4500ecacc73d" />

## Protseduur lisamine toiduaine
```sql
CREATE Procedure lisaToiduaine
@nimi varchar(100)
AS
BEGIN
	INSERT INTO toiduaine(toiduaine_nimi)
	VALUES (@nimi);
	SELECT * FROM toiduaine;
END;
```
<img width="246" height="272" alt="{9356EC8D-7D97-4992-A19F-0C1517E45CCA}" src="https://github.com/user-attachments/assets/bafefa12-22da-4d39-a7bd-343bcbeca671" />

## Protseduur lisamine yhik
```sql
CREATE Procedure lisaYhik
@nimi varchar(100)
AS
BEGIN
	INSERT INTO yhik(yhik_nimi)
	VALUES (@nimi);
	SELECT * FROM yhik;
END;
```
<img width="254" height="246" alt="{F69E4BC0-E336-4353-892F-B564618A515D}" src="https://github.com/user-attachments/assets/22806134-8dad-4aa7-b797-76bd92d2755b" />

## Protseduur lisamine retsept
```sql
CREATE PROCEDURE lisaRetsept
@nimi VARCHAR(100),
@kirjeldus VARCHAR(200),
@juhend VARCHAR(500),
@kp DATE,
@kasutaja INT,
@kategooria INT
AS
BEGIN
	INSERT INTO retsept(retsepti_nimi,kirjeldus,juhend,sisestatud_kp,kasutaja_id,kategooria_id)
	VALUES(@nimi,@kirjeldus,@juhend,@kp,@kasutaja,@kategooria);
	SELECT * FROM retsept;
END;
```
<img width="800" height="155" alt="{3AD549B3-FFAE-4299-AEC8-FB479BB02C96}" src="https://github.com/user-attachments/assets/246db47c-af83-4c93-96dc-e19c7345ff9a" />

## Protseduur lisamine koostised
```sql
CREATE PROCEDURE lisaKoostis
@kogus INT,
@retsept INT,
@toiduaine INT,
@yhik INT
AS
BEGIN
	INSERT INTO koostis(kogus,retsept_retsept_id,toiduaine_id,yhik_id)
	VALUES(@kogus,@retsept,@toiduaine,@yhik);
END;
```
<img width="379" height="195" alt="{49D2BDFF-8D84-4C8B-819B-891B535D30BE}" src="https://github.com/user-attachments/assets/68fa8e8f-cac8-4b2f-a13d-2b96b4bb1d0e" />

## Protseduur lisamine tehtud
```sql
CREATE PROCEDURE lisaTehtud
@kuupaev DATE,
@retsept INT
AS
BEGIN
	INSERT INTO tehtud(tehtud_kp,retsept_id)
	VALUES(@kuupaev,@retsept);
	SELECT * FROM tehtud;
END;
```
<img width="351" height="166" alt="{F6799181-C593-4D5F-B795-488D81A2F0BF}" src="https://github.com/user-attachments/assets/8d1a9ce6-8bdb-4d8e-b5d7-70fd9d078c07" />

## Protseduur muutmine tabelid
```sql
CREATE PROCEDURE muudaTabel
	@tegevus VARCHAR(10),
	@tabelinimi VARCHAR(50),
	@veerunimi VARCHAR(50),
	@tyyp VARCHAR(50)=NULL
AS
BEGIN
	DECLARE @sqltegevus VARCHAR(MAX)

	SET @sqltegevus = CASE
		WHEN @tegevus='add' THEN
			CONCAT ('ALTER TABLE ', @tabelinimi, ' ADD ', @veerunimi, ' ', @tyyp)

		WHEN @tegevus='drop' THEN
			CONCAT('ALTER TABLE ', @tabelinimi, ' DROP COLUMN ', @veerunimi)

		WHEN @tegevus='alter' THEN
			CONCAT('ALTER TABLE ', @tabelinimi, ' ALTER COLUMN ', @veerunimi, ' ', @tyyp)

		END;

	PRINT @sqltegevus;
	EXEC(@sqltegevus);

END;
```
<img width="541" height="231" alt="{92302230-84BC-40CA-A598-51EDD2E1A597}" src="https://github.com/user-attachments/assets/dc8c4be4-8510-4885-aa9d-c976a06e511e" />

## Select
### 1. Päring kuvab kasutaja eesnime, perekonnanime ja tema retseptide nimetused.
```sql
SELECT kasutaja.eesnimi, kasutaja.perenimi, retsept.retsepti_nimi FROM kasutaja, retsept
WHERE kasutaja.kasutaja_id=retsept.kasutaja_id;
```
<img width="764" height="171" alt="{5D660DD7-48B3-4C48-B559-649FFAE9C7F5}" src="https://github.com/user-attachments/assets/8e1d328e-99d2-4db2-ad2e-4dec8b4b9f23" />

### 2. Päring kuvab retsepti nimetuse ja sellele vastava kategooria.
```sql
SELECT retsept.retsepti_nimi,kategooria.kategooria_nimi FROM retsept, kategooria
WHERE retsept.kategooria_id=kategooria.kategooria_id;
```
<img width="702" height="184" alt="{04DF4058-FA01-4B58-A30E-D96C7EB939DA}" src="https://github.com/user-attachments/assets/9ad9d388-e4b4-401c-9ef5-b15eec8ae3a0" />

### 3. Päring kuvab koostises kasutatud toiduained ja nende kogused.
<img width="621" height="209" alt="{8C37DADC-B81D-45F0-8550-57BA3C285F51}" src="https://github.com/user-attachments/assets/95a10b07-ce87-44c2-9ecb-5b63329a1a61" />

## Too lisamine
### kommentaar table
```sql
CREATE TABLE kommentaar(
kommentaar_id INT IDENTITY(1,1) PRIMARY KEY,
tekst VARCHAR(200),
retsept_id INT,
FOREIGN KEY(retsept_id) REFERENCES retsept(retsept_id));
```
### Protseduur lisamine
```sql
CREATE PROCEDURE lisaKommentaar
@tekst VARCHAR(200),
@retsept INT
AS
BEGIN
	INSERT INTO kommentaar
	VALUES(@tekst,@retsept);
	SELECT * FROM kommentaar;
END;
```
<img width="340" height="212" alt="{60B4941E-D449-477F-B000-5381858ECE64}" src="https://github.com/user-attachments/assets/c90c0e6c-ef23-4854-8624-1535330824fc" />

### Protseduur kustutamine
```sql
CREATE PROCEDURE kustutaKommentaar
@id INT
AS
BEGIN
	DELETE FROM kommentaar
	WHERE kommentaar_id=@id;
	SELECT * FROM kommentaar;
END;
```
<img width="268" height="204" alt="{79A57107-F8A0-4CE0-97B2-C02CA7E7497D}" src="https://github.com/user-attachments/assets/7384cfa3-3c66-42c1-8993-7f546f29e376" />

## Kasutaja Staff õigused
### 1. Omab ligipääsu tabelitele: toiduaine, kategooria, kasutaja
```sql
GRANT SELECT ON kasutaja TO staff;
GRANT SELECT ON toiduaine TO staff;
GRANT SELECT ON kategooria TO staff;
```

### 2. Tohib lisada ja vaadata toiduaineid ja kategooriaid
```sql
GRANT INSERT ON toiduaine TO staff;
GRANT INSERT ON kategooria TO staff;
```

### 3. Ei tohi muuta ega kustutada toiduaineid ja kategooriaid
```sql
DENY UPDATE, DELETE ON toiduaine TO staff;
DENY UPDATE, DELETE ON kategooria TO staff;
```

### 4. Tabelis kasutaja on lubatud ainult vaatamine
```sql
DENY INSERT, UPDATE, DELETE ON kasutaja TO staff;
```

## Kasutaja Manager õigused
## 1. Omab ligipääsu kõigile tabelitele
```sql
GRANT SELECT ON kasutaja TO manager;
GRANT SELECT ON toiduaine TO manager;
GRANT SELECT ON kategooria TO manager;
GRANT SELECT ON yhik TO manager;
GRANT SELECT ON tehtud TO manager;
GRANT SELECT ON kommentaar TO manager;
```

### 2. Ei tohi lisada uusi toiduaineid (toiduaine) ega uusi kasutajaid (kasutaja)
```sql
DENY INSERT ON kasutaja TO manager;
DENY INSERT ON toiduaine TO manager;
```

### 3. Omab täielikku haldusõigust retseptidega seotud tabelites (retsept ja koostis)
```sql
GRANT SELECT, INSERT, UPDATE, DELETE ON retsept TO manager;
GRANT SELECT, INSERT, UPDATE, DELETE ON koostis TO manager;
```


