# Jazyk C++

## Strukturované paradigma

### Datové typy

```cpp
// char to int ('5' -> 5)
char ch = '5';
int num = ch - '0';

// std::string to int
int num = std::stoi("50");
```

### std::vector

```cpp
#include <vector>
#include <iostream>

// Vytvoření
std::vector<int> list;

// Zjištění velikosti
std::cout << list.size();

// Odebrání všech prvků
list.clear();

// Přidání prvku na konec
list.push_back(0);

// Procházení prvků
for (const int &item : list) {
    std::cout << item << "\n";
}

// Seřazení
std::sort(list.begin(), list.end());
std::sort(list.begin(), list.end(), std::greater<int>());

// Custom seřazení
std::sort(list.begin(), list.end(), [](int& i1, int& i2) {
    return (i1 < i2); // Zde můžeme definovat/volat vlastní řadící funkci
});

// indexOf (first occurence)
int num = 80;
int numIndex = find(list.begin(), list.end(), num) - list.begin();
std::cout << numIndex << "\n"; // Při žádné shodě vrací list.size() (out of bounds!)

// Smazání prvku na indexu i
list.erase(list.begin() + i);

// Smazání posledního prvku
list.erase(list.end() - 1);
```

### std::tuple

```cpp
#include <tuple>

// Vytvoření
std::tuple<int, int> nums(0, 2); // Pro dvě hodnoty je lepší použít std::pair

// Získání hodnoty
int item1 = std::get<0>(nums);

// Nastavení hodnoty
std::get<0>(nums) = 1;

// Vložení nové tuple to vectoru tuplů
std::vector<std::tuple<int, int>> list;
list.emplace_back(0, 2);
```

### String

```cpp
#include <string>

// -- Split string by char ---------------------------------------------------------------
#include <sstream>

std::stringstream ss("abd,123,def,456");
std::string part;
std::vector<std::string> parts;

while (getline(ss, part, ',')) {
    parts.push_back(part);
}

// -- Replace all occurences -------------------------------------------------------------
#include <regex>

std::string text = "abcdoldefgholdijkl";
text = std::regex_replace(text, std::regex("old"), "new");
std::cout << text; //abcdnewefghnewijkl
```

## Objektové paradigma
