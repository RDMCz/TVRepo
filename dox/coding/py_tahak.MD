# Python tahák

(Toto zdaleka není veškerý výčet funkcí a vlastností daného programovacího jazyka. Jedná se pouze o&nbsp;mé zápisky vzniklé během řešení [AoC](https://github.com/RDMCz/AdventOfCode) a/nebo studia na VŠ.)

## Slicing

```python
a[start:stop]       # Od start do stop-1
a[start:]           # Od start do konce
a[:stop]            # Od  nuly do stop-1

a[-1]               # Poslední prvek
a[-2:]              # Poslední dva prvky (od start do konce) (-2 je předposlední prvek)
a[:-2]              # Vše kromě posledních dvou prvků (od nuly do stop-1)
```

## Split řetězce podle 𝑛-tého výskytu znaku

```python
def split_string_at_nth_occurrence(string, delimiter, n):
    parts = string.split(delimiter)
    return [delimiter.join(parts[i:i+n]) for i in range(0, len(parts), n)]

string = "1,2,3,4,5,6,7,8,9,10"
delimiter = ","
n = 3
result = split_string_at_nth_occurrence(string, delimiter, n)

# Result: ['1,2,3', '4,5,6', '7,8,9', '10']
```
