---
title: Hoofdstuk 23 – Iteratoren en Generatoren
description: __iter__, __next__, generatoren met yield en de itertools module.
---

# Hoofdstuk 23 – Iteratoren en Generatoren

Je hebt `for ... in ...` al uitgebreid gebruikt met strings, lists, tuples en dictionaries. Al deze objecten zijn **iterabelen** — ze kunnen één voor één elementen afleveren. In dit hoofdstuk leer je hoe je zelf iterabele klassen maakt, en hoe **generatoren** dat nog eenvoudiger maken.

---

## 23.1 Iteratoren

Een **iterator** levert bij iedere aanroep van `next()` een nieuw element op. Als er niets meer is, gooit hij een `StopIteration` exception:

```python
iterator = iter(["appel", "banaan", "kers"])
print(next(iterator, "END"))   # appel
print(next(iterator, "END"))   # banaan
print(next(iterator, "END"))   # kers
print(next(iterator, "END"))   # END (geen StopIteration door default waarde)
```

---

### 23.1.1 Iterabele objecten zelf maken

Een iterabele klasse heeft twee verplichte methodes:

- `__iter__()` — retourneert de iterabele (meestal `self`)
- `__next__()` — retourneert het volgende element, of gooit `StopIteration`

**Aanpak 1 — elementen verwijderen uit een list:**

```python
class Fibo:
    def __init__(self):
        self.seq = [1, 1, 2, 3, 5, 8, 13, 21, 34, 55]
    def __iter__(self):
        return self
    def __next__(self):
        if len(self.seq) > 0:
            return self.seq.pop(0)
        raise StopIteration()

for n in Fibo():
    print(n, end=" ")
```

**Aanpak 2 — index bijhouden (herbruikbaar via `reset()`):**

```python
class Fibo:
    def __init__(self):
        self.seq = [1, 1, 2, 3, 5, 8, 13, 21, 34, 55]
        self.index = -1
    def __iter__(self):
        return self
    def __next__(self):
        if self.index < len(self.seq) - 1:
            self.index += 1
            return self.seq[self.index]
        raise StopIteration()
    def reset(self):
        self.index = -1
```

**Aanpak 3 — elementen berekenen (meest flexibel, kan oneindig zijn):**

```python
class Fibo:
    def __init__(self, maxnum=1000):
        self.maxnum = maxnum
        self.reset()
    def reset(self):
        self.nr1 = 0
        self.nr2 = 1
    def __iter__(self):
        return self
    def __next__(self):
        if self.nr2 > self.maxnum:
            raise StopIteration()
        nr3 = self.nr1 + self.nr2
        self.nr1 = self.nr2
        self.nr2 = nr3
        return self.nr1

for n in Fibo(100):
    print(n, end=" ")
```

!!! warning "Pas op met oneindige iterabelen"
    Een `for ... in ...` loop wordt eindeloos als je iterabele nooit `StopIteration` gooit. Stel altijd een maximum in.

---

### 23.1.2 Gedelegeerde iteratie

Je kunt de iteratie delegeren aan een apart object — zo is de originele klasse herbruikbaar zonder `reset()`:

```python
class FiboIterable:
    def __init__(self, seq):
        self.seq = seq
    def __next__(self):
        if len(self.seq) > 0:
            return self.seq.pop(0)
        raise StopIteration()

class Fibo:
    def __init__(self, maxnum=1000):
        self.maxnum = maxnum
    def __iter__(self):
        nr1, nr2, seq = 0, 1, []
        while nr2 <= self.maxnum:
            nr3 = nr1 + nr2
            nr1 = nr2
            nr2 = nr3
            seq.append(nr1)
        return FiboIterable(seq)

fseq = Fibo()
for n in fseq:
    print(n, end=" ")
print()
for n in fseq:   # opnieuw! werkt zonder reset()
    print(n, end=" ")
```

---

### 23.1.3 `zip()`, `reversed()` en `sorted()`

```python
# zip() — tuples van meerdere iterabelen
for x in zip([1, 2, 3], [4, 5, 6], [7, 8, 9]):
    print(x)   # (1, 4, 7), (2, 5, 8), (3, 6, 9)

# reversed() — omgekeerde volgorde
for fruit in reversed(["appel", "peer", "kers"]):
    print(fruit)

# sorted() — gesorteerde volgorde
for fruit in sorted(["banaan", "appel", "kers"]):
    print(fruit)

# sorted met key functie
for fruit in sorted(["banaan", "appel", "kers"], key=len, reverse=True):
    print(fruit)   # van lang naar kort
```

---

## 23.2 Generatoren

Een **generator** is een functie met `yield` — de eenvoudigste manier om een iterabele te maken. Je hoeft geen klasse te schrijven:

```python
def fibo(maxnum):
    nr1, nr2 = 0, 1
    while nr2 <= maxnum:
        nr3 = nr1 + nr2
        nr1 = nr2
        nr2 = nr3
        yield nr1   # geeft waarde terug en "pauzeert" de functie

for n in fibo(1000):
    print(n, end=" ")
```

!!! info "Hoe werkt `yield`?"
    Als `__next__()` wordt aangeroepen, wordt de functie uitgevoerd tot de `yield`. De waarde bij `yield` wordt teruggegeven, en de functie **pauzeert** op die plek. Bij de volgende aanroep gaat de functie verder waar hij gestopt was.

### Generator expressies

Net als list comprehensions, maar met ronde haken — elementen worden **lui** (on-demand) gegenereerd:

```python
# List comprehension — maakt de hele list in één keer
sl = [x * x for x in range(1, 11)]

# Generator expressie — genereert elementen één voor één
sg = (x * x for x in range(1, 11))

for x in sg:
    print(x, end=" ")
```

!!! tip "Generator expressie of list comprehension?"
    Gebruik een **generator expressie** als je de resultaten één voor één verwerkt (minder geheugen). Gebruik **list comprehension** als je de volledige list nodig hebt (bijv. voor `len()` of meerdere keren doorlopen).

---

## 23.3 `itertools` module

De `itertools` module bevat krachtige functies voor het manipuleren van iterabelen:

### `chain()` — aaneenrijgen

```python
from itertools import chain

for item in chain([1, 2, 3], [11, 12, 13], [x*x for x in range(1, 4)]):
    print(item, end=" ")
# 1 2 3 11 12 13 1 4 9
```

### `zip_longest()` — zip met vulwaarde

```python
from itertools import zip_longest

for item in zip_longest("appel", "framboos", fillvalue=" "):
    print(item)
```

### `product()` — Cartesisch product

```python
from itertools import product

for item in product([1, 2], "AB", ["appel", "banaan"]):
    print(item)
# (1, 'A', 'appel'), (1, 'A', 'banaan'), ...
```

### `permutations()` — alle rangschikkingen

```python
from itertools import permutations

for item in permutations([1, 2, 3], 2):
    print(item)
# (1,2), (1,3), (2,1), (2,3), (3,1), (3,2)
```

### `combinations()` — alle combinaties

```python
from itertools import combinations

for item in combinations([1, 2, 3], 2):
    print(item)
# (1,2), (1,3), (2,3)
```

### `combinations_with_replacement()` — combinaties met herhaling

```python
from itertools import combinations_with_replacement

for item in combinations_with_replacement([1, 2, 3], 2):
    print(item)
# (1,1), (1,2), (1,3), (2,2), (2,3), (3,3)
```

---

## Wat je geleerd hebt

In dit hoofdstuk heb je het volgende gezien:

- Iteratoren en iterabelen
- `__iter__()` en `__next__()` — zelf iterabele klassen maken
- Drie aanpakken: pop-uit-list, index bijhouden, berekenen
- Gedelegeerde iteratie
- `zip()`, `reversed()`, `sorted()`
- Generatoren met `yield`
- Generator expressies
- `itertools`: `chain()`, `zip_longest()`, `product()`, `permutations()`, `combinations()`, `combinations_with_replacement()`

---

## Opgaven

### Opgave 23.1 — Iterator voor gefilterde getallen

!!! example "Opgave 23.1"
    Schrijf een programma dat de gebruiker vraagt om positieve integers. De gebruiker stopt met `0`. Toon via een `for ... in ...` loop alle getallen tussen 1 en 100 die **niet** deelbaar zijn door de ingegeven getallen. Gebruik een iterator.

### Opgave 23.2 — Faculteiten generator

!!! example "Opgave 23.2"
    Schrijf een generator die de faculteiten `1!` t/m `10!` produceert. Bewaar steeds het vorige getal om het volgende te berekenen (niet opnieuw berekenen).

### Opgave 23.3 — Anagrammen

!!! example "Opgave 23.3"
    Vraag de gebruiker om een woord. Produceer alle anagrammen van dat woord via `permutations()` uit `itertools`. Zorg dat elk anagram uniek is, ook als het woord dubbele letters bevat.

### Opgave 23.4 — Subset som probleem

!!! example "Opgave 23.4"
    Gegeven een list van integers: bestaat er een deelverzameling waarvan de som nul is? Gebruik `combinations()` uit `itertools` om alle mogelijke deelverzamelingen te testen.

    Voorbeeld: `[1, 4, -3, -5, 7]` → ja, want `1 + 4 - 5 = 0`

### Opgave 23.5 — Acht koninginnen

!!! example "Opgave 23.5"
    Bepaal hoe je acht koninginnen op een schaakbord kunt plaatsen zodat geen enkele koningin een andere aanvalt. Gebruik `permutations()` slim om de zoekruimte te beperken.

---

*Volgende: [Hoofdstuk 24 – Command Line Verwerking](h24-command-line.md)*
