---
title: Hoofdstuk 11 – Tuples
description: Wat zijn tuples, hoe gebruik je ze, en wanneer zijn ze handig?
---

# Hoofdstuk 11 – Tuples

Een **tuple** is een geordende groep van één of meer waardes die als geheel worden behandeld. Je kent ze eigenlijk al — functies die meerdere waardes teruggeven doen dat via een tuple. In dit hoofdstuk leer je alles over tuples en hun toepassingen.

---

## 11.1 Gebruik van tuples

Een tuple maak je door waardes te scheiden met komma's. De haakjes zijn optioneel, maar worden sterk aanbevolen voor de leesbaarheid:

```python
t1 = ("appel", "mango")
print(type(t1))       # <class 'tuple'>

t2 = "banaan", "kers"  # haakjes zijn optioneel
print(type(t2))       # <class 'tuple'>
```

Je kunt **verschillende data types** mixen in één tuple:

```python
t1 = ("appel", 3, 1.4)
t2 = ("appel", 3, 1.4, ("banaan", 5))   # tuple in een tuple!
```

Handige functies op tuples:

```python
t1 = ("appel", "mango")
t2 = ("appel", 3, 1.4)
t3 = ("appel", 3, 1.4, ("banaan", 5))

print(len(t1))   # 2
print(len(t2))   # 3
print(len(t3))   # 4  (niet 5! de binnenste tuple telt als één element)
```

!!! note "Geneste tuple telt als één element"
    `("banaan", 5)` is het **vierde** element van `t3`, niet twee losse elementen. Een tuple die in een andere tuple zit, telt als één geheel.

Je kunt tuples doorlopen, en statistische functies gebruiken:

```python
# Doorlopen met for loop
t1 = ("appel", 3, 1.4, ("banaan", 5))
for element in t1:
    print(element)

# max, min, sum (alleen voor numerieke tuples)
t1 = (327, 419, 101, 667, 925, 225)
print(max(t1))    # 925
print(min(t1))    # 101
print(sum(t1))    # 2664

# Lidmaatschap testen
t1 = ("appel", "banaan", "kers")
print("banaan" in t1)   # True
print("mango" in t1)    # False
```

---

### 11.1.1 Tuple assignments

**Tuple assignment** stelt je in staat meerdere variabelen tegelijk een waarde te geven:

```python
t1, t2 = "appel", "banaan"
print(t1)   # appel
print(t2)   # banaan
```

Je kunt ook tuples aan de rechterkant plaatsen:

```python
t1, t2 = ("appel", "banaan"), "kers"
print(t1)   # ('appel', 'banaan')
print(t2)   # kers
```

!!! tip "Variabelen verwisselen zonder hulpvariabele"
    In hoofdstuk 4 heb je geleerd variabelen te verwisselen via een tijdelijke hulpvariabele. Met tuple assignment kan het in één regel:

    ```python
    a = 5
    b = 3
    a, b = b, a    # ✅ elegant!
    print(a, b)    # 3 5
    ```

!!! warning "Aantal variabelen moet overeenkomen"
    Als je meer variabelen links zet dan waardes rechts (of omgekeerd), krijg je een runtime error:

    ```python
    a, b, c = 1, 2    # ❌ not enough values to unpack
    ```

#### Tuple met één element

Een tuple met slechts **één element** maak je door een komma toe te voegen na het element:

```python
t1 = ("appel",)         # ✅ tuple met één element
print(type(t1))         # <class 'tuple'>
print(len(t1))          # 1

t2 = ("appel")          # ❌ dit is gewoon een string!
print(type(t2))         # <class 'str'>
```

!!! note "Die komma is verplicht!"
    Zonder de komma ziet Python `("appel")` als een getal of string tussen haakjes — niet als een tuple. Het is een eigenaardige Python-conventie, maar er is historisch geen betere oplossing bedacht.

---

### 11.1.2 Tuple indices

Net als strings hebben tuples **indices**. Je benadert elementen via `tuple[index]`:

```python
t1 = ("appel", "banaan", "kers", "doerian")
print(t1[2])     # "kers"
print(t1[-1])    # "doerian"
```

Je kunt ook **sub-tuples** maken — precies zoals substrings:

```python
t1 = ("appel", "banaan", "kers", "doerian", "mango")
print(t1[1:4])   # ('banaan', 'kers', 'doerian')
```

Met indices kun je ook een `while` loop gebruiken om een tuple te doorlopen:

```python
t1 = ("appel", "banaan", "kers", "doerian", "mango")
i = 0
while i < len(t1):
    print(t1[i])
    i += 1
```

!!! example "Tussenvraag"
    Schrijf een `for` loop die alle elementen van een tuple toont, **samen met hun index**. Zoiets als:

    ```
    0 : appel
    1 : banaan
    2 : kers
    ...
    ```

??? note "Antwoord (klik om te openen)"
    ```python
    t1 = ("appel", "banaan", "kers", "doerian", "mango")
    for i in range(len(t1)):
        print(i, ":", t1[i])
    ```

    Of nog eleganter met de ingebouwde `enumerate()` functie:

    ```python
    for i, element in enumerate(t1):
        print(i, ":", element)
    ```

---

### 11.1.3 Tuple vergelijkingen

Je kunt tuples vergelijken met de gewone vergelijkingsoperatoren. Python vergelijkt **element voor element**, van links naar rechts:

```python
t1 = ("appel", "banaan")
t2 = ("appel", "banaan")
t3 = ("appel", "kers")
t4 = ("appel", "banaan", "kers")

print(t1 == t2)   # True   (alle elementen gelijk)
print(t1 < t3)    # True   ("banaan" < "kers")
print(t1 > t4)    # False  (t1 is een prefix van t4, dus korter = kleiner)
print(t3 > t4)    # True   ("kers" > "banaan" al bij het tweede element)
```

!!! info "Hoe werkt de vergelijking stap voor stap?"
    1. Vergelijk het eerste element van beide tuples
    2. Als ze gelijk zijn → ga naar het tweede element
    3. Als ze verschillen → geef het resultaat van die vergelijking terug
    4. Als één tuple een **prefix** is van de andere (alle elementen gelijk, maar korter) → de kortere is kleiner

---

### 11.1.4 Tuples als retourwaarden van functies

Je weet al dat functies **meerdere waarden tegelijk** kunnen teruggeven. Achter de schermen retourneert de functie een tuple, en je vangt die op met tuple assignment:

```python
import datetime

def plus_dagen(jaar, maand, dag, increment):
    startdatum = datetime.datetime(jaar, maand, dag)
    einddatum = startdatum + datetime.timedelta(days=increment)
    return einddatum.year, einddatum.month, einddatum.day

y, m, d = plus_dagen(2015, 11, 13, 55)
print("{}/{}/{}".format(y, m, d))   # 7/1/2016
```

!!! tip "Altijd tuple assignment gebruiken"
    Als een functie meerdere waarden retourneert, gebruik dan altijd tuple assignment om ze op te vangen. Vermijd `resultaat = functie()` gevolgd door `resultaat[0]`, `resultaat[1]`, enz. — dat maakt de code moeilijker leesbaar.

---

## 11.2 Tuples zijn onveranderbaar

Net als strings zijn tuples **immutable** — je kunt een element niet overschrijven via een assignment:

```python
t1 = ("appel", "banaan", "kers", "doerian")
t1[0] = "mango"   # ❌ TypeError: 'tuple' object does not support item assignment
```

Wil je een "gewijzigde" tuple, dan bouw je een **nieuwe tuple** op:

```python
t1 = ("appel", "banaan", "kers", "doerian")
t1 = ("mango",) + t1[1:]   # nieuwe tuple met eerste element vervangen
print(t1)   # ('mango', 'banaan', 'kers', 'doerian')
```

!!! note "Tuple vs. list"
    Als je een veranderbare verzameling nodig hebt, gebruik dan een **list** (zie hoofdstuk 12). Tuples gebruik je als de inhoud **vast** is en niet gewijzigd mag worden — denk aan coördinaten, datums, of constante configuratiewaarden.

---

## 11.3 Toepassingen van tuples

Tuples worden niet heel vaak gebruikt in Python code (behalve als retourwaarden van functies). Een goede toepassing is wanneer je waarden hebt die conceptueel **bij elkaar horen** en niet los van elkaar gebruikt worden.

### Coördinaten in 2D en nD ruimte

In plaats van aparte `x` en `y` parameters te gebruiken, kun je een punt als tuple meegeven:

```python
from math import sqrt

def afstand(p1, p2):
    return sqrt((p1[0] - p2[0])**2 + (p1[1] - p2[1])**2)

punt1 = (1, 2)
punt2 = (5, 5)
print("Afstand:", afstand(punt1, punt2))   # 5.0
```

En diezelfde functie werkt ook voor **n-dimensionale ruimte**:

```python
from math import sqrt

def afstand(p1, p2):
    totaal = 0
    for i in range(len(p1)):
        totaal += (p1[i] - p2[i])**2
    return sqrt(totaal)

# 1D
print("1D:", afstand((1,), (5,)))            # 4.0

# 2D
print("2D:", afstand((1, 2), (5, 5)))        # 5.0

# 3D
print("3D:", afstand((1, 2, 4), (5, 5, 8))) # 6.4...
```

!!! tip "Waarom tuples en niet losse parameters?"
    Als je de functie uitbreidt naar 3D, hoef je alleen de tuple te veranderen — niet de signatuur van de functie. De functie werkt automatisch voor elke dimensie.

---

## Wat je geleerd hebt

In dit hoofdstuk heb je het volgende gezien:

- Wat tuples zijn: geordende, onveranderbare groepen waardes
- Tuples aanmaken (met en zonder haakjes)
- `len()`, `max()`, `min()`, `sum()` en `in` op tuples
- Tuple assignment — meerdere variabelen tegelijk een waarde geven
- Variabelen verwisselen met tuple assignment
- Tuple met één element (de verplichte komma!)
- Tuple indices en sub-tuples
- Tuple vergelijkingen — element voor element
- Tuples als retourwaarden van functies
- Tuples zijn onveranderbaar (immutable)
- Toepassingen: coördinaten en n-dimensionale afstand

---

## Opgaven

### Opgave 11.1 — Complexe getallen optellen

!!! example "Opgave 11.1"
    Een complex getal heeft de vorm `a + bi`, waarbij `a` en `b` getallen zijn en `i` de imaginaire eenheid (de wortel uit -1). Representeer een complex getal als een tuple `(a, b)`.

    Schrijf een functie `complex_optellen(c1, c2)` die twee complexe getallen (als tuples) optelt en het resultaat als tuple retourneert.

    Formule: `(a + bi) + (c + di) = (a + c) + (b + d)i`

    Test:
    ```python
    c1 = (3, 2)    # 3 + 2i
    c2 = (1, -4)   # 1 - 4i
    print(complex_optellen(c1, c2))   # (4, -2)  → 4 - 2i
    ```

### Opgave 11.2 — Complexe getallen vermenigvuldigen

!!! example "Opgave 11.2"
    Schrijf een functie `complex_vermenigvuldigen(c1, c2)` die twee complexe getallen vermenigvuldigt.

    Formule: `(a + bi) × (c + di) = (a×c - b×d) + (a×d + b×c)i`

    Test:
    ```python
    c1 = (3, 2)    # 3 + 2i
    c2 = (1, -4)   # 1 - 4i
    print(complex_vermenigvuldigen(c1, c2))   # (11, -10)  → 11 - 10i
    ```

### Opgave 11.3 — Geneste inttuple doorlopen

!!! example "Opgave 11.3"
    Een **inttuple** is recursief gedefinieerd: het is ofwel een integer, ofwel een tuple van inttuples.

    Schrijf een recursieve functie `druk_inttuple_af(t)` die alle integers in een inttuple afdrukt, in volgorde.

    Test met:
    ```python
    inttuple = (1, 2, (3, 4), 5, ((6, 7, 8, (9, 10), 11), 12, 13),
                ((14, 15, 16), (17, 18, 19, 20)))
    druk_inttuple_af(inttuple)   # drukt 1 t/m 20 af
    ```

    **Hint:** Gebruik `isinstance(element, int)` om te controleren of een element een integer is. Als het geen integer is, is het een tuple — roep de functie dan recursief aan.

!!! tip "Verband met hoofdstuk 9"
    Als je hoofdstuk 9 (Recursie) hebt overgeslagen, sla dan ook deze opgave over. Als je dat hoofdstuk wel gedaan hebt, is dit een mooie oefening in recursie op geneste structuren.

---

*Volgende: [Hoofdstuk 12 – Lists](h12-lists.md)*
