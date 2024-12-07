# Lua 5.4 tahák

(Toto zdaleka není veškerý výčet funkcí a vlastností daného programovacího jazyka. Jedná se pouze o&nbsp;mé zápisky vzniklé během řešení [AoC](https://github.com/RDMCz/AdventOfCode) a/nebo studia na VŠ.)

## Kolekce

Lua má jediný typ kolekce zvaný `table`, který může sloužit jako pole, slovník, struktura, ... a indexuje jej od jedničky.

### Table jako 1D pole

```lua
-- Tables jsou heterogenní:
local collection = {7, 8, "A"}

-- Přidání prvku:
table.insert(collection, 3, 9) -- parametry: vložit do jaké table, volitelně na jaký index, co vložit

-- Výpis prvků:
print(table.concat(collection, ", ")) --> 7, 8, 9, A

-- Počet prvků:
print(#collection) -- Protože indexujeme od jedničky, počet prvků je roven poslednímu indexu

-- Iterace:
for i=1,#collection do -- i iteruje od 1 (exkluzivní) do #collection (exkluzivní)
    print(collection[i])
end
-- nebo
for i,item in ipairs(collection) do
    print(i .. " " .. item)
end

-- IndexOf – Lua nemá indexOf, můžeme si ho ale pro konkrétní pole jednoduše napsat:
local index_of = {}
for index,value in pairs(collection) do
    index_of[value] = index
end
print(index_of[9])   --> 3
print(index_of["A"]) --> 4
-- ⇒ při více výskytech stejného prvku vrací index posledního výskytu
```

### Table jako asociativní pole (dictionary)

```lua
local dict = {["Key1"] = "Value1"}
dict["Key2"] = "Value2"
print(dict["Key1"] .. " " .. dict["Key2"]) --> Value1 Value2
```

V jedné table lze kombinovat „normální“ a „slovníkové“ položky:

```lua
local dict = {
    ["Key1"] = "Value1",
    "Item1",
    [2] = "Value2",
    --"Item2", -- Nelze, znamenalo by duplicitní index
}

print(dict["Key1"]) --> Value1
print(dict[1])      --> Item1
print(dict[2])      --> Value2
print(dict[3])      --> nil
print(dict["ABCD"]) --> nil
```

Pro zjištění počtu klíčů ve slovníku je bohužel nutno iterovat přes všechny páry a inkrementovat si proměnnou.

### Table jako množina (set)

Množina je v podstatě slovník bez hodnot, použijeme proto slovník s boolean hodnotami:

```lua
local pseudo_set = {}
pseudo_set["Key1"] = true
if pseudo_set["Key1"] then
    -- Množina obsahuje Key1
end
```

## Řetězce

### Zjištění indexu podřetězce

```lua
local text = "1,2,3,(4,5),6,7,(8,9,10),11,(12,13),14"
local start_i, end_i
```

V řetězci chceme hledat indexy uzávorkovaných dvojic čísel `(𝑑,𝑑)`, ty se zde nacházejí na indexech 7–11 a 29–35. Pokud nás zajímá konkrétní dvojice čísel (např. `(4,5)`) použijeme `string.find` následovně:

```lua
start_i, end_i = string.find(text, "(4,5)", 1, true)
print(start_i .. ", " .. end_i) --> 7, 11
```

Parametry funkce `string.find` jsou:

1. Kde hledat
2. Co hledat
3. Počáteční index (1 znamená hledání od začátku stringu)
4. Ignorovat „regex“? (true znamená, že hledáme přesně to, co je uvedeno jako druhý parametr)

Lua nemá plnohodnotnou podporu regexu, některé výrazy ale zvládá. Pro nalezení indexu libovolného `(𝑑,𝑑)` změníme druhý a čtvrtý parametr:

```lua
-- Procento `%` se používá jako únikový znak a zároveň pro zápis skupiny znaků
start_i, end_i = string.find(text, "%(%d+,%d+%)", 1, false)
print(start_i .. ", " .. end_i) --> 7, 11
```

Hledání po prvním nálezu končí, jak nalézt všechny výskyty?

```lua
local search_i = 1
while true do
    local searched_start_i, searched_end_i = string.find(text, "%(%d+,%d+%)", search_i, false)
    if searched_start_i == nil then break end
    io.write(searched_start_i .. ", " ..searched_end_i .. "; ") -- 7, 11; 29, 35;
    search_i = searched_end_i + 1
end
io.write("\n")
```

Kratší způsob je použití `string.gmatch`, který slouží pro hledání výrazů (nejen indexů). Ne-escapnuté závorky ve výrazu značí jednotlivé skupiny, které jsou přiřazeny do proměnných. Pokud jsou závorky prázdné, vrací svůj index. Přidáním závorek před a za náš výraz tedy dostáváme počáteční a koncový index výrazu. Koncový index je ale nyní exkluzivní, nikoliv inkluzivní.

```lua
for start_i, end_i in string.gmatch(text, "()%(%d+,%d+%)()") do
    print(start_i .. ", " .. end_i)
    --> 7, 12
    --> 29, 36
end
```

### Hledání podřetězců podle vzoru

Použijeme již zmíněné `string.gmatch`:

```lua
for match_value in string.gmatch(text, "%(%d+,%d+%)") do
    print(match_value)
    --> (4,5)
    --> (12,13)
end
```

Přidáním skupin lze hledat jak samotné výrazy, tak jejich indexy:

```lua
for start_i, match_value in string.gmatch(text, "()(%(%d+,%d+%))") do
    print("index " .. start_i .. ": " .. match_value)
    --> index 7: (4,5)
    --> index 29: (12,13)
end
```

### String split?

Lua nemá metodu pro splitování řetězce. Pokud chceme splitovat podle znaku / sady znaků, použijeme opět `string.gmatch`, které lze v tomto případě chápat jako opak splitu. Chceme-li splitovat podle mezery, hledáme vše, co není mezera a má libovolnou nenulovou délku:

```lua
local text = "The quick brown fox   jumps over      the lazy dog..."
local result = {}
for word in string.gmatch(text, "[^ ]+") do -- Místo "[^ ]+" by šlo použít např. "%a+" (https://www.lua.org/pil/20.2.html)
    table.insert(result, word)
end
print(table.concat(result, ", ")) --> The, quick, brown, fox, jumps, over, the, lazy, dog...
```

Split podle konrétního podřetězce / výrazu je komplikovanější, můžeme zkusit funkci:

```lua
local function string_split(text, pattern)
    local result = {}
    local last_index_after
    -- "(.-)pattern" matches least amount of characters followed by the pattern; "()" matches index after said pattern
    for match, index_after in string.gmatch(text, "(.-)" .. pattern .. "()") do
        table.insert(result, match)
        last_index_after = index_after
    end
    -- If there was no match (result table is empty) then return one item table with input text (no splitting was made)
    if next(result) == nil then
        return { text }
    end
    -- If the text does not end with pattern, then there's still last group of characters missing from the table,
    -- that's why we keep track of the index after last match
    if last_index_after ~= nil and last_index_after <= #text then
        table.insert(result, string.sub(text, last_index_after))
    end
    return result
end
```

### Vyzobání hodnot z řetězce à la `sscanf`

Popíšeme-li daný řetězec vzorem a ozávorkujeme části, jež nás zajímají, můžeme je pomocí `string.match` rovnou uložit do proměnných:

```lua
local text = "brown     fox"
local left, right = text:match("(%a+)%s+(%a+)") -- Zkrácení string.match(text, "(%a+)%s+(%a+)")
print(left .. "-" .. right) --> brown-fox
```

(Oproti `gmatch`, který iteruje přes všechny nálezy, se `match` zastaví u prvního nálezu.)

## Převody

```lua
-- string → int
local number = tonumber("20") --> 20

-- string → char array
local text = "Abrakadabra Alakazam"
local collection = {}
for i=1,#text do table.insert(collection, text:sub(i,i)) end -- text:sub(i,i) == string.sub(text,i,i)
print(table.concat(collection, ", ")) --> A, b, r, a, k, a, d, a, b, r, a,  , A, l, a, k, a, z, a, m
```

## Skoky

Skoky typu `goto` nebývají považovány za _good practice_, lze je ale např. použít pro „breaknutí“ vnějšího cyklu:

```lua
for i=1,10 do
    for j=1,10 do
        if i == 5 and j == 7 then
            goto outer_loop_end
        end
    end
end
::outer_loop_end::
```

Pomocí skoků lze také implementovat `continue`, které Lua nemá.
