# PC Webshop Adatbázis Tervezés - relációs adatbázis

Egy egyszerűsített informatikai webáruház adatbázis-modellje elkészítése a célom, mely bemutatja az entitások közötti kapcsolatokat, idegen kulcsok (`FOREIGN KEY`) használatát és statisztikai lekérdezéseket.



A munkát a tudásom megalapozásával kezdtem.
Az abszolút kezdőknek, mint én, ajánlom, hogy olvassák el ezeket a cikkeket:

[SQL alapok kezdőknek](https://webiskola.hu/sql-alapok-kezdoknek/ "SQL alapok kezdőknek")



Az alapok elsajátítása után, egy olyan online elérhető felületet ,"játszóteret" kerestem, amit böngészőből tudok futtatni.

Ezt választottam:

[MySQL Online ](https://onecompiler.com/mysql "Online MySQL lekérdezés futtató")



## A feladatunk lesz egy képzeletbeli informatikai webshop adatbázisának elkészítése.

Elsőként az adatbázis elkészítését és használatát kérjük a rendszertől.

Ezt az alábbi paranccsal tehetjük meg:

létrehozzuk az adatbázis a `CREATE` paranccsal.

```bash
CREATE DATABASE webshop_adatbazis;
```
az elkészített adatbázist így használjuk, hogy tudjunk benne dolgozni.

```
USE DATABASE WEBSHOP_ADATBAZIS;
```

ezzel létrehoztuk és használatra bírtuk az adatbázist.

* FONTOS - a Create Database és Use database a választott nem fog működni, mert ez egy előre korlátozott MySQL adatbázis szerkesztő



### Táblák elkészítése: 
Az adatbázis logikailag úgy épül fel, hogy a megrendelések leadásához a törzsadatoknak (kategóriák, vásárlók, termékek, fizetési és szállítási módok) már létezniük kell a rendszerben.
* a való életben előbb dobáljuk a kosárba a termékeket, csak aztán adjuk meg az adatainkat, a fizetési módot, kiszállítási módot. Ám a felépített rendszer áll mögötte. Próbáltam más módon is elkészíteni, de nem működött.
  
### Adatbázis táblák listája:
*   **Categories:** Termékkategóriák (Alaplapok, Processzorok, stb.)
*   **Customers:** Regisztrált vásárlók adatai.
*   **Payments** Elérhető fizetési módok
*   Shipping:** Elérhető szállítási módok.
*   **Produts:** A webshopban elérhető alkatrészek és szoftverek árakkal. ( itt a Foreign key használata )
*   **Orders:** A rendelések törzsadatai (Ki rendelte, hogyan fizetett, mivel kéri a kiszállítást).
*   **Order_items:** A kosár tartalma (Melyik rendelésben milyen termékből mennyit vettek).


```
-- Customers tábla létrehozása
CREATE TABLE Customers (
    customer_id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    email VARCHAR(100) NOT NULL UNIQUE,
    phone_number VARCHAR(50)
);
```
vagy a Shipping tábla

```
-- Shipping tábla létrehozása
CREATE TABLE Shipping(
  shipping_id INT AUTO_INCREMENT PRIMARY KEY,
  shipping_method VARCHAR(30) NOT NULL
);

```



SQL adattípusok
Egy adatbázis-kezelő esetében az adattípusok határozzák meg azt, hogy egy oszlop milyen értéket vehet fel. Az SQL elvei szerint, egy oszlopnak (mezőnek) minden esetben meg kell határozni az adattípusát már a tábla létrehozásakor, ez mutatja meg, hogy az egyes oszlopokban milyen adatokat, értékeket lehet tárolni.

A MySQL-ben három főkategóriába sorolhatóak az adattípusok:

szöveg
* szám
* dátum
* idő

az adatbázisban használt adattípusokról itt olvashatsz többet:

https://webiskola.hu/sql-ismeretek/adattipusok-mysql-ben/ 



A táblák létrehozása után, feltöltjük őket adattal az `INSERT INTO` paranccsal.

` `
` Customers ` tábla adatainak feltöltése:

```
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

vagy a ` szállítási módok` tábla:

```
-- adatok felvitele Shipping táblába
INSERT INTO Shipping (shipping_method)VALUES
('Személyes átvétel'), -- 1
('MPL házhozszállítás'), -- 2
('GLS házhozszállítás'), -- 3
('DPD házhozszállítás'), -- 4 
('DHL házhozszállítás');-- 5

```

## Most j00ön egy bonyolulu a products stb...


## --------------------

---

## 📊 Hasznos SQL Lekérdezések (Riportok)

### 1. Részletes Megrendelés Lista (Minden adat összekapcsolva)
Ez a lekérdezés emberi fogyasztásra alkalmas formában listázza ki az összes beérkezett rendelést.

```sql
SELECT 
    Orders.order_id AS 'Rendelés ID',
    Customers.name AS 'Vásárló neve',
    Produts.product_name AS 'Termék',
    Produts.price AS 'Egységár (Ft)',
    Order_items.quantity AS 'Mennyiség',
    (Produts.price * Order_items.quantity) AS 'Részösszeg (Ft)', 
    Payments.payment_method AS 'Fizetési mód',
    Shipping.shipping_method AS 'Szállítási mód'
FROM Orders
JOIN Customers ON Orders.customer_id = Customers.customer_id
JOIN Order_items ON Orders.order_id = Order_items.order_id
JOIN Produts ON Order_items.products_id = Produts.products_id
JOIN Payments ON Orders.payment_id = Payments.payment_id
JOIN Shipping ON Orders.shipping_id = Shipping.shipping_id
ORDER BY Orders.order_id;
```

### 2. Webáruház Összesített Bevétele
```sql
SELECT 
    SUM(Order_items.quantity) AS 'Webáruház összes eladott termék (db)',
    SUM(Produts.price * Order_items.quantity) AS 'WEBÁRUHÁZ MINDÖSSZESÍTETT BEVÉTELE (Ft)'
FROM Order_items
JOIN Produts ON Order_items.products_id = Produts.products_id;
```

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

### 4. Passzív vásárlók keresése (Akik még sosem rendeltek)
```sql
SELECT Customers.name, Customers.email FROM Customers
LEFT JOIN Orders ON Customers.customer_id = Orders.customer_id
WHERE Orders.order_id IS NULL;
```

---

## 💡 Tibi válaszol: Miért így épül fel a Rendelési Rendszer?

**A kérdésed:** *„A való életben először teszem be a kosárba a terméket, aztán adom meg a szükséges adatokat. Miért a kódban fordítva működik?”*

**A logikai magyarázat:**
A weboldalon (frontend) a látogató valóban a kosárral kezd, de a **háttértároló adatbázisban (backend)** a relációs szabályok (Referential Integrity) miatt ezt meg kell fordítani:
1.  A **Kosár (Order_items)** egy összekötő tábla. Nem létezhet benne olyan tétel, ami nem tartozik egy valós rendelés fejléchez (`order_id`).
2.  A **Rendelés (Orders)** fejléc pedig nem létezhet anélkül, hogy ne tudnánk, ki a vásárló (`customer_id`).
3.  **A folyamat a háttérben valójában így fut le a fizetés gomb megnyomásakor:**
    *   A rendszer elmenti a vásárlót a `Customers` táblába (ha új vendég).
    *   Létrehoz egy üres számlát/rendelés fejlécet az `Orders` táblában a kiválasztott szállítási és fizetési móddal.
    *   Végül ebbe a frissen generált `order_id`-ba önti bele a kosár elemeit az `Order_items` táblába.

Így biztosítja az SQL, hogy ne lehessen "szellem" termékeket rendelni olyan kosarakban, amik nem tartoznak senkihez! 🚀
