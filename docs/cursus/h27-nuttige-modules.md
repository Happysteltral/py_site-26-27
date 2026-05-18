---
title: Hoofdstuk 27 – Diverse Nuttige Modules
description: datetime, collections, urllib, glob en statistics — handige standaardmodules van Python.
---

# Hoofdstuk 27 – Diverse Nuttige Modules

Dit laatste inhoudelijke hoofdstuk geeft een beknopt overzicht van vijf handige Python modules. Je hoeft niet alles uit het hoofd te kennen — het gaat erom dat je weet dat ze bestaan, zodat je ze kunt opzoeken wanneer je ze nodig hebt.

---

## 27.1 `datetime`

De `datetime` module biedt klassen voor het werken met datums en tijden.

```python
from datetime import datetime, timedelta

# Huidige datum en tijd
nu = datetime.now()
print(nu)   # 2024-03-15 14:32:07.123456

# Specifieke datum aanmaken
kerst = datetime(2024, 12, 25, 23, 59, 59)

# Verschil berekenen
verschil = kerst - nu
print("Nog", verschil.days, "dagen tot kerst!")

# Datum berekenen
morgen = nu + timedelta(days=1)
over_een_week = nu + timedelta(weeks=1)
```

### Handige `datetime` methodes

```python
from datetime import datetime

nu = datetime.now()
print(nu.year, nu.month, nu.day)     # 2024 3 15
print(nu.hour, nu.minute, nu.second) # 14 32 7
print(nu.strftime("%d-%m-%Y"))        # 15-03-2024
```

### `timedelta` attributen

| Attribuut | Beschrijving |
|-----------|-------------|
| `days` | Aantal dagen |
| `seconds` | Resterende seconden (na dagen) |
| `microseconds` | Resterende microseconden |
| `total_seconds()` | Totaal in seconden |

---

## 27.2 `collections`

De `collections` module biedt uitgebreide versies van standaard data structuren.

### `Counter` — tel elementen

```python
from collections import Counter

data = ["appel", "banaan", "appel", "banaan", "appel", "kers"]
c = Counter(data)
print(c)                         # Counter({'appel': 3, 'banaan': 2, 'kers': 1})
print(c.most_common(2))          # [('appel', 3), ('banaan', 2)]

# Bijwerken met nieuwe data
c.update(["mango", "kers", "kers"])
print(c.most_common())
```

### `deque` — efficiënte queue

```python
from collections import deque

dq = deque([1, 2, 3])
dq.append(4)          # toevoegen rechts
dq.appendleft(0)      # toevoegen links
print(dq)             # deque([0, 1, 2, 3, 4])

dq.pop()              # verwijderen rechts
dq.popleft()          # verwijderen links
print(dq)             # deque([1, 2, 3])
```

!!! tip "Wanneer `deque` vs `list`?"
    Gebruik `deque` als je vaak elementen toevoegt of verwijdert aan **beide** uiteinden. `list` is efficiënter voor willekeurige toegang via index. `deque.popleft()` is veel sneller dan `list.pop(0)`.

---

## 27.3 `urllib`

De `urllib` module geeft toegang tot webpagina's als waren het bestanden.

```python
from urllib.request import urlopen
from urllib.error import HTTPError, URLError
from sys import exit

try:
    u = urlopen("https://www.python.org")
except HTTPError as e:
    print("HTTP fout:", e)
    exit()
except URLError as e:
    print("URL fout:", e)
    exit()

for i in range(5):
    print(u.readline())

u.close()
```

!!! tip "Gebruik `requests` voor echte projecten"
    De `requests` module (niet standaard, maar immens populair) is veel gebruiksvriendelijker dan `urllib`. Installeer via `pip install requests`.

---

## 27.4 `glob`

De `glob` module zoekt bestanden via patronen (vergelijkbaar met de command shell).

```python
from glob import glob

# Alle Python bestanden in huidige directory
for naam in glob("*.py"):
    print(naam)

# Alle bestanden die beginnen met "data_"
for naam in glob("data_*"):
    print(naam)

# Patronen:
# ?    — elk willekeurig teken
# *    — nul of meer willekeurige tekens
# [abc] — één teken uit de reeks
```

!!! warning "Glob ≠ reguliere expressies"
    De patroonnotatie van `glob` lijkt op reguliere expressies maar is anders. Een `*` in glob betekent "willekeurige tekens", niet "nul of meer herhalingen van het vorige".

---

## 27.5 `statistics`

De `statistics` module biedt basale statistische functies.

```python
from statistics import mean, median, mode, stdev, variance, StatisticsError

data = [4, 5, 1, 1, 2, 2, 2, 3, 3, 3]

print("Gemiddelde:", mean(data))         # 2.6
print("Mediaan:", median(data))           # 2.5
try:
    print("Modus:", mode(data))           # 2 (meest voorkomend)
except StatisticsError as e:
    print("Geen unieke modus:", e)
print("Std.dev.: {:.3f}".format(stdev(data)))     # 1.174
print("Variantie: {:.3f}".format(variance(data))) # 1.378
```

| Functie | Beschrijving |
|---------|-------------|
| `mean(data)` | Rekenkundig gemiddelde |
| `median(data)` | Middelste waarde |
| `mode(data)` | Meest voorkomende waarde |
| `stdev(data)` | Standaard deviatie |
| `variance(data)` | Variantie |

!!! note "Voor geavanceerde statistiek"
    De `statistics` module is bedoeld voor basisgebruik. Voor wetenschappelijke toepassingen zijn `numpy`, `scipy` en `pandas` de standaard.

---

## Wat je geleerd hebt

In dit hoofdstuk heb je het volgende gezien:

- `datetime` — werken met datums, tijden en tijdsverschillen
- `collections.Counter` — elementen tellen en rangschikken
- `collections.deque` — efficiënte queue met operaties aan beide uiteinden
- `urllib` — webpagina's benaderen als bestanden
- `glob` — bestanden zoeken via patronen
- `statistics` — gemiddelde, mediaan, modus, standaard deviatie

---

## Opgaven

### Opgave 27.1 — Meest voorkomende letters

!!! example "Opgave 27.1"
    Gebruik de `Counter` klasse om de **vijf meest voorkomende letters** in een tekst te tonen, inclusief hun aantallen. Maak geen onderscheid tussen hoofd- en kleine letters, en tel alleen letters (geen spaties of leestekens).

### Opgave 27.2 — Statistieken van gebruikersinvoer

!!! example "Opgave 27.2"
    Vraag de gebruiker om getallen totdat hij `0` ingeeft. Toon daarna het gemiddelde, de mediaan en de modus. Voor de modus: toon **alle** getallen die het meest voorkomen (zelfs als dat meerdere zijn). Als elk getal uniek is, meld dan dat er geen modus is.

    **Hint:** Gebruik `Counter` voor de modus en de `statistics` module voor gemiddelde en mediaan.

---

## 🎓 Gefeliciteerd!

Je hebt alle 27 hoofdstukken van **De Programmeursleerling** doorgewerkt!

Als je de meeste opgaven zelfstandig hebt kunnen maken, ben je nu een programmeur. Je hebt de basiskennis die toepasbaar is op elke programmeertaal, en je kunt de meeste programmeerproblemen aanpakken die je tegenkomt in je studie of carrière.

!!! success "Wat nu?"
    - Verdiep je in een specifiek domein: **data science** (`pandas`, `numpy`), **webontwikkeling** (`Flask`, `Django`), of **automatisering**
    - Leer een tweede taal: met Python als basis ga je snel in JavaScript, Java of C#
    - Bouw je eigen projecten — echte projecten leren je meer dan elk boek
