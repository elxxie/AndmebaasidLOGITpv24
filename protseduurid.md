## SQL protseduur

[Põhimõisted](README.md) | [Kasutaja](kasutaja.md) | [ Trigerid](triger.md) | [Protseduurid](protseduurid.md) | [Sales](sales.md) | [Võtmed/Keys](keys.md) | [Foorum ülesanne](foorum.md) | [Retseptiraamat](retseptiraamat.md)

### store procedure - salvestatud protseduur - sama mis on funktsioonid programmerimises, mingi tegevus, mis on salvestatud andmebaasi, ja mida saab automaatsel teha (INSERT, SELECT, UPDATE).

### 1. categories
```sql
create table categories(
category_id int primary key identity(1,1),
category_name varchar(25) unique);

insert into categories(category_name)

select * from categories;

### Protseduur mis lisab andmeid tabelisse ja kohe kuvab neid
create procedure lisaKategooria
@uusKategooria varchar(36)
as
begin
--kirjeldus
	Insert into categories(category_name)
	values (@uusKategooria);
	select * from categories;
end

--kutse
exec lisaKategooria 'Auto'
```
<img width="263" height="243" alt="{6EC6A1A6-60B1-4E27-BE17-A128CB295738}" src="https://github.com/user-attachments/assets/bff07394-db02-4e01-9819-0062dd275a21" />

### Protseduur, mis kustutab kategooria id
```sql
create procedure kustutaKategooria
@kustutaKategooria int
as
begin
	select * from categories;
	delete from categories where category_id=@kustutaKategooria;
	select * from categories;
end

--kutse
exec kustutaKategooria 1
```
<img width="297" height="369" alt="{C8893EA7-AA3A-4C56-A3CD-A90DEAD41880}" src="https://github.com/user-attachments/assets/abadecc5-6bca-4c70-bc9c-29e56fe72bdd" />

### Protseduur, mis kuvab kategooriad sisestatud tähe jargi
```sql
create procedure otsing1taht
@taht char(1)
as
begin
	Select * from categories
	where category_name like @taht + '%'; --% - teised sümbolid
end

--kutse
exec otsing1taht 'Auto2';
```
<img width="222" height="82" alt="{01B063A0-C547-4DD6-A829-FDE8F8842ED0}" src="https://github.com/user-attachments/assets/7e27c28f-97c6-4779-a6ab-7470ed01571d" />

### Brands
```sql
CREATE TABLE brands(
brand_id int PRIMARY KEY identity(1,1),
brand_name varchar(15) UNIQUE);

INSERT INTO brands(brand_name)
VALUES ('Samsung');
```
### Products
```sql
Create TABLE products(
product_id int PRIMARY KEY identity(1,1),
product_name varchar(50) not null,
brand_id int,
FOREIGN KEY (brand_id) references  brands(brand_id),
category_id int,
FOREIGN KEY (category_id) references categories(category_id),
model_year int,
list_price money);

select * from products;

insert into products
values ('Iphone 13', 1, 2, 2023, 400);
```

### Protseduur, mis kuvab tooded, kus on hind suurem kui sisestatud hind
```sql
create procedure suuremHind
@hind int
as
begin
	select * from products
	where list_price > @hind;
end;

--kutse
exec suuremHind 1000;
```
<img width="475" height="178" alt="{53FBC0F2-62F8-43AB-8089-C7A041F86FFF}" src="https://github.com/user-attachments/assets/21445098-ade5-47e3-bd8c-9fb130cb37a2" />

### OUTPUT parameetr
```sql
CREATE PROCEDURE minmaxHind
    @minHind MONEY OUTPUT,
    @maxHind MONEY OUTPUT
AS
BEGIN
    SELECT 
        @minHind = MIN(list_price),
        @maxHind = MAX(list_price)
    FROM products;
END;

--kutse
DECLARE @minHind MONEY, @maxHind MONEY;

EXEC minmaxHind @minHind OUTPUT, @maxHind OUTPUT;

PRINT 'Min hind = ' + CONVERT(varchar, @minHind);
PRINT 'Max hind = ' + CONVERT(varchar, @maxHind);
```

<img width="472" height="270" alt="{097ED3CC-4598-4062-983F-20F5A63914DF}" src="https://github.com/user-attachments/assets/ba9d186d-f959-4b9d-b713-727823189225" />


### 6. Dünaamiline SQL protseduuris (ALTER TABLE)
```sql
CREATE PROCEDURE muudatus
    @tegevus varchar(10),
    @tabelinimi varchar(25),
    @veerunimi varchar(25),
    @tyyp varchar(25) = NULL
AS
BEGIN
    DECLARE @sqltegevus varchar(max);

    SET @sqltegevus = CASE 
        WHEN @tegevus = 'add' THEN 
            CONCAT('ALTER TABLE ', @tabelinimi, ' ADD ', @veerunimi, ' ', @tyyp)

        WHEN @tegevus = 'drop' THEN 
            CONCAT('ALTER TABLE ', @tabelinimi, ' DROP COLUMN ', @veerunimi)
    END;

    PRINT @sqltegevus;
    EXEC (@sqltegevus);
END;

exec muudatus 'add', 'categories', 'testVeerg', 'int'

select * from categories

exec muudatus 'drop', 'categories', 'testVeerg'
```
<img width="508" height="377" alt="{CBE7F0FB-98AB-47AE-A9A1-A8C4F7E3095A}" src="https://github.com/user-attachments/assets/b2193315-c9c0-4ece-aea5-1143d3126f14" />
<img width="521" height="287" alt="{7A7284C1-0598-4A89-AEAF-7BC1D82BAB41}" src="https://github.com/user-attachments/assets/5faa768c-c52f-47ba-a2e3-b5ca7e1bdb2f" />










