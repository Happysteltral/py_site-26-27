---
title: Hoofdstuk 22 – Overerving
description: Subklassen, superklassen, overschrijven, super() en interfaces in Python.
---

# Hoofdstuk 22 – Overerving

**Overerving** (Engels: *inheritance*) laat je een nieuwe klasse baseren op een bestaande klasse. De nieuwe klasse erft automatisch alle attributen en methodes van de bestaande klasse, en je hoeft alleen de verschillen te beschrijven. Dit is een van de krachtigste concepten in object-georiënteerd programmeren.

---

## 22.1 Class overerving

Je specificeert de superklasse tussen haakjes bij de klassedefinitie:

```python
class Persoon:
    def __init__(self, voornaam, achternaam, leeftijd):
        self.voornaam = voornaam
        self.achternaam = achternaam
        self.leeftijd = leeftijd
    def __repr__(self):
        return "{} {}".format(self.voornaam, self.achternaam)
    def minderjarig(self):
        return self.leeftijd < 18

class Student(Persoon):  # Student erft van Persoon
    pass

albert = Student("Albert", "Applebaum", 19)
print(albert)              # Albert Applebaum
print(albert.minderjarig()) # False
```

`Student` heeft automatisch alle attributen en methodes van `Persoon` — ook al staat er alleen `pass` in de definitie.

!!! info "Terminologie"
    - **Subklasse** — de nieuwe klasse die erft (`Student`)
    - **Superklasse** / ouderklasse — de klasse waarvan geërfd wordt (`Persoon`)

---

### 22.1.1 "Is een" relaties

Gebruik overerving alleen als de relatie "**X is een Y**" geldt:

- ✅ Een Student **is een** Persoon → `Student(Persoon)`
- ✅ Een Auto **is een** Voertuig → `Auto(Voertuig)`
- ❌ Een Fruitmand **is geen** Fruit → geen overerving
- ❌ Een Molecuul **is geen** Atoom → geen overerving

---

### 22.1.2 Uitbreiden en overschrijven

Je kunt methodes **uitbreiden** (nieuwe toevoegen) of **overschrijven** (bestaande vervangen):

```python
class Student(Persoon):
    def __init__(self, voornaam, achternaam, leeftijd, programma):
        super().__init__(voornaam, achternaam, leeftijd)  # superklasse aanroepen
        self.cursussen = []
        self.programma = programma

    def minderjarig(self):          # overschrijven: studenten zijn volwassen bij 21
        return self.leeftijd < 21

    def inschrijven(self, cursus):  # nieuw: cursus toevoegen
        self.cursussen.append(cursus)

albert = Student("Albert", "Applebaum", 19, "CSAI")
print(albert.minderjarig())         # True (nieuwe definitie)
albert.inschrijven("Programmeren")
albert.inschrijven("Wiskunde")
print(albert.cursussen)
```

!!! tip "Gebruik `super()` om de superklasse aan te roepen"
    `super().__init__(...)` roept de `__init__()` van de superklasse aan. Zo hoef je de initialisatie van `Persoon` niet te kopiëren in `Student`. Als `Persoon` later verandert, werkt `Student` automatisch mee.

---

### 22.1.3 Meervoudige overerving

Python ondersteunt overerving van meerdere superklassen:

```python
class C(A, B):
    pass
```

!!! danger "Gebruik meervoudige overerving zo min mogelijk"
    Meervoudige overerving is complex en foutgevoelig. Veel talen ondersteunen het niet eens. Vermijd het tenzij er geen andere oplossing is.

---

## 22.2 Interfaces

Een **interface** (of abstracte klasse) definieert methodes zonder implementatie. Het dwingt subklassen om de methodes zelf in te vullen:

```python
class Voertuig:
    def __init__(self):
        self.naam = ""
        self.werkwoord = ""

    def isStartpunt(self, p):
        return NotImplemented

    def isEindpunt(self, p):
        return NotImplemented

    def snelheid(self, p1, p2):
        return NotImplemented

    def reisWerkwoord(self):
        return NotImplemented

class Auto(Voertuig):
    def __init__(self):
        super().__init__()
        self.naam = "Auto"
        self.werkwoord = "Rijden"

    def isStartpunt(self, p):
        return True   # auto kan overal starten

    def isEindpunt(self, p):
        return True   # auto kan overal eindigen

    def snelheid(self, p1, p2):
        return 120    # km/u

    def reisWerkwoord(self):
        return self.werkwoord
```

!!! info "Waarom interfaces?"
    Je kunt functies schrijven die werken met elk `Voertuig`-object, ongeacht het exacte type. Ze roepen de methodes van de interface aan, en elk voertuig voert ze op zijn eigen manier uit. Dit is een krachtige vorm van **polymorfisme**.

---

## Wat je geleerd hebt

In dit hoofdstuk heb je het volgende gezien:

- Overerving — een subklasse baseren op een superklasse
- "Is een" relaties als maatstaf voor overerving
- Methodes uitbreiden en overschrijven
- `super()` — de superklasse aanroepen vanuit de subklasse
- Meervoudige overerving (en waarom je het moet vermijden)
- Interfaces / abstracte klassen

---

## Opgaven

### Opgave 22.1 — Vierkant als subklasse van Rechthoek

!!! example "Opgave 22.1"
    Gegeven een klasse `Rechthoek` met x, y coördinaat, breedte en hoogte. Maak een klasse `Vierkant` die zoveel mogelijk erft van `Rechthoek`.

### Opgave 22.2 — Klasse hiërarchie voor vormen

!!! example "Opgave 22.2"
    Definieer een interface `Vorm` met methodes `oppervlakte()` en `omtrek()`. Leid `Rechthoek`, `Vierkant` en `Cirkel` af van `Vorm`.

### Opgave 22.3 — Iterated Prisoner's Dilemma

!!! example "Opgave 22.3"
    Implementeer het "Iterated Prisoner's Dilemma" speltheoretisch probleem. Gebruik de interface klasse `Strategie` en implementeer minstens vier strategieën als subklassen: `AltijdD`, `OogOmOog`, `OogOmTweeOgen` en `Meerderheid`. Laat twee strategieën 100 rondes spelen en toon de eindscores.

---

*Volgende: [Hoofdstuk 23 – Iteratoren en Generatoren](h23-iteratoren-generatoren.md)*
