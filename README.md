# PC Webshop Adatbázis Tervezés - relációs adatbázis

Egy egyszerűsített informatikai webáruház adatbázis-modelljének elkészítése a célom, amely bemutatja az entitások közötti kapcsolatokat, és statisztikai lekérdezéseket.


## A tanulási folyamat 

A projekt megvalósítását az alapvető ismeretek elsajátításával kezdtem. Hozzám hasonlóan azoknak, akik még csak most ismerkednek a témával, az alábbi cikkeket ajánlom kiindulópontként:

[SQL alapok kezdőknek](https://webiskola.hu/sql-alapok-kezdoknek/ "SQL alapok kezdőknek")



Az alapok elsajátítását követően egy olyan online felületet "játszóteret" kerestem, ahol böngészőből, telepítés nélkül tudok kísérletezni és gyakorolni.

Végül az alábbi oldalt választottam:

[MySQL Online ](https://onecompiler.com/mysql "Online MySQL lekérdezés futtató")



## A feladatunk lesz egy képzeletbeli informatikai webshop adatbázisának elkészítése.

A feladat során több fontos SQL-paranccsal, és azok gyakorlati alkalmazásával ismerkedünk meg. 


Többek között a `CREATE`, a `USE`, za `INSERT INTO`, a `FOREIGN KEY`, a `JOIN`, a `LEFT JOIN`, az `AS`, a `WHERE` és az `UPDATE` használatát gyakoroljuk.

### <ins> 1.  Az adatbázis létrehozása és használata<ins>
Első lépésként létrehozzuk ` CREATE DATABASE ` paranccsal a webáruház adatbázisát, majd beállítjuk a ` USE ` paranccsal, hogy a következőkbenn ebben tudjunk dolgozni.


```sql
CREATE DATABASE webshop_adatbazis;
```

A létrehozott adatbázis kiválasztásához a ` USE ` parancsot használjuk:

```sql
USE DATABASE WEBSHOP_ADATBAZIS; -
```

Ezzel sikeresen létrehoztuk és kiválasztottuk az adatbázist, amit létrehoztunk így megkezdhetjük a táblák létrehozását és az adatok feltöltését.


* FONTOS - a `CREATE DATABASE ` és `USE` paranc a választott online szerkesztőben nem fog működni, mert ez egy előre korlátozott MySQL adatbázis.



---------------



### <ins> 2. Az adatbázis felépítése és a táblák tervezése, elkészítése:<ins>

Az adatbázis logikai felépítésénél abból indultam ki, hogy a rendelések rögzítéséhez először a szükséges törzsadatoknak kell rendelkezésre állniuk. Ide tartoznak például a kategóriák, vásárlók, termékek, fizetési és szállítási módok.


A való életben először a termékeket helyezzük a kosárba, és csak a rendelés véglegesítésekor adjuk meg a vásárlói adatainkat, illetve választjuk ki a fizetési és szállítási módot, ám a felépített rendszer áll mögötte.

Próbálkoztam másfajta felépítéssel is, de végül ennél maradtam, mert így sikerült megfelelően kialakítanom az adatbázis kapcsolatait.


<ins>2.1. A törzsadatok létrehozása<ins>

Első lépésként a törzsadatokat tartalmazó táblákat hoztam létre. Ezek biztosítják az alapot a későbbi rendelések és a hozzájuk kapcsolódó adatok kezeléséhez.

Adatbázistáblák listája
* **Categories:** Termékkategóriák (pl. alaplapok, processzorok).
* **Customers:** A regisztrált vásárlók adatai.
* **Payments:** Az elérhető fizetési módok.
* **Shipping:** Az elérhető szállítási módok.
* **Products:** A webáruházban elérhető alkatrészek és szoftverek, árakkal együtt. A kategóriákhoz való kapcsolódásnál a FOREIGN KEY használatával ismerkedünk meg.
* **Orders:** A rendelések főbb adatai, például a vásárló, a fizetési és a szállítási mód.
* **Order_items:** A rendelések tételei, vagyis hogy egy adott rendelésben milyen termékből hány darabot vásároltak.

<ins>2.2. SQL adattípusok<ins>

Az adatbázis-kezelés során fontos szerepet játszanak az adattípusok. Ezek határozzák meg, hogy egy adott oszlopban milyen típusú értékeket tárolhatunk.

Az SQL-ben minden oszlophoz meg kell határozni az adattípust a tábla létrehozásakor. Ez biztosítja, hogy az adatok megfelelő formátumban kerüljenek az adatbázisba.

A MySQL adattípusait három fő csoportba sorolhatjuk:

* Szöveges adattípusok: például VARCHAR, TEXT.
* Szám típusú adattípusok: például INT, DECIMAL.
* Dátum- és időtípusok: például DATE, DATETIME, TIME.
* 
Az adatbázisban használható adattípusokról bővebben az alábbi oldalon olvashatsz:

[Adattípusok a mySQL-ben ](https://webiskola.hu/sql-ismeretek/adattipusok-mysql-ben/ )


###  <ins> 3. Táblák létrehozása: <ins>


A `Customers` tábla létrehozása a `CREATE` paranccsal:

```sql
-- Customers tábla létrehozása
CREATE TABLE Customers (
    customer_id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    email VARCHAR(100) NOT NULL UNIQUE,
    phone_number VARCHAR(50)
);
```


az adatok feltöltése a `Customers` táblába `INSERT INTO ` paranccsal.

```sql
-- adatok felvitele Customers táblába (bővített lista)
INSERT INTO Customers (name, email, phone_number) VALUES 
('Nagy Szabina', 'szabina.nagy@email.com', '+36 30 987 6543'), -- 1
('Kiss Péter', 'peter.kiss@email.com', '+36701112223'), -- 2
('Tóth Gábor', 'gabor.toth@email.com', NULL), -- 3
('Kovács Dániel', 'daniel.kovacs@email.com', '+36203334444'), -- 4
('Molnár Anikó', 'aniko.molnar@email.com', '+36305556666'), -- 5
('Horváth Balázs', 'balazs.horvath@email.com', NULL), -- 6 
('Szabó Eszter', 'eszter.szabo@email.com', '+36707778888'), -- 7
('Farkas Bence', 'bence.farkas@email.com', '+36209991111'), -- 8
('Németh Dorina', 'dora.nemeth@email.com', '+36302223333'), -- 9
('Varga Tamás', 'tamas.varga@email.com', NULL), -- 10
('Takács Kinga', 'kinga.takacs@email.com', '+36704445555'), -- 11
('Juhász Attila', 'attila.juhasz@email.com', '+36206667777'), -- 12
('Papp Zsófia', 'zsofia.papp@email.com', '+36308889999'), -- 13
('Mizse Dóra', 'dora.mizse@email.com', '+36 20 519 7532'); -- 14
```




a ` Shipping` tábla létrehozása szintén a `CREATE` paranccsal:

```sql
-- Shipping tábla létrehozása
CREATE TABLE Shipping(
  shipping_id INT AUTO_INCREMENT PRIMARY KEY,
  shipping_method VARCHAR(30) NOT NULL
);

```


az adatok feltöltése a ` Shipping` táblába `INSERT INTO ` paranccsal.
```sql
-- adatok felvitele Shipping táblába
INSERT INTO Shipping (shipping_method)VALUES
('Személyes átvétel'), -- 1
('MPL házhozszállítás'), -- 2
('GLS házhozszállítás'), -- 3
('DPD házhozszállítás'), -- 4 
('DHL házhozszállítás');-- 5

```




egy FOREIGN kEY-s tábla létrehozása `Prodcts2` tábla összeköt ` Categories2 ` táblával

a ` Categories2 ` tábla létrehozása ismét a `CREATE` paranccsal:
```sql
-- NR 1b. #####################################
-- tábla létrehozása Categories2
CREATE TABLE Categories2 (
category_id INT AUTO_INCREMENT PRIMARY KEY,
category_name VARCHAR(20) NOT NULL
);

```

adatok feltöltése a `Categories2` táblába az `INsERT INTO` paranccsal:

```sql
-- adatok felvitele Categories2
INSERT INTO Categories2 (category_name) VALUES
('Alaplapok'), -- 1
('Processzorok'),  -- 2
('Memóriák'),  -- 3
('Videokártyák'),  -- 4
('Tápegységek'),  -- 5
('Háttértárak'),  -- 6
('Szoftverek');  -- 7 

```


a `Products2` tábla létrehozása ismét a `CREATE` paranccsal, `FOREIGN KEY`-jel.
a `(category_id)` -re hivatkozunk a `Categories2` táblából.
```sql

-- tábla létrehozása Products2 (Foreign Key-el összekötve
CREATE TABLE Produts2(
  products_id INT AUTO_INCREMENT PRIMARY KEY,
  product_name VARCHAR (50) NOT NULL,
  category_id  INT NOT NULL,
  price        DECIMAL(10,2) NOT NULL,
  FOREIGN KEY (category_id) REFERENCES Categories2(category_id)
);

```

az adatok felt0öltése `INSERT INTO` paranccsal `FOREIGN KEY` -jel a `Products2` táblába:
```sql
-- adatok felvitele Products2 (a szöveges kategóriák helyett a Categories2 táblát össekötjük
INSERT INTO Produts2 (product_name, category_id, price) VALUES
('Normál alaplap', 1, 29990.00), -- 1
('Irodai alaplap', 1, 39990.00), -- 2
('Gamer alaplap', 1, 49990.00), -- 3
('Normál HDD', 6, 15990.00), -- 4
('Irodai SSD', 6, 19990.00), -- 5
('Gamer SSD', 6, 35990.00), -- 6 
('Normál Processzor', 2, 25990.00), -- 7
('Irodai Processzor', 2, 35990.00), -- 8
('Gamer Processzor', 2, 45990.00), -- 9
('Normál Memória ', 3, 15990.00), -- 10
('Irodai Memória ', 3, 25990.00), -- 11
('Gamer Memória ', 3, 35990.00), -- 12
('Normál Videokártya ', 4, 14990.00), -- 13
('Irodai Videokártya ', 4, 24990.00), -- 14 
('Gamer Videokártya ', 4, 34990.00), -- 15
('Normál Tápegység ', 5, 14990.00), -- 16
('Irodai Tápegység ', 5, 19990.00),  -- 17
('Gamer Tápegység ', 5, 29990.00), -- 18
('Normál Szoftvercsomag ', 7, 24990.00), -- 19
('Irodai Szoftvercsomag ', 7, 44990.00), -- 20
('Gamer Szoftvercsomag ', 7, 74990.00); -- 21

```


A törzsadatokat elkészítettük, rendeléseket generáltunk.

--------------------------------------------------




## <ins> 4. Alap lekérdezések:<ins>

### 1. a törzsadatok listázása

a `Customers` tábla listázása a `SELECT` paranccsal:



```sql
-- Customers listázása
SELECT * FROM Customers; 

```

<img width="600" alt="image" src="https://github.com/user-attachments/assets/fdf1ee9f-01a9-4672-9284-e4de8e4f4b71" />


a `Shipping` tábla listázása

```sql
-- Shipping listázása
SELECT * FROM Shipping;
```

<img width="350" alt="image" src="https://github.com/user-attachments/assets/88203d31-316e-4a04-b81c-756c2dbdc068" />


a `Products` tábla listázása `FOREIGN KEY nélkül`, listázással:

<img width="707" height="654" alt="image" src="https://github.com/user-attachments/assets/040e37eb-fa4a-450a-a24f-b7bfea99907a" />


A `Products2` listázása `FOREIGN KEY -jel`:

<img width="681" height="658" alt="image" src="https://github.com/user-attachments/assets/b6502ad8-e921-4632-9158-d667f5297234" />


Az összes megrendelés listázása elnevezve mint...:


```sql
-- összes megrendelés listázása "elnevezve" mint...
-- ugyanaz a lekérdezés, csak elnevezve
SELECT 
    Orders.order_id AS 'Rendelés ID',
    Customers.name AS 'Vásárló neve',
    Produts.product_name AS 'Termék',
    Produts.price AS 'Egységár (Ft)',
    Order_items.quantity AS 'Mennyiség',
    (Produts.price * Order_items.quantity) AS 'Részösszeg (Ft)', 
    Payments.payment_method AS 'Fizetési mód',
    Shipping.shipping_method AS 'Szállítási mód'
FROM Orders                                                   -- össekötjük mint feljebb, de már AS paranccsal elnevezve
JOIN Customers ON Orders.customer_id = Customers.customer_id
JOIN Order_items ON Orders.order_id = Order_items.order_id
JOIN Produts ON Order_items.products_id = Produts.products_id
JOIN Payments ON Orders.payment_id = Payments.payment_id
JOIN Shipping ON Orders.shipping_id = Shipping.shipping_id
ORDER BY Orders.order_id;
```


<img width="1571" height="533" alt="image" src="https://github.com/user-attachments/assets/dec312e2-9962-4711-8774-c322ac579b6b" />



# Hasznos SQL Lekérdezések (Riportok)

### 1. Részletes Megrendelés Lista (Minden adat összekapcsolva)

Ez a lekérdezés emberi fogyasztásra alkalmas formában listázza ki az összes beérkezett rendelést.


```sql
-- összes megrendelés listázása elnevezés nélkül ... csúnyán 
SELECT *
FROM Orders                                                        -- az Orders "dobozt"
JOIN Customers ON Orders.customer_id = Customers.customer_id       -- össekötjük a customer_id -val
JOIN Order_items ON Orders.order_id = Order_items.order_id         -- össekötjük az order_id -val 
JOIN Produts ON Order_items.products_id = Produts.products_id      -- össekötjük a products_id -val
JOIN Payments ON Orders.payment_id = Payments.payment_id           -- össekötjük a payment_id -val
JOIN Shipping ON Orders.shipping_id = Shipping.shipping_id         -- össekötjük a szállítási móddal
ORDER BY Orders.order_id;                                          -- növekvőbe tesszük a megrendelést order_id szerint
```




```sql

-- összes megrendelés listázása "elnevezve" mint...
-- ugyanaz a lekérdezés, csak elnevezve
SELECT 
    Orders.order_id AS 'Rendelés ID',
    Customers.name AS 'Vásárló neve',
    Produts.product_name AS 'Termék',
    Produts.price AS 'Egységár (Ft)',
    Order_items.quantity AS 'Mennyiség',
    (Produts.price * Order_items.quantity) AS 'Részösszeg (Ft)', 
    Payments.payment_method AS 'Fizetési mód',
    Shipping.shipping_method AS 'Szállítási mód'
FROM Orders                                                   -- összekötjük a JOIN paranccsal mint feljebb, de már AS paranccsal elnevezve
JOIN Customers ON Orders.customer_id = Customers.customer_id
JOIN Order_items ON Orders.order_id = Order_items.order_id
JOIN Produts ON Order_items.products_id = Produts.products_id
JOIN Payments ON Orders.payment_id = Payments.payment_id
JOIN Shipping ON Orders.shipping_id = Shipping.shipping_id
ORDER BY Orders.order_id;
```

### 2. Webáruház teljes bevétele:

```sql
-- szintén az AS és a JOIN parancsokat használva
SELECT 
    SUM(Order_items.quantity) AS 'Webáruház összes eladott termék (db)',
    SUM(Produts.price * Order_items.quantity) AS 'WEBÁRUHÁZ MINDÖSSZESÍTETT BEVÉTELE (Ft)'
FROM Order_items
JOIN Produts ON Order_items.products_id = Produts.products_id;
```
<img width="700" alt="image" src="https://github.com/user-attachments/assets/7fc03009-10da-4594-89ca-9c4967220be7" />





### 3. Bevételek listázása Kategóriák szerint (Legjobban tejelők elöl)
```sql
SELECT 
    Produts.category AS 'Termékkategória',
    SUM(Order_items.quantity) AS 'Eladott darabszám (db)',
    SUM(Produts.price * Order_items.quantity) AS 'Teljes bevétel (Ft)'
FROM Order_items
JOIN Produts ON Order_items.products_id = Produts.products_id
GROUP BY Produts.category
ORDER BY SUM(Produts.price * Order_items.quantity) DESC;
```


### 4. Passzív vásárlók (Akik még sohssem rendeltek)
```sql
-- vásárlók akik nem rendeltek ... a LEFT JOIN
-- ------------------------------------------
SELECT * FROM Customers
LEFT JOIN Orders ON Customers.customer_id = Orders.customer_id
WHERE Orders.order_id IS NULL; -- Csak azokat tartja meg, akikhez nem kapcsolódik rendelés
```

<img width="1269" height="211" alt="image" src="https://github.com/user-attachments/assets/128588d1-ec6e-4008-8456-3b83951888f8" />


---

