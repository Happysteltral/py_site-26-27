---
title: Hoofdstuk 5 – Eenvoudige Functies
description: Wat zijn functies, hoe gebruik je ze, en welke ingebouwde functies biedt Python?
---

# Hoofdstuk 5 – Eenvoudige Functies

Je hebt al functies gebruikt zoals `print()`, `int()` en `float()`. In dit hoofdstuk leer je precies wat functies zijn, hoe ze werken, en maak je kennis met een reeks handige ingebouwde functies en modules.

---

## 5.1 Elementen van een functie

Een **functie** is een blok herbruikbare code dat een bepaalde taak uitvoert. Je hoeft niet te weten hoe een functie van binnen werkt — je hoeft slechts drie dingen te kennen:

1. De **naam** van de functie
2. De **parameters** die de functie nodig heeft (indien aanwezig)
3. De **retourwaarde** die de functie teruggeeft (indien aanwezig)

!!! tip "Functienamen schrijven"
    Als je in tekst naar een functie verwijst, schrijf je altijd de haakjes erbij: `print()`, `int()`, `len()`. Dat maakt duidelijk dat het om een functie gaat, ook als ze op dat moment niet worden aangeroepen.

---

### 5.1.1 Functienaam

Functienamen volgen dezelfde regels als variabelenamen: alleen letters, cijfers en underscores, en niet beginnen met een cijfer. Vrijwel alle ingebouwde Python-functies bestaan uit kleine letters.

---

### 5.1.2 Parameters

**Parameters** (ook wel *argumenten* genoemd) zijn de waarden die je meegeeft aan een functie, tussen de haakjes, gescheiden door komma's.

```python
x = 1.56
print(int(x))   # int() krijgt x als parameter
print(x)        # x is ongewijzigd!
```

!!! info "Functies wijzigen hun parameters niet"
    Een functie krijgt een **kopie** van de waarde van de parameter — niet de variabele zelf. Hierdoor kan een functie de originele variabele niet aanpassen. Dit heet *doorgeven per waarde* (pass by value).

    (In latere hoofdstukken zie je data types waarbij dit anders werkt, maar voor strings, integers en floats geldt dit altijd.)

De **volgorde** van parameters is belangrijk. Zo werkt `pow()` met twee parameters: de eerste is de basis, de tweede de exponent:

```python
basis = 2
exponent = 3
print(pow(basis, exponent))    # 2³ = 8
print(pow(exponent, basis))    # 3² = 9  ← andere volgorde, ander resultaat!
```

!!! danger "Verkeerde parameters → runtime error"
    Een functie aanroepen met waarden waar ze niet mee overweg kan, leidt tot een runtime error:

    ```python
    x = pow(3, "2")              # ❌ string als exponent
    y = int("twee-en-een-half")  # ❌ geen getal in de string
    ```

---

### 5.1.3 Retourwaarde

Veel functies **geven een waarde terug**. Je kunt die opslaan in een variabele, meteen afdrukken, of als parameter doorgeven aan een andere functie:

```python
x = 2.1
y = "3"
z = int(x)       # retourwaarde opslaan in z
print(z)
print(int(y))    # retourwaarde direct afdrukken
```

Je kunt zelfs een functieaanroep als parameter aan een andere functie meegeven. Python evalueert dan eerst de binnenste aanroep:

```python
print(int(3.7))   # eerst int(3.7) = 3, dan print(3)
```

#### `None` — geen retourwaarde

Niet elke functie geeft iets terug. `print()` heeft **geen** retourwaarde. Als je dat vergeet, kunnen er vreemde dingen gebeuren:

```python
print(print("Hello, world!"))
```

Dit geeft twee regels uitvoer:
```
Hello, world!
None
```

!!! question "Waarom `None`?"
    Python evalueert dit van binnen naar buiten:

    1. De binnenste `print("Hello, world!")` wordt uitgevoerd → drukt `Hello, world!` af
    2. De retourwaarde van `print()` is... niets. Python noemt dat `None`.
    3. De buitenste `print(None)` drukt dan `None` af.

`None` is een speciale waarde die betekent: *er is geen waarde*. Het is niet hetzelfde als `0`, `False` of `""`. Het is letterlijk niets.

---

### 5.1.4 Een functie is een zwarte doos

Je hoeft **niet** te weten hoe een functie van binnen werkt. Je hoeft alleen te weten: naam, parameters en retourwaarde. Alles wat de functie intern doet, heeft geen effect op de rest van je code — zolang het een **pure functie** is.

!!! note "Pure functies vs. modifiers"
    Een **pure functie** heeft geen bijwerkingen op de rest van de code. Alle functies in dit hoofdstuk zijn puur.

    Later zie je ook **modifiers** — functies die wel variabelen buiten zichzelf kunnen aanpassen. Die worden duidelijk aangegeven als ze aan bod komen.

---

## 5.2 Basisfuncties

### 5.2.1 Type casting

Je kent ze al uit hoofdstuk 3, maar hier de volledige beschrijving:

| Functie | Wat doet het? | Bijzonderheden |
|---------|--------------|----------------|
| `int(x)` | Geeft `x` terug als integer | Float wordt naar beneden afgerond; string werkt alleen als ze een geheel getal bevat |
| `float(x)` | Geeft `x` terug als float | Integer krijgt `.0`; string werkt als ze een getal bevat |
| `str(x)` | Geeft `x` terug als string | Werkt altijd |

!!! example "Tussenvraag"
    Wat doet de volgende code? Probeer het eerst zelf te bedenken, en test daarna:

    ```python
    print(10 * int("100,000,000"))
    ```

    Dit geeft een runtime error. Kun je het probleem oplossen door **precies twee tekens** te verwijderen?

??? note "Antwoord (klik om te openen)"
    De komma's in `"100,000,000"` zorgen ervoor dat `int()` de string niet herkent als een getal. Verwijder de komma's: `int("100000000")`. Dat zijn precies twee komma's verwijderd.

---

### 5.2.2 Rekenfuncties

Python heeft een aantal handige ingebouwde rekenfuncties:

| Functie | Beschrijving | Voorbeeld |
|---------|-------------|-----------|
| `abs(x)` | Absolute waarde (altijd positief) | `abs(-7)` → `7` |
| `max(a, b, ...)` | Grootste van twee of meer waarden | `max(3, 7, 2)` → `7` |
| `min(a, b, ...)` | Kleinste van twee of meer waarden | `min(3, 7, 2)` → `2` |
| `pow(x, y)` | `x` tot de macht `y` (= `x ** y`) | `pow(2, 3)` → `8` |
| `round(x, n)` | Wiskundig afronden op `n` decimalen | `round(3.567, 2)` → `3.57` |

!!! note "pow() met drie parameters"
    `pow(x, y, z)` berekent `x` tot de macht `y`, modulo `z`. Dat is efficiënter dan `(x ** y) % z` voor grote getallen.

!!! note "round() zonder tweede parameter"
    `round(x)` rondt af op een geheel getal:
    ```python
    print(round(3.7))    # 4
    print(round(3.2))    # 3
    ```

!!! example "Tussenvraag"
    Wat drukt de volgende code af? Bedenk het eerst, en controleer dan:

    ```python
    x = -2
    y = 3
    z = 1.27
    print(abs(x))
    print(max(x, y, z))
    print(min(x, y, z))
    print(pow(x, y))
    print(round(z, 1))
    ```

---

### 5.2.3 `len()`

`len()` geeft de **lengte** van zijn parameter terug. Voor een string is dat het aantal tekens:

```python
print(len("man"))      # 3
print(len("mango"))    # 5
print(len(""))         # 0  (lege string)
```

!!! example "Tussenvraag"
    Hoeveel tekens heeft de string `'mango\'s'`? Bedenk het eerst, en test dan:

    ```python
    print(len('mango\'s'))
    ```

    Denk goed na over de backslash!

??? note "Antwoord (klik om te openen)"
    De backslash `\'` is een **escapeteken** — het geeft aan dat het volgende teken letterlijk in de string staat. De backslash zelf telt **niet** mee als teken. `'mango\'s'` heeft dus 7 tekens: m, a, n, g, o, ', s.

---

### 5.2.4 `input()`

Met `input()` kun je de gebruiker om invoer vragen. De functie toont een **prompt** op het scherm, wacht tot de gebruiker iets typt en op ++enter++ drukt, en geeft de ingevoerde tekst terug als **string**:

```python
tekst = input("Geef een tekst in: ")
print("Je hebt het volgende ingetypt:", tekst)
```

!!! danger "`input()` geeft altijd een string terug"
    Dit is een veelgemaakte fout:

    ```python
    nummer = input("Geef een getal: ")
    print("Het kwadraat is", nummer * nummer)   # ❌ runtime error!
    ```

    `nummer` is een string, en twee strings met `*` vermenigvuldigen kan niet. De oplossing is type casting:

    ```python
    nummer = input("Geef een getal: ")
    nummer = float(nummer)
    print("Het kwadraat is", nummer * nummer)   # ✅
    ```

!!! warning "Wat als de gebruiker iets raars ingeeft?"
    Als de gebruiker geen getal ingeeft maar je doet `float(input(...))`, krijg je een runtime error. De nette oplossing daarvoor komt pas in hoofdstuk 17. Gebruik voor nu de `pcinput` module (zie sectie 5.3.3) als je dit wilt vermijden.

---

### 5.2.5 `print()` — uitgebreid

Je kent `print()` al, maar er zijn twee handige extra opties: `sep` en `end`.

**`sep`** — bepaalt wat er tussen de parameters wordt geplaatst (standaard: een spatie):

```python
print("X", "X", "X", sep="x")    # XxXxX
print("a", "b", "c", sep=", ")   # a, b, c
print("a", "b", "c", sep="")     # abc
```

**`end`** — bepaalt wat er na de laatste parameter komt (standaard: een nieuwe regel):

```python
print("X", end="")
print("Y", end="")
print("Z")
# Uitvoer: XYZ  (alles op één regel)
```

`print()` zonder parameters drukt een **lege regel** af:

```python
print("Eerste regel")
print()
print("Derde regel")
```

---

### 5.2.6 `format()` — opmaak van strings

Met `format()` kun je waarden op een precieze manier in een string inlassen. Je markeert de plaatsen met accolades `{}`:

```python
print("De eerste drie zijn {}, {} en {}.".format("een", "twee", "drie"))
```

Je kunt de volgorde bepalen via nummers (de eerste parameter is `0`):

```python
print("Achterstevoren: {2}, {1} en {0}.".format("een", "twee", "drie"))
```

#### Opmaak voor strings

Gebruik een dubbele punt `:` gevolgd door opmaakopties. Voor strings:

| Opmaak | Betekenis | Voorbeeld |
|--------|----------|-----------|
| `{:10}` | Reserveer 10 posities | links uitgelijnd |
| `{:<10}` | Links uitlijnen in 10 posities | `"hoi       "` |
| `{:^10}` | Centreren in 10 posities | `"   hoi    "` |
| `{:>10}` | Rechts uitlijnen in 10 posities | `"       hoi"` |

```python
print("{:<7}, {:^7} en {:>7}.".format("een", "twee", "drie"))
```

#### Opmaak voor getallen

| Opmaak | Betekenis |
|--------|----------|
| `{:d}` | Integer |
| `{:f}` | Float (standaard 6 decimalen) |
| `{:.2f}` | Float met 2 decimalen |
| `{:8.2f}` | Float met 2 decimalen in 8 posities breedte |
| `{:>8.2f}` | Idem, rechts uitgelijnd |

```python
print("{:.2f} gedeeld door {:.2f} is {:.2f}".format(1, 2, 1/2))
# 1.00 gedeeld door 2.00 is 0.50
```

Handig voor nette tabellen:

```python
s = "{:>5d} keer {:>5.2f} is {:>5.2f}"
print(s.format(1, 3.75, 1 * 3.75))
print(s.format(2, 3.75, 2 * 3.75))
print(s.format(3, 3.75, 3 * 3.75))
```

#### f-strings (Python 3.6+)

Een modernere en leesbaardere manier van string opmaak zijn **f-strings**. Je schrijft een `f` voor de string en plaatst variabelen of expressies direct tussen de accolades:

```python
a = 3.75
print(f"{2:>5d} keer {a:>5.2f} is {2 * a:>5.2f}")
```

!!! tip "f-strings of format()?"
    Beide werken. f-strings zijn moderner en leesbaarder. Het boek gebruikt `format()` voor compatibiliteit met oudere versies, maar in de praktijk gebruik je beter f-strings.

---

## 5.3 Modules

Naast de ingebouwde functies heeft Python een groot aantal **modules** — verzamelingen van extra functies. Je importeert een module bovenaan je programma:

```python
import math
print(math.sqrt(4))   # 2.0
```

Of je importeert specifieke functies zodat je de modulenaam niet telkens hoeft te typen:

```python
from math import sqrt
print(sqrt(4))        # 2.0
```

Je kunt een functie ook onder een andere naam importeren met `as`:

```python
from math import sqrt as wortel
print(wortel(4))      # 2.0
```

!!! tip "Zoek eerst een module!"
    Voor vrijwel elk algemeen probleem bestaat er al een Python module. **Zoek eerst** of er een module bestaat voordat je het zelf gaat programmeren.

---

### 5.3.1 De `math` module

De `math` module bevat wiskundige functies. Enkele nuttige:

| Functie | Beschrijving |
|---------|-------------|
| `sqrt(x)` | Vierkantswortel van `x` |
| `exp(x)` | e tot de macht `x` |
| `log(x)` | Natuurlijk logaritme van `x` |
| `log10(x)` | Logaritme met grondtal 10 van `x` |

```python
from math import exp, log

print("De waarde van e is bij benadering", exp(1))
e_sqr = exp(2)
print("e kwadraat is", e_sqr, "wat betekent")
print("dat log(", e_sqr, ") gelijk is aan", log(e_sqr))
```

---

### 5.3.2 De `random` module

De `random` module genereert **pseudo-willekeurige getallen** (voor alle praktische doeleinden: echt willekeurig):

| Functie | Beschrijving |
|---------|-------------|
| `random()` | Float tussen 0.0 (inclusief) en 1.0 (exclusief) |
| `randint(a, b)` | Willekeurig geheel getal tussen `a` en `b` (beide inclusief) |
| `seed(n)` | Initialiseert de generator — zelfde seed = zelfde reeks |

```python
from random import random, randint, seed

seed()
print("Een toevalsgetal tussen 1 en 10:", randint(1, 10))
print("Nog een:", randint(1, 10))

seed(0)
print("3 toevalsgetallen:", random(), random(), random())
seed(0)
print("Dezelfde 3:", random(), random(), random())
```

!!! note "Waarom `seed()`?"
    Met een vaste seed krijg je elke keer dezelfde reeks willekeurige getallen. Handig bij het **testen** van programma's: je weet precies wat de invoer zal zijn. Voor echte willekeur gebruik je `seed()` zonder parameter.

---

### 5.3.3 De `pcinput` module

`pcinput` is een module speciaal voor dit boek, geschreven door de auteur. Ze bevat functies die de gebruiker om specifieke invoer vragen en blijven vragen tot die correct is:

| Functie | Wat doet ze? |
|---------|-------------|
| `getInteger(prompt)` | Vraagt om een geheel getal |
| `getFloat(prompt)` | Vraagt om een getal met decimalen |
| `getString(prompt)` | Vraagt om een tekst (spaties rondom worden verwijderd) |
| `getLetter(prompt)` | Vraagt om één letter (geeft hoofdletter terug) |

Je kunt de module downloaden via [spronck.net/pythonbook](http://www.spronck.net/pythonbook). Sla het bestand op in **dezelfde map** als je Python programma's.

```python
from pcinput import getInteger

num1 = getInteger("Geef een geheel getal: ")
num2 = getInteger("Geef een ander geheel getal: ")
print(num1, "+", num2, "=", num1 + num2)
```

!!! info "Hoe werkt pcinput van binnen?"
    Dat hoef je nu niet te weten — de techniek die ervoor nodig is, wordt pas uitgelegd in hoofdstuk 17. Gebruik de functies gewoon als een zwarte doos: je weet wat erin gaat en wat eruit komt, en dat is genoeg.

---

## Wat je geleerd hebt

In dit hoofdstuk heb je het volgende gezien:

- Wat functies zijn: naam, parameters en retourwaarde
- Parameters worden doorgegeven per waarde — de originele variabele wijzigt niet
- `None` is de speciale waarde voor "geen retourwaarde"
- Een functie is een zwarte doos — je hoeft de implementatie niet te kennen
- Basisfuncties: `abs()`, `max()`, `min()`, `pow()`, `round()`, `len()`, `input()`
- `print()` met `sep` en `end`
- String opmaak met `format()` en f-strings
- Modules importeren met `import` en `from ... import`
- De `math` module: `sqrt()`, `exp()`, `log()`, `log10()`
- De `random` module: `random()`, `randint()`, `seed()`
- De `pcinput` module: `getInteger()`, `getFloat()`, `getString()`, `getLetter()`

---

## Opgaven

### Opgave 5.1 — Stringlengte opvragen

!!! example "Opgave 5.1"
    Vraag de gebruiker om een string in te geven met `input()` (gebruik **niet** `getString()` — die verwijdert spaties aan het begin en einde). Druk daarna de lengte van de ingegeven string af.

### Opgave 5.2 — Stelling van Pythagoras

!!! example "Opgave 5.2"
    Schrijf een programma dat de gebruiker vraagt om de lengtes van de **twee rechthoekszijden** van een rechthoekige driehoek. Bereken dan de lengte van de **schuine zijde** (hypotenusa).

    Formule: `c = √(a² + b²)`

    Toon het resultaat netjes opgemaakt met twee decimalen. Je hoeft geen rekening te houden met negatieve invoer of nul.

    **Hint:** Gebruik `sqrt()` uit de `math` module.

### Opgave 5.3 — Grootste, kleinste en gemiddelde

!!! example "Opgave 5.3"
    Vraag de gebruiker om **drie getallen** in te geven. Toon dan:

    - Het grootste getal
    - Het kleinste getal
    - Het gemiddelde, afgerond op **twee decimalen**

### Opgave 5.4 — Machten van e

!!! example "Opgave 5.4"
    Bereken de waarde van **e** tot de machten `-1`, `0`, `1`, `2` en `3`. Toon de resultaten netjes opgemaakt met **vijf decimalen**.

    De uitvoer zou er zo uit moeten zien:
    ```
    e^-1 = 0.36788
    e^ 0 = 1.00000
    e^ 1 = 2.71828
    e^ 2 = 7.38906
    e^ 3 = 20.08554
    ```

    **Hint:** Gebruik `exp()` uit de `math` module en `format()` of een f-string.

### Opgave 5.5 — Willekeurig geheel getal zonder `randint()`

!!! example "Opgave 5.5"
    Je wil een **willekeurig geheel getal tussen 1 en 10** (inclusief beide grenzen) genereren, maar je mag **alleen `random()`** gebruiken (je mag wel andere functies en modules erbij gebruiken).

    `random()` geeft een float tussen 0.0 en 1.0 (0.0 inclusief, 1.0 exclusief). Hoe gebruik je dat om een getal tussen 1 en 10 te krijgen?

    Schrijf de code en test hem een paar keer om te controleren of hij werkt.

!!! tip "Hint"
    Denk na: als `random()` een getal tussen 0 en 1 geeft, wat geeft `random() * 10` dan? En hoe zet je een float om naar een integer?

---

*Volgende: [Hoofdstuk 6 – Condities](h06-condities.md)*
