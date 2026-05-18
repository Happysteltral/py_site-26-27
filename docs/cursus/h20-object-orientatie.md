---
title: Hoofdstuk 20 – Object Oriëntatie
description: Classes, objecten, methodes, attributen en geheugenbeheer in Python.
---

# Hoofdstuk 20 – Object Oriëntatie

Tot nu toe schreven we **gestructureerde programma's**: een reeks statements, beslissingen en loops. Object oriëntatie (OO) is een krachtig paradigma dat grote programma's beter beheersbaar maakt. Python is van nature een object georiënteerde taal.

---

## 20.1 De object georiënteerde wereld

In de echte wereld werken we met **objecten**: appels, peren, tafels. Objecten met gemeenschappelijke eigenschappen groeperen we in **klassen**. Een appel *is een* soort fruit — Appel is een subklasse van Fruit.

Een computerprogramma is een model van een deel van de wereld. Object oriëntatie laat ons dat model bouwen via klassen en objecten.

### 20.1.1 Klassen, objecten en hiërarchieën

- Een **klasse** is een generiek model (een data type) — beschrijft attributen en methodes
- Een **object** is een instantie van een klasse — een concrete waarde
- Klassen bestaan in **hiërarchieën**: subklassen erven van superklassen

!!! info "Klasse vs. object"
    Een klasse is een blauwdruk. Een object is een gebouw dat op basis van die blauwdruk is gebouwd.

    ```python
    # Klasse = data type
    # Object = waarde (instantie van die klasse)
    x = 5        # x is een instantie van int
    s = "hallo"  # s is een instantie van str
    ```

### 20.1.2 Klassen en data types in Python

Sinds Python 3 is **elk data type een klasse**. Strings, integers, floats — het zijn allemaal klassen. Methodes als `"hallo".upper()` zijn een gevolg van dit principe.

---

## 20.2 Object oriëntatie in Python

### 20.2.1 `class` — een nieuwe klasse definiëren

```python
class Punt:
    pass    # pass = doe niks (tijdelijke invulling)

p = Punt()
print(type(p))    # <class '__main__.Punt'>
```

!!! tip "Naamgeving van klassen"
    Klassennamen beginnen met een **hoofdletter** en gebruiken CamelCase: `Punt`, `Rechthoek`, `FilippineWoord`.

### 20.2.2 `__init__()` — initialisatie

De `__init__()` methode wordt automatisch aangeroepen bij het aanmaken van een nieuw object. Gebruik hem om attributen te initialiseren:

```python
class Punt:
    def __init__(self, x=0.0, y=0.0):
        self.x = x
        self.y = y

p1 = Punt()           # x=0.0, y=0.0
p2 = Punt(3.5, 5.0)   # x=3.5, y=5.0
print(p2.x, p2.y)
```

!!! warning "self is verplicht"
    Elke methode krijgt altijd **`self`** als eerste parameter — een referentie naar het object zelf. Vergeet je hem, dan krijg je een runtime error.

!!! tip "Definieer alle attributen in `__init__()`"
    Het is goede gewoonte om **alle** attributen uitsluitend in `__init__()` te creëren. Zo weet je altijd welke attributen een object heeft.

### 20.2.3 `__repr__()` en `__str__()`

`__repr__()` bepaalt wat er getoond wordt als je een object afdrukt:

```python
class Punt:
    def __init__(self, x=0.0, y=0.0):
        self.x = x
        self.y = y
    def __repr__(self):
        return "({}, {})".format(self.x, self.y)

p = Punt(3.5, 5.0)
print(p)    # (3.5, 5.0)
```

!!! note "`__repr__()` vs `__str__()`"
    `__repr__()` — volledige, technische representatie (altijd definiëren!)
    `__str__()` — gebruikersvriendelijke versie (optioneel, voor `print()`)
    Als `__str__()` niet gedefinieerd is, wordt `__repr__()` gebruikt.

---

## 20.3 Methodes

Definieer je eigen methodes binnen een klasse. Ze krijgen altijd `self` als eerste parameter:

```python
from math import sqrt

class Punt:
    def __init__(self, x=0.0, y=0.0):
        self.x = x
        self.y = y
    def __repr__(self):
        return "({}, {})".format(self.x, self.y)
    def afstand_tot_oorsprong(self):
        return sqrt(self.x ** 2 + self.y ** 2)
    def translatie(self, shift_x, shift_y):
        self.x += shift_x
        self.y += shift_y

p = Punt(3.5, 5.0)
print(p.afstand_tot_oorsprong())
p.translatie(-3, 7)
print(p)
```

!!! tip "Naamgevingsconventies voor methodes"
    - `is_...()` — retourneert True/False over het object
    - `get_...()` — haalt een waarde op uit het object
    - `set_...()` — stelt een waarde in op het object

---

## 20.4 Nesten van objecten

Objecten kunnen andere objecten bevatten:

```python
from copy import copy

class Punt:
    def __init__(self, x=0.0, y=0.0):
        self.x = x
        self.y = y
    def __repr__(self):
        return "({}, {})".format(self.x, self.y)

class Rechthoek:
    def __init__(self, punt, breedte, hoogte):
        self.punt = copy(punt)    # kopie, geen alias!
        self.breedte = breedte
        self.hoogte = hoogte
    def __repr__(self):
        return "[{}, b={}, h={}]".format(self.punt, self.breedte, self.hoogte)

p = Punt(3.5, 5.0)
r = Rechthoek(p, 4.0, 2.0)
print(r)
p.x = 1.0    # wijzigt p, maar NIET r (want we gebruikten copy())
print(r)
```

!!! warning "Objecten worden als referentie doorgegeven"
    Net als lists en dictionaries worden objecten doorgegeven als **alias** aan methodes en functies. Gebruik `copy()` of `deepcopy()` als je de originele waarde wilt beschermen.

---

## 20.5 Geheugenbeheer

Python heeft **garbage collection**: geheugen van objecten zonder referenties wordt automatisch vrijgegeven. Je hoeft hier zelf niets voor te doen.

!!! info "Wanneer geheugen een probleem is"
    Problemen kunnen ontstaan als je grote hoeveelheden data **tegelijkertijd** in het geheugen laadt (zoals een grote list van objecten). In dat geval moet je met bestanden of databases werken.

---

## Wat je geleerd hebt

In dit hoofdstuk heb je het volgende gezien:

- Het object georiënteerde denken: klassen, objecten en hiërarchieën
- `class` — een nieuwe klasse definiëren
- `__init__()` — objecten initialiseren, `self` als eerste parameter
- `__repr__()` en `__str__()` — objecten afdrukken
- Eigen methodes definiëren
- Nesten van objecten en het kopiëren van referenties
- Garbage collection

---

## Opgaven

### Opgave 20.1 — Rechthoek klasse

!!! example "Opgave 20.1"
    Creëer een klasse `Rechthoek` met een `Punt` als linkerbovenhoek, en een breedte en hoogte. Garandeer dat breedte en hoogte positief zijn. Voeg methodes toe voor:

    - Oppervlakte berekenen
    - Omtrek berekenen
    - Rechteronderhoek als `Punt` retourneren
    - Overlapping van twee rechthoeken berekenen (uitdagend!)

### Opgave 20.2 — Student en Cursus

!!! example "Opgave 20.2"
    Definieer een klasse `Student` (voornaam, achternaam, geboortedatum, administratienummer) en een klasse `Cursus` (naam, nummer). Schrijf studenten in voor cursussen. Druk een lijst af van studenten met hun ingeschreven cursussen.

---

*Volgende: [Hoofdstuk 21 – Operator Overloading](h21-operator-overloading.md)*
