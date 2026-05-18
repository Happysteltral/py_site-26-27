---
title: Hoofdstuk 24 – Command Line Verwerking
description: sys.argv, command line argumenten en flexibele batch verwerking.
---

# Hoofdstuk 24 – Command Line Verwerking

Grote hoeveelheden data verwerken doe je het best via de **command line** — je start je Python programma vanuit de terminal met argumenten, zodat je het kunt automatiseren in batchbestanden.

---

## 24.1 De command line

Je start een Python programma in de command shell via:

```bash
python programma.py
```

### 24.1.1 Batch verwerking

Stel je hebt een programma dat één bestand verwerkt. Je wilt het draaien voor duizenden bestanden. De oplossing: **batchbestanden** — tekstbestanden met een reeks commando's die de shell automatisch uitvoert.

Om dit te laten werken, moet je programma **command line argumenten** accepteren in plaats van de gebruiker om invoer te vragen.

---

### 24.1.2 Command line argumenten

Je geeft argumenten mee op de command line, gescheiden door spaties:

```bash
python programma.py input.txt output.txt 3
```

Als een argument zelf een spatie bevat, zet je het tussen dubbele aanhalingstekens:

```bash
python programma.py "mijn bestand.txt" output.txt
```

---

### 24.1.3 `sys.argv`

De command line argumenten zijn beschikbaar als een list van strings via `sys.argv`:

```python
import sys
print(sys.argv)
# ['programma.py', 'input.txt', 'output.txt', '3']
```

- `sys.argv[0]` — altijd de programmanaam zelf
- `sys.argv[1]` en verder — de argumenten die je hebt meegegeven
- `len(sys.argv)` — totaal aantal (inclusief programmanaam)

---

## 24.2 Flexibele command line verwerking

De beste aanpak: gebruik **default waarden** voor alle parameters, zodat je het programma ook zonder argumenten kunt uitvoeren (handig tijdens ontwikkeling):

```python
import sys

# Default waarden
invoer = "input.txt"
uitvoer = "output.txt"
shift = 3

# Overschrijven met command line argumenten indien aanwezig
if len(sys.argv) > 1:
    invoer = sys.argv[1]
if len(sys.argv) > 2:
    uitvoer = sys.argv[2]
if len(sys.argv) > 3:
    try:
        shift = int(sys.argv[3])
    except ValueError:
        print(sys.argv[3], "is geen geldig getal.")
        sys.exit(1)

print("Invoer:", invoer)
print("Uitvoer:", uitvoer)
print("Shift:", shift)
```

!!! tip "Voordelen van deze aanpak"
    - Tijdens ontwikkeling: draai het programma gewoon vanuit je editor (default waarden worden gebruikt)
    - In productie: geef argumenten mee via de command line of een batchbestand
    - Je hoeft slechts één keer te testen in de echte command shell

---

### 24.2.1 `sys.exit()`

Gebruik `sys.exit(n)` om het programma te stoppen met een foutcode. Conventie:

- `sys.exit(0)` — normaal einde
- `sys.exit(1)` of hoger — fout opgetreden

```python
from sys import exit

if len(sys.argv) < 2:
    print("Gebruik: python programma.py <bestandsnaam>")
    exit(1)
```

Batchbestanden kunnen de foutcode opvangen en reageren (bijv. stoppen of een waarschuwing tonen).

---

### 24.2.2 `argparse`

Voor complexere command line interfaces bestaat de module `argparse`, die automatisch helpteksten genereert en argumenten valideert. Voor de meeste programma's is de aanpak met `sys.argv` voldoende — raadpleeg de Python documentatie als je meer nodig hebt.

---

## Wat je geleerd hebt

In dit hoofdstuk heb je het volgende gezien:

- Wat batch verwerking is en waarom het nuttig is
- Command line argumenten meegeven aan een Python programma
- `sys.argv` — de list van command line argumenten
- Flexibele verwerking via default waarden
- `sys.exit(n)` — programma stoppen met foutcode

---

## Opgaven

### Opgave 24.1 — Opteller via command line

!!! example "Opgave 24.1"
    Maak een programma dat je kunt starten met nul of meer numerieke argumenten. Als een niet-numeriek argument wordt meegegeven, geef je een foutmelding. Als alle argumenten numeriek zijn, tel je ze op en druk je de som af. Test het programma in de command line.

---

*Volgende: [Hoofdstuk 25 – Reguliere Expressies](h25-reguliere-expressies.md)*
