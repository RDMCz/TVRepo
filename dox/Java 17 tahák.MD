# Java tahák II: Objektové paradigma

## Knihovní třída

* `public final` – třída je viditelná pro všechny, z třídy nelze dědit
* Privátní konstruktor – nelze tvořit instance
* Všechny metody statické

```java
public final class Library {
    private Library() {}
    // public static metody
}
```

## Seřazení podle Stringu v češtině

```java
public class Human implements Comparable<Human> {
    private String surname;

    @Override
    public int compareTo(Human other) {
        Collator collator = Collator.getInstance(new Locale("cs","cz"));
        return collator.compare(this.surname, other.surname);        
    }
}

// Poté můžeme:
ArrayList<Human> list = new ArrayList<>( ... );        
Collections.sort(list); // Objekty typu Human budou v listu seřazeny podle atributu surname s ohledem na českou abecedu
```
