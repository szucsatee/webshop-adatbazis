# PC Webshop Adatbázis Tervezés - relációs adatbázis

Egy egyszerűsített informatikai webáruház adatbázis-modellje elkészítése a célom, mely bemutatja az entitások közötti kapcsolatokat, és statisztikai lekérdezéseket.


## A tanulási folyamat 

A projekt megvalósítását az alapvető ismeretek elsajátításával kezdtem. Hozzám hasonlóan azoknak, akik még csak most ismerkednek a témával, az alábbi cikkeket ajánlom kiindulópontként:

[SQL alapok kezdőknek](https://webiskola.hu/sql-alapok-kezdoknek/ "SQL alapok kezdőknek")



Az alapok elsajátítását követően egy olyan online felületet "játszóteret" kerestem, ahol böngészőből, telepítés nélkül tudok kísérletezni és gyakorolni.

Végül az alábbi oldalt választottam:

[MySQL Online ](https://onecompiler.com/mysql "Online MySQL lekérdezés futtató")



## A feladatunk lesz egy képzeletbeli informatikai webshop adatbázisának elkészítése.

A feladat során több fontos SQL-paranccsal, azok gyakorlati alkalmazásával ismerkedünk meg. Többek között a `CREATE`, a `USE`, za `INSERT INTO`, a `FOREIGN KEY`, a `JOIN`, a `LEFT JOIN`, az `AS`, a `WHERE` és az `UPDATE` használatát gyakoroljuk.

<ins> 1. Az adatbázis létrehozása és használata <ins>
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






### <ins> 2. Táblák elkészítése: <ins>

Az adatbázis logikailag úgy épül fel, hogy a megrendelések leadásához a törzsadatoknak (kategóriák, vásárlók, termékek, fizetési és szállítási módok) már létezniük kell a rendszerben.
* a való életben előbb dobáljuk a kosárba a termékeket, csak aztán adjuk meg az adatainkat, a fizetési módot, kiszállítási módot. Ám a felépített rendszer áll mögötte. Próbáltam más módon is elkészíteni, de nem működött.

Először a törzsadatokat fogjk elkészíteni.
  
### Adatbázis táblák listája:
*   **Categories:** Termékkategóriák (Alaplapok, Processzorok, stb.)
*   **Customers:** Regisztrált vásárlók adatai.
*   **Payments** Elérhető fizetési módok
*   Shipping:** Elérhető szállítási módok.
*   **Produts:** A webshopban elérhető alkatrészek és szoftverek árakkal. ( itt a Foreign key használata )
*   **Orders:** A rendelések törzsadatai (Ki rendelte, hogyan fizetett, mivel kéri a kiszállítást).
*   **Order_items:** A kosár tartalma (Melyik rendelésben milyen termékből mennyit vettek).
*   

SQL adattípusok
Egy adatbázis-kezelő esetében az adattípusok határozzák meg azt, hogy egy oszlop milyen értéket vehet fel. Az SQL elvei szerint, egy oszlopnak (mezőnek) minden esetben meg kell határozni az adattípusát már a tábla létrehozásakor, ez mutatja meg, hogy az egyes oszlopokban milyen adatokat, értékeket lehet tárolni.

A MySQL-ben három főkategóriába sorolhatóak az adattípusok:

szöveg
* szám
* dátum
* idő

az adatbázisban használt adattípusokról itt olvashatsz többet:


[Adattípusok a mySQL-ben ](https://webiskola.hu/sql-ismeretek/adattipusok-mysql-ben/ )


###  <ins> Táblák létrehozása: <ins>


A ` Customers ` tábla létrehozása:

```sql
-- Customers tábla létrehozása
CREATE TABLE Customers (
    customer_id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    email VARCHAR(100) NOT NULL UNIQUE,
    phone_number VARCHAR(50)
);
```
az `insert into `-val feltöltjük a táblát

` Customers ` tábla adatainak feltöltése:

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




a ` Shipping ` tábla létrehozása

```sql
-- Shipping tábla létrehozása
CREATE TABLE Shipping(
  shipping_id INT AUTO_INCREMENT PRIMARY KEY,
  shipping_method VARCHAR(30) NOT NULL
);

```

```sql
-- adatok felvitele Shipping táblába
INSERT INTO Shipping (shipping_method)VALUES
('Személyes átvétel'), -- 1
('MPL házhozszállítás'), -- 2
('GLS házhozszállítás'), -- 3
('DPD házhozszállítás'), -- 4 
('DHL házhozszállítás');-- 5

```




egy FOREIGN kEY-s tábla létrehozása ` Prodcts2 ` tábla összeköt ` Categories2 ` táblával
```sql
-- NR 1b. #####################################
-- tábla létrehozása Categories2
CREATE TABLE Categories2 (
category_id INT AUTO_INCREMENT PRIMARY KEY,
category_name VARCHAR(20) NOT NULL
);

```

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

az `INSERT INTO` -val `FOREIGN KEY` -jel feltöltjük a `Products2` táblát
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



--------------------------------------------------




## Alap lekérdezések:

### 1. a törzsadatok listázása

a `Customers` tábla listázása

```sql
-- Customers listázása
SELECT * FROM Customers; 

```

<img width="814" height="477" alt="image" src="https://github.com/user-attachments/assets/3ed366a5-5d40-4966-a6a6-4cca9c950d7d" />

a `Shipping` tábla listázása

```sql
-- Shipping listázása
SELECT * FROM Shipping;
```

<img width="442" height="242" alt="image" src="https://github.com/user-attachments/assets/38b63800-5396-409a-83ed-a684e0f84cbc" />



## Hasznos SQL Lekérdezések (Riportok)

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
<img width="904" height="133" alt="image" src="https://github.com/user-attachments/assets/0b080ca9-885d-4925-9ad5-4bd515afa2db" />




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

