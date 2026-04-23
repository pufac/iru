Rendben, vágjunk is bele! Ez egy szuper megközelítés. Kigyűjtöm neked a legjellemzőbb típusfeladatokat, és elkészítjük hozzájuk a "cheat sheet" jellegű levezetést. 

Kezdjük rögtön két nagyon gyakori számolós feladattípussal: a **mentési kapacitás számításával** és az **IP alhálózatok (subnetting)** kérdéseivel.

---

### 1. Típusfeladat: Mentési kapacitás (Inkrementális vs. Differenciális)

Ezeknél a feladatoknál mindig megadják az alap adatmennyiséget, a napi változás százalékát, és egy konkrét napot, amire kíváncsiak. A kulcs a két technológia működésének megértése.

* **Alapszabály (Cheat sheet)**:
    * **Inkrementális mentés**: Mindig csak az *előző mentés óta* megváltozott adatokat menti. A mentett adatmennyiség minden nap **konstans** (ha a változás százaléka fix).
    * **Differenciális mentés**: Mindig az *utolsó teljes (full) mentés óta* megváltozott adatokat menti. A mentett adatmennyiség naponta **halmozódik** (nő).

**Példa 1: Differenciális mentés**
> **Feladat:** Egy szerverparkban 1000TB adatmennyiséget kell menteni. Differenciális mentést használunk. A változás mértéke kb. 10%/nap, de az adatmennyiség nem növekszik, csak a tartalom módosul. Vasárnap van a full mentés. Hány TB adat kerül a mentőrendszerbe egy átlagos **szerdai** napon?
* **Levezetés:**
    * Napi változás: 1000 TB 10%-a = 100 TB.
    * Vasárnap: Full mentés (1000 TB)
    * Hétfő: 1 napi változás = 100 TB
    * Kedd: 2 napi változás (hétfő+kedd) = 200 TB
    * Szerda: 3 napi változás (hétfő+kedd+szerda) = **300 TB**.

**Példa 2: Inkrementális mentés**
> **Feladat:** 10TB adatmennyiség, inkrementális mentés, 10%/nap változás. Vasárnap full mentés. Mennyi adat kerül a mentőrendszerbe **csütörtökön**?
* **Levezetés:**
    * Napi változás: 10 TB 10%-a = 1 TB.
    * Mivel inkrementális, minden nap csak az előző nap óta történt 1 TB változást mentjük le. Így hétfőn, kedden, szerdán és csütörtökön is pontosan **1 TB** lesz a mentett adat.

---

### 2. Típusfeladat: IP hálózatok, Subnetting és Broadcast

Ezeknél a kérdéseknél a CIDR jelölésből (pl. /26, /27) kell kitalálni a hálózat méretét, a kiadható IP-k számát, vagy a broadcast címet.

* **Alapszabály (Cheat sheet)**:
    * Az IPv4 cím 32 bites. A "per" (/) utáni szám mutatja, hány bit a hálózaté. A maradék bit (32 - netmask) a hosztoké (gépeké).
    * **Összes IP cím száma egy alhálózatban:** 2 a hoszt bitek hatványán.
    * **Kiadható (gépek által használható) IP-k száma:** Összes IP - 2 (mert a hálózati címet és a broadcast címet nem adhatjuk gépnek).

**Példa 1: Gépek vs. IP címek száma**
> **Feladat:** A 152.130.246.0/27 hálózat maximum hány **gépet** tartalmaz? És hány **IP címet**?
* **Levezetés:**
    * Hoszt bitek száma: 32 - 27 = 5.
    * Összes IP cím: 2^5 = **32**.
    * Gépek (hosztok) száma: 32 - 2 = **30**. (Mindig figyelj a vizsgán a kérdés pontos megfogalmazására!)

**Példa 2: Broadcast cím kiszámítása**
> **Feladat:** Abban a hálózatban, amelyik a 168.32.12.29/26 IP című gépet tartalmazza, mi a broadcast cím utolsó bájtja (decimálisan)?
* **Levezetés:**
    * A /26 azt jelenti, hogy az utolsó oktettben (8 bitből) 2 bit a hálózaté, 6 bit a hosztoké.
    * A hálózatok mérete: 2^6 = 64 IP cím.
    * A hálózatok így indulnak az utolsó bájtban: 0, 64, 128, 192.
    * A ".29"-es IP cím a 0-val kezdődő hálózatba esik (aminek a tartománya 0-tól 63-ig tart).
    * A broadcast cím mindig a tartomány legutolsó címe, ami ebben az esetben a **63**.

---

Ezek lennének a legfontosabb alapok a mentésből és az IP címzésből. Mehetünk tovább a **FlashCopy blokkszámításos** és az **adatközponti topológiás (Spine-Leaf/Fat Tree) túlméretezéses** típusfeladatokkal?

Szuper, haladjunk tovább a következő két izgalmas témakörrel! Ezen a ponton a **FlashCopy kapacitásszámítás** és az **adatközponti architektúrák (Spine-Leaf, Fat Tree, túlméretezés)** típusfeladatait nézzük meg, amikből szintén rengeteg van a ZH-ban.

---

### 3. Típusfeladat: FlashCopy blokkszámítás

A FlashCopy egy "Copy-on-Write" (íráskor másol) technológia, ami egy pillanatképet (snapshot) készít. A vizsgafeladatokban azt kell kiszámolni, hogy a verziók tárolása összesen hány blokkot foglal el.

* **Alapszabály (Cheat sheet):**
    * **Kiindulási állapot:** A fájl eredeti mérete blokkokban megadva (pl. 6 blokk).
    * **Módosítás:** Minden egyes módosított blokk naponta (vagy verziónként) **+1 blokk** tárhelyet igényel.
    * **Beszúrás:** Minden új blokk beszúrása **+1 blokk** igényt jelent.
    * **Törlés:** A blokk törlése **NEM igényel** extra blokkot (0 blokk), hiszen csak egy logikai mutató szűnik meg.
    * **Számítás:** Egyszerűen add össze az eredeti blokkszámot és az összes plusz módosítást/beszúrást!

**Példa 1: Egyszerű módosítások**
> **Feladat:** Létrehozunk egy 6 blokkból álló fájlt. Egy nap múlva a 3. és 4. blokkot módosítjuk. Következő nap a 3. blokkot újra megváltoztatjuk. Hány blokkot igényel ez a 3 változat? 
* **Levezetés:** * Eredeti fájl: 6 blokk.
    * 1. nap után: 3. és 4. blokk módosul (+2 blokk).
    * 2. nap után: 3. blokk újra módosul (+1 blokk).
    * Összesen: 6 + 2 + 1 = **9 blokk**.

**Példa 2: Törlést és beszúrást is tartalmazó feladat**
> **Feladat:** 10 blokkból álló fájl. 1. nap: 3., 5. és 9. blokk módosul. 2. nap: utolsó blokkot töröljük, első elé egyet beszúrunk. 3. nap: 5. blokk újra módosul. Hány blokkot igényel a 4 változat? 
* **Levezetés:**
    * Eredeti: 10 blokk.
    * 1. nap: 3 blokk módosul (+3 blokk).
    * 2. nap: 1 törlés (+0 blokk), 1 beszúrás (+1 blokk).
    * 3. nap: 1 blokk módosul (+1 blokk).
    * Összesen: 10 + 3 + 0 + 1 + 1 = **15 blokk**.

---

### 4. Típusfeladat: Adatközponti hálózatok (Oversubscription, Spine-Leaf, Fat Tree)

Ezek a hálózattervezési és méretezési kérdések elsőre bonyolultnak tűnhetnek, de nagyon egyszerű ökölszabályok alapján működnek.

* **Alapszabály (Cheat sheet):**
    * **Túlméretezés (Oversubscription) 1 switch-en:** Kiszámítása: `Lefelé menő (downlink) portok száma / Felfelé menő (uplink) portok száma`. Ha összesen X port van, és Y a felfelé menő, akkor a lefelé menő = (X - Y).
    * **Teljes túlméretezés a hálózatban:** A különböző rétegek túlméretezési arányait **össze kell szorozni**.
    * **Spine-Leaf architektúra:** Egy Leaf switch-nek annyi Spine switch-hez lehet maximum csatlakoznia, ahány szabad portja marad, miután a szervereket bekötöttük rá. Képlet: `Max Spine = Összes port - Szerverek száma`.
    * **Fat Tree architektúra:** A maximális Pod-ok száma (és egyúttal egy switch portjainak száma) pontosan **kétszerese** az egy access switch-hez kapcsolható szerverek számának. `Max Pod = 2 * Access szerverek`.

**Példa 1: Switch túlméretezés kiszámítása**
> **Feladat:** Egy aggregációs switch 32 porttal rendelkezik. 8 portját használjuk a core réteg felé redundáns összeköttetésre. Mekkora a túlméretezés ezen a switchen? 
* **Levezetés:** * Felfelé (Core felé) menő portok = 8.
    * Lefelé menő portok = Összes (32) - Felfelé (8) = 24.
    * Túlméretezés = 24 / 8 = **3**.

**Példa 2: Többrétegű túlméretezés**
> **Feladat:** Egy adatközpont egyik rétegében a túlméretezés 9, a másikban 2. Mennyi a teljes túlméretezés? 
* **Levezetés:** * A rétegek értékeit egyszerűen összeszorozzuk: 9 * 2 = **18**.

**Példa 3: Spine and Leaf méretezés**
> **Feladat:** Egy Spine and Leaf architektúrában 48 portos switcheket használunk. Egy Leaf switch-hez 38 szerver kapcsolódik. Maximális kiépítésben hány Spine switch-ünk lehet? 
* **Levezetés:** * Összes port a Leaf-en = 48.
    * Ebből szerverek (lefelé) = 38.
    * Szabadon maradó portok felfelé (Spine irányába) = 48 - 38 = **10**. 
    * Mivel minden felfelé menő port egy külön Spine switch-hez csatlakozhat, így maximum **10** Spine switchünk lehet.

---

Ezzel a legfajsúlyosabb számolós feladatokat ki is végeztük a dokumentumból. 
Szeretnéd, ha a maradék néhány logikai/hardveres típusfeladatot (pl. RAID szalagok száma, mentési ablak frissítési idők) gyűjteném ki, vagy térjünk át az elméleti Fogalomtár felépítésére?

Igen, nagyon jó, hogy rákérdeztél! Bár a fő számolásokat és a nagy fogalmakat átvettük, a dokumentumban lapul még néhány nagyon specifikus, mondhatni **"beugratós" vizsgakérdés**, illetve pár olyan **párosított fogalom**, amit könnyű összekeverni. 

Ezekből is készítettem egy kiegészítő "Cheat Sheet"-et, hogy a trükkösebb tesztkérdések se fogjanak ki rajtad:

### 1. Pénzügyi és Beruházási trükk-kérdések (Ezt a kettőt könnyű összekeverni!)
* **Kevés beruházási keret, de üzemeltetésre is csak ugyanennyi lesz:** Ha az induló és a későbbi költség is alacsony/korlátozott, akkor **Disk Array** technológiát kell választani.
* **Sok beruházási pénz (pl. pályázatból), de üzemeltetésre már nem lesz sok:** Mivel az induló keret nagy, de a fenntartást minimalizálni kell (pl. alacsonyabb áramfogyasztás, kevesebb hűtés), itt az **All Flash Array** a helyes válasz.

### 2. Termékcsaládok jellemzői (A 3 archetípus)
A ZH egy-egy mondattal kérdez rá a különböző kategóriákra:
* **Otthoni:** Alacsony induló ár, drága bővítés.
* **Üzleti:** Magasabb induló ár, de lassabban avuló megoldások.
* **Szerver:** Teljesítményhez viszonyított minimális költség, robusztus kivitel.

### 3. Mentési idők és teljesítmények összehasonlítása
* **Melyik mentés tart tovább?** A **differenciális mentés** tovább tart, mint az inkrementális. (Illetve differenciális mentésnél egy átlagos csütörtökön *több* adatot mentünk, mint szerdán, míg inkrementálisnál egyforma a két nap ).
* **Mentés vagy Visszaállítás?** Egy tipikus mentőrendszer esetén a **visszaállítási idő nagyobb**, mint a mentési idő.
* **Szalagos megtalálási idő:** A szalagokon egy fájl megtalálási ideje trükkös. Ha a fájlokat ugyanolyan sorrendben írtuk ki a szalagra, mint ahogy a diszken voltak, akkor egyforma. Minden **ellenkező esetben a szalagé sokkal nagyobb**.
* **Archiválás "vészhelyzeti" kelléke:** Hosszútávú adatmegőrzés esetén az archív szalagokon felül a fizikai **olvasó egységet** is érdemes eltárolni (hiszen 10-20 év múlva lehet, hogy már nem gyártanak olyat).

### 4. Nagyteljesítményű Clusterek (HPC) felosztása
* **1. típusú (szorosan csatolt) clusterek:** Olyan feladatokra jók, ahol a folyamatok szorosan összefüggnek. Tipikus vizsgaválasz: **Időjárás előrejelzés**.
* **2. típusú (elosztott I/O) clusterek:** Olyan feladatokra jók, ahol sok független adatot kell párhuzamosan feldolgozni. Tipikus vizsgaválasz: **Keresőmotor**.

### 5. DNS feloldás logikája (Szerverek)
* Ha az `xx.zzz.com` IP címét keressük, azt biztosan **a zzz.com DNS szervere** fogja tudni.
* Ugyanígy, ha az `inet.company.com` feloldása a kérdés, azt a **company.com DNS szervere** tudja biztosan.

### 6. Egyéb hardveres / hálózati apróságok
* **Dedikált külső tár csatlakoztatása:** Ha egy szerverhez közvetlenül dedikált külső tárat (DAS) akarunk illeszteni, ahhoz **SAS** vagy **Fibre Channel** technológiát használhatunk.
* **MAC cím és biztonság:** Ha egy adatbázist feltörnek, célszerű-e megváltoztatni a szerver MAC címét is? A helyes válasz: **Azt nem lehet**. (Bár a gyakorlatban szoftveresen spoofolható, hálózattervezési szempontból a fizikai cím a kártyába van égetve).
* **Protokoll specifikáció:** A protokoll specifikáció *statikus* része **NEM** tartalmazza az **üzenetek kódolását**.

Ezekkel a kiegészítésekkel már tényleg 100%-osan kiürítettük a kiadott dokumentumot! Ha ezeket és az előzőleg átbeszélt szabályokat, definíciókat tudod, semmilyen meglepetés nem érhet a teszt során. Sok sikert a ZH-hoz!