# Java tahák I: Strukturované paradigma

## Úryvky

### Datové typy

```java
// char to long
long l = Character.getNumericValue(ch);

// char to String
String s = Character.toString('s');
String s = 's' + "";

// long to int (safe)
int i = Math.toIntExact(l);

// long to String
String s = Long.toString(l);

// String to int
int num = Integer.parseInt("20");

// String to long
long num = Long.parseLong("20");
```

### Číselné soustavy

```java
// binary to decimal
int num = Integer.parseInt("110", 2);

// decimal to binary
String binNum = Integer.toBinaryString(6);
```

### Řetězce

```java
// split String
String[] parts = "a b c".split(" ");

// split String – speciální znaky (tečka, rouřítko, ...)
String[] parts = "a|b|c".split("\\|");

// contains
"abcd".contains("a")
"abcd".contains(c + "") // pro char c = 'a';

// count chars in String
long count = "elephant".chars().filter(ch -> ch == 'e').count();
```

### Kolekce

```java
// array of arrays (~2D array)
String[][] arrays = new String[5][]; // (lze definovat i velikost "druhé dimenze")

// array
char[] chars = "abcd".toCharArray();
Arrays.sort(chars);
Arrays.equals(chars, chars2)

// List
List<String> list = new ArrayList<>();
list.add("item");
list.size();
list.get(index);
Collections.sort(list);
Collections.reverse(list);

// Stack
Stack<Character> stack = new Stack<>();
stack.push('a');
char ch = stack.pop();

// HashMap ("Dictionary")
HashMap<String, Integer> map = new HashMap<>();
map.put("abcd", 3);
for (String s : map.keySet()) { System.out.println("key: " + s + " value: " + map.get(s)); }
for (int i : map.values()) { System.out.println("value: " + s) }
map.containsKey("abcd");
map.put("abcd", map.get("abcd") + 1); // Zvyš value o jedna
```

### String.format()

* `%[flags][width][.precision]conversion-character`
* Conversion characters:
  * `%s` string
  * `%d` decimal integers
  * `%f` floating-point numbers
  * `%t` date/time
  * `%b` boolean
  * `%c` character

```java
float f;
System.out.printf("%.2f%n", f);     // Zaokrouhlí na 2 desetinná místa
System.out.printf("%10.2f%n", f);   // Zaokrouhlí na 2 desetinná místa
                                    // + minimální délka celého výpisu je 10 (doplní vlevo mezery, pokud je třeba)

int i;
System.out.printf("%04d%n", i);     // Mininální délka vypsaného čísla je 4 – doplní zleva nulami, pokud je třeba
```

### Locale

```java
Scanner sc = new Scanner(System.in);
Locale loc = new Locale("cs", "cz"); // ("en", "us")
sc.useLocale(loc);
System.out.printf(loc, "%f%n", f);
```

### Matice

```java
int[][] mx = new int[cols][rows];
// Hlavní diagonála: i == j
// Vedlejší diagonála i + j == rows - 1
```

## Vzory

### Random

```java
// Náhodné číslo v rozmezí min a max (obě inkluzivní)
Random rand = new Random();
int randomNumber = rand.nextInt((max - min) + 1) + min;
```

### Největší společný dělitel

```java
// Největší společný dělitel čísel x1 a x2
int ogX1 = x1;
int ogX2 = x2;

// Eukleidův algoritmus
while (x1 != x2) {
    if (x1 > x2) {
        x1 -= x2;
    }
    else {
        x2 -= x1;
    }
}
int delitel = x1;

// Nejmenší společný násobek
int nasobek = (ogX1 * ogX2) / delitel;
```

### Bubble sort

```java
for (int i = 0; i < pole.length; i++) {
    for (int j = 0; j < pole.length - i - 1; j++) {
        if (pole[j] > pole[j + 1]) {
            int temp = pole[j + 1];
            pole[j + 1] = pole[j];
            pole[j] = temp;
        }
    }
}
```

### Taylor sinus

```java
double result = 0;
double citatel = x;
double jmenovatel = 1;
double number = 1;
double plusMinus = 1;
while (true) {
    zlomek = citatel/jmenovatel;
    if (Math.abs(zlomek) < eps) break;
    result += zlomek * plusMinus;
    plusMinus *= -1;
    jmenovatel *= ++number * ++number;            
    citatel = Math.pow(x, number);
}
System.out.println(result);
```
