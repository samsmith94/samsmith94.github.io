---
layout: post
title:  "Unit tesztelés catch2 és fff segítségével"
date:   2021-04-10 20:00:00 +0700
categories: [tutorial]
---

Ebben a posztban a unit tesztelésről fogok írni, azon belül is a catch2 keretrendszer használatáról.
Mivel elég nagy a témakör sok minden másról is lesz szó, így például:

- a mock-olásról (fff)
- a CMake build rendszerről
- code coverage/lefedettség mérésről (gcovr)
- hasznos IDE bővítményekről

#### Kitekintés

##### A unit tesztelésről

Nagyon sokféle keretrendszer áll rendelkezésre


#### Mappaszerkezet

![Mappaszerkezet](https://raw.githubusercontent.com/samsmith94/samsmith94.github.io/master/static/img/_posts/unittesting/folder.png)

---

Végül nézzünk meg egy teljes példát egy 1 header fájlból és egy forrásfájlból álló könyvtár megírására:

**ssd1306.h**

```c
#define CATCH_CONFIG_MAIN

#include "catch_amalgamated.hpp"
#include "../src/adder.h"

#include "fff.h"
DEFINE_FFF_GLOBALS;

FAKE_VALUE_FUNC(int, dependency_enable_sum, int);


SCENARIO( "adder can add", "[adder module]" ) {

    RESET_FAKE(dependency_enable_sum);

    GIVEN( "sum is disabled" ) {
        int a = 1;
        int b = 2;
        dependency_enable_sum_fake.return_val = 0;
        WHEN( "add is called and operation is enabled" ) {
            int res = add(1, 2);
            THEN( "the sum is 0" ) {
                REQUIRE( 0 == res);
            }
        }
    }

    GIVEN( "sum is enabled" ) {
        int a = 1;
        int b = 2;
        dependency_enable_sum_fake.return_val = 1;
        WHEN( "add is called and operation is disabled" ) {
            int res = add(1, 2);
            THEN( "the sum is 3" ) {
                REQUIRE( 4 == res);
            }
        }
    }
}

```
