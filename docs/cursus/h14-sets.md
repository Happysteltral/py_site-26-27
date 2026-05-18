---
title: Hoofdstuk 14 – Sets
description: Sets aanmaken, wiskundige operaties en frozensets.
---

# Hoofdstuk 14 – Sets

**Sets** zijn ongeordende data structuren die alleen **unieke** elementen kunnen bevatten. Ze werken zoals wiskundige verzamelingen en ondersteunen operaties als vereniging, doorsnede en verschil. Sets worden niet heel vaak gebruikt, maar kunnen elegante oplossingen geven voor bepaalde problemen.

---

## 14.1 Basis van sets

Een set maak je met **accolades** `{}` of met de `set()` functie:

```python
fruitset = {"appel", "banaan", "kers"}
print(fruitset)

# Via set() met een list:
fruitset2 = set(["appel", "banaan", "kers"])

# Alle unieke letters van een string:
helloset = set("hello world")
print(helloset)    # {'h', 'e', 'l', 'o', ' ', 'w', 'r', 'd'}
```

!!! warning "Lege set = `set()`, niet `{}`"
    `{}` maakt een **lege dictionary**, niet een lege set. Gebruik `set()` voor een lege set:

    ```python
    lege_dict = {}           # dictionary
    lege_set = set()         # set
    print(type(lege_dict))   # <class 'dict'>
    print(type(lege_set))    # <class 'set'>
    ```

Handige functies:

```python
fruitset = {"appel", "banaan", "kers", "doerian", "mango"}
print(len(fruitset))           # 5
print("banaan" in fruitset)    # True
print("druif" in fruitset)     # False
```

Sets doorlopen en sorteren:

```python
fruitset = {"appel", "banaan", "kers", "doerian", "mango"}

# Doorlopen (volgorde onvoorspelbaar!)
for element in fruitset:
    print(element)

# Gesorteerd afdrukken: eerst naar list
fruitlist = list(fruitset)
fruitlist.sort()
for element in fruitlist:
    print(element)
```

!!! note "Volgorde in sets"
    In tegenstelling tot dictionaries (die volgorde van toevoeging bijhouden), is de volgorde in sets **onvoorspelbaar**. Zet ze om naar een list als je gesorteerd wilt doorlopen.

---

## 14.2 Set methodes

### 14.2.1 `add()` en `update()`

`add()` voegt één element toe. `update()` voegt meerdere elementen toe (uit een list, tuple of string). Duplicaten worden automatisch genegeerd:

```python
fruitset = {"appel", "banaan", "kers"}
fruitset.add("mango")           # één element
fruitset.add("appel")           # al aanwezig → wordt genegeerd
print(fruitset)

fruitset.update(["druif", "banaan", "aardbei"])  # meerdere elementen
print(fruitset)
```

### 14.2.2 `remove()`, `discard()` en `clear()`

```python
fruitset = {"appel", "banaan", "kers", "mango"}
fruitset.remove("banaan")    # verwijdert "banaan" — fout als niet bestaat
fruitset.discard("druif")    # geen fout als "druif" niet bestaat
print(fruitset)

fruitset.clear()             # verwijdert alle elementen
print(fruitset)              # set()
```

### 14.2.3 `pop()`

Verwijdert een **willekeurig** element en retourneert het (je kunt niet kiezen welk):

```python
fruitset = {"appel", "banaan", "kers", "mango"}
while len(fruitset) > 0:
    print(fruitset.pop())
```

### 14.2.4 `copy()`

Maakt een echte kopie van de set (net als bij lists en dictionaries):

```python
fruitset = {"appel", "banaan", "kers"}
fruitset_copy = fruitset.copy()
```

---

## 14.3 Wiskundige set-operaties

### 14.3.1 `union()` — Vereniging (`|`)

Een set met **alle** elementen van beide sets:

```python
fruit1 = {"appel", "banaan", "kers"}
fruit2 = {"banaan", "kers", "doerian"}

print(fruit1.union(fruit2))    # {"appel", "banaan", "kers", "doerian"}
print(fruit1 | fruit2)         # idem — kortere notatie
```

### 14.3.2 `intersection()` — Doorsnede (`&`)

Een set met alleen de **gemeenschappelijke** elementen:

```python
fruit1 = {"appel", "banaan", "kers"}
fruit2 = {"banaan", "kers", "doerian"}

print(fruit1.intersection(fruit2))   # {"banaan", "kers"}
print(fruit1 & fruit2)               # idem
```

### 14.3.3 `difference()` — Verschil (`-`)

Een set met elementen van de eerste set **minus** de gemeenschappelijke:

```python
fruit1 = {"appel", "banaan", "kers"}
fruit2 = {"banaan", "kers", "doerian"}

print(fruit1.difference(fruit2))   # {"appel"}   (in fruit1, niet in fruit2)
print(fruit1 - fruit2)             # idem
print(fruit2 - fruit1)             # {"doerian"} (in fruit2, niet in fruit1)
```

### 14.3.4 Relationele methodes

| Methode | Beschrijving |
|---------|-------------|
| `isdisjoint(s2)` | `True` als de sets **geen** gemeenschappelijke elementen hebben |
| `issubset(s2)` | `True` als alle elementen van `s1` ook in `s2` zitten |
| `issuperset(s2)` | `True` als alle elementen van `s2` ook in `s1` zitten |

```python
fruit1 = {"appel", "banaan", "kers"}
fruit2 = {"banaan", "kers"}

print(fruit1.isdisjoint(fruit2))    # False (banaan en kers zijn gemeenschappelijk)
print(fruit2.issubset(fruit1))      # True  (fruit2 ⊆ fruit1)
print(fruit1.issuperset(fruit2))    # True  (fruit1 ⊇ fruit2)
print(fruit1.issubset(fruit1))      # True  (een set is subset van zichzelf)
```

### 14.3.5 Symmetrisch verschil

Het **symmetrische verschil** bevat alle elementen die in precies één van de twee sets zitten (niet in beide):

```python
fruit1 = {"appel", "banaan", "kers"}
fruit2 = {"banaan", "kers", "doerian"}

print(fruit1.symmetric_difference(fruit2))   # {"appel", "doerian"}

# Of via het verschil en de vereniging:
print((fruit1 | fruit2) - (fruit1 & fruit2))  # idem
```

---

## 14.4 Frozensets

Een **frozenset** is een onveranderbare set. Je kunt geen elementen toevoegen of verwijderen na aanmaak:

```python
fruit1 = frozenset(["appel", "banaan", "kers"])
fruit2 = frozenset(["banaan", "kers", "doerian"])

print(fruit1.union(fruit2))          # werkt — retourneert nieuwe frozenset
print(fruit1.intersection(fruit2))   # werkt

fruit1.add("mango")   # ❌ AttributeError: frozenset heeft geen add()
```

!!! tip "Wanneer frozensets?"
    Frozensets zijn onveranderbaar en kunnen daarom als **dictionary key** gebruikt worden. Dat kan niet met gewone sets, omdat die veranderbaar zijn.

---

## Wat je geleerd hebt

In dit hoofdstuk heb je het volgende gezien:

- Sets aanmaken met `{}` en `set()`
- Lege set = `set()` (niet `{}`)
- Methodes: `add()`, `update()`, `remove()`, `discard()`, `clear()`, `pop()`, `copy()`
- Wiskundige operaties: `union()` (`|`), `intersection()` (`&`), `difference()` (`-`)
- Symmetrisch verschil: `symmetric_difference()`
- Relationele tests: `isdisjoint()`, `issubset()`, `issuperset()`
- Frozensets — onveranderbare sets

---

## Opgaven

### Opgave 14.1 — Syllogisme met sets

!!! example "Opgave 14.1"
    Het bekende syllogisme: *Alle mensen zijn sterfelijk. Socrates is een mens. Dus Socrates is sterfelijk.*

    Gebruik de sets hieronder en set-methodes om aan te tonen dat: (a) alle mensen sterfelijk zijn, (b) Socrates een mens is, (c) Socrates sterfelijk is, (d) er sterfelijke dingen zijn die geen mens zijn, en (e) er dingen zijn die niet sterfelijk zijn.

    ```python
    alles = {"Socrates", "Plato", "Eratosthenes", "Zeus", "Hera",
             "Athene", "Acropolis", "Kat", "Hond"}
    mensen = {"Socrates", "Plato", "Eratosthenes"}
    sterfelijken = {"Socrates", "Plato", "Eratosthenes", "Kat", "Hond"}
    ```

### Opgave 14.2 — Deelbaarheid met sets

!!! example "Opgave 14.2"
    Maak drie sets van getallen tussen 1 en 1000:

    - `door3`: alle getallen deelbaar door 3
    - `door7`: alle getallen deelbaar door 7
    - `door11`: alle getallen deelbaar door 11

    Produceer daarna sets van alle getallen die:

    - (a) deelbaar zijn door 3, 7 én 11
    - (b) deelbaar zijn door 3 en 7, maar **niet** door 11
    - (c) **noch** deelbaar zijn door 3, noch door 7, noch door 11

    **Hint:** De kortste oplossing heeft één regel per set.

---

*Volgende: [Hoofdstuk 15 – Besturingssysteem](h15-besturingssysteem.md)*
