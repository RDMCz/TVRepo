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
for (const int& item : list) {
    std::cout << item << "\n";
}

// Seřazení
std::sort(list.begin(), list.end());
std::sort(list.begin(), list.end(), std::greater<int>()); // Sestupně

// Custom seřazení
std::sort(list.begin(), list.end(), [](int& i1, int& i2) {
    return (i1 < i2); // Zde můžeme definovat/volat vlastní řadící funkci
});

// indexOf (first occurence)
int num = 80;
int numIndex = find(list.begin(), list.end(), num) - list.begin(); // Při žádné shodě vrací list.size() (out of bounds!)

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
std::cout << text;//abcdnewefghnewijkl
```

## Objektové paradigma

### Dědičnost, polymorfismus

```cpp
// .hpp
class FormElement {
public:
    virtual const std::string getHTML() const = 0;
};

class Input : public FormElement {
public:
    virtual const std::string getHTML() const override;
};

class Password : public Input {
public:
    virtual const std::string getHTML() const override final;
};
```

* Dědíme klasicky přes dvojtečku, mezi dvojtečku a název předka ještě píšeme modifikátor přístupu určující dostupnost předkových metod a atributů
* `virtual` – metoda může být přepsána v potomcích
  * Přepsané třídy jsou automaticky také virtuální, v potomcích již tedy není třeba psát `virtual`
  * Přidáním `= 0` říkáme, že se jedná o abstraktní metodu a tedy i o abstraktní třídu
* `override` – metoda přepisuje metodu předka, nepovinné
* `final` – metoda již nemůže být přepsána v dalších potomcích
* `const`
  * Na začátku – návratový typ je neměnný
  * Na konci – uvnitř metody nelze měnit atributy třídy (this pointer ukazuje na neměnný objekt)

### Kopírovací konstruktor

```cpp
// .hpp
class Text {
private:
    char* cstring;
public:
    Text(const char* cstring = "");
    Text(const Text& other);
};

// .cpp
Text::Text(const char* cstring) : cstring(new char[std::strlen(cstring) + 1]) {
    std::memcpy(this->cstring, cstring, std::strlen(cstring) + 1);
}

Text::Text(const Text& other) : Text(other.cstring) {}
```

### Přetížení operátoru

```cpp
// .hpp
class Text {
private:
    char* cstring;
public:
    Text(const char* cstring = "");
    Text& operator=(const Text& other);
};

// .cpp
Text::Text(const char* cstring) : cstring(new char[std::strlen(cstring) + 1]) {
    std::memcpy(this->cstring, cstring, std::strlen(cstring) + 1);
}

Text& Text::operator=(const Text& other) {
    if (this == &other) return *this;
    char* newCstring = new char[std::strlen(other.cstring) + 1];
    std::memcpy(newCstring, other.cstring, std::strlen(other.cstring) + 1);
    delete[] cstring;
    cstring = newCstring;
    return *this;
}
```

### "toString"

```cpp
// .hpp
class Text {
private:
    std::string value;
public:
    friend std::ostream& operator<<(std::ostream& stream, const Text& text);
};

// .cpp
std::ostream& operator<<(std::ostream& stream, const Text& text) {
    return stream << text.value;
}
```
