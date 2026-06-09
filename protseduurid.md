## SQL protseduur
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
```sql
--protseduur, mis kuvab kategooriad sisestatud tähe jargi
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

### Protseduur mis lisab andmeid tabelisse ja kohe kuvab neid
```sql
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
exec otsing1taht 'A';
```
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


--OUTPUT parameetr
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










