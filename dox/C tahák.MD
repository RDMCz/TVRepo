# Jazyk C

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
FILE* fwt;
fwt = fopen("soubor.txt", "r");
(void)fscanf(fwt, "%d %f %lf", &i, &f, &d);
fclose(fwt);

// Binární – zápis
FILE* fwb;
fwb = fopen("soubor.bin", "wb");
fwrite(&i, sizeof(i), 1, fwb); // ukazatel na první prvek (odkud bereme zapisovaný blok dat), velikost prvku, počet prvků, file stream
fwrite(&f, sizeof(f), 1, fwb);
fwrite(&d, sizeof(d), 1, fwb);
fclose(fwb);

// Binární – čtení
FILE* fwb;
fwb = fopen("soubor.bin", "rb");
fread(&i, sizeof(i), 1, fwb);
fread(&f, sizeof(f), 1, fwb);
fread(&d, sizeof(d), 1, fwb);
fclose(fwb);
```

## Ukazatele

```c
void addTen(int num) {
    num += 10;
}

void main() {
    int num = 0;
    addTen(num);
    printf("%d\n", num); // 0
}
```

* Takto se změna num metodou addTen v mainu neprojeví, je třeba použít pointer:

```c
void addTen(int* num_p) {
    *num_p += 10; // Napsáním hvězdy před pointer přistupujeme k hodnotě toho, na co pointer ukazuje
}

void main() {
    int num = 0;
    int* num_p = &num; // Hodnota num_p je adresa num (num_p ukazuje na num)
    addTen(num_p);
    printf("%d\n", num); // 10
}
```

## Pole

```c
#include <stdlib.h>

int main() {
    // Alokace pole o velikosti size
    int size = 9;
    int* array_p;
    array_p = (int*)malloc(size * sizeof(int));
    if (array_p == NULL) return 1;

    // Naplnění pole
    for (int i = 0; i < size; i++) {
        *(array_p + i) = i; // array_p je ukazatel na první prvek pole, přičítáním k jeho hodnotě se dostáváme na následující adresy (následující prvky v poli)
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

    return 0;
}
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

* Velikost pole se nedá nijak zjistit, musíme ji mít uloženou někde v proměnné nebo pro ní vyhradit např. první položku v poli

### Dvourozměrné pole

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
*/
```

## Řetězce

* Jednorozměrné pole charů, řetězec je vždy ukončen znakem `\0`
* Knihovna `string.h`:
  * `int strlen(char* s);`
  * `char* strcpy(char* kam, char* odkud);`
  * ...

## Struktury

```c
typedef struct miry {
    int vaha;
    int vyska;
} MIRY;

// main
MIRY osoba;
osoba.vaha = 80;
osoba.vyska = 180;
printf("%d %d\n", osoba.vaha, osoba.vyska); // 80 180

// Přístup k prvkům struktury pomocí pointeru
MIRY* osoba_p;
osoba_p = &osoba;
osoba_p->vaha = 90; // Operátor takzvané šipky, ekvivalentní k `(*osoba_p).vaha = 90;`
osoba_p->vyska = 190;
printf("%d %d\n", osoba.vaha, osoba.vyska); // 90 190
```

## Random

```c
srand(time(NULL)); // Nastavení seedu pro generaci pseudonáhodných čísel; time(NULL) vrací unix timestamp
int randomNumber = (rand() % (max - min)) + min; // Generuje číslo mezi min [inclusive] a max [exclusive]
```
