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

## Porovnání podle Stringu v češtině

```java
public class Human implements Comparable<Human> {
    private String name;

    @Override
    public int compareTo(Human other) {
        Collator collator = Collator.getInstance(new Locale("cs","cz"));
        return collator.compare(this.name, other.name);
    }
}

// Poté můžeme řadit:
ArrayList<Human> humans = new ArrayList<>( ... );
Collections.sort(humans); // Objekty typu Human budou v humans seřazeny podle atributu name s ohledem na českou abecedu (Collections.sort využívá metody compareTo ve třídě Human)
```

## Další metody řazení

### Předdefinovaný komparátor

```java
public static final Collator col = Collator.getInstance(new Locale("cs","cz"));
public static final Comparator<Human> COMP_BY_NAME = (Human h1, Human h2) -> {
    return col.compare(h1.getName(), h2.getName());
};

public void sortByNameConst() {
    Collections.sort(humans, COMP_BY_NAME)
}
```

### Vytvoření třídy Comparator

```java
public class ComparatorHumanByName implements Comparator<Human> {
    @Override
    public int compare(Human h1, Human h2) {
        return h1.getName().compareTo(h2.getName());
    }
}

// Poté můžeme řadit:
public void sortByName() {
    Collections.sort(humans, new ComparatorHumanByName());
}
```

### Anonymní vnitřní třída

```java
public void sortByNameAnonymous() {
    Collections.sort(humans, new Comparator<Human>(){
        @Override
        public int compare(Human h1, Human h2) {
            return h1.getName().compareTo(h2.getName());
        }
    });
}
```

### Lambda výraz

```java
public void sortByNameLambda() {
    Collections.sort(humans, (Human h1, Human h2) -> h1.getName().compareTo(h2.getName());
}
```
