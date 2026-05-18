---
title: Hoofdstuk 25 – Reguliere Expressies
description: Patronen zoeken in tekst met de re module, speciale tekens, herhaling en groepen.
---

# Hoofdstuk 25 – Reguliere Expressies

Reguliere expressies (ook wel *regex*) zijn krachtige patronen waarmee je complexe zoekopdrachten in tekst uitvoert. Ze zijn in het begin intimiderend, maar onmisbaar voor iedereen die met tekstuele data werkt.

---

## 25.1 Reguliere expressies met Python

### 25.1.1 De `re` module

```python
import re

patroon = re.compile(r"a+")   # de r"..." maakt een "raw string"
resultaten = patroon.findall("aardvarken")
print(resultaten)   # ['aa', 'a']
```

!!! info "Waarom `r\"...\"`?"
    De `r` voor de string zorgt dat Python de backslashes niet interpreteert. Zo betekent `r"\b"` een woordgrens in regex, en niet een backspace. Gebruik **altijd** `r"..."` voor reguliere expressies.

### 25.1.2 Verkorte compilatie

```python
import re

resultaten = re.findall(r"a+", "aardvarken")
print(resultaten)
```

Als een patroon maar een paar keer gebruikt wordt, is dit prima. Bij veel hergebruik gebruik je `re.compile()` voor snelheid.

### 25.1.3 Match objecten

```python
import re

m = re.search(r"a+", "Kijk uit voor het aardvarken!")
print("{} gevonden op index {}".format(m.group(), m.start()))
```

| Methode | Wat het retourneert |
|---------|-------------------|
| `group()` of `group(0)` | Het gevonden patroon als geheel |
| `start()` | Index waar het patroon begint |
| `end()` | Index waar het patroon eindigt |

`match()` controleert alleen **het begin** van de string. `search()` doorzoekt de hele string.

### 25.1.4 Alle matches

```python
import re

for m in re.finditer(r"a+", "Kijk! Een gevaarlijk aardvarken ontsnapte!"):
    print("{} gevonden bij index {} tot {}.".format(m.group(), m.start(), m.end()))
```

---

## 25.2 Reguliere expressies schrijven

### 25.2.1 Vierkante haken

Beschrijf een **keuze uit meerdere tekens**:

```python
re.findall(r"b[aeiou]ll", "bell bill boll bull baal")
# ['bell', 'bill', 'boll', 'bull']  — maar niet 'baal' (twee klinkers)

# Bereiken:
# [a-z]   — alle kleine letters
# [A-Za-z] — alle letters
# [0-9]   — alle cijfers
# [^0-9]  — alles BEHALVE cijfers
```

### 25.2.2 Speciale tekens

| Code | Betekenis |
|------|----------|
| `\d` | Cijfer `[0-9]` |
| `\D` | Geen cijfer `[^0-9]` |
| `\w` | Alfanumeriek `[A-Za-z0-9_]` |
| `\W` | Niet-alfanumeriek |
| `\s` | Spatie, tab, newline |
| `\S` | Geen spatie |
| `\b` | Woordgrens (breedte nul) |
| `\B` | Geen woordgrens |
| `^` | Begin van string |
| `$` | Einde van string |
| `.` | Elk teken (behalve newline) |
| `\\` | Letterlijke backslash |

### 25.2.3 Herhaling

| Operator | Betekenis |
|---------|----------|
| `*` | Nul of meer keer |
| `+` | Één of meer keer |
| `?` | Nul of één keer |
| `{p}` | Precies p keer |
| `{p,q}` | Minimaal p, maximaal q keer |
| `{p,}` | Minimaal p keer |

```python
# Eén of meer 'a's
re.findall(r"a+", "aardvarken")      # ['aa', 'a']

# Haakjes voor groepering:
re.findall(r"(ba)+", "ba baba babababa") # ['ba', 'ba', 'ba']

# Pipe voor keuze:
re.findall(r"(appel|banaan|peer)", "appel en peer en mango")
# ['appel', 'peer']
```

!!! note "Gulzige matching"
    Herhaling is **gulzig** — het matcht zo veel mogelijk tekens. `a+` in `"aaaa"` geeft `['aaaa']`, niet `['a', 'a', 'a', 'a']`.

---

## 25.3 Groeperen

Haakjes maken **groepen** — je kunt ze apart opvragen:

```python
import re

m = re.search(r"(\d{1,2})-(\d{1,2})-(\d{4})", "Brief van 25-3-2015.")
if m:
    print("Datum:", m.group(0))   # 25-3-2015
    print("Dag:", m.group(1))     # 25
    print("Maand:", m.group(2))   # 3
    print("Jaar:", m.group(3))    # 2015
```

### Benoemde groepen

```python
m = re.search(r"(?P<dag>\d{1,2})-(?P<maand>\d{1,2})-(?P<jaar>\d{4})", "25-3-2015")
print("Dag:", m.group("dag"))
print("Maand:", m.group("maand"))
print("Jaar:", m.group("jaar"))
```

### Refereren binnen een patroon

`\1` verwijst naar de inhoud van de eerste groep:

```python
# Teken dat twee keer voorkomt:
m = re.search(r"(\S).*\1", "magnetron")
if m:
    print(m.group(1), "komt twee keer voor")   # n
```

### `findall()` met groepen

Als er meerdere groepen zijn, retourneert `findall()` een list van tuples:

```python
import re

datums = re.findall(r"(\d{1,2})-(\d{1,2})-(\d{4})", "25-3-2015 en 1-1-2024")
for datum in datums:
    print(datum)   # ('25', '3', '2015'), ('1', '1', '2024')
```

---

## 25.4 Vervangen

Met `re.sub()` vervang je patronen door andere tekst:

```python
import re

s = re.sub(r"([iy])z(eert)", r"\g<1>s\g<2>",
           "Of je nu categorizeert of analyzeert, gebruik een s!")
print(s)   # ...categoriseert...analyseert...
```

!!! note "`\\g<1>` in de vervanging"
    In de vervangingstekst gebruik je `\g<1>` (niet `\1`) om naar een groep te verwijzen. Dit voorkomt dubbelzinnigheid bij getallen van meer dan één cijfer.

---

## Wat je geleerd hebt

In dit hoofdstuk heb je het volgende gezien:

- Wat reguliere expressies zijn en waarom ze nuttig zijn
- De `re` module: `compile()`, `search()`, `match()`, `findall()`, `finditer()`
- Match objecten: `group()`, `start()`, `end()`
- Speciale tekens: `\d`, `\w`, `\s`, `\b`, `^`, `$`, `.`
- Vierkante haken voor tekenkeuze
- Herhalingsoperatoren: `*`, `+`, `?`, `{p,q}`
- Groepen met haakjes — benoemd en anoniem
- Refereren binnen een patroon via `\1`
- Vervangen met `re.sub()`

---

## Opgaven

### Opgave 25.1 — Woorden extraheren

!!! example "Opgave 25.1"
    Schrijf code die met een reguliere expressie alle woorden (alleen letters) uit een tekst in een list plaatst.

### Opgave 25.2 — Het woord "de" tellen

!!! example "Opgave 25.2"
    Gebruik `findall()` om het woord "de" te tellen in een zin, zonder onderscheid in hoofd/kleine letters. Zorg dat "de" als onderdeel van een ander woord (bijv. "onderdeel") niet meegeteld wordt.

### Opgave 25.3 — Namen zoeken

!!! example "Opgave 25.3"
    Schrijf een reguliere expressie die twee-woords combinaties vindt die waarschijnlijk namen van personen zijn: twee woorden waarbij elk woord begint met een hoofdletter gevolgd door kleine letters.

### Opgave 25.4 — Data uit HTML halen

!!! example "Opgave 25.4"
    Schrijf een reguliere expressie die IDs (9 cijfers, omsloten door `<id>` en `</id>`) en bijbehorende namen (omsloten door `<naam>` en `</naam>`) extraheert uit een HTML-tekst. Toon elke ID met bijbehorende naam.

---

*Volgende: [Hoofdstuk 26 – Bestandsformaten](h26-bestandsformaten.md)*
