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
