---
layout: project_single
title:  "Aktivitásmérő és alvásfigyelő okosóra"
slug: "smartwatch"
---
Egy egyetemi tárgyunk (**Önálló Projekt I.**) teljesítéséhez egy saját témát
választottam egy vezeték nélkül tölthető "okosórát", amivel úgy gondolom elég sokat tanulhatok.

A cél, hogy mindent nulláról fejlesszek egy **STM32L4** 32-bites low-power mikrokontrollerre.
Ráadásul nem szeretnék érintőkijelzőt használni, sem gombot nem akarok az órára,
ezért egy **gesture sensor**-ral oldom meg a HMI-t.
Az órának nyilván tudnia kell tudnia pontos időt, dátumot kijeleznie, és lesz benne **stopper**,
**időzítő** is.
Ami érdekes az az ébresztő funkció, mert szeretném ha **okos ébresztőként** tudna funkcionálni, vagyis
az órát a matracra helyezve tudnia kell **megkülönböztetni**e **a REM és non-REM fázisokat** az IMU adatai alapján.
Ezenkívül szeretném, ha a töltő modul szintén a matracon helyezkedne el azaz két legyet ütni egy csapásra: ugyan nem tudom
mennyire lesz energiatakarékos az óra, de az sem probléma, ha naponta kell tölteni, hiszen ha az alvásfigyelő funkció miatt muszály
a matracra (és az azon lévő töltőre) helyezni az órát.

Nem tudom mennyire lehet csak IMU-val megoldani az alvásfigyelést, ezért szerettem volna, ha egy MEMS mikrofon is kerül az órába
(az okostelefonos alkalmazások is figyelik elvileg a környezeti zajokat is), de mivel szeretném ha vízálló is lenne az óra, ezért ezt még nem tudom, hogyan oldom meg.

Az IMU fog egyébként **lépésszámláló**ként és aktivitás monitorozásra is felhasználni.

Amiért belevágok ebbe a projektbe az motiváció, hogy talán én is tudok egy Xiaomi Mi Band szerű órát csinálni, és talán **nem is sokkal drágábban**.
Meg ráadásul én nagyon rossz alvó vagyok és próbáltam egy telefonos alvásfigyelő appot, de aztán egyszer betörtem a kijelzőjét álmomban, még egyet nem akarom összezúzni. :D
Meg hát nagyon meríti is az aksit, ezért töltőre kell tenni, amit nem tartok olyan jó ötletnek. Elég csak arra gondolni, mi van ha kihúzódik a töltőzsínór és arra ébredek, hogy az aksim lemerült teljesen.
A gesture control pedig azért tetszik, mert a saját órámon (ami egyébknt egy Mi Band 3) nem olyan kényelmes a kis érintőképernyő használata.
Meg a gesture control egyedi és menő. :D

Szóval az órában mindenképpen lesz:
- egy OLED kijelző (SSD1306)
- egy pulzusmérő IC (MAX30102)
- egy IMU (MPU6050)
- egy gesture sensor IC (APDS9960)
- vibramotor(ok) a különböző jelzésekhez, értesítésekhez
- fogyasztásmérő
- vezeték nélküli töltőegység

Szeretnék egy **dizájnos órát** tervezni, szóval az óra tokja kb. **3,5 cm * 3,5 cm** lesz
és maximum **1,5 cm vastag**.
Ezen kívül az órához nyilván illene valamilyen **PC-s vagy okostelefonos alkalmazást** is csinálni, ezt valószínűleg Android-ra fogom megcsinálni, illetve a PC-s alkalmazást Spring Boot keretrendszerrel.
Szeretném, ha tudna megjeleníteni:
- alvási statisztikákat
- egyéb aktivitásokat (lépésszám, sport)
- pulzus-, szívritmus-adatokat
- jó lenne, ha a felhasználó testre tudná szabni az óra menürendszerét

14 hetünk van a projektre, amihez egy ütemtervet kell készíteni, de mivel elég sok mindenbe
szeretek egyszerre belekapni, valószínűleg nem fogom tarttani az ütemtervet :):

- **1. hét**: Témaválasztás
- **2. hét**: OLED kijelző, gesture szenzor, és a pulzusmérő firmware-ének implementálása STM32L4 mikrokontrollerre. A vezeték nélküli akkumulátorok töltésének tanulmányozása, a piacon fellelhető alkatrészek kiválasztása. Az óra kapcsolási rajzának megtervezése.
- **3. hét**: OLED kijelző, gesture szenzor, és a pulzusmérő firmware-ének implementálása STM32L4 mikrokontrollerre. A vezeték nélküli akkumulátorok töltésének tanulmányozása, a piacon fellelhető alkatrészek kiválasztása. Az óra kapcsolási rajzának megtervezése.
- **4. hét**: Vibramotor, RTC, ébresztő, stopper és egyéb időzítő funkciók implementálása valamint az IMU firmware-ének implementálása. Vezeték-nélküli töltő kapcsolási rajzának megtervezése. OTA megvalósításának lehetőségeinek vizsgálata.
- **5. hét**: OLED kijelző menü-rendszerének megtervezése. Lépésszámlálás és REM–non-REM alvásfázis megkülönböztetésének implementációja.
- **6. hét**: OLED kijelző menü-rendszerének megtervezése. Lépésszámlálás és REM–non-REM alvásfázis megkülönböztetésének implementációja.
- **7. hét**: Nyomtatott áramköri lemez tervezése.
- **8. hét**: Nyomtatott áramköri lemez tervezése és legyártatása.
- **9. hét**: Óratok 3D tervezése.
- **10. hét**: Óratok 3D tervezése.
- **11. hét**: Webalkalmazás endpoint-jainak megtervezése.
- **12. hét**: Android és/vagy Java Spring Boot kliens implementációja.
- **13. hét**: Android és/vagy Java Spring Boot kliens implementációja.
- **14. hét**: Android és/vagy Java Spring Boot kliens implementációja.