---
layout: post
title:  "C könyvtár tervezése"
date:   2019-09-23 13:00:00 +0700
categories: [tutorial]
---

Ebben a posztban azt fogom megmutatni, hogy hogyan érdemes könyvtárat tervezni C-ben.

Ahhoz, hogy megértsük, miért így érdemes csinálni, értenünk kell a fordítás folyamatát is.

Csapjunk is bele!

### A fordítás folyamata

kép
![SSD1306](https://raw.githubusercontent.com/samsmith94/samsmith94.github.io/master/static/img/_posts/build.jpg)
szöveg
LINKING

---
---

### Mi kerüljön a header-fájlba:

#### 1. Inclusion guard
A legelső mindenképpen az úgynevezett inclusion guard legyen.
Hogy mi is ez? Nos, ha rengeteg file-al dolgozunk, nem biztos, hogy át tudjuk látni,
hogy mit, hova és hányszor inklúdolunk. Simán lehet, hogy beinklúdoljuk a hierarchia
legteteján lévő `top.h` header-t a `middle.h`-ba, majd `bottom.h`-ba beinklúdoljuk a `middle.h`-t de szükségünk van
a `top.h`-ra is, ezért azt is beinklúdoljuk, de elfeledkezünk róla, hogy az már ott van a `middle.h`-ba beinklúdolva.
Vagyis az inclusion guard-al azt érjük el, hogy ne legyen többszörös inklúdolás.
Egyébként inclusion guard-ok nélkül úgyis hibát dobna a fordító, de azért érdemes használni.

#### 2. `#include`-ok
- Ezután jönnek az `#include` direktívák, de figyeljünk arra, hogy csakis annyi inklúdolás legyen,
amivel már fordul a kód, ha ezt az adott header fájlt beinklúdoljuk egy .c fájlba.
Ezekkel arra utasítjuk a fordítót, hogy az utasításban szereplő állomány tartalmát építse be a programunkba.
Szintaktikája:
```c
#include <stdio.h>
```
Ezzel inklúdolhatjuk az ún. system header fájlokat.
A saját magunk által írt header fájlokat pedig így inklúdoljuk:
```c
#include "i2c.h"
```

#### 3. `#define` direktívák
- Ezután jöhetnek a `#define` direktívák, amit más néven szimbolikus konstans, vagy makróknak is neveznek.
Ezek lehetnek változó szerű konstansok és úgynevezett függvényszerű makrók.
Használatuknál figyeljünk a zárójelezésre.

#### 4. Típusdefiníciók
- A következő legyen a típusdefiníciók, ahol én szeretem megtartani a következő sorrendet:
`typedef`-elt primitívek, `typedef`-elt `enum`-ok, `typedef`-elt `struct`-ok, esetleg `union`-ok, bitfield-ek

#### 5. Deklarációk
Ezután jöhetnek a globális változók, majd a függvény prototípusok.
Globális változók természetesen nemcsak primitív típusok lehetnek, hanem általunk korábban
definiált típusok is.

#### Dokumentáció

---
---

### Mi NE kerüljön a header-fájlba:

#### 1. Felesleges `#include`-ok
Mindig csak annyi `#include` legyen a header fájlokban, amennyi éppen szükséges a kód fordulásához.

#### 2. Definíciók
C-ben két dolgot lehet definiálni:
- változókat
- függvényeket
A szabály mindkettőre érvényes.

Ne definiáljunk tehát változókat, csak deklaráljuk őket.
Ne felejtsük el, hogy most a globális névtérről beszélünk, vagyis globális változókról van szó.

Ha C-ben ennyit írunk, hogy:
```c
int var = 5;
```
az már defíníció (értékadó definíció).
De mi van, ha csak ennyit írunk?:
```c
int var;
```
Az a helyzet, hogy már ez is definíció, hiszen a globális névtérben vagyunk.
Ekkor annyi történik, hogy 0 értéket kap automatikusan.
Akkor mégis, hogy a csudába tudunk egy változót csak deklarálni?
Hát így:
```c
extern int var;
```
Na ezt tehetjük a header fájlba.
Bejön tehát az extern kulcsszó és látható, hogy értéket sem adtam a változónak (nem is szabad, hiszen az már definíció lenne).
Az extern kulcsszó használatáról lesz még szó a későbbiekben, de most jöjjenek a függvényekre
vonatkozó definíció-tilalom:

Soha ne fejtsünk ki függvényeket egy header fájlban, azaz ne írjunk ilyeneket:
```c
int fun(int param)
{
  return param * 5;
}
```
Ez ugyanis már definíció.
Álljon csak ennyi helyette:
```c
extern int fun(int);
```
Na ez deklaráció. Nevezik még függvény-prototípusnak, esetleg még a szignatúra kifejezést szokták rájuk használni.
Látható, hogy nemcsak a kapcsos zárójeleket hagytam le, hanem pontosvesszőt is raktam utána,
illetve a paraméter változó nevét elhagytam. Ezt azért tehetjük meg, mert deklarálásnál csak a típusokat
kell, hogy lássa a fordító, nem érdekli a paraméter neve. Az majd a defíníciónál érdekes, ahol esetleg hivatkozunk
a param változóra.
Amit még észrevehettek, az az `extern` kulcsszó használata. Az az érdekes, hogy itt nem lenne muszály kitenni, mint a
globális változók előtt, hiszen az `extern` kulcsszó nélkül megjelölt függvények automatikusan `extern minősítést` kapnak, vagyis
ahová beinklúdoljuk a header fájlunkat, ott használhatók. Én mégis azt javaslom, hogy tegyük ki, használjuk előtte.

#### Bármi, amit `static` kulcsszóval jelölünk meg
Vissza az előbb leírtakra: ha egy változót szeretnénk több forrás között megosztani,
akkor muszály a header fájlba tenni. De szeretnénk betartani azt a szabályt is, hogy nem definiálun semmit (vagyis nem foglalunk helyet)
a header fájlban. Ezért tettük ki a `extern` kulcsszót mind a függvények prototípusa mind a globális változóink neve
előtt.
Amiről az előb nem beszéltem az `extern` kulcsszó kapcsán, hogy mivel a header fájlban csak deklaráció áll, vagyis
még nem történt heyfoglalás, muszály valahol elvégeznünk, hogy egyáltalán tudjuk azt a változót használni.
Erre van a .c fájl, ott már lehet helyet foglalni. De azt se felejtsük el, hogy ez egy megosztott globális változó, vagyis
több .c fájlban is szeretnénk használni.
Éppen ezért, amelyik .c fájlba inklúdoljuk az adott header-fájlt, mindenhol definiálnunk is kell azt:
```c

```
Ha egy globális változót nem szeretnénk megosztani, akkor nem is kell header fájlba tenni, de még tegyünk meg annyit,
hogy megjelöljük `static` kulcsszóval.

```c

```

---
---

### Egyéb fontos szabályok:

#### Globális változók használata
Ha lehet, igyekezzünk minél kevesebb globális változót használni.
Természetesen néha muszály, például ha egy változót
több forrás között szeretnénk megosztani, vagy például mikrokontrollerek
esetén megszakításoknál, hiszen az interrupt-függvények
nem fogadhatnak és nem adhatnak vissza semmilyen változót.
Tehát fontos, hogy egyáltalán nem tilos globális változókat használni, de mindig
fontoljuk meg a használatukat.

#### A static és extern kulcsszók használata
Ez a pont összefüggésben van az előzővel. Mint mondtam, egyáltalán nem tilos globális
változókat használni, de ha egy változót nem kell megosztanunk több forrás között,
vagyis csak 

#### Konstansok használata
C-ben háromféle módon, hozhatunk létre "konstansokat":
- `const` kulcsszó használatával
- `enum` kulcsszó használatával
- `#define` direktívával

Azért tettem idézőjelbe a konstansokat, mert valójában a #define nem igazi konstans.
Ezt bővebben kifejteni...


makró zárójelezés


feltételes fordítás


függvényszerű makró

Az enum-okat azért célszerű használni, mert gyakorlatilag egy értékkészletet teremthetünk egy függvény számára
mind visszatérési értéknek, mind bemenő paraméternek. Ráadásul így beszédes neveket használhatunk.
Ezt bővebbenn kifejteni...

#### Típusdefiníciók sorrendje

struktúra inicializáció
sorrendiség (struct, enum-ok
önhivatkozás
pointer paraméter def illetve defl







Végül nézzünk meg egy teljes példát egy 1 header fájlból és egy forrásfájlból álló könyvtár megírására:

**`ssd1306.h`**
```c
#ifndef SSD1306_H
#define SSD1306_H

#include <stdio.h>
#include "i2c.h"

#define SSD1306_I2C_ADDR			(0b0111100<<1)

#define SET_MEMORY_ADDRESSING_MODE	(0x20)
...
#define SET_MULTIPLEX_RATIO			(0xA8)
...

typedef unsigned char uint8_t;

typedef enum {
    SSD1306_RESULT_OK,
    SSD1306_RESULT_FAIL
} ssd1306_result_t;

typedef enum {
    PAGE_ADDRESSING_MODE,
    HORIZONTAL_ADDRESSING_MODE,
    VERTICAL_ADDRESSING_MODE
} ssd1306_memory_addressing_mode_t;

typedef struct {
    ...
    ssd1306_memory_addressing_mode_t memory_addressing_mode;
    ...
    uint8_t multiplex_ratio;
    ...
} ssd1306_init_struct_t;

extern int shared_var
extern //struct

extern ssd1306_result_t ssd1306_set_memory_addressing_mode(ssd1306_memory_addressing_mode_t);
extern ssd1306_result_t ssd1306_set_multiplex_ratio(uint8_t);
extern ssd1306_result_t ssd1306_init(ssd1306_init_struct_t);

#endif

```

---

**`ssd1306.c`**
```c
#include "ssd1306.h"

int shared_var = 10;
mystruct_t mystruct = ;

static not_shared_var;

#if defined (USE_I2C)
static send_command_via_i2c(uint8_t command);
#elif defined (USE_SPI)
static send_command_via_spi(uint8_t command);
#else
#error You must define either USE_I2C or USE_SPI.
#endif




extern ssd1306_result_t ssd1306_set_memory_addressing_mode(memory_addressing_mode_t memory_addressing_mode)
{
    //
}

extern ssd1306_result_t ssd1306_set_multiplex_ratio(uint8_t multiplex_ratio)
{
    //
}

extern ssd1306_result_t ssd1306_init(ssd1306_init_struct_t ssd1306_init_struct)
{
    //
}

static legyen inkább pl egy segédfüggvény mondjuk paraméter check
static void send_command_via_i2c(uint8_t command)
{
    //
}

static void send_command_via_spi(uint8_t command)
{
    //
}

```

---

**`menu.h`**
```c

```

---

**`main.c`**
```c

```



