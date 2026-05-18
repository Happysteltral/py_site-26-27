---
title: Hoofdstuk 16 – Tekstbestanden
description: Tekstbestanden openen, lezen, schrijven en sluiten met Python.
---

# Hoofdstuk 16 – Tekstbestanden

Een van de belangrijkste toepassingen van Python is het verwerken van **tekstbestanden**. Data wordt vaak opgeslagen als tekstbestand omdat dit formaat gemakkelijk uitgewisseld kan worden tussen programma's. Dit hoofdstuk legt uit hoe je bestanden opent, leest, schrijft en sluit.

---

## 16.1 Platte tekstbestanden

**Platte tekstbestanden** bevatten alleen leesbare tekens — zoals Python broncode, HTML bestanden of CSV bestanden. Tekstverwerkingsbestanden (Word, PDF) zijn geen platte tekstbestanden.

Tekstbestanden bestaan uit regels, afgesloten met een **newline** teken (`\n`). Python converteert automatisch de OS-specifieke regeleindes (`\r\n` op Windows) naar `\n`.

### 16.1.1 File handles en pointers

Wanneer je een bestand opent, krijg je een **file handle** terug — een variabele die toegang biedt tot het bestand. De handle bevat een **pointer** die aangeeft waar in het bestand je je bevindt.

| Modus | Pointer startt op |
|-------|-----------------|
| Lezen (`"r"`) | Begin van het bestand |
| Schrijven (`"w"`) | Begin (bestand wordt leeggemaakt!) |
| Toevoegen (`"a"`) | Einde van het bestand |

---

## 16.2 Lezen van tekstbestanden

### 16.2.1 Openen met `open()`

```python
fp = open("bestand.txt")         # openen voor lezen (default)
fp = open("bestand.txt", "r")    # expliciet lezen modus
```

De functie retourneert een **file handle** die je voor alle verdere bewerkingen gebruikt.

### 16.2.2 Lezen met `read()`

Leest het **volledige bestand** als één string:

```python
fp = open("bestand.txt")
inhoud = fp.read()
print(inhoud)
fp.close()
```

!!! warning "Tweede `read()` geeft lege string"
    Na `read()` staat de pointer aan het einde. Een tweede `read()` retourneert een lege string.

### 16.2.3 Sluiten met `close()`

**Altijd verplicht** — sluit het bestand en geeft systeembronnen vrij:

```python
fp = open("bestand.txt")
print(fp.read())
fp.close()
```

#### De `with` constructie — automatisch sluiten

```python
with open("bestand.txt") as fp:
    print(fp.read())
# bestand is hier automatisch gesloten
```

!!! tip "Gebruik `with` wanneer mogelijk"
    De `with` constructie sluit het bestand automatisch, ook bij een runtime error. Dit is de aanbevolen aanpak.

### 16.2.4 Regels lezen met `readline()`

Leest één regel per aanroep (inclusief het `\n` teken):

```python
fp = open("bestand.txt")
while True:
    buffer = fp.readline()
    if buffer == "":    # leeg = einde bestand
        break
    print(buffer, end="")   # end="" voorkomt dubbele regelsprong
fp.close()
```

!!! note "Extra lege regels?"
    `readline()` retourneert de regel inclusief `\n`. Als je `print(buffer)` gebruikt (zonder `end=""`), drukt `print()` ook nog een `\n` af — dat geeft een extra lege regel. Gebruik `end=""` of `strip()` om dit te voorkomen.

### 16.2.5 Alle regels lezen met `readlines()`

Leest **alle regels** in één keer als list van strings (inclusief `\n`):

```python
fp = open("bestand.txt")
regels = fp.readlines()
for regel in regels:
    print(regel, end="")
fp.close()
```

### 16.2.6 Wanneer welke methode?

| Methode | Wanneer gebruiken |
|---------|------------------|
| `read()` | Kleine bestanden, volledige inhoud nodig |
| `readlines()` | Kleine bestanden, alle regels als list |
| `readline()` | Grote bestanden, of bij onbekende bestandsgrootte |

!!! tip "Debuggen: verwerk alleen de eerste paar regels"
    Voeg tijdens het ontwikkelen een teller toe om alleen de eerste 5 regels te verwerken. Als de code werkt, verwijder je de teller:

    ```python
    fp = open("groot_bestand.txt")
    teller = 0
    while teller < 5:
        buffer = fp.readline()
        if buffer == "":
            break
        print(buffer, end="")
        teller += 1
    fp.close()
    ```

---

## 16.3 Schrijven in tekstbestanden

### 16.3.1 Openen voor schrijven

```python
fp = open("output.txt", "w")    # schrijven — bestand wordt leeggemaakt!
```

!!! danger "Bestaand bestand wordt gewist!"
    Als het bestand al bestaat, wordt de inhoud **zonder waarschuwing** verwijderd. Controleer eerst of het bestand bestaat met `os.path.exists()` (zie sectie 16.5).

### 16.3.2 Schrijven met `write()`

Schrijft een string naar het bestand. **Newlines worden niet automatisch toegevoegd** — je moet ze zelf schrijven:

```python
fp = open("output.txt", "w")
while True:
    tekst = input("Geef een regel tekst: ")
    if tekst == "":
        break
    fp.write(tekst + "\n")    # newline expliciet toevoegen
fp.close()
```

### 16.3.3 Schrijven met `writelines()`

Schrijft een **list van strings** in één keer naar het bestand:

```python
regels = ["Eerste regel\n", "Tweede regel\n", "Derde regel\n"]
fp = open("output.txt", "w")
fp.writelines(regels)
fp.close()
```

!!! note "Geen automatische newlines"
    `writelines()` voegt geen newlines toe tussen de strings. Zorg dat de strings zelf `\n` bevatten als je dat wil.

---

## 16.4 Toevoegen aan tekstbestanden

Met modus `"a"` (append) worden nieuwe data aan het **einde** van het bestand toegevoegd — de bestaande inhoud blijft intact:

```python
NAAM = "logboek.txt"

fp = open(NAAM, "a")
while True:
    tekst = input("Geef een regel tekst: ")
    if tekst == "":
        break
    fp.write(tekst + "\n")
fp.close()
```

| Modus | Bestand bestaat al | Bestand bestaat niet |
|-------|------------------|---------------------|
| `"r"` | Leest inhoud | Runtime error |
| `"w"` | Wist en schrijft | Maakt nieuw bestand |
| `"a"` | Voegt toe aan einde | Maakt nieuw bestand |

---

## 16.5 `os.path` methodes

De `os.path` module bevat handige functies voor het werken met bestandspaden.

### 16.5.1 `exists()` — bestaat het pad?

```python
from os.path import exists

if exists("bestand.txt"):
    print("bestand.txt bestaat")
else:
    print("bestand.txt bestaat niet")
```

### 16.5.2 `isfile()` — is het een bestand?

```python
from os.path import isfile

if isfile("bestand.txt"):
    print("het is een bestand")
```

### 16.5.3 `isdir()` — is het een directory?

```python
from os.path import isdir

if isdir("mijnmap"):
    print("het is een directory")
```

### 16.5.4 `join()` — pad samenvoegen

Voegt directory en bestandsnaam samen op de juiste manier voor het OS:

```python
from os import listdir, getcwd
from os.path import join

for naam in listdir("."):
    volledig_pad = join(getcwd(), naam)
    print(volledig_pad)
```

### 16.5.5 `basename()` en `dirname()`

```python
from os.path import basename, dirname

pad = "/home/gebruiker/documents/verslag.txt"
print(basename(pad))   # "verslag.txt"
print(dirname(pad))    # "/home/gebruiker/documents"
```

### 16.5.6 `getsize()` — bestandsgrootte

Retourneert de grootte in bytes:

```python
from os.path import getsize

print(getsize("bestand.txt"), "bytes")
```

---

## 16.6 Encoding

Tekstbestanden hebben een **encoding** — een standaard die voorschrijft hoe bytes vertaald worden naar tekens.

```python
from sys import getfilesystemencoding
print(getfilesystemencoding())   # bijv. "utf-8" of "cp1252"
```

Als je een `UnicodeDecodeError` krijgt bij het lezen, probeer dan:

```python
fp = open("bestand.txt", encoding="latin-1")
```

!!! tip "Gebruik `latin-1` als vangnet"
    `latin-1` kan alle 256 byte-waardes lezen zonder fouten. Als je niet zeker bent van de encoding van een bestand, is `latin-1` een veilige keuze.

---

## Wat je geleerd hebt

In dit hoofdstuk heb je het volgende gezien:

- Platte tekstbestanden, file handles en pointers
- `open()` en `close()`, en de `with` constructie
- Lezen: `read()`, `readline()`, `readlines()`
- Schrijven: `write()`, `writelines()`
- Modi: lezen (`"r"`), schrijven (`"w"`), toevoegen (`"a"`)
- `os.path` functies: `exists()`, `isfile()`, `isdir()`, `join()`, `basename()`, `dirname()`, `getsize()`
- Encoding en het omgaan met `UnicodeDecodeError`

---

## Opgaven

### Opgave 16.1 — Woordfrequentie uit bestand

!!! example "Opgave 16.1"
    Schrijf een programma dat een tekstbestand leest, de tekst splitst in woorden (alles wat geen letter is = scheidingsteken), en een dictionary bouwt die voor elk woord bijhoudt hoe vaak het voorkomt (case-insensitief). Toon alle woorden met hun aantallen in alfabetische volgorde.

### Opgave 16.2 — Regel voor regel verwerken

!!! example "Opgave 16.2"
    Schrijf hetzelfde programma als opgave 16.1, maar lees nu het bestand **regel voor regel** met `readline()`. Dit is beter voor grote bestanden.

### Opgave 16.3 — Klinkers verwijderen

!!! example "Opgave 16.3"
    Schrijf een programma dat een tekstbestand regel voor regel inleest en een nieuw bestand schrijft met dezelfde inhoud, maar waarbij alle klinkers (a, e, i, o, u — hoofd- en kleine letters) verwijderd zijn. Druk af hoeveel tekens je hebt gelezen en hoeveel je hebt geschreven.

### Opgave 16.4 — Gemeenschappelijke woorden

!!! example "Opgave 16.4"
    Schrijf een programma dat bepaalt welke woorden van minimaal drie letters voorkomen in **alle drie** de opgegeven tekstbestanden. Geen onderscheid maken tussen hoofd- en kleine letters; alles wat geen letter is = scheidingsteken.

---

*Volgende: [Hoofdstuk 17 – Exceptions](h17-exceptions.md)*
