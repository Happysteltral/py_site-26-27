---
title: Hoofdstuk 13 – Dictionaries
description: Dictionaries aanmaken, doorzoeken, methodes en complexe data structuren.
---

# Hoofdstuk 13 – Dictionaries

Strings, tuples en lists zijn **geordende** data structuren die via indices benaderd worden. Maar niet alle data heeft een logische numerieke volgorde. Python biedt **dictionaries** voor ongeordende data waarbij je elementen opzoekt via een **key**.

---

## 13.1 Dictionary basis

Een **dictionary** bevat key-value paren. Elke key is uniek en onveranderbaar; de waarde kan van elk data type zijn.

Je maakt een dictionary met **accolades** `{}`:

```python
fruitmand = {"appel": 3, "banaan": 5, "kers": 50}
```

Een element opzoeken via zijn key:

```python
print(fruitmand["banaan"])   # 5
```

Doorlopen met een `for` loop — de variabele krijgt de **keys**:

```python
fruitmand = {"appel": 3, "banaan": 5, "kers": 50}
for key in fruitmand:
    print("{}: {}".format(key, fruitmand[key]))
```

Een nieuw element toevoegen of een bestaand overschrijven:

```python
fruitmand["mango"] = 1       # nieuw element
fruitmand["appel"] = 10      # bestaande waarde overschrijven
print(fruitmand)
```

Een element verwijderen met `del`:

```python
del fruitmand["banaan"]
print(fruitmand)
```

Twee dictionaries samenvoegen met `|` (Python 3.9+):

```python
fruitmand1 = {"appel": 3, "banaan": 5, "kers": 50}
fruitmand2 = {"appel": 2, "mango": 7}
fruitmand3 = fruitmand1 | fruitmand2
print(fruitmand3)
# {'appel': 2, 'banaan': 5, 'kers': 50, 'mango': 7}
# appel komt van fruitmand2 (rechts wint bij conflict)
```

!!! info "Volgorde in Python 3.7+"
    Sinds Python 3.7 wordt de **volgorde van toevoeging** bijgehouden als je een dictionary afdrukt. Maar conceptueel zijn dictionaries nog steeds ongeordend — je kunt ze niet sorteren of inverteren op basis van positie.

!!! warning "Key niet gevonden → runtime error"
    Als je een key probeert op te zoeken die niet bestaat, krijg je een `KeyError`. Gebruik `get()` (zie sectie 13.2.3) als je niet zeker bent of een key bestaat.

---

## 13.2 Dictionary methodes

### 13.2.1 `copy()`

Net als bij lists maakt een assignment een **alias**, geen kopie. Gebruik `copy()` voor een ondiepe kopie:

```python
fruitmand = {"appel": 3, "banaan": 5, "kers": 50}
fruitmandalias = fruitmand         # alias
fruitmandcopy = fruitmand.copy()   # echte kopie

print(id(fruitmand) == id(fruitmandalias))   # True
print(id(fruitmand) == id(fruitmandcopy))    # False
```

!!! tip "Diepe kopie"
    `copy()` maakt een ondiepe kopie. Voor een diepe kopie gebruik je `deepcopy()` uit de `copy` module, net als bij lists.

---

### 13.2.2 `keys()`, `values()` en `items()`

Deze methodes retourneren **iteratoren** (niet direct lists). Gebruik `list()` om ze om te zetten:

```python
fruitmand = {"appel": 3, "banaan": 5, "kers": 50}
print(list(fruitmand.keys()))    # ['appel', 'banaan', 'kers']
print(list(fruitmand.values()))  # [3, 5, 50]
print(list(fruitmand.items()))   # [('appel', 3), ('banaan', 5), ('kers', 50)]
```

Rechtstreeks in `for` loops:

```python
fruitmand = {"appel": 3, "banaan": 5, "kers": 50, "druif": 0, "mango": 2}

for key in fruitmand.keys():
    print("{}: {}".format(key, fruitmand[key]))

print("Totaal:", sum(fruitmand.values()))
```

Keys **gesorteerd** doorlopen:

```python
keylist = list(fruitmand.keys())
keylist.sort()
for key in keylist:
    print("{}: {}".format(key, fruitmand[key]))
```

!!! warning "Je kunt keys() niet direct sorteren"
    `list(fruitmand.keys()).sort()` werkt **niet** — `sort()` heeft geen retourwaarde. Sla de list eerst op in een variabele, sorteer dan:

    ```python
    keylist = list(fruitmand.keys())
    keylist.sort()
    ```

---

### 13.2.3 `get()`

Zoek een waarde op **zonder risico op KeyError**. Retourneert `None` als de key niet bestaat, of een standaardwaarde als je die meegeeft:

```python
fruitmand = {"appel": 3, "banaan": 5, "kers": 50, "druif": 0, "mango": 2}

appel = fruitmand.get("appel")
if appel:
    print("appel is in de mand")
else:
    print("geen appels in de mand")

# Met standaardwaarde — ideaal voor tellers!
print("aantal bananen:", fruitmand.get("banaan", 0))     # 5
print("aantal aardbeien:", fruitmand.get("aardbei", 0))  # 0
```

!!! tip "get() met standaardwaarde = ideaal voor tellers"
    Als je bijhoudt hoeveel keer iets voorkomt, is `get(key, 0)` perfect: je krijgt de huidige telling terug, of 0 als de key nog niet bestaat. Geen aparte check nodig.

---

## 13.3 Keys

Elk **onveranderbaar** data type mag als key dienen: strings, integers, floats, en ook **tuples**:

```python
# Tuple als key — handig voor coördinaten!
locaties = {}
locaties[(1, 2)] = "startpunt"
locaties[(5, 8)] = "eindpunt"
locaties[(3, 3)] = "checkpoint"

print(locaties[(1, 2)])   # "startpunt"
```

!!! note "Waarom tuples als key?"
    Een 2D punt kun je niet goed als getal of string opslaan. Een tuple `(x, y)` als key is de meest natuurlijke representatie.

---

## 13.4 Complexe waardes

Dictionary waardes mogen van elk type zijn — ook lists of andere dictionaries:

```python
# Studenten per cursus:
curses = {
    "880254": ["u123456", "u383213", "u234178"],
    "822177": ["u123456", "u223416", "u234178"],
    "822164": ["u123456", "u223416", "u383213", "u234178"]
}

for c in curses:
    print(c)
    for s in curses[c]:
        print(s, end=" ")
    print()
```

Nog complexer — dictionary van dictionaries:

```python
curses = {
    "880254": {
        "naam": "Onderzoeksvaardigheden",
        "ects": 3,
        "studenten": {"u123456": 8, "u383213": 7.5, "u234178": 6}
    },
    "822177": {
        "naam": "Logica",
        "ects": 6,
        "studenten": {"u123456": 5, "u223416": 7, "u234178": 9}
    }
}

for c in curses:
    print("{}: {} ({} ECTS)".format(c, curses[c]["naam"], curses[c]["ects"]))
    for s in curses[c]["studenten"]:
        print("  {}: {}".format(s, curses[c]["studenten"][s]))
```

!!! tip "Overweeg object oriëntatie"
    Als je data structuren zo complex worden, is het beter om object oriëntatie te gebruiken (hoofdstuk 20+). Maar voor kleine programma's zijn geneste dictionaries prima.

---

## 13.5 Snelheid: list vs. dictionary

Bij het **opzoeken** van waarden is een dictionary veel sneller dan een list:

| | List | Dictionary |
|--|------|-----------|
| **Opzoeken via index** | Snel (`lijst[i]`) | N.v.t. |
| **Opzoeken via waarde (`in`)** | Langzaam — doorzoekt sequentieel | Snel — hash tabel |
| **Geheugengebruik** | Minder | Meer |

!!! info "Wanneer dictionary vs. list?"
    - Gebruik een **list** als je elementen benadert via hun **positie** (index)
    - Gebruik een **dictionary** als je elementen zoekt via een **waarde** (key)

    De `in` operator op een grote list is traag. Op een dictionary is het vrijwel onmiddellijk.

---

## Wat je geleerd hebt

In dit hoofdstuk heb je het volgende gezien:

- Dictionaries aanmaken en doorlopen
- Key-value paren opzoeken, toevoegen, overschrijven en verwijderen
- Dictionaries samenvoegen met `|`
- Methodes: `copy()`, `keys()`, `values()`, `items()`, `get()`
- Tuples als keys
- Complexe waardes: lists en dictionaries als waarden
- Snelheidsverschil tussen list en dictionary bij opzoeken

---

## Opgaven

### Opgave 13.1 — Woorden tellen

!!! example "Opgave 13.1"
    Schrijf een programma dat de tekst hieronder splitst in woorden (alles dat geen letter is = scheidingsteken), en een dictionary bouwt die voor elk woord bijhoudt hoe vaak het voorkomt (case-insensitief). Druk alle woorden met hun aantallen af in alfabetische volgorde.

    ```python
    tekst = """Kapper Knap, de knappe kapper, knipt en kapt heel
    knap, maar de knecht van kapper Knap, de knappe kapper, knipt
    en kapt nog knapper dan kapper Knap, de knappe kapper."""
    ```

### Opgave 13.2 — Filmscores

!!! example "Opgave 13.2"
    De code hieronder bevat een list van films en bijbehorende scores. Sla alle data op in één dictionary en druk de **gemiddelde score** per film af, afgerond op één decimaal.

    ```python
    films = ["Monty Python and the Holy Grail",
             "Monty Python's Life of Brian",
             "Monty Python's Meaning of Life",
             "And Now For Something Completely Different"]
    grail_scores  = [9, 10, 9.5, 8.5, 3, 7.5, 8]
    brian_scores  = [10, 10, 0, 9, 1, 8, 7.5, 8, 6, 9]
    life_scores   = [7, 6, 5]
    different_scores = [6, 5, 6, 6]
    ```

### Opgave 13.3 — Bibliotheekcatalogus

!!! example "Opgave 13.3"
    Een bibliotheek wil boeken opslaan zodat de bibliothecaris snel kan vinden waar een boek staat als hij de schrijver en titel kent, en ook alle boeken van een bepaalde schrijver kan opvragen.

    Ontwerp een geschikte data structuur (combinatie van dictionaries, lists, tuples) en schrijf een klein programma dat:

    1. Minimaal 5 boeken opslaat
    2. Een boek kan opzoeken op schrijver + titel
    3. Alle boeken van een schrijver kan tonen

---

*Volgende: [Hoofdstuk 14 – Sets](h14-sets.md)*
