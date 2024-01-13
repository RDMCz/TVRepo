# Go tahák

## Datové typy

```go
// string to int
import "strconv"
text := "123"
num, err := strconv.Atoi(text)
```

## Řetězce

### Získání hodnot ze stringu

```go
import "fmt"
var number int
n, err = fmt.Sscanf("Number 52 ...", "Number %d ...", &number)
```

## Cykly

### Iterace přes string

```go
// Iterace po ~znacích
for _, char := range text { 
    // char ...
}

// Iterace pozpátku
for i := len(text) - 1; i >= 0; i-- {
    // text[i] ...
}

// Pro každý řádek ve stringu
import "strings"
for _, line := range strings.Split(text, "\n") {
    // line ...
}
```

### break/continue z vnořeného cyklu

```go
loopalias:
    for ... {
        for ... {
            break loopalias
        }
    }
```

## Pole, slices

* Slice je wrapper nad polem, který mu přidává další funkcionalitu

```go
// new array
array := [3]int{1, 2, 3}
array := [...]int{1, 2, 3} // Délku je možné nahradit trojtečkou

// new slice
slice := []int // Inicializace prázdné slice
slice := []int{1, 2, 3}

// array to slice
slice := array[:]

// přidat na konec
slice = append(slice, 4) // 1 2 3 4

// přidat na začátek
slice = append([]int{0}, slice...) // 0 1 2 3 4
```
