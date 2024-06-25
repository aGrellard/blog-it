---
title: PROGMEM
description: Dans le domaine de la programmation des microcontrôleurs, en particulier avec le framework Arduino, le mot-clé PROGMEM joue un rôle crucial dans la gestion efficace de la mémoire.
tags:
  - c++
  - arduino
  - esp8266
---

# Présentation de PROGMEM

Dans le domaine de la programmation des microcontrôleurs, en particulier avec le framework Arduino, le mot-clé PROGMEM joue un rôle crucial dans la gestion efficace de la mémoire. PROGMEM, abréviation de "Program Memory", est utilisé pour stocker des données constantes directement dans la mémoire flash du microcontrôleur plutôt que dans la mémoire SRAM.

## Qu'est-ce que PROGMEM?

`PROGMEM` est une directive de préprocesseur fournie par les compilateurs AVR-GCC utilisés pour programmer les microcontrôleurs de la famille AVR, tels que ceux utilisés dans les cartes Arduino. En déclarant des variables avec `PROGMEM` , les développeurs peuvent optimiser l'utilisation de la mémoire en plaçant les données qui ne changent pas, comme les chaînes de caractères, les tables de données ou les images, dans la mémoire flash plutôt que dans la SRAM.

## Quelle bibliothèque utilise PROGMEM?

Le mot-clé `PROGMEM`  fait partie intégrante de la bibliothèque avr/pgmspace.h dans l'environnement Arduino. Cette bibliothèque fournit les macros et les fonctions nécessaires pour déclarer et accéder aux données stockées en mémoire flash.

## Type de PROGMEM

`PROGMEM`  est en fait un modificateur de type qui indique au compilateur que la variable doit être placée dans la mémoire flash. Par exemple, vous pouvez déclarer une chaîne de caractères en utilisant **`const char`** avec `PROGMEM`  pour la stocker en mémoire flash.

## Rôle de la SRAM

La SRAM (Static Random Access Memory) est une mémoire volatile utilisée pour stocker des variables temporaires, des structures de données et des objets qui peuvent changer pendant l'exécution du programme. La SRAM est limitée en capacité, ce qui signifie que son utilisation doit être optimisée pour éviter les dépassements de mémoire.

### Traitement des structures, objets, variables sans PROGMEM

Sans PROGMEM, toutes les variables, structures et objets sont stockés dans la SRAM. Lorsqu'une variable est déclarée, elle occupe un espace dans la SRAM et peut être modifiée à tout moment pendant l'exécution du programme. Par exemple :

```cpp
char myString[] = "Hello, world!";
```

Dans ce cas, la chaîne de caractères "Hello, world!" est stockée dans la SRAM, ce qui réduit la quantité de mémoire disponible pour d'autres données dynamiques.

### Traitement des structures, objets, variables avec PROGMEM

Avec PROGMEM, les données constantes sont stockées dans la mémoire flash, libérant ainsi de l'espace dans la SRAM. Voici comment cela fonctionne :

```cpp
const char myString[] PROGMEM = "Hello, world!";
```

## Exemple d'utilisation de PROGMEM

Dans ce cas, la chaîne de caractères "Hello, world!" est stockée en mémoire flash. Pour accéder à ces données, des fonctions spécifiques comme pgm_read_byte ou des macros comme PSTR sont utilisées pour lire les données depuis la mémoire flash et les copier dans la SRAM au moment de l'accès.

Voici un exemple de déclaration et d'accès à une chaîne de caractères stockée en mémoire flash avec PROGMEM :


```cpp
#include <avr/pgmspace.h>

const char myString[] PROGMEM = "Hello, world!";

void setup() {
  Serial.begin(9600);
  // Copie la chaîne de caractères de la mémoire flash à la SRAM
  char buffer[20];
  strcpy_P(buffer, myString);
  Serial.println(buffer);
}

void loop() {
  // Code principal
}
```

Dans cet exemple, la chaîne de caractères est initialement stockée en mémoire flash. Lors de l'exécution du programme, strcpy_P copie cette chaîne de la mémoire flash vers un buffer en SRAM, où elle peut être utilisée par le programme.