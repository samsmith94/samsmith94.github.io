---
layout: post
title:  "Unit tesztelés catch2 és fff segítségével"
date:   2021-04-10 20:00:00 +0700
categories: [tutorial]
---

Ebben a posztban unit tesztelésről fogok írni, azon belül is a catch2 keretrendszer használatáról.
Mivel elég nagy a témakör elég sok minden másról is szó lesz, például
- a mock-olásról (fff)
- a CMake build rendszerről
- code coverage/lefedettség mérésről (gcovr)
- hasznos IDE bővítményekről

### Kitekintés
#### A unit tesztelésről
Nagyon sokféle keretrendszer áll rendelkezésre

![Mappaszerkezet](https://raw.githubusercontent.com/samsmith94/samsmith94.github.io/master/static/img/_posts/unittesting/folder.png)

### Mappaszerkezet




---



Végül nézzünk meg egy teljes példát egy 1 header fájlból és egy forrásfájlból álló könyvtár megírására:

**ssd1306.h**
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
