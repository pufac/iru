Szuper, a típusfeladatokon túl vagyunk! A dokumentum feldolgozása során összegyűjtöttem a legfontosabb elméleti fogalmakat, definíciókat és technológiai összehasonlításokat. Ezt a részt tematikusan bontottam szét, hogy egy igazi "puskázható" vagy könnyen memorizálható fogalomtárat kapj.

Íme a ZH-hoz szükséges elméleti "Cheat Sheet":

### 1. Adattárolás és Virtualizáció
* **NAS (Network Attached Storage):** Hálózaton elérhető nagykapacitású tároló terület.
* **SAN (Storage Area Network):** Adatforgalomra dedikált hálózat. A SAN hálózatokban (és a konszolidált hálózatokban is) a tároló eszközök az egyes szerverektől fizikailag elválnak.
* **IP SAN:** Olyan technológia, amelyet akkor használunk, ha több, egymástól távol lévő telephely között szeretnénk blokkos adatmásolatokat továbbítani a biztonság növelése érdekében.
* **Dedikált tároló eszköz:** Egy adott szerverhez rendelt tároló.
* **In-band virtualizáció:**
    * *Előnye:* Egyszerű a helyreállítás meghibásodás után.
    * *Hátránya:* Lassú a működése.
    * *Működése:* Blokkszintű hozzáférést biztosít.
* **Out-of-band virtualizáció:**
    * *Előnye:* Gyors. (Emiatt például nagy felbontású filmek streameléséhez is ezt kell választani ).
    * *Hátránya:* Speciális vezérlést igényel.

### 2. Mentés, Archiválás és Pillanatképek (Snapshots)
* **Archiválás célja és jellemzői:** Jogszabályban rögzített ok miatt dokumentumok hosszú távú megőrzésére szolgál. Archiválás során valódi másolat készül, és tipikusan passzív adatokat mentünk.
* **Mágnesszalag (Tape):** Archiválási célokra elsősorban azért használják, mert a szalagon az adatok tárolása nem igényel áramot/energiát. Az archív fájlokat tipikusan mágnesszalagokon tárolják. A művelet (archív másolat készítése diszkről szalagra) akkor a leggyorsabb, ha a diszk olvasási sebessége megegyezik a szalag írási sebességével.
* **Split Mirror:** Ezt a technikát nagyméretű adatbázisok mentésére használják , illetve ezzel valósítják meg a zero-down-time (leállás nélküli) mentést is.
* **Progresszív mentés:** Legnagyobb előnye, hogy a helyreállításnál a törölt fájlok már nem jelennek meg.
* **Volume Copy:** Back-up, analitikai és adatbányászati alkalmazáshoz is jó, mivel teljes másolatot készít.
* **Kollokáció (Collocation):** Adatmentés során azt jelenti, hogy az egy klienshez tartozó adatokat egy szalagra másoljuk.
* **FlashCopy tábla:** Egy fájl egy változatának blokkjaira mutató táblázat. A Flash Copy technika elsősorban verziókezelésre használatos.

### 3. Hardver, Szerverek és Adatközpont
* **SSD (Solid State Drive) memóriák:**
    * Kapacitásukat adatkompresszióval (tömörítéssel) növelik.
    * Egységnyi input-output (I/O) műveletre eső ára kisebb, mint a diszkeké.
    * Működtetési költsége alacsony.
    * Megbízhatóságát RAID segítségével növelik (nem elég önmagában a megbízhatósága).
* **Hibrid tömb (Hybrid Array):** Egy tipikus hibrid tömb esetén a megoszlás kb. 5% SSD és 95% HDD.
* **Blade szerverek:**
    * Azért, hogy tehermentesítsék az adatközpontok hűtőrendszerét, nagyteljesítményű saját hűtéssel rendelkeznek.
    * **Trükkös kérdés a ZH-ban:** Maga a "server blade" tipikusan **NEM** tartalmaz diszket. Azonban a szervereket befogadó **keret (chassis)** tipikusan tartalmaz diszket.
* **Top of Rack (ToR) kétszintű struktúra:** Ennek a megoldásnak a legnagyobb hátránya, hogy tipikus eszközökkel csak relatíve kis méretű adatközpont alakítható ki így.
* **RAID rendszerek védelme:** Fontos tudni, hogy egyik RAID technológia sem véd a logikai hibák ellen!. Továbbá a RAID 0 nem használható redundáns rendszerek kialakítására.

### 4. Hálózat, Architektúrák és Protokollok
* **OSI rétegek a gyakorlatban:**
    * **1. réteg (Fizikai):** Hub.
    * **2. réteg (Adatkapcsolati):** Bridge (Híd).
    * **3. réteg (Hálózati):** Router.
* **Hub vs. Bridge:** Mikor jobb egy hub? Akkor előnyösebb, ha protokoll analizátort kell a hálózatra illesztenünk.
* **DNS (Domain Name System):** A DNS használatával szimbolikus nevekhez rendelhetünk IP címet.
* **DHCP:** Segítségével a kliens IP címe mellett a szerver megadhatja a használandó Name Server vagy az alapértelmezett átjáró (Gateway) címét is.
* **NAT (Network Address Translation):** Akkor van szükség a portok módosítására is (PAT), ha a router nem rendelkezik elegendő nyilvános IP címmel.
* **VPN Típusok:**
    * **Remote-Access VPN:** Használókat csatlakoztat a hálózathoz , és szükség van speciális program telepítésére a kliens gépen.
    * **Site-to-Site VPN:** Telephelyeket kapcsol össze , és használatakor nincs szükség speciális program telepítésére a kliens gépeken.
* **Adatközponti Topológiák:**
    * **Spine and Leaf:** Legnagyobb előnye a "full mesh" (szövevényes) hálózattal szemben, hogy redundáns útvonalak vannak benne. (Ezt válaszd HPC - High Performance Cluster szolgáltatáshoz ).
    * **Fat Tree:** Videómegosztó szolgáltatás kiépítéséhez ezt a háromszintű hálózatot érdemes választani.
    * **Lánc topológia:** Az a topológia, amelyikbe alapértelmezésben nincs beépítve semmilyen védettség.

### 5. Adattípusok osztályozása
* **Aktív adatok:** Alkalmazás adatbázis, fejlesztés alatt álló szoftverek.
* **Passzív adatok:** Mentések, archívumok, szkennelt képek, biztonsági videó állományok.
* **Referencia adatok:** Riportok, tranzakciós jelentések, publikált dokumentumok, weboldalak, e-mail üzenetek, audió/videó felvételek.