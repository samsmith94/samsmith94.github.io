---
layout: project_single
title:  "Robotpilóta és OSD fejlesztés LoRa alapú távirányítással"
slug: "autopilot"
---
Körülbelül 5 éve vagyok beleszerelmesedve az FPV repülésbe, ennek ellénre eddig csak egy egyszerű slowflyerem volt,
ami nagyon lomhán repült, ráadásul nem is robotpilótával.
Csak mostanában szántam rá magam, hogy jól bevásároljak, úgyhogy 4 hónapja vettem egy quadcopter vázat, meg mindent
ami szükséges hozzá (akkumulátor, motorvezérlő, motorok, mini FPV kamera adóval együtt) és még külön vettem egy akrobatikus, tolómotoros hab gépet is 3 hete.
Meg egy akciókamerát, hogy jó minőségben tudjam rögzíteni a repüléseimet.

Egy dolgot nem vettem: flight controllert. De nem azért, mert elfelejtettem, hanem mert
azt saját magam szeretném megtervezni (a hardvert és a szoftvert is).

Ráadásul nem csak az a cél, hogy működjön valamennyire. Sokkal ambíciózusabb terveim voltak:
- szeretném ha a modulon a **firmware-frissítés és a konfiguráció** miatt lenne egy **bluetooth** modul
- jó lenne, ha a flight controller működne **mindenféle típusú géppel** és konfigurációval: tricopter, **quadcopter**, octocopter és **merevszárnyas géppel** is
- idegesít, hogy **4 motorhoz 4 ESC**-et kell bekábeleznem és még külön kell **feszültség és áramszenzor** a biztonságos repüléshez, ezért szeretném, hogy
**egy board-ra** kerüljenek a következő funkciók: 4 BLDC vezérlő, energiaelosztó, áramszenzor, feszültségszenzor
- szeretném megoldani, hogy **a létező összes RC-s** vevő-protokollal kompatibilis legyen (**PWM, PPWM, SBus, IBus**, stb...)
- jó lenne tartani a szabványos **36 mm * 36 mm-es** méretet minden egyes board-nál
(mivel természetesen nem tudok rátenni egy ekkora board-ra minden alkatrészt), ezért a terv a következő:
	- legyen **legalul** a **motorvezérlő board**
	- **felette** egy board ami maga a **flight controller mikrovezérlővel, 9-DOF IMU-val (gyroscope, accelerometer, magnetic compass),
	légnyomásmérővel, SD-kártya foglalattal, bluetooth modullal**
	- később szeretném, ha lenne **efölött** egy **OSD modul** is, amivel a lesugárzott videójelre rá tudnék keverni telemetria adatokat
	- a **legtetején** kell, hogy legyen a **GPS**, hiszen mindig rálátással kell hogy legyen az égre

Egyébként ami mindenképpen kell az nyilván a **mikrokontroller**, ami az irányítás lelke, a **giroszkóp** és a **gyorsulásmérő**.
Ami nem feltétlenül muszály, de nagyon hasznos az elsődlegesen egy **GPS** modul, **OSD** (dedikált mikrokontrollerrel, vagy OSD-chip-pel),
**légnyomásmérő**, **mágneses iránytű** és valamilyen **távolságmérő szenzor** (infravörös, vagy ultrahangos, esetleg time-of-flight szenzor) az automatikus leszálláshoz.

---
---

#### És most jön a poén.

Rendeltem egy olcsó (~15000 Ft) 6-csatornás távirányító-vevő párost is, de az nem érkezett meg, és mivel tracking number nélkül rendeltem, (hogy olcsóbb legyen)
már baszhatom. Valószínűleg soha nem fog már megérkezni és még csak reklamálni sem tudok.
De ahelyett, hogy felbasztam volna ezen magam, eszembe jutott, hogy rendeltem 3 hónapja egy **LoRa modult** (tranceiver pár).
Csak úgy. Nem tudtam, mire fogom használni. Az természetesen akkor eszembe se jutott, hogy **RC modellezésben** is hasznát tudom venni.

De végülis adja magát. Az FPV miatt **messzire szeretnék repülni**, és mivel **robotpilótát** szeretnék használni, az sem kritikus,
ha nem tudok minden mikroszekundumban irányító parancsot küldeni neki.

#### A célom, hogy kb. 4 hónap alatt elkészüljön a *version1*, amiben a következő funkciók lennének:

- **RC**-s protokollok közül csak **PWM és PPM**
- quadcopter és merevszárnyas beállítás
- **6-DOF IMU**
- a **légnyomásmérő** és az **iránytű**, illetve a **GPS** adatait **csak telemetriára** használnám, semmi szenzorfúzió meg ilyesmi
- **USB-s joystick**
- egy motorvezérlő board (4 BLDC motor controller, power distribution, current-sensor, voltage-sesnor
- egy flight controller board
- egy GPS board, a LoRa-val együtt

---
---

Miközben elkezdem tervezni a 3 board-ot, (majd legyártatni Kínában, ami kb. 1 hónap alatt jön meg) azért szeretném
már tesztelni a moduljaimat, de nem csak külön-külön, hanem úgy együtt, hogy akár már fel is tudjon szállni.
Ehhez már egyébként meg is vannak a moduljaim, csupán egy nyomtatott áramkört kell hozzá csinálni (amit házilag fogok kivitelezni, **annak a tervét látjátok a borítóképen**) egy csomó header-rel, hogy
ezeket a modulokat később bármi másra fel tudjam majd használni.

#### Szóval a következő eszközöket fogom a version0.1-es flight-controllerre tenni:

- egy Xnucleo board (F401RE típusú mikrokontrollerrel)
![STM32F401RE](https://raw.githubusercontent.com/samsmith94/samsmith94.github.io/master/static/img/_posts/autopilot/STM32F401RE.jpg){:.thumbnail.bordered :height="150px" width="150px"}
- 9-DOF IMU: Ivensense MPU-9250
![MPU-9250](https://raw.githubusercontent.com/samsmith94/samsmith94.github.io/master/static/img/_posts/autopilot/MPU-9250.jpg){:.thumbnail.bordered :height="150px" width="150px"}
- légnyomás (és magasság-mérő): Bosch Sensortec BMP-280
![BMP-280](https://raw.githubusercontent.com/samsmith94/samsmith94.github.io/master/static/img/_posts/autopilot/BMP-280.jpg){:.thumbnail.bordered :height="150px" width="150px"}
- GPS: GY-NEO6MV2
![GY-NEO6MV2](https://raw.githubusercontent.com/samsmith94/samsmith94.github.io/master/static/img/_posts/autopilot/GY-NEO6MV2.jpg){:.thumbnail.bordered :height="150px" width="150px"}
- bluetooth: RN-42
![RN-42](https://raw.githubusercontent.com/samsmith94/samsmith94.github.io/master/static/img/_posts/autopilot/RN-42.jpg){:.thumbnail.bordered :height="150px" width="150px"}
- telemetria adó-vevő páros: APC-220
![APC-220](https://raw.githubusercontent.com/samsmith94/samsmith94.github.io/master/static/img/_posts/autopilot/APC-220.jpg){:.thumbnail.bordered :height="150px" width="150px"}
- távolságmérő szenzor: HC-SR04
![HC-SR04](https://raw.githubusercontent.com/samsmith94/samsmith94.github.io/master/static/img/_posts/autopilot/HC-SR04.jpg){:.thumbnail.bordered :height="150px" width="150px"}
- Lora modul: HopeRF RFM95
![RFM-95](https://raw.githubusercontent.com/samsmith94/samsmith94.github.io/master/static/img/_posts/autopilot/RFM-95.jpg){:.thumbnail.bordered :height="150px" width="150px"}

A motor controller + current-sensor + voltage-sensor board-nak majd később állok neki.

Ami nem még biztosan **nem lesz benne**, az az **OSD** modul. Viszont mégis szeretnék valami ilyesmi funkciót,
ezért **külön** fogom **lesugározni** a **kamerajelet**, és **külön elküldeni a telemetriaadatokat**, és **PC-oldalon** szeretném
**rárajzolni** ezeket a fogadott kameraképre.
PC-oldalon szeretém azt is megoldani, hogy joystick-kal is irányíthassam a gépeimet.

#### És majd a *version2*-ben:
- **további RC-s protokollok** támogatása (SBus, IBus)
- további multirotor konfigurációk
- **szenzorfúzió** alkalmazása, ahol csak lehet
- **hazarepülés** funkció
- **útvonalrepülés**
- PC-s szoftver a repülés-konfigurációhoz
- **mentő-ejtőernyő**
- **OSD board**
- joystick-hoz külön USB-host-shield(MAX3421E)
![usb-host-shield](https://raw.githubusercontent.com/samsmith94/samsmith94.github.io/master/static/img/_posts/autopilot/usb-host-shield.jpg){:.thumbnail.bordered :height="150px" width="150px"}

#### Amiket rendeltem

##### Banggood-ról:
- [Eachine RTOG01 5,8 GHz FPV vevő](https://www.banggood.com/Eachine-ROTG01-UVC-OTG-5_8G-150CH-Full-Channel-FPV-Receiver-For-Android-Mobile-Phone-Smartphone-p-1147692.html?rmmds=myorder&ID=224&cur_warehouse=CN): 18.17 $
- [APC220 Adó-vevő páros](https://www.banggood.com/APC220-Wireless-Data-Communication-Module-USB-Adapter-Kit-For-Arduino-p-939407.html?rmmds=myorder&cur_warehouse=CN): 20.99 $
- [GPS-modul](https://www.banggood.com/GY-NEO6MV2-Flight-Controller-GPS-Module-For-Arduino-EEPROM-MWC-APM-2_5-p-915384.html?rmmds=myorder&cur_warehouse=CN): 12.66 $
- [USB Host Shield](https://www.banggood.com/USB-Host-Shield-Compatible-For-Google-Android-ADK-Support-U-NO-MEGA-Module-p-1384907.html?rmmds=myorder&cur_warehouse=CN): 7.99 $
- [5.8 GHz FPV Transmitter és kamera](https://www.banggood.com/EWRF-TS5823-5_8G-40CH-200mW-600mW-FPV-Transmitter-VTX-With-COMS-1000TVL-Camera-For-RC-Drone-p-1388365.html?rmmds=myorder&ID=558324&cur_warehouse=CN): 19.56 $

##### Hobbyking-től:
- [4 az 1-ben BLDC motor controller](https://hobbyking.com/en_us/turnigy-multistar-blheli-32-4-in-1-32bit-31a-11g-race-spec-esc-2-4s.html): 37.74 €
- [BLDC motorok](https://hobbyking.com/en_us/floater-jet-replacement-motor-axn-2208-2150.html): 4 * 9.31 €
- [Quadcopter Kit](https://hobbyking.com/en_us/hobbykingtm-totem-q250-quadcopter-kit.html): 10.49 €
- [Li-Po akkupakk](https://hobbyking.com/en_us/turnigy-2200mah-3s-25c-lipo-pack.html): 9.85 €
- propellerek
- propeller balanszer
- glue-and-go repcsi
- hozzávalók
- [](): €
- [](): €

##### Minden mást a HE Store-ból rendeltem, illetve személyesen vettem [modellboltokban](http://jakomodell.hu/), ahol nagyon segítőkész és felkészült eladó vár minden kedves vásárlót.

- 500 Ft ...

---
---

A quadcopter építéséről és a flight controller hardveres és szoftveres tervezéséről szeretnék
**hetente írni egy-egy cikket** a **KiCAD** és az **STM32** programozásáról, amelynek a

#### Tartalma:
- **STM32CubeIDE** fejlesztőkörnyezet
- **Digitális ki-, és bemenetek** használata, illetve **külső megszakítások**
- **Időzítő-számlálók** használata (alapvető működésük, **PWM**, **Input Capture**)
- Kommunikációs protokollok használata:
	- **UART**
	- **I2C**
	- **SPI**
- **Debuggolás**i és programfeltöltési lehetőségek
- **Valós-idejű operációs-rendszerek** használata
- Saját **schematic könyvtár** és **alkatrész footprint** tervezése
- **Kapcsolási rajz** tervezés
- **Nyomtatott áramköri** lemez tervezése


