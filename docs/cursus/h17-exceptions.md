---
title: Hoofdstuk 17 – Exceptions
description: Runtime errors afvangen en afhandelen met try/except in Python.
---

# Hoofdstuk 17 – Exceptions

Soms treden runtime errors op die je niet kunt vermijden — niet door een programmeerfout, maar door onvoorziene omstandigheden zoals een ontbrekend bestand of ongeldige gebruikersinvoer. Met **exception afhandeling** kun je zulke fouten netjes afvangen en verwerken in plaats van je programma te laten crashen.

---

## 17.1 Errors en exceptions

Python kent twee soorten fouten:

| Type | Wanneer? | Voorbeeld |
|------|---------|-----------|
| **Syntax error** | Vóór uitvoering, bij controle van de code | Vergeten dubbele punt, fout inspringing |
| **Runtime error** | Tijdens uitvoering | Delen door nul, bestand niet gevonden |

Een runtime error genereert een **exception** — een object dat informatie bevat over de fout. Als je een exception niet afhandelt, crasht het programma met een foutmelding.

```python
# Voorbeeld: ZeroDivisionError
num = int(input("Geef een getal: "))
print("3 gedeeld door", num, "is", 3 / num)   # crash als num == 0!
```

---

## 17.2 Afhandelen van exceptions

### 17.2.1 `try ... except`

De basisvorm: alles in het `try` blok wordt bewaakt. Als er een exception optreedt, springt Python naar het `except` blok:

```python
num = int(input("Geef een getal: "))
try:
    print("3 gedeeld door", num, "is", 3 / num)
except:
    print("Je kunt niet delen door nul")
print("Tot ziens!")
```

!!! note "Uitvoering gaat verder na except"
    Na de exception afhandeling gaat het programma **gewoon verder** met de code na de `try ... except` constructie.

### 17.2.2 Specifieke exceptions afhandelen

Je kunt verschillende exception types afzonderlijk afvangen:

```python
try:
    print(3 / int(input("Geef een getal: ")))
except ZeroDivisionError:
    print("Je kunt niet delen door nul")
except ValueError:
    print("Je gaf geen getal")
except:
    print("Iets onverwachts ging fout")
print("Tot ziens!")
```

**Veelvoorkomende exceptions:**

| Exception | Oorzaak |
|-----------|---------|
| `ZeroDivisionError` | Delen door nul |
| `IndexError` | Index buiten het bereik van een list/tuple |
| `KeyError` | Onbekende dictionary key |
| `ValueError` | Fout bij type casting (bijv. `int("abc")`) |
| `TypeError` | Verkeerd data type bij een operatie |
| `IOError` / `OSError` | Bestandsfout (synoniem) |
| `FileNotFoundError` | Bestand niet gevonden (subklasse van `IOError`) |

### 17.2.3 `else` — als er geen exception was

Het `else` blok wordt uitgevoerd als er **geen** exception optrad:

```python
try:
    num = 3 / int(input("Geef een getal: "))
except ZeroDivisionError:
    print("Je kunt niet delen door nul")
except ValueError:
    print("Je gaf geen getal")
else:
    print("Resultaat:", num)    # alleen als alles goed ging
print("Tot ziens!")
```

### 17.2.4 `finally` — altijd uitvoeren

Het `finally` blok wordt **altijd** uitgevoerd, ongeacht of er een exception was of niet. Handig voor opruimwerk zoals het sluiten van bestanden:

```python
try:
    fp = open("bestand.txt")
    print(fp.read())
finally:
    fp.close()
    print("Bestand gesloten")
```

### 17.2.5 Informatie uit een exception halen

Met `as` sla je de exception op in een variabele en kun je extra informatie opvragen:

```python
try:
    print(int("GeenInteger"))
except ValueError as ex:
    print(ex.args)    # ('invalid literal for int()...',)
```

```python
try:
    fp = open("NietBestaandBestand")
    fp.close()
except IOError as ex:
    print(ex.args)    # (fout_nummer, fout_beschrijving)
```

---

## 17.3 Exceptions bij bestandsmanipulatie

Bestandsfouten genereren een `IOError` (ook wel `OSError`). Je kunt het foutnummer gebruiken om de exacte oorzaak te achterhalen:

```python
import errno

try:
    fp = open("NietBestaandBestand")
    fp.close()
except IOError as ex:
    if ex.args[0] == errno.ENOENT:
        print("Bestand niet gevonden!")
    elif ex.args[0] == errno.EACCES:
        print("Geen toegang tot het bestand!")
    else:
        print("Bestandsfout:", ex.args[0], ex.args[1])
```

**Veelgebruikte errno constanten:**

| Constante | Betekenis |
|-----------|----------|
| `errno.ENOENT` | Bestand of directory bestaat niet |
| `errno.EACCES` | Toegang geweigerd |
| `errno.ENOSPC` | Schijf vol |

!!! tip "Vermijd exceptions liever dan ze af te vangen"
    Test altijd eerst of een bestand bestaat met `os.path.exists()` of `os.path.isfile()` **voordat** je het opent. Dat is netter dan wachten op een exception.

---

## 17.4 Zelf exceptions genereren

Met het gereserveerde woord `raise` gooi je zelf een exception:

```python
def get_string_max10(prompt):
    s = input(prompt)
    if len(s) > 10:
        raise ValueError("Lengte groter dan 10", len(s))
    return s

try:
    tekst = get_string_max10("Gebruik 10 tekens of minder: ")
    print("Je gaf in:", tekst)
except ValueError as ex:
    print("Fout:", ex.args)
```

!!! info "Waarom exceptions gooien?"
    Als je een module schrijft voor andere programmeurs, is het netter om een exception te gooien dan een foutmelding af te drukken. De aanroepende code kan dan zelf beslissen hoe de fout afgehandeld wordt.

Je kunt ook een exception **doorgeven** aan het hoger liggende niveau:

```python
fp = open("bestand.txt")
try:
    buf = fp.read()
    print(buf)
except IOError:
    fp.close()    # netjes opruimen
    raise         # exception verder doorgeven
fp.close()
```

---

## Wat je geleerd hebt

In dit hoofdstuk heb je het volgende gezien:

- Verschil tussen syntax errors en runtime errors
- `try ... except` — exceptions afvangen
- Specifieke exception types: `ZeroDivisionError`, `ValueError`, `IOError`, enz.
- `else` — code bij geen exception
- `finally` — altijd uitvoeren
- `as` — extra informatie uit een exception halen
- `errno` constanten voor bestandsfouten
- `raise` — zelf exceptions genereren

---

## Opgaven

### Opgave 17.1 — Exceptions identificeren en afhandelen

!!! example "Opgave 17.1"
    De code hieronder kan verschillende exceptions genereren. Identificeer ze allemaal en breid de code uit om ze elk apart netjes af te handelen (minimaal drie).

    ```python
    numlist = [100, 101, 0, "103", 104]
    i1 = int(input("Geef een index: "))
    print("100 /", numlist[i1], "=", 100 / numlist[i1])
    ```

---

*Volgende: [Hoofdstuk 18 – Binaire Bestanden](h18-binaire-bestanden.md)*
