---
title: Hoofdstuk 3 – Expressies
description: Data types, berekeningen, string expressies en type casting in Python.
---

# Hoofdstuk 3 – Expressies

In dit hoofdstuk leer je hoe Python omgaat met **data** en **berekeningen**. Je maakt kennis met de drie basistypen gegevens (strings, integers en floats), en je leert hoe je ze combineert tot **expressies**.

---

## 3.1 De `print()` functie

Alles wat je op het scherm wilt tonen, doe je met `print()`. Je hebt dit al gezien in de vorige hoofdstukken. Hier is het nogmaals in actie:

```python
print("Hallo, wereld!")
print(42)
print(3.14)
```

Je mag meerdere waarden meegeven aan `print()`, gescheiden door komma's. Python plaatst dan automatisch een spatie tussen de waarden:

```python
print("Het antwoord is", 42)
```

---

## 3.2 Data types

Elke waarde in Python heeft een **data type**. De drie basistypen zijn strings, integers en floats.

### 3.2.1 Strings

Een **string** is een stuk tekst. Je schrijft een string door de tekst te omhullen met aanhalingstekens — enkelvoudig (`'`) of dubbel (`"`):

```python
print("Hallo")
print('Wereld')
```
Ik raad aan om steeds te werken met dubbele aanhalingstekens, dit is de standaard is in andere talen.

Als je een  dubbel aanhalingsteken wilt gebruiken binnen een string die ook met aanhalingstekens begrensd is, gebruik dan een **backslash** (`\`) als ontsnappingsteken:

```python
print('mango\'s')
print("Hij zei: \"Hallo!\"")
```

!!! tip "Strings kunnen elke lengte hebben"
    Een string mag nul tekens lang (leeg) zijn (`""`), één teken ("x"), of duizenden tekens. Dat maakt niet uit.

---

### 3.2.2 Integers

Een **integer** is een **geheel** getal — positief, negatief, of nul.

```python
print(42)
print(-7)
print(0)
```

In Python zijn integers **onbeperkt groot** — er is geen maximum zoals bij veel andere talen. Wel kan de verwerking trager worden bij extreem grote getallen.

!!! danger "Geen scheidingstekens!"
    In België schrijven we 1 miljard als `1.000.000.000`, maar dat werkt **niet** in Python. Je moet schrijven:

    ```python
    print(1000000000)   # ✅ correct
    print(1.000.000.000) # ❌ fout!
    print(1,000,000,000) # ❌ fout!
    ```


---

### 3.2.3 Floats

Een **float** (kort voor *floating-point getal*) is een getal met decimalen.

```python
print(3.14159265)
print(-0.5)
print(13.0)     # ook een float, ook al zijn er geen decimalen zichtbaar
```

!!! note "Komma of punt?"
    In België gebruiken we een **komma** als decimaalscheider (`3,14`). Python gebruikt de Amerikaanse standaard **punt** (`3.14`). Let hier goed op!

!!! warning "Afrondingsfouten bij floats"
    Door de manier waarop floats intern worden opgeslagen, kunnen kleine afrondingsfouten optreden:

    ```python
    print((431 / 100) * 100)
    # Geeft: 430.99999999999994 (niet 431!)
    ```

    Als je zeker weet dat de uitkomst een geheel getal moet zijn, gebruik dan `round()` (zie hoofdstuk 5).

---

## 3.3 Expressies

Een **expressie** is een combinatie van waardes en operatoren die een nieuwe waarde oplevert. Denk er aan als een berekening.

### 3.3.1 Eenvoudige berekeningen

Python ondersteunt de volgende rekenkundige operatoren:

| Operator | Betekenis | Voorbeeld | Uitkomst |
|----------|-----------|-----------|---------|
| `+` | Optelling | `15 + 4` | `19` |
| `-` | Aftrekking | `15 - 4` | `11` |
| `*` | Vermenigvuldiging | `15 * 4` | `60` |
| `/` | Deling | `15 / 4` | `3.75` |
| `//` | Integer deling | `15 // 4` | `3` |
| `**` | Machtsverheffing | `15 ** 4` | `50625` |
| `%` | Modulo (rest) | `15 % 4` | `3` |

Probeer ze allemaal uit:

```python
print(15 + 4)
print(15 - 4)
print(15 * 4)
print(15 / 4)
print(15 // 4)
print(15 ** 4)
print(15 % 4)
```

!!! info "Integer deling (`//`)"
    De integer deling rondt het resultaat **altijd naar beneden** af op een geheel getal:

    ```python
    print(15 // 4)   # 3  (want 3.75 naar beneden afgerond)
    print(-7 // 2)   # -4 (want -3.5 naar beneden afgerond)
    ```

!!! info "Modulo (`%`) — de rest na deling"
    De modulo operator geeft de **rest** na een deling. Een bekende toepassing: nagaan of een getal even of oneven is.

    **Voorbeeld met koekjes:** Je hebt 14 koekjes en 5 kinderen.

    - `14 // 5` = `2` → ieder kind krijgt 2 koekjes
    - `14 % 5` = `4` → er blijven 4 koekjes over

    ```python
    print(14 // 5)   # 2
    print(14 % 5)    # 4
    ```

!!! tip "Statements en regels"
    Elke regel code is één **statement**. In Python hoef je geen puntkomma (`;`) aan het einde te zetten, maar ieder statement moet wél op zijn eigen regel staan. Meerdere statements per regel is mogelijk (met `;` ertussen), maar wordt sterk afgeraden.

---

### 3.3.2 Complexe berekeningen

Je kunt operatoren combineren tot grotere berekeningen, net als op een rekenmachine. Gebruik **haakjes** om de volgorde te bepalen.

**Volgorde van bewerkingen** (zonder haakjes):
> Machtsverheffen → Vermenigvuldigen → Delen → Optellen → Aftrekken

Dit is de wiskundige volgorde — ook wel *PEMDAS* genoemd (Parentheses, Exponents, Multiplication, Division, Addition, Subtraction).

!!! question "Wat is de uitkomst?"
    ```python
      print(5 * 2 - 3 + 4 / 2)
    ```
    Probeer de uitkomst van `5 * 2 - 3 + 4 / 2` te berekenen **vóórdat** je het uitvoert.

    Let op:

    - Het resultaat is een **float**, ook al zijn er geen decimalen zichtbaar. Dat komt omdat er een deling (`/`) in de berekening zit — Python maakt de uitkomst dan automatisch een float.
    - **Spaties worden genegeerd** door Python. `5*2-3+4/2` en `5 * 2 - 3 + 4 / 2` zijn exact hetzelfde.

    ??? success "Uitkomst"
        De uitkomst is `9.0`.

Met haakjes kun je de volgorde aanpassen:

```python
print((5 * 2) - (3 + 4) / 2)    # anders dan...
print(((5 * 2) - (3 + 4)) / 2)  # ...dit!
```

!!! warning "Spaties ≠ prioriteit"
    Veel beginners denken dat spaties rondom een operator invloed hebben op de volgorde. Dat is **niet zo**. Alleen haakjes bepalen de volgorde. Spaties zijn puur voor de leesbaarheid.

---

### 3.3.3 String expressies

Niet alle operatoren werken voor strings, maar twee wel:

```python
print("tot" + " ziens")       # Vastplakken: "tot ziens"
print(3 * "hallo ")           # Herhalen: "hallo hallo hallo "
print("tot ziens " * 3)       # Ook herhalen
```

| Operator | Met strings | Effect |
|----------|------------|--------|
| `+` | `"hoi" + "dag"` | Samenvoegen → `"hoidag"` |
| `*` | `3 * "ha"` | Herhalen → `"hahaha"` |

Andere operatoren (zoals `-`, `/`, `**`) werken **niet** voor strings en geven een foutmelding.

---

### 3.3.4 Type casting

Soms moet je een waarde omzetten naar een ander data type. Dat doe je met **type casting functies**:

| Functie | Wat doet het? | Voorbeeld |
|---------|--------------|-----------|
| `int()` | Omzetten naar integer (afronden naar beneden) | `int(3.9)` → `3` |
| `float()` | Omzetten naar float | `float(5)` → `5.0` |
| `str()` | Omzetten naar string | `str(42)` → `"42"` |

Zie het verschil:

```python
print(15 / 4)          # 3.75  (float)
print(int(15 / 4))     # 3     (integer, naar beneden afgerond)

print(15 + 4)          # 19    (integer)
print(float(15 + 4))   # 19.0  (float)
```

!!! example "Getal vastplakken aan een string"
    Je kunt `+` niet gebruiken om een getal direct aan een string te plakken. Gebruik `str()`:

    ```python
    # ❌ Dit geeft een fout:
    print("Ik heb " + 15 + " appels.")

    # ✅ Dit werkt:
    print("Ik heb " + str(15) + " appels.")
    ```

---

## 3.4 Stijl

Goede code is **leesbaar** code. Python is vrij in hoe je spaties plaatst rondom operatoren en haakjes — maar een consistente stijl maakt je code veel beter te begrijpen.

```python
# Alle vier regels zijn equivalent:
print(2 + 3)
print (2+3)
print( 2+3 )
print(
    2 + 3
)
```

!!! tip "Kies een stijl en houd je eraan"
    Het maakt niet zoveel uit welke stijlkeuzes je maakt, zolang je ze **consequent** toepast. De meest gangbare Python-stijl is beschreven in [PEP 8](https://peps.python.org/pep-0008/).

### Commentaar

Met een **hash mark** (`#`) voeg je commentaar toe aan je code. Alles rechts van de `#` op dezelfde regel wordt door Python genegeerd:

```python
# Dit is een commentaarregel
print(2 + 3)   # Dit is commentaar achter een statement
```

Je kunt ook **meerdere regels commentaar** schrijven met drievoudige aanhalingstekens:

```python
"""
Dit is een commentaarblok
dat meerdere regels beslaat.
"""
print("Klaar.")
```

!!! note "Wanneer commentaar gebruiken?"
    Voeg commentaar toe als de code niet voor zichzelf spreekt. Goede variabelenamen (zie hoofdstuk 4) kunnen veel commentaar overbodig maken.

---

## Wat je geleerd hebt

In dit hoofdstuk heb je het volgende gezien:

- De `print()` functie om iets op het scherm te tonen
- De drie basistypen: **string**, **integer** en **float**
- Rekenkundige operatoren en de volgorde van bewerkingen
- String expressies met `+` (samenvoegen) en `*` (herhalen)
- Type casting met `int()`, `float()` en `str()`
- Stijl en commentaar

---

## Opgaven

### Opgave 3.1 — Boekwinkelrekening

!!! example "Opgave 3.1"
    Een boek kost in de winkel €24,95, maar boekwinkels krijgen **40% korting** bij inkoop. Het versturen kost **€3,00** voor het eerste boek en **€0,75** voor elk volgend boek.

    Schrijf een Python programma dat berekent hoeveel de winkel betaalt voor **60 boeken**.

    Doe de berekening volledig in Python — gebruik geen rekenmachine.

### Opgave 3.2 — Fouten zoeken

!!! example "Opgave 3.2"
    Kun je de fouten vinden in de volgende regels code? Verbeter ze.

    ```python
    print("Een boodschap").
    print("Een boodschap')
    print('Een boodschap"')
    ```

    Er zijn drie regels — elke regel bevat minstens één fout. Identificeer de fout en schrijf de verbeterde versie.

### Opgave 3.3 — Runtime errors

!!! example "Opgave 3.3"
    Python geeft twee soorten fouten:

    - **Syntax errors** — de code is verkeerd geschreven (Python begrijpt hem niet)
    - **Runtime errors** — de code klopt syntactisch, maar loopt vast tijdens uitvoering

    Een bekend voorbeeld van een runtime error is de `ZeroDivisionError` — je probeert te delen door nul.

    Schrijf een kort programma dat een `ZeroDivisionError` veroorzaakt als je het uitvoert.

!!! tip "Hint"
    Wat gebeurt er als je `print(10 / 0)` uitvoert?

### Opgave 3.4 — Haakjes fout

!!! example "Opgave 3.4"
    Voer de volgende code uit en bestudeer de foutmelding. Kun je het probleem oplossen?

    ```python
    print(((2 * 3) /4 + (5 - 6/7) * 8)
    print(((12 * 13) /14 + (15 - 16) /17) * 18)
    ```

    **Hint:** Tel het aantal openings- en sluithaakjes op elke regel.

### Opgave 3.5 — Hoe laat is het?

!!! example "Opgave 3.5"
    Je kijkt op de klok: het is **14:00u**. Je zet een alarm dat **535 uur later** af moet gaan.

    Hoe laat is het als het alarm afgaat?

    Schrijf een Python programma dat het antwoord afdrukt.

!!! tip "Hint"
    Gebruik de **modulo operator** (`%`). Denk na: hoeveel uur na middernacht is 14:00u? En hoeveel uur na middernacht is het alarm?

---

*Volgende: [Hoofdstuk 4 – Variabelen](h04-variabelen.md)*
