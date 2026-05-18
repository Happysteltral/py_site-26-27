---
title: Hoofdstuk 10 – Strings
description: Strings in detail: indices, substrings, methodes, speciale tekens en ASCII/UTF-8.
---

# Hoofdstuk 10 – Strings

Tot nu toe gebruikten de meeste voorbeelden getallen. Maar in het dagelijks leven werk je veel vaker met **tekst**. In dit hoofdstuk duiken we diep in strings — hoe je ze opbouwt, doorzoekt, aanpast en verwerkt.

---

## 10.1 Herhaling

Je kent strings al uit eerdere hoofdstukken. Een korte opfrissing:

```python
s1 = "appel"
s2 = 'banaan'

print(s1 + s2)      # samenvoegen: "appelbanaan"
print(3 * s1)       # herhalen: "appelappelappel"
print(len(s1))      # lengte: 5
print("an" in s2)   # lidmaatschapstest: True

for letter in s1:   # doorlopen met for loop
    print(letter)
```

---

## 10.2 Strings over meerdere regels

Soms wil je een lange string over meerdere regels in je code schrijven. Er zijn twee manieren:

### Backslash aan het einde van een regel

De string wordt als één lange aaneengesloten tekst behandeld — **zonder** automatische regeleinden:

```python
lange_zin = "Dit is een heel lange zin die \
verder gaat op de volgende regel, \
maar in de uitvoer op één regel staat."
print(lange_zin)
```

### Drievoudige aanhalingstekens `"""` of `'''`

De string behoudt de opmaak inclusief regelovergangen:

```python
lange_zin = """Dit is een string
die meerdere regels beslaat
en ook zo wordt afgedrukt."""
print(lange_zin)
```

!!! tip "Wanneer welke methode?"
    - **Backslash** → je wil geen automatische regeleinden, slechts leesbaardere code
    - **Drievoudige aanhalingstekens** → je wil dat de uitvoer ook meerdere regels beslaat

---

## 10.3 Speciale tekens

**Speciale tekens** (escape sequences) zijn combinaties van een backslash en een code. Python interpreteert ze niet letterlijk, maar als één speciaal teken:

| Code | Betekenis |
|------|-----------|
| `\n` | Newline — ga naar een nieuwe regel |
| `\t` | Tab — invoegen van een tabulatie |
| `\\` | Letterlijke backslash |
| `\'` | Enkel aanhalingsteken in een string |
| `\"` | Dubbel aanhalingsteken in een string |
| `\xnn` | Teken met hexadecimale code `nn` (bijv. `\x20` = spatie) |

```python
print("Regel 1\nRegel 2\nRegel 3")
print("Kolom1\tKolom2\tKolom3")
print("Hij zei: \"Hallo!\"")
print('mango\'s')
```

!!! note "Over hexadecimale getallen"
    Hexadecimale getallen gebruiken 16 cijfers: `0–9` en `A–F`. De decimale waarde berekenen doe je door elk cijfer te vermenigvuldigen met een oplopende macht van 16 van rechts naar links. Zo is hexadecimaal `1F` = `1×16 + 15×1 = 31`. Je hebt dit nu misschien nog niet nodig, maar het komt later van pas bij bestandsverwerking.

---

## 10.4 Tekens in een string

### 10.4.1 String indices

Elk teken in een string heeft een **positie** (index). Indices beginnen bij `0`:

```
p  y  t  h  o  n
0  1  2  3  4  5
-6 -5 -4 -3 -2 -1
```

Je benadert een teken via `string[index]`:

```python
fruit = "aardbei"
print(fruit[0])    # 'a'
print(fruit[4])    # 'b'
print(fruit[-1])   # 'i'  (laatste teken)
print(fruit[-3])   # 'b'
```

!!! warning "Index buiten bereik → runtime error"
    Als je een index gebruikt die niet bestaat, krijg je een `IndexError`:

    ```python
    print(fruit[99])   # ❌ IndexError: string index out of range
    ```

### 10.4.2 Substrings

Met `string[begin:einde]` haal je een **deelstring** op. Het begin-index is inclusief, het einde-index **exclusief**:

```python
fruit = "aalbes"
print(fruit[:])      # "aalbes"  (alles)
print(fruit[0:])     # "aalbes"  (vanaf begin)
print(fruit[:4])     # "aalb"    (eerste 4 tekens)
print(fruit[1:-1])   # "albe"    (alles behalve eerste en laatste)
print(fruit[2])      # "l"       (teken op index 2)
print(fruit[1:5])    # "albe"
```

!!! tip "Substrings gaan nooit out-of-range"
    Bij substrings mag je getallen buiten het bereik gebruiken — Python past ze automatisch aan:

    ```python
    print(fruit[:100])   # "aalbes" — geen fout!
    ```

### 10.4.3 Substrings met stappen

Net als `range()` ondersteunen substrings een **stapgrootte** als derde argument:

```python
fruit = "banaan"
print(fruit[::2])    # "bna"   (elk tweede teken)
print(fruit[1::2])   # "aan"   (elk tweede teken vanaf index 1)
print(fruit[::-1])   # "naanab" (omgekeerd)
print(fruit[::-2])   # "nab"   (elk tweede teken, omgekeerd)
```

!!! tip "String omdraaien"
    `string[::-1]` is de elegante manier om een string te inverteren in Python. Onthoud hem!

### 10.4.4 Strings doorlopen

Je kunt strings doorlopen met een `for` loop (meest elegant), een `for` met `range()`, of een `while` loop:

```python
fruit = "appel"

# Meest leesbaar:
for teken in fruit:
    print(teken, "-", end=" ")

# Met indices (handig als je de index nodig hebt):
for i in range(len(fruit)):
    print(fruit[i], "-", end=" ")

# Met while:
i = 0
while i < len(fruit):
    print(fruit[i], "-", end=" ")
    i += 1
```

---

## 10.5 Strings zijn onveranderbaar

Strings zijn **immutable** — je kunt een individueel teken niet wijzigen via een assignment:

```python
fruit = "aaldbei"
fruit[2] = "r"   # ❌ TypeError: 'str' object does not support item assignment
```

Wil je een teken vervangen, dan bouw je een **nieuwe string** op uit de delen:

```python
fruit = "aaldbei"
fruit = fruit[:2] + "r" + fruit[3:]
print(fruit)   # "aardbei"
```

!!! note "Variabele overschrijven is wél mogelijk"
    Je kunt de variabele een heel nieuwe string geven. Alleen het wijzigen van een individueel teken via index is verboden.

---

## 10.6 String methodes

Python heeft een reeks ingebouwde string methodes. Omdat strings onveranderbaar zijn, **retourneren ze altijd een nieuwe string** — ze passen de originele nooit aan.

Aanroepsyntax: `string.methode()` of `string.methode(parameters)`.

### 10.6.1 `strip()`

Verwijdert **spaties, tabs en newlines** aan het begin en einde:

```python
s = "   En nu iets heel anders   \n"
print("[" + s + "]")
s = s.strip()
print("[" + s + "]")
```

Je kunt ook specifieke tekens meegeven om te verwijderen:

```python
s = "###hallo###"
print(s.strip("#"))   # "hallo"
```

### 10.6.2 `upper()` en `lower()`

Zet alle letters om naar **hoofd- of kleine letters**:

```python
s = "The Meaning of Life"
print(s.upper())   # "THE MEANING OF LIFE"
print(s.lower())   # "the meaning of life"
```

!!! tip "Handige toepassing: case-insensitief vergelijken"
    ```python
    woord = "Python"
    if woord.lower() == "python":
        print("Gevonden!")
    ```

### 10.6.3 `find()`

Zoekt een substring en retourneert de **index van het eerste voorkomen** (of `-1` als niet gevonden):

```python
s = "Humpty Dumpty zat op de muur"
print(s.find("zat"))    # 15
print(s.find("t"))      # 5
print(s.find("t", 12))  # 14  (zoek vanaf index 12)
print(s.find("q"))      # -1  (niet gevonden)
```

### 10.6.4 `replace()`

Vervangt **alle** voorkomens van een substring door een andere:

```python
s = "Humpty Dumpty zat op de muur"
print(s.replace("zat op", "viel van"))
# "Humpty Dumpty viel van de muur"
```

Optioneel derde argument: maximaal aantal vervangingen:

```python
s = "aaa"
print(s.replace("a", "b", 2))   # "bba"
```

### 10.6.5 `split()`

Splitst een string op in een **lijst van woorden** (standaard op spaties):

```python
s = "Humpty Dumpty zat op de muur"
lijst = s.split()
for woord in lijst:
    print(woord)
```

Met een specifieke separator (bijv. komma voor CSV-data):

```python
csv = "2024,september,28,Data Science"
waardes = csv.split(",")
for waarde in waardes:
    print(waarde)
```

### 10.6.6 `join()`

Het tegengestelde van `split()`: plakt een lijst van strings samen met een separator:

```python
s = "Humpty;Dumpty;zat;op;de;muur"
lijst = s.split(";")
s = " ".join(lijst)
print(s)   # "Humpty Dumpty zat op de muur"
```

!!! note "Vreemd maar correct: separator.join(lijst)"
    De separator staat **voor** de aanroep van `join()`, en de lijst is de parameter. Dit voelt omgekeerd aan, maar is gewoon hoe het historisch is gedefinieerd.

---

## 10.7 Codering van tekens

### 10.7.1 ASCII

Computers slaan tekens intern op als getallen. De **ASCII** standaard koppelt getallen aan tekens. Enkele voorbeelden:

| Decimaal | Teken | Decimaal | Teken | Decimaal | Teken |
|---------|-------|---------|-------|---------|-------|
| 32 | (spatie) | 65 | A | 97 | a |
| 48 | 0 | 66 | B | 98 | b |
| 57 | 9 | 90 | Z | 122 | z |

Twee handige functies:

- `ord(teken)` → geeft het ASCII/Unicode nummer van een teken
- `chr(nummer)` → geeft het teken horend bij een nummer

```python
print(ord("A"))    # 65
print(ord("a"))    # 97
print(chr(65))     # "A"
print(chr(97))     # "a"

# Handig: rekenkundige manipulaties
print("De twaalfde letter na 'g' is:", chr(ord("g") + 12))   # "s"
```

!!! note "Waarom hoofdletters < kleine letters?"
    In de ASCII tabel heeft `A` nummer 65 en `a` nummer 97. Alle hoofdletters (65–90) hebben lagere nummers dan kleine letters (97–122). Vergelijkingen tussen strings gebruiken deze nummers — vandaar dat `"Python" < "python"` `True` is.

!!! tip "Gebruik `ord()` in je code, niet het getal zelf"
    Schrijf `ord("A")` in plaats van `65`. De code is dan begrijpelijk voor iedereen, ook voor mensen die ASCII codes niet van buiten kennen.

### 10.7.2 UTF-8

Python ondersteunt **Unicode** (UTF-8), wat betekent dat je ook speciale tekens kunt gebruiken. Wil je een Unicode teken in een string opnemen, gebruik dan `\uxxxx` met de hexadecimale Unicode code:

```python
# Het Griekse alfabet afdrukken
alpha = "\u0391"
for i in range(25):
    print(chr(ord(alpha) + i), end=" ")
```

!!! warning "Pas op bij kopiëren uit tekstverwerkers"
    Tekstverwerkers vervangen soms rechte aanhalingstekens (`"`) door gekrulde (`"`), of een min-teken door een em-dash (`—`). Python herkent deze als gewone tekens — niet als string-begrenzing of operator. Typ code altijd in een echte teksteditor.

---

## Wat je geleerd hebt

In dit hoofdstuk heb je het volgende gezien:

- Strings samenvoegen (`+`), herhalen (`*`) en doorlopen (`for`)
- Strings over meerdere regels schrijven
- Speciale tekens: `\n`, `\t`, `\\`, `\'`, `\"`
- Positieve en negatieve indices
- Substrings met `[begin:einde:stap]`
- Strings zijn onveranderbaar (immutable)
- String methodes: `strip()`, `upper()`, `lower()`, `find()`, `replace()`, `split()`, `join()`
- `ord()` en `chr()` voor ASCII/Unicode bewerkingen

---

## Opgaven

### Opgave 10.1 — Klinkers tellen

!!! example "Opgave 10.1"
    Vraag de gebruiker om een tekst. Tel hoe vaak elke klinker (a, e, i, o, u) voorkomt — zowel hoofd- als kleine letters. Druk voor elke klinker een regel af met de telling.

    Voorbeeld uitvoer voor `"Hallo Python"`:
    ```
    a: 1
    e: 0
    i: 0
    o: 2
    u: 0
    ```

### Opgave 10.2 — Tekens tussen haakjes

!!! example "Opgave 10.2"
    Doorloop de onderstaande tekst en druk alle tekens af die tussen vierkante haakjes `[` en `]` staan (de haakjes zelf niet).

    ```python
    tekst = """En ze stu[re]n [i]ngekleurde prentbriefkaarten van
    plekken waarvan ze zich niet reali[s]eren dat ze er nooit
    geweest zijn [a]an iedereen op nummer 22, weer is prachti[g],
    onz[e] kamer is aa[n]gekruisd. E[t]en[ ]i[s]
    vettig, maar we hebben een geweldig leuk restaurantje gevonden
    in de achterstraatjes waar ze Heine[ke]n hebben."""
    ```

### Opgave 10.3 — ROT-13

!!! example "Opgave 10.3"
    Druk een rij af met alle hoofdletters A t/m Z. Druk er direct onder een rij met de letter die 13 posities verder staat in het alfabet (circulair: na Z komt A weer).

    Uitvoer:
    ```
    A B C D E F G H I J K L M N O P Q R S T U V W X Y Z
    N O P Q R S T U V W X Y Z A B C D E F G H I J K L M
    ```

    Gebruik `ord()` en `chr()` — geen twee losse print-statements met de letters hardcoded.

### Opgave 10.4 — Woord tellen

!!! example "Opgave 10.4"
    Tel hoe vaak het woord `"knap"` voorkomt in de tekst hieronder. Het moet als zelfstandig woord tellen (niet als deel van een ander woord), en zowel hoofd- als kleine letters moeten meegeteld worden.

    ```python
    tekst = """Kapper Knap, de knappe kapper, knipt en kapt heel
    knap, maar de knecht van kapper Knap, de knappe kapper,
    knipt en kapt nog knapper dan kapper Knap, de knappe kapper."""
    ```

    **Hint:** Gebruik `split()` om woorden te extraheren, `lower()` voor hoofdletter-onafhankelijkheid, en `strip()` of `replace()` om leestekens te verwijderen.

### Opgave 10.5 — Tekens sorteren

!!! example "Opgave 10.5"
    Schrijf een programma dat een string neemt en een nieuwe string maakt met exact dezelfde tekens, maar **gesorteerd op ASCII-code**. Gebruik alleen string bewerkingen (geen lists — die komen in hoofdstuk 12).

    Voorbeeld: `"Hallo, wereld!"` → `" !,Hadellloorw"`

    **Hint:** Bouw de gesorteerde string op door telkens het "kleinste" resterende teken te zoeken en toe te voegen.

### Opgave 10.6 — Autocorrectie

!!! example "Opgave 10.6"
    Schrijf een autocorrectie-functie die de volgende wijzigingen toepast op een zin:

    1. Als een woord begint met **twee hoofdletters gevolgd door een kleine letter**, maak dan de tweede hoofdletter klein (bijv. `"EErwaarde"` → `"Eerwaarde"`)
    2. Als een woord **twee keer achter elkaar** voorkomt, verwijder het tweede exemplaar
    3. Als de zin begint met een **kleine letter**, maak die dan een hoofdletter
    4. Als een woord **volledig uit hoofdletters bestaat behalve de eerste letter** die een kleine letter is, keer dan de hoofd/kleine-letter verhouding om (bijv. `"aRTHUR"` → `"Arthur"`)
    5. Als een **dagnaam** (maandag, dinsdag, ..., zondag) niet met een hoofdletter begint, maak de eerste letter dan een hoofdletter

    Test met:
    ```python
    zin = "en zo gebeurde het dat dat onze toevallige ontmoeting \
    met de EErwaarde aRTHUR BElling een ommekeer betekende in ons \
    leven, en vanaf dat moment gingen we iedere zondag naar \
    de kerk van Sint sIMPEL bij Roombroodje MEt Jam."
    ```

---

*Volgende: [Hoofdstuk 11 – Tuples](h11-tuples.md)*
