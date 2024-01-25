# Go tahák

## Datové typy

```go
// int to string
import "strconv"
num := 123
text := strconv.Itoa(num) // "123"

// string to int
text := "123"
num, err := strconv.Atoi(text)
```

## Řetězce

### Získání hodnot ze stringu

```go
import "fmt"
var number int
n, err := fmt.Sscanf("Number 52 ...", "Number %d ...", &number)
```

## Cykly

### Iterace přes string

```go
// Iterace po "znacích"
for _, char := range text { 
    // char ...
}

// Iterace pozpátku (pozor na len() u řetězců obsahující non-ASCII znaky)
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
// Nové pole
array := [3]int{1, 2, 3}
array := [...]int{1, 2, 3} // Délku je možné nahradit trojtečkou

// Nová slice
var slice []int // Inicializace prázdné slice
slice := make([]int, nItems) // Slice s délkou určenou proměnnou
slice := []int{1, 2, 3} // Bez trojtečky se jedná o slice

// array to slice
slice := array[:]

// Přidat na konec
slice = append(slice, 4) // 1 2 3 4
slice = append(slice, 5, 6) // 1 2 3 4 5 6

// Přidat na začátek
slice = append([]int{0}, slice...) // 0 1 2 3 4 5 6
slice = append([]int{-2, -1}, slice...) // -2 -1 0 1 2 3 4 5 6

// Řazení
sort.Sort(sort.Reverse(sort.StringSlice(poleNeboSliceStringů)))

// Odebrání ze slice
slice = slices.Delete(slice, startInclusive, endExclusive)
```

## Mapy

```go
// Mapa (slovník) přiřazující stringu int
var mapa map[string]int

// Inicializace mapy (dva způsoby)
mapa = map[string]int{}
mapa = make(map[string]int)

// Editace hodnot klasicky
mapa["klíč"] = 5
mapa["klíč"]++

// Existuje klíč v mapě?
value, exists := mapa["klíč"]
if exists {
    // Klíč existuje (exists je bool)
}
```

## Misc

<details>
  <summary>Generování kombinací (bez balíku gonum)</summary>

  ```go
      n := 5
      k := 3
  
      combination := make([]int, k)
      for i := 0; i < k; i++ {
          combination[i] = i
      }
  
      var index, maxValue, newValue int
  
  combinationsloop:
      for {
          fmt.Println(combination)
          index = k - 1
          maxValue = n - 1
          for {
              if index == 0 && combination[index] == maxValue {
                  break combinationsloop
              }
              if combination[index] < maxValue {
                  break
              }
              index--
              maxValue--
          }
          combination[index]++
          newValue = combination[index]
          index++
          for index <= k-1 {
              newValue++
              combination[index] = newValue
              index++
          }
      }
  ```

</details>
