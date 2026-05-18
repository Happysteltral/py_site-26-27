---
title: Hoofdstuk 26 – Bestandsformaten
description: CSV lezen en schrijven, pickling, JSON en HTML/XML verwerking.
---

# Hoofdstuk 26 – Bestandsformaten

Data wordt opgeslagen in gestandaardiseerde bestandsformaten. Python biedt voor de meest gebruikte formaten kant-en-klare modules.

---

## 26.1 CSV

**CSV** (Comma-Separated Values) is het meest gebruikte formaat voor het uitwisselen van tabeldata tussen spreadsheets en databases. Elke regel is één record; velden zijn gescheiden door komma's.

### 26.1.1 CSV lezen met `reader()`

```python
from csv import reader

fp = open("data.csv", newline='')
csvreader = reader(fp)
for regel in csvreader:
    print(regel)   # elke regel is een list van strings
fp.close()
```

Opties voor `reader()`:

```python
# Andere separator (bijv. puntkomma):
csvreader = reader(fp, delimiter=';')

# Andere quote-char:
csvreader = reader(fp, quotechar="'")
```

!!! note "Waarom `newline=''`?"
    De Python documentatie raadt aan om `newline=''` te gebruiken bij het openen van CSV bestanden. Dit is nodig als tekstvelden zelf newlines bevatten.

### 26.1.2 CSV schrijven met `writer()`

```python
from csv import writer

fp = open("output.csv", "w", newline='')
csvwriter = writer(fp)
csvwriter.writerow(["FILM", "SCORE"])
csvwriter.writerow(["Monty Python and the Holy Grail", 8])
csvwriter.writerow(["Life of Brian", 8.5])
fp.close()
```

Quoteer-opties:

```python
import csv
csvwriter = writer(fp, quoting=csv.QUOTE_ALL)          # altijd aanhalingstekens
csvwriter = writer(fp, quoting=csv.QUOTE_NONNUMERIC)   # alleen niet-getallen
```

---

## 26.2 Pickling

**Pickling** sla je een volledige Python data structuur op in een bestand — inclusief type en structuur. Laden geeft precies dezelfde structuur terug.

```python
from pickle import dump, load

# Opslaan
data = [("Roquefort", 12, 15.23), ("Cheddar", 5, 0.67)]
fp = open("data.pck", "wb")
dump(data, fp)
fp.close()

# Laden
fp = open("data.pck", "rb")
geladen = load(fp)
fp.close()
print(type(geladen))   # <class 'list'>
print(geladen)
```

Werkt ook voor eigen klassen:

```python
from pickle import dump, load

class Punt:
    def __init__(self, x, y):
        self.x = x
        self.y = y
    def __repr__(self):
        return "({},{})".format(self.x, self.y)

p = Punt(2, 5)
fp = open("punt.pck", "wb")
dump(p, fp)
fp.close()

fp = open("punt.pck", "rb")
q = load(fp)
fp.close()
print(type(q))   # <class '__main__.Punt'>
print(q)         # (2,5)
```

!!! note "Binair formaat"
    Pickle bestanden zijn binaire bestanden — je kunt ze niet lezen in een teksteditor. Gebruik JSON als je een leesbaar formaat wilt.

---

## 26.3 JSON

**JSON** (JavaScript Object Notation) slaat data op in een voor mensen leesbaar tekstformaat. Het wordt veel gebruikt in webservices en APIs, en wordt door veel programmeertalen ondersteund.

```python
from json import dump, load

data = [("Roquefort", 12, 15.23), ("Cheddar", 5, 0.67)]

# Schrijven naar bestand
fp = open("data.json", "w")
dump(data, fp)
fp.close()

# Lezen uit bestand
fp = open("data.json", "r")
geladen = load(fp)
fp.close()
print(geladen)
```

### `dumps()` en `loads()` — werken met strings

```python
from json import dumps, loads

data = {"naam": "Python", "versie": 3, "cool": True}
json_string = dumps(data, indent=2)   # indent voor mooie opmaak
print(json_string)

terug = loads(json_string)
print(terug["naam"])
```

!!! warning "Beperkingen van JSON"
    JSON ondersteunt alleen standaard Python data types (dict, list, str, int, float, bool, None). Tuples worden omgezet naar lists. Eigen klassen moet je eerst converteren.

| Eigenschap | Pickle | JSON |
|-----------|--------|------|
| Leesbaar voor mensen | ❌ | ✅ |
| Ondersteunt eigen klassen | ✅ | ❌ (extra werk) |
| Uitwisselbaar met andere talen | ❌ | ✅ |
| Bestandstype | Binair | Tekst |

---

## 26.4 HTML en XML

Voor het extraheren van data uit webpagina's kun je reguliere expressies gebruiken (zie hoofdstuk 25), maar de **Beautiful Soup** module (`bs4`) is eenvoudiger voor goed opgemaakte HTML.

```bash
pip install beautifulsoup4
```

```python
from bs4 import BeautifulSoup

html = "<html><body><p>Hallo <b>wereld</b>!</p></body></html>"
soup = BeautifulSoup(html, "html.parser")
print(soup.find("b").text)   # wereld
```

!!! note "Beautiful Soup is een externe module"
    `bs4` moet apart geïnstalleerd worden. Voor eenvoudige HTML-parsing met reguliere expressies zie hoofdstuk 25.

---

## Wat je geleerd hebt

In dit hoofdstuk heb je het volgende gezien:

- CSV bestanden lezen met `csv.reader()` en schrijven met `csv.writer()`
- Pickling — data structuren opslaan en laden met de `pickle` module
- JSON — leesbare opslag en uitwisseling met `json.dump()`, `json.load()`, `json.dumps()`, `json.loads()`
- Vergelijking van pickle en JSON
- HTML/XML verwerken met Beautiful Soup

---

## Opgaven

### Opgave 26.1 — CSV converteren

!!! example "Opgave 26.1"
    Lees een CSV bestand met de standaard komma als delimiter. Schrijf de inhoud naar een nieuw CSV bestand met een spatie als delimiter en enkele aanhalingstekens als quotechar. Open het bestand in tekst modus en toon de inhoud ter controle.

### Opgave 26.2 — CSV naar JSON

!!! example "Opgave 26.2"
    Laad de inhoud van een CSV bestand in een list van lists (elke regel = één list). Sla de list van lists op in JSON formaat. Open het JSON bestand en toon de inhoud.

---

*Volgende: [Hoofdstuk 27 – Diverse Nuttige Modules](h27-nuttige-modules.md)*
