```SQL
--1.categories
create table categories(
category_id int PRIMARY KEY identity(1,1),
category_name varchar(25) UNIQUE);

INSERT INTO categories(category_name)
VALUES ('Arvuti');

SELECT * FROM categories;
<img width="207" height="90" alt="{88D7956E-1A00-4745-A7E2-08F921B86F47}" src="https://github.com/user-attachments/assets/bfb45c92-f252-4249-892e-35ec908bcd80" />


```SQL
--2.brands
CREATE TABLE brands(
brand_id int PRIMARY KEY identity(1,1),
brand_name varchar(15) UNIQUE);

INSERT INTO brands(brand_name)
VALUES ('Samsung');

SELECT * FROM brands;
<img width="206" height="97" alt="{8A6BC7D2-E27C-4B26-AAA8-93817B7805D3}" src="https://github.com/user-attachments/assets/36888d02-35d7-44c1-b48d-4548ea4a214d" />


```SQL
--3.products
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

INSERT INTO products
VALUES ('nutitelefon X10', 1, 1, 2025, 600);
<img width="468" height="85" alt="{31564204-5A8B-4CF2-94E0-040BA4C80010}" src="https://github.com/user-attachments/assets/d0534037-8aa0-4d7d-8f7a-e924edf0bc53" />



```SQL
--4.stores
CREATE TABLE stores(
store_id int PRIMARY KEY identity (1,1),
store_name varchar(20) not null,
phone varchar(13),
email varchar(40),
street varchar(20),
city varchar(10),
state varchar(10),
zip_code char(5));

SELECT * FROM stores;

INSERT INTO stores
VALUES ('Kristiine','56475824', 'kristiine@gmail.com', 'Endla', 'Tallinn', 'Harjumaa','13425');
<img width="594" height="110" alt="{DAEE475B-F94D-461E-A633-796866B0BD1C}" src="https://github.com/user-attachments/assets/eb1e5ea7-33f3-45ae-bd04-6e9f57e74f14" />



```SQL
--6.customers
CREATE TABLE customers(
customer_id int PRIMARY KEY identity (1,1),
first_name varchar(20) not null,
last_name varchar(20) not null,
phone varchar(10),
email varchar(30),
street varchar(30),
city varchar(15) check (city='Tallinn' or city='Narva'),
state varchar(15),
zip_code char(5));

SELECT * FROM customers;

INSERT INTO customers
VALUES ('Oleg','Uustal','56574857','uustal@gmail.com', 'Läänemere tee','Narva','Ida-Virumaa','13457');
<img width="619" height="84" alt="{259BB671-A86B-4C5A-BD2C-111F7356D0F8}" src="https://github.com/user-attachments/assets/53a977db-28c2-4ea2-9a1e-0362d00aa9c5" />



```SQL
--7.staffs
CREATE TABLE staffs(
staff_id int PRIMARY KEY identity (1,1),
first_name varchar(20) not null,
last_name varchar(20) not null,
phone varchar(10),
active bit,
store_id int,
FOREIGN KEY (store_id) references stores(store_id),
manager bit);

SELECT * FROM staffs;

INSERT INTO staffs
VALUES ('Kirill','Leht', '56245878', 0,1,0);
<img width="522" height="106" alt="{5FB1FDCC-95F2-4F9A-A312-D05122FA95BF}" src="https://github.com/user-attachments/assets/4464c584-fae8-45de-8947-b56c8a6bae23" />



--8.orders
CREATE TABLE orders(
order_id int PRIMARY KEY identity (1,1),
customer_id int,
foreign key (customer_id) references customers(customer_id),
order_status varchar(15) check (order_status='complete' or order_status='incomplete'),
order_date date,
required_date date,
shipped_date date,
store_id int,
FOREIGN KEY (store_id) references stores(store_id),
staff_id int,
FOREIGN KEY (staff_id) references staffs(staff_id));

SELECT * FROM orders;

INSERT INTO orders
VALUES (3, 'incomplete', '2026-04-25','2026-05-18','2026-04-29',2,1);
<img width="589" height="80" alt="{3BC6D0F2-29BA-46E2-8625-F0FB86B3C02D}" src="https://github.com/user-attachments/assets/75a36dd0-882a-411b-8d18-ced9da03c096" />









