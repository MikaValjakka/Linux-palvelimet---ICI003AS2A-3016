## Vapaaehtoinen bonus: suosikkiohjelma Linuxilla – GCC

**GCC (GNU Compiler Collection)** on yksi tärkeimmistä Linux-ympäristön kehitystyökaluista. Sitä käytetään C-, C++- ja useiden muiden ohjelmointikielten kääntämiseen. GCC kuuluu GNU-projektiin ja on oletuksena saatavilla useimmissa Linux-jakeluissa.
On myös aika näpsäkkä

### Esimerkkiohjelma: hello_world.c

Alla yksinkertainen C-ohjelma, joka tulostaa tekstin ruudulle:

```c
#include <stdio.h>

int main(void) {
    printf("So if you see my downeaster "Alexa"\n
              And if you work with the rod and the reel\n
              Tell my wife I am trolling Atlantis\n
              And I still have my hands on the wheel\n");
    return 0;
}
```

### Yksinkertainen käännös ja ajo

```bash
gcc hello_world.c -o hello_world
./hello_world
```
Tämä kääntää ohjelman ilman lisävaroituksia tai debug-tietoja ja tuottaa suoritettavan binääritiedoston.

### Käännös hyödyllisillä GCC-lipuilla

```bash
gcc -Wall -Wextra -Wpedantic -std=c11 -g -O2 hello_world.c -o hello_world
```

## Käytettyjen lippujen selitys

-Wall
Ottaa käyttöön yleisimmät kääntäjän varoitukset.

-Wextra
Lisää tarkempia varoituksia mahdollisista virheistä.

-Wpedantic
Pakottaa C-standardin tiukan noudattamisen.

-std=c11
Määrittää käytettäväksi C11-standardin.

-g
Lisää debug-symbolit ohjelman virheenkorjausta varten.

-O2
Optimoi ohjelman suorituskykyä.

-o hello_world
Määrittää luotavan ohjelmatiedoston nimen.

## Debuggaus GDB:llä

Kun ohjelma on käännetty -g-lipulla, sitä voidaan debugata GNU Debuggerilla seuraavasti:

```
gdb ./hello_world
```
Tämä mahdollistaa ohjelman ajamisen askel askeleelta sekä muuttujien ja ohjelman tilan tarkastelun.

### Yhteenveto

GCC on keskeinen työkalu Linux-kehityksessä. Jo yksinkertainen esimerkkiohjelma havainnollistaa sen käyttöä.

