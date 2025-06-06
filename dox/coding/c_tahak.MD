# Jazyk C

(Toto zdaleka není veškerý výčet funkcí a vlastností daného programovacího jazyka. Jedná se pouze o&nbsp;mé zápisky vzniklé během řešení [AoC](https://github.com/RDMCz/AdventOfCode) a/nebo studia na VŠ.)

* Kompilovaný (obvykle), staticky typovaný, strukturovaný, ... programovací jazyk
* Obsahuje minimum abstrakce / syntaktického cukru
* Poskytuje/vyžaduje přímou kontrolu nad (de)alokací paměti
* Několik standardů a kompilátorů

## Základní datové typy

* Standard jazyka C určuje pouze minimální velikosti/rozsahy datových typů a vztahy mezi nimi
  * Nejsou specifikovány přesné a pevné a velikosti, to už záleží na architektuře/OS/...
  * Proto např. někde `int` má 16 bitů a někde 32 bitů
  * Pokud si nejsme jistí, můžeme použít makra z `<limits.h>`

Typ|Min. bitů|Minimální rozsah
:--|--:|--:
`char`|8|—
`signed char`|8|-127&ensp;až&ensp;+127
`unsigned char`|8|0&ensp;až&ensp;+255
`short`|16|-32 767&ensp;až&ensp;+32 767
`unsigned short`|16|0&ensp;až&ensp;+65 535
`int`|16|-32 767&ensp;až&ensp;+32 767
`unsigned int`|16|0&ensp;až&ensp;+65 535
`long`|32|−2 147 483 647&ensp;až&ensp;+2 147 483 647
`unsigned long`|32|0&ensp;až&ensp;+4 294 967 295
`float`|—|—
`double`|—|—
`long double`|—|—

* Poznámky k tabulce:
  * `char` je typicky použit pro uchování jednoho znaku; není specifikováno, zdali je un/signed
  * `signed char` je char, kde je zaručeno, že bude znaménkový
  * `unsigned char` je char, kde je zaručeno, že bude bezznaménkový
  * `float` je obvykle IEEE 754 single (32 bit)
  * `double` je obvykle IEEE 754 double (64 bit)
  * `long double` je obvykle extended (80 bit), nebo quadruple (128 bit)
* Zároveň by mělo platit `sizeof(char) <= sizeof(short) <= sizeof(int) <= sizeof(long) <= sizeof(long long)`
* Některé standardy rozšířily jazyk o nové typy, C99 přidal např.:
  * `long long`, min. 64 bitů
  * `int16_t` apod., umožňující specifikovat přesný počet bitů
  * `bool`

## Pointer (ukazatel)

* Pointer ukazuje na místo v paměti
* Hodnota pointerové proměnné je adresa
  * Na této adrese se nachází hodnota toho, na co pointer ukazuje
* Operátor `*` slouží pro definici pointeru a zároveň pro dereferenci
  * Dereference je přístup k hodnotě toho, na co pointer ukazuje
* Operátor `&` slouží k získání adresy proměnné
* Jazyk C podporuje pointerovou aritmetiku
* V jazyce C se při volání metody předávají parametry hodnotou, bez pointerů se tedy neobejdeme

```c
void add_ten(int num) {
    num += 10;
}

void main() {
    int num = 0;
    addTen(num);
    printf("%d\n", num); // 0
}

/* Metoda `add_ten` sice přičte k proměnné `num` číslo 10,
   toto `num` je ale zkopírované a na původním `num` z metody `main` se změna neprojeví.
   Je třeba využít pointeru: */

void add_ten(int* num_p) {
    *num_p += 10;        // Napsáním hvězdy před název pointeru provedeme dereferenci
}

void main() {
    int num = 0;
                         // Napsáním hvězdy při deklaraci mezi datový typ a název proměnné vytvoříme pointer
    int* num_p = &num;   // Hodnota num_p je adresa num („num_p ukazuje na num“)
    addTen(num_p);
    printf("%d\n", num); // 10
}
```

* Součástí definice pointeru je tedy i datový typ, na který pointer ukazuje
  * Void pointer `void*` lze použít pro ukazování na libovolný typ

## Dynamická alokace paměti

* (zjednodušeně) Operační systém každému procesu poskytuje přístup k části paměti, která je rozdělena na stack a heap
* Dynamická alokace paměti je přidělování paměti z heapu za běhu programu
* Paměť lze přidělit pomocí funkce `malloc`
  * Parametr je bezznaménkové číslo, které udává, kolik bajtů chceme alokovat
  * Vrací void pointer na první prvek právě alokované paměti
  * Případně vrací NULL, pokud se alokace nezdařila
* Jakmile už část paměti nepotřebujeme, měli bychom ji uvolnit funkcí `free`
  * Její parametr je void pointer ukazující na začátek alokované paměti
* Po dealokaci je vhodné původní pointer nastavit na NULL, aby neukazoval na nealokované místo v paměti
  * Pokud tak učiníme bez dealokace, vzniká nám memory leak (máme alokovanou paměť, ale nemáme se k ní jak dostat)

## Pole

* Fixed-length pole musí mít pevně danou velikost, nelze ji určit proměnnou – `int pole[9];`
* Dynamická pole:

```c
// Alokace pole o velikosti size
int size = 9;
int* array_p;
array_p = (int*)malloc(size * sizeof(int));
if (array_p == NULL) return 1;

// Naplnění pole
for (int i = 0; i < size; i++) {
    // array_p je ukazatel na první prvek pole, přičítáním k jeho hodnotě se dostáváme na adresy následujících prvků v poli
    *(array_p + i) = i;
    // Je možné použít i tento zápis (méně demostrativní ale obvyklejší)
    array_p[i] = i;
}

// Reverse pole (pomocí postupného prohazování krajních čísel)
int* left = array_p;
int* right = array_p + size - 1;
for (int i = 0; i < size / 2; i++) {
    int temp = *left;
    *left = *right;
    *right = temp;
    left++;
    right--;
}

// Dealokace pole
free((void*)array_p);
array_p = NULL;
```

* Pokud bychom chtěli pole alokovat v jiné metodě, potřebujeme pointer na pointer:

```c
#include <stdlib.h>

void array_alloc(int** array_p_p, int size) {
    *array_p_p = (int*)malloc(size * sizeof(int));
}

void main() {
    // Alokace pole o velikosti size
    int size = 9;
    int* array_p;
    array_alloc(&array_p, size);
}
```

* Velikost dynamického pole se nedá nijak zjistit, musíme ji mít uloženou někde v proměnné, nebo pro ní vyhradit např. první položku v poli

### 2D pole

* Opět možné fixed-length `int pole[3][4];`
  * Prvky prvního rozměru jsou pointery na 1D pole
* Dále lze kombinovat fixed-length/dynamický přístup, nebo udělat pole kompletně dynamicky:

```c
int radky = 3;
int sloupce = 4;

int** pole = (int**)malloc(radky * sizeof(int*));

for (int i = 0; i < radky; i++) {
    *(pole + i) = (int*)malloc(sloupce * sizeof(int));
}

char value = 'A';

for (int y = 0; y < radky; y++) {
    for (int x = 0; x < sloupce; x++) {
        *(*(pole + y) + x) = value++;
    }
}

for (int y = 0; y < radky; y++) {
    for (int x = 0; x < sloupce; x++) {
        printf("%c", *(*(pole + y) + x));
    }
    printf("\n");
}

/*
ABCD
EFGH
IJKL
*/ // Při dealokaci je nutné nejprve uvolnit jednotlivá 1D „podpole“ a pak až samotné `pole` (jinak máme memory leak)
```

## Řetězce

* 1D pole tvořené prvky typu char
  * Alokace `char str[10];` nebo `char* str = (char*)malloc(10 * sizeof(char));`
* Řetězec musí končit znakem `\0`
  * Pokud v kódu píšeme řetězec do dvojitých uvozovek, přidá se znak automaticky
  * Manuálně je znak potřeba přidávat na konec, pokud s řetězcem pracujeme jako s polem
* Funkce z knihovny `string.h`
  * `strlen` (délka)
  * `strcpy` (kopírovat)
  * `strcat` (spojit)
  * `strch` (hledat znak)
  * `strcmp` (porovnat)
  * `strstr` (hledat podřetězec)
  * `strtok` (~split)

## Strukturovaný datový typ

* Umožňuje uchovat více hodnot různých typů pod jednou proměnnou

```c
// Definice struktury
struct datum {
    int den;
    int mesic;
    int rok;
};

// Vytvoření struktury – automatická alokace
struct datum moje_datum;

// Abychom nemuseli psát `struct datum`, můžeme si zavést vlastní typ
typedef struct datum Datum;
Datum velky_tresk;

// Nastavení a výpis hodnot – automatická alokace
velky_tresk.den = 1;
velky_tresk.mesic = 1;
velky_tresk.rok = 1970;
printf("%i. %i. %i\n", velky_tresk.den, velky_tresk.mesic, velky_tresk.rok);

// Vytvoření struktury – dynamická alokace
Datum* konec_sveta = (Datum*)malloc(sizeof(Datum));
if (konec_sveta == NULL) return 1;

// Nastavení a výpis hodnot – dynamická alokace
(*konec_sveta).den = 19; // Možný styl zápisu
konec_sveta->mesic = 1;  // Preferovaný styl zápisu pomocí šipky
konec_sveta->rok = 2038;
printf("%i. %i. %i\n", konec_sveta->den, konec_sveta->mesic, konec_sveta->rok);

// Úklid
free((void*)konec_sveta);
konec_sveta = NULL;

// Pole struktur
Datum* pole = (Datum*)malloc(12 * sizeof(Datum));
int i;
for (i = 0; i < 12; i++) {
    (*(pole + i)).rok = 2024;  // Možný styl zápisu
    (pole + i)->mesic = i + 1; // Preferovaný styl zápisu
}
free((void*)pole);
pole = NULL;
```

## Terminál

```c
#define _CRT_SECURE_NO_WARNINGS

#include <stdio.h>

void main() {
    printf("Zadejte int, float a double oddelene mezerami:\n")

    int i;
    float f;
    double d;

    (void)scanf("%d %f %lf", &i, &f, &d);

    printf("Zadali jste %d, %f a %lf\n", i, f, d);
}
```

* Formátové specifikace:
  * `%c` – char
  * `%d` – signed int
  * `%u` – unsigned int
  * `%f` – float
  * `%ld` – signed long
  * `%lu` – unsigned long
  * `%lf` – double

## Soubory

```c
// Textový – zápis
FILE* fwt;
fwt = fopen("soubor.txt", "w");
fprintf(fwt, "%d %f %lf", i, f, d);
fclose(fwt);

// Textový – čtení
FILE* frt;
frt = fopen("soubor.txt", "r");
(void)fscanf(frt, "%d %f %lf", &i, &f, &d);
fclose(frt);

// Binární – zápis
FILE* fwb;
fwb = fopen("soubor.bin", "wb");
fwrite(&i, sizeof(i), 1, fwb); //ukazatel na první prvek (odkud bereme blok dat pro zápis), velikost prvku, počet prvků, stream
fwrite(&f, sizeof(f), 1, fwb);
fwrite(&d, sizeof(d), 1, fwb);
fclose(fwb);

// Binární – čtení
FILE* frb;
frb = fopen("soubor.bin", "rb");
fread(&i, sizeof(i), 1, frb);
fread(&f, sizeof(f), 1, frb);
fread(&d, sizeof(d), 1, frb);
fclose(frb);
```

## Random

```c
srand(time(NULL)); // Nastavení seedu pro generaci pseudonáhodných čísel; time(NULL) vrací unix timestamp
int randomNumber = (rand() % (max - min)) + min; // Generuje číslo mezi min [inclusive] a max [exclusive]
```
