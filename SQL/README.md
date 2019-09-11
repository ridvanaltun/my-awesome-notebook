# SQL Cheat Sheet

// TODO

# Table of Contents

- [SQL Cheat Sheet](#sql-cheat-sheet)
- [Table of Contents](#table-of-contents)
- [Basics](#basics)
  - [SQL Syntax](#sql-syntax)
  - [Comment Syntax](#comment-syntax)
  - [Data Types](#data-types)
  - [Create Table](#create-table)
  - [Insert](#insert)
  - [Select](#select)
    - [Between](#between)
    - [Like](#like)
    - [Order By](#order-by)
    - [Functions](#functions)
    - [Group By](#group-by)
    - [Having](#having)
  - [Operators](#operators)
  - [In](#in)
- [Merge Tables and Multiple Table Usage](#merge-tables-and-multiple-table-usage)
- [UPDATE, DELETE, ALTER, EXEC](#update-delete-alter-exec)
- [VIEW](#view)
- [WIP](#wip)
- [Technical Terms](#technical-terms)
  - [Primary Key](#primary-key)
  - [UML](#uml)

# Basics

## SQL Syntax

```sql
SELECT --functions--- , --*-- , --parameter-- --as--
FROM --TABLE_name---
WHERE --LIKE-- , --is null--, --is not null--, --AND--, --or--, --between--, --in--
GROUP BY
HAVING --function sart--, --son sart--
ORDER BY --ASC--, --desc--
```

## Comment Syntax

Çoklu satır yorum:

```sql
/*Select all the columns
of all the records
in the Customers table:*/
SELECT *
FROM Customers;
```

Tek satır yorum:

```sql
SELECT *
FROM Customers; -- This is a simple comment
```

## Data Types

```sql
char(n) -- sabit uzunluktaki veriler için, telefon numarasi, plaka vs 
varchar(n) -- diyelim ki isim kaydedecegiz, herkesin ismi farkli boyutta oldugu için bunu kullanmak daha mantikli-alan kaybi yasamayiz
int -- bildigimiz int, tam sayi verileri içinde kaydeder
tinyint -- çok daha küçük int degerler için 0-255 arasi
smallint -- daha küçük int
numeric ve decimal -- ondalik ve tam sayi verileri tutabilir
float -- bildigimiz float
binary -- 0 veya 1 kaydedilebilir
money -- ??
smallmoney -- ??
```

## Create Table

```sql
-- database oluştur
CREATE DATABASE database_name;

-- tablo oluştur
CREATE TABLE table_name(per_no int, name varchar(12), surname varchar(16), tel_no char(12), gender binary, primary key(per_no));

-- tablo oluşturmaya örnek
CREATE TABLE citizen(tc char(11), name varchar(12), surname varchar(16), tel_no char(12), gender binary, primary key(per_no));
```

## Insert

```sql
-- tabloya belirtilen kolonlar ile kayıt gir
INSERT INTO table1(name, surname, id)
VALUES(value1, value2, value3);

-- veri girişine örnek
INSERT INTO citizen
VALUES(12, 'ridvan', 'altun', '905436444849', 0);
```

## Select

```sql
-- yapilan sorgu eger bir kosul belirtilmemisse anahtar sag sirasiyla ekrana düser
SELECT *
FROM citizen
WHERE gender = 1;

-- gender kolonu 1 olan herkes ekrana düser
SELECT count(distinct name)
FROM citizen;

-- ismi benzersiz olanlarin hespi sayilir ekrana düser
SELECT count(name)
FROM citizen
WHERE name = 'ahmet';

-- isim kolonunda ismi ahmet olan kisi sayisi ekrana düser
SELECT count(name)
FROM citizen
WHERE name = 'ahmet' AND gender = 0;

-- name kolonunda ismi ahmet olup knamen olan kisi sayisi ekrana düser
SELECT id AS kimlik_no, gender, count(name)
FROM citizen;

-- id numarasini, genderi ve ismi ahmet olan herkesin verisi ekrana düser
```

### Between

```sql
-- bir aralikta bir sey belirtmek istedigimizde kullaniriz
SELECT *
FROM citizen
WHERE id BETWEEN 1 AND 5;

-- tc numarasi 1 ve 5 arasinda olan vatANDaslarin bilgisini ekrana getirir
```

### Like

```sql
-- dikkat et yüzde ifadeleri tirnak içinde yaziyor
SELECT *
FROM citizen
WHERE name LIKE %a%;

-- ismi içinde a geçen vatandaşların tablosunu anahtar sag sirasina göre siralar
SELECT *
FROM citizen
WHERE name LIKE '%van%';

-- ismi içinde van geçen vatandaşların tablosunu anahtar sag sirasina göre siralar
```

### Order By

Sıralama.

```sql
-- siralama amaçli kullanilir, ASC ve desc ile birlikte kullanilabilir
-- ASC - a dan z ye , desc z den a ya
-- dikkat et burnamea anahtar sag ile siralamiyor

-- citizen tablosundan ismi içinde a geçen knamenlari alfabetik olarak sirala
SELECT *
FROM citizen
WHERE (name LIKE %a%) AND (gender = 0)
ORDER BY name ASC;
```

### Functions

```sql
avg() -- averageyi cikarir misal average yas-nullari hesaplamaz
min() -- tablodaki minimum degeri getirir, misal ismi min olan ali, ayse
max() -- tablodaki maximum degeri getirir, misal ismi max olan zeynep - yusuf vs
sum() -- toplami getirir, misal maaslarin toplami
count() -- satirlari sayar ve ekrana getirir, count(*) yaparsak eger tüm satirlari sayar, eger count(isim) yaparsak isim tablosunda null olmayan tüm satirlari döndürür

-- citizen tablosunda name içinde ar geçip name ayni olan vatANDaslarin ismini ve maas averagesini alfabetik olarak küçükten büyüge a-z dogru sirala
SELECT name AS name, count(distinct name) AS salary-avg
FROM citizen
WHERE name LIKE '%ar%'
ORDER BY name ASC;
```

### Group By

Gruplama.

```sql
-- GROUP BY gender dedigimizde her gender degeri için bir grup açar, yani toplam 2 grup yani toplam iki satir veri olur
SELECT gender AS gender, count(salary)
FROM citizen
ORDER BY salary ASC,
GROUP BY gender;

/* gendere göre gruplnamegi için iki tane satir gelir, bu iki satir da maasi en yüksek olan en basta yer alir */

-- eger GROUP BY genderin yanina baska bir sey eklersek ne olur??
```

### Having

Having yaparken GROUP BY kullanmak zorunlu, anladığım kadarıyla gruplar arasında having yapılıyor

```sql
-- tüm tablo siralama islemi bittikten sonra sarta uyanlari listeler
SELECT avg(salary)
FROM citizen
GROUP BY gender
HAVING avg(salary) > 1200;

-- citizen tablosunda gendere gore gruplayarak maas averagesi 1200'ün üstende olanlari anahtar sag'a göre siralar
SELECT avg(salary)
FROM citizen
ORDER BY gender
GROUP BY gender
HAVING avg(salary) > 1200;

-- citizen tablosunda gendere gore gruplayarak maas averagesi 1200'ün üstende olanlari genderi büyükten küçüge dogru siralar
```

## Operators

```sql
a = b  -- a b ye esit mi
a < b  -- a b den kucuktur
a > b  -- a b den buyuktur
a <> b -- a b den farkli midir

-- is null
-- is not null
```

## In

```sql
-- baska bir tablodan bilgileri WHERE ve IN kullanarak almak ???

SELECT distinct customer_name 
FROM borrower
WHERE customer_name
IN (SELECT customer_name FROM depositor)

-- customer_name tablosu içinde depositor da bulunan ve disaridaki borrower tablosu içinde customer_name'ler ayniysa; ayni customer_name ' e sahip kisileri tek satirda, anahtar sag'a göre sirala
```

# Merge Tables and Multiple Table Usage

`INNER JOIN` kullanırken birleştirdiğimiz tablo sayısından 1 çıkartırtırsak; kod içinde eşitlik için kaç tane ifade yazmamız gerekiğini buluruz. Örneğin 3 tane tablo birleştireceksek 2 tane eşitlik yazmamız gerekecekti.

```sql
-- INNER JOIN ve tabloları direkt olarak birbirine bağlayarak sorgu yazmışız
SELECT s.no, s.name, s.surname, c.class_name, cn.class_note 
FROM student s, class_name c, class_note cn
WHERE (s.student_id = n.student_id AND cc.class_code = c.class_code) AND c.class_name = 'Database' AND n.note >= (SELECT avg(note) FROM class_note WHERE class_code IN (SELECT class_code FROM class WHERE class_name = 'Database'));

-- hic bir dersten not almamis studentlerin name surname kolonlarını getir 
SELECT student_id, name, surname
FROM student
WHERE student_id
IN (SELECT student_id FROM notes WHERE note is null);

-- bak burnamea dikkat et, bu hiçbir dersten not almamis olanlari listeLEMEZ
SELECT student_id, name, surname
FROM student
WHERE student_id not
IN (SELECT student_id FROM notes);

--ama bu listeler
-- burnamea 3 tane tablo birleştirdik ve 2 tane eşitlik yazdık
SELECT s.student_id, s.name, s.surname, b.bolum_name, n.note
FROM student s
INNER JOIN(s.student_id = b.student_id), note n
INNER JOIN(n.student_id = s.student_id); 
```

# UPDATE, DELETE, ALTER, EXEC

```sql
-- bütün satirlarin column1 değerini 'foo' yap
UPDATE table1
SET column1 = 'foo';

-- column1 içinde a harfi içeren bütün satirların column1 değerini 'foo' yap
-- database kullanımdayken büyük sistemlerde bu yöntem ile güncelleme yapmak riskli bir işlem, değiştirmek istemediğimiz bir veriyi değiştirmiş olabiliriz
UPDATE table1
SET column1 = 'foo'
WHERE column1
LIKE '%a%';

-- column1 değeri 1 olan bütün satırları sil
DELETE FROM table1
WHERE column1 = 1;

-- tabloya 30 karakter tutabilen, column_name adında yeni bir kolon ekle
ALTER TABLE table1 ADD column_name varchar(30);

-- column1 alanı 'foo' olan bütün satırların column1 değerini 'bar' olarak değiştir
UPDATE table1
SET column1 = 'foo'
WHERE column1 = 'bar';

-- column1 kolonunun veri tipini 40 olarak değiştir
-- eğer bu kolonda 40 karakter üstü bir veri girişi yapılmışsa hata verecektir
ALTER TABLE table1 ALTER COLUMN column1 varchar(40);

-- tinyint 255 karakter, int bir suru karakter yani tinyint -> int her halukarda olabilir, int->tinyint olabilir-olmayabilirde
-- eger veriler varchar olarak 11,22,1 falan kaydedildiyse tinyint-int degistirilebilir cunku icinde karakter kaydedilmemis, eger karakter kaydi varsa zinhar olmaz--

-- table1 tablosunun adını 'foo' olarak değiştir
EXEC sp_rename 'table1', 'foo';

-- table1 tablosundaki column1 kolonunun adını 'foo' olarak değiştir
EXEC sp_rename 'table1.column1', 'foo';

-- table1 tablosundaki column1 kolonunu sil
ALTER TABLE table1 DROP COLUMN column1;

-- database1 veritabanını sil
DROP DATABASE database1;

-- table1 tablosunu sil
DROP TABLE table1;
```

# VIEW

```sql
-- tabloya bir kisayol -kural- kaydettik
CREATE VIEW view1
AS SELECT column1, column2
FROM table1
WHERE column3 = 0 

-- sonrasinda bu kisayolu cagirabiliyoruz
SELECT *
FROM view1 

-- bu kurali siliyoruz
DROP VIEW view1

-- ikincil bir sorguyla dönen cevabı view içinde kullanıp yeni bir cevap elde ettik
CREATE VIEW view1(name, surname)
AS SELECT name, surname
FROM student
WHERE gender = 0

-- calismaz cunku anahtar sag yok bunda 
INSERT INTO view1( 'A', 'B' )

-- student_id oldugu için veri girisi olur
INSERT INTO view1( 'A', 'B', 0, 45 )

-- bayan sorgusu snameece knamenlari getiriyordu ancak erkek giriside yapabiliriz
INSERT INTO view1( 'A', 'B', 1, 45 )

-- cinsiyeti 1 olan kimse girilemez (insert) artik
CREATE VIEW view1(name, surname)
AS SELECT name, surname
FROM student
WHERE gender = 0
WITH CHECK option
```

# WIP

```sql
-- her student kayıtı ile birlikte bölüm kayıtlarını birleştirip getirir
SELECT s.name, s.surname, b.dept_name FROM student s, department d WHERE s.dept_no = c.dept_no

-- iki tabloda ayni olmayan kolonlar oldugu icin baslarina kisaltma kullanmadık, sadece adı aynı olan kolonların başında tablonun adını kullandık. Ancak bu güzel bir kullanım örneği değil. Kolonlarda tablo adı bildirmek kodun okunabilirliğini arttırıyor.
SELECT name, surname, dept_name FROM student INNER JOIN department on(student.dept_no = department.dept_no)

CREATE VIEW get_students_with_deph AS SELECT name, surname, dept_name FROM student INNER JOIN department on(student.dept_no = student.dept_no)

SELECT s.name, s.surname, c.class_name, n.note FROM student o, class c, notes n WHERE s.student_id = n.student_id AND c.class_code = n.class_code


-- sinif averagesinin ustunde olanlari goster - olmname
SELECT s.name, s.surname, c.class_name, n.note FROM student o, class c, notes n WHERE s.student_id = n.student_id AND c.class_code = n.class_code AND (c.class_name = 'Math') AND n.note >= (SELECT //AVG(note)// FROM class WHERE class_name = 'Math')


SELECT s.name, s.surname, c.class_name, n.class_note FROM student s, class c, note n WHERE s.student_id = n.student_id AND c.class_code = n.class_code 

CREATE VIEW average(ders, average) AS SELECT class_code, AVG(note) FROM notes GROUP BY class_code

-- bu sekilde yeni VIEW tanimlamasini kullanabiliriz
SELECT * FROM average

SELECT s.student_id, s.name, s.surname, c.class_name, n.note, ort.average FROM student o
INNER JOIN note n on(s.student_id = n.student_id)
INNER JOIN class c on(c.class_code = n.class_code)
INNER JOIN average ort on(s.student_id = n.student_id)
```

// TODO: en son 14.12 uzantılı 1. sınıf vize 1 dosyası açıldı

# Technical Terms

## Primary Key

Primary Key ile her kayıt eşsiz bir değer alır. Bu nedenle spesfik verilere erişimimiz kolaylaşıyor. Primary Key yapısını kullanmak zorunda değiliz ancak şiddetle tavsiye ediliyor.

## UML

// TODO