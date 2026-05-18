---
title: Hoofdstuk 6 – Condities
description: Boolean expressies, vergelijkingen, logische operatoren en conditionele statements in Python.
---

# Hoofdstuk 6 – Condities

In een programma wil je soms bepaalde code alleen uitvoeren onder bepaalde omstandigheden. Dat doe je met **conditionele statements** — ook wel *if-statements* of gewoon *condities* genoemd. Dit zijn de bouwstenen van elke echte beslissingslogica.

---

## 6.1 Boolean expressies

Een conditioneel statement bestaat uit een **test** en **acties**. De test is een *boolean expressie* — een expressie die evalueert als ofwel `True` (waar) of `False` (onwaar).

### 6.1.1 Booleans

In Python zijn `True` en `False` de twee boolean waarden. Ze hebben het data type `bool`:

```python
print(type(True))    # <class 'bool'>
print(type(False))   # <class 'bool'>
```

In Python kan **elke waarde** als boolean worden geïnterpreteerd. De volgende waarden worden beschouwd als `False`:

| Waarde | Type |
|--------|------|
| `False` | bool |
| `None` | NoneType |
| `0` of `0.0` | int / float |
| `""` | lege string |
| Lege collecties (later) | list, dict, ... |

!!! tip "Alles wat niet `False` is, is `True`"
    Als een waarde niet in de tabel hierboven staat, wordt hij beschouwd als `True`. Dus `1`, `"hallo"`, `3.14` zijn allemaal `True`.

---

### 6.1.2 Vergelijkingen

De meestgebruikte boolean expressies zijn **vergelijkingen**. Je plaatst een vergelijkingsoperator tussen twee waarden:

| Operator | Betekenis |
|----------|----------|
| `<` | kleiner dan |
| `<=` | kleiner dan of gelijk aan |
| `==` | gelijk aan |
| `>=` | groter dan of gelijk aan |
| `>` | groter dan |
| `!=` | niet gelijk aan |

```python
print("1.", 2 < 5)
print("2.", 2 <= 5)
print("3.", 3 > 3)
print("4.", 3 >= 3)
print("5.", 3 == 3.0)
print("6.", 3 == "3")
print("7.", "syntax" == "syntax")
print("8.", "syntax" == "semantiek")
print("9.", "Python" != "rotzooi")
print("10.", "Python" > "Perl")
print("11.", "banaan" < "mango")
print("12.", "banaan" < "Mango")
```

!!! warning "Gebruik `==`, niet `=` voor vergelijken"
    Een veelgemaakte fout: twee waarden vergelijken met `=` in plaats van `==`. De enkele `=` is de **toekenningsoperator**. Gebruik altijd `==` om te vergelijken.

    ```python
    x = 5        # ✅ toekenning: sla 5 op in x
    x == 5       # ✅ vergelijking: is x gelijk aan 5?
    ```

!!! note "Strings vergelijken"
    Vergelijkingen tussen strings zijn **alfabetisch**. Hoofdletters worden als kleiner beschouwd dan kleine letters, en cijfers kleiner dan letters.

    Daarom: `"banaan" < "Mango"` → `False` (want kleine letter `b` is groter dan hoofdletter `M`)!

!!! example "Tussenvraag"
    Begrijp je waarom `3 < 13` de waarde `True` oplevert, maar `"3" < "13"` de waarde `False`?

??? note "Antwoord (klik om te openen)"
    Bij getallen: `3` is wiskundig kleiner dan `13`. ✅

    Bij strings vergelijkt Python teken voor teken. Het eerste teken van `"3"` is `"3"`, en het eerste teken van `"13"` is ook `"1"`. Alfabetisch komt `"3"` na `"1"`, dus `"3" > "13"`. ❌

Je kunt een boolean expressie ook opslaan in een variabele:

```python
groter = 5 > 2
print(groter)          # True
groter = 5 < 2
print(groter)          # False
print(type(groter))    # <class 'bool'>
```

---

### 6.1.3 De `in` operator

Met de `in` operator test je of een waarde **voorkomt in een collectie**. Voor strings kun je testen of een teken of deelstring aanwezig is:

```python
print("y" in "Python")     # True
print("x" in "Python")     # False
print("p" in "Python")     # False  (hoofdlettergevoelig!)
print("th" in "Python")    # True
print("to" in "Python")    # False
print("y" not in "Python") # False
```

!!! note "`not in`"
    `not in` is het tegengestelde van `in` — het geeft `True` als de waarde **niet** voorkomt in de collectie.

---

### 6.1.4 Logische operatoren

Boolean expressies kun je combineren met **logische operatoren**: `and`, `or` en `not`.

| Operator | Beschrijving |
|----------|-------------|
| `and` | `True` als **beide** expressies `True` zijn |
| `or` | `True` als **minstens één** expressie `True` is |
| `not` | Keert de boolean waarde om |

```python
t = True
f = False

print(t and t)   # True
print(t and f)   # False
print(f and t)   # False
print(f and f)   # False

print(t or t)    # True
print(t or f)    # True
print(f or t)    # True
print(f or f)    # False

print(not t)     # False
print(not f)     # True
```

!!! warning "Gebruik haakjes bij combinaties van `and` en `or`"
    Combinaties van `and` en `or` kunnen tot verrassingen leiden. Maak altijd expliciet met haakjes hoe ze geëvalueerd moeten worden:

    ```python
    # Onduidelijk:
    a and b or c

    # Duidelijk:
    (a and b) or c
    a and (b or c)
    ```

    Deze twee geven **niet altijd hetzelfde resultaat**!

#### Kortsluitingsevaluatie

Python evalueert boolean expressies van **links naar rechts** en stopt zodra de uitkomst vaststaat. Dit heet *kortsluitingsevaluatie* (short-circuit evaluation):

```python
x = 1
y = 0
print((x == 0) or (y == 0) or (x / y == 1))
```

Hier zou `x / y` een `ZeroDivisionError` geven — maar dat gebeurt niet! Python evalueert `(y == 0)` en ziet dat dat `True` is. Omdat de hele expressie via `or` verbonden is en al één onderdeel `True` is, stopt Python en evalueert `x / y == 1` niet meer.

!!! tip "Volgorde is belangrijk"
    Zorg bij dergelijke constructies dat de "gevaarlijke" expressie **rechts** staat van de veiligheidscheck.

---

## 6.2 Conditionele statements

### 6.2.1 Het `if` statement

De basisvorm van een conditioneel statement:

```python
if <boolean expressie>:
    <acties>
```

Voorbeeld:

```python
x = 5
if x == 5:
    print("x is 5")
```

Het blok code onder de `if` wordt **alleen uitgevoerd** als de boolean expressie `True` is. Staat er `False`, wordt het blok overgeslagen.

---

### 6.2.2 Blokken code en inspringing

Python herkent blokken code aan de **inspringing** (indentatie). Alle regels die even ver inspringen en direct onder een `if` staan, vormen één blok:

```python
x = 7
if x < 10:
    print("Deze regel wordt alleen uitgevoerd als x < 10.")
    print("En dat geldt ook voor deze regel.")
print("Deze regel wordt altijd uitgevoerd.")
```

!!! danger "Inspringing is verplicht in Python!"
    Zonder correcte inspringing werkt je code niet. Python kan dan niet zien welke regels bij welk blok horen.

    De standaard inspringing is **vier spaties**. De meeste editors (IDLE, VS Code) doen dit automatisch na een `:`.

!!! tip "Tab vs. spaties"
    Gebruik consequent **spaties** (geen Tab) — of zorg dat je editor tabs automatisch omzet naar vier spaties. Mixen van tabs en spaties geeft `IndentationError`s.

!!! example "Opgave: inspringfouten zoeken"
    De volgende code bevat inspringfouten. Verbeter ze:

    ```python
    # Deze code bevat inspringfouten!
    x = 3
    y = 4
    if x == 3 and y == 4:
    print("x is 3")
        print("y is 4")
    if x > 2 and y < 5:
        print("x > 2")
      print("y < 5")
    ```

---

### 6.2.3 Twee-weg beslissingen: `if`/`else`

Als je wil dat er iets *anders* gebeurt wanneer de conditie `False` is, gebruik je een `else` tak:

```python
if <boolean expressie>:
    <acties als True>
else:
    <acties als False>
```

Voorbeeld:

```python
x = 4
if x > 2:
    print("x is groter dan 2")
else:
    print("x is kleiner dan of gelijk aan 2")
```

!!! note "Altijd precies één tak"
    Bij een `if`/`else` wordt altijd **exact één** van de twee blokken uitgevoerd — nooit beide, nooit geen.

!!! note "Uitlijning van `else`"
    De `else` moet **uitgelijnd zijn** met de bijbehorende `if`. Doe je dit niet, krijg je een inspring- of syntaxfout.

---

### 6.2.4 Stroomdiagrammen

Een **stroomdiagram** is een visuele weergave van een algoritme. Ze worden vandaag de dag minder gebruikt, maar kunnen helpen om de werking van condities te begrijpen.

De bouwstenen:

- **Rechthoek** → een actie/statement
- **Ruit** → een conditie (`True`/`False`)
- **Afgeronde rechthoek** → Start of Stop

Voor de `if`/`else` hierboven ziet het stroomdiagram er zo uit:

```
        ┌─────────┐
        │  Start  │
        └────┬────┘
             ▼
        ◇ x > 2? ◇
       True ↓   ↓ False
   ┌──────────┐ ┌────────────────┐
   │ print(   │ │ print(         │
   │ "groter" │ │ "kleiner/gelijk│
   │ dan 2)   │ │ aan 2")        │
   └────┬─────┘ └───────┬────────┘
        └───────┬────────┘
                ▼
          ┌──────────┐
          │   Stop   │
          └──────────┘
```

---

### 6.2.5 Meer-weg beslissingen: `elif`

Als je meer dan twee mogelijkheden hebt, gebruik je `elif` ("else if"):

```python
if <boolean expressie>:
    <acties>
elif <boolean expressie>:
    <acties>
elif <boolean expressie>:
    <acties>
else:
    <acties>
```

Voorbeeld:

```python
leeftijd = 21

if leeftijd < 12:
    print("Je bent een kind!")
elif leeftijd < 18:
    print("Je bent een teenager!")
elif leeftijd < 30:
    print("Je bent nog jong!")
elif leeftijd < 50:
    print("Beginnen grijze haren te komen?")
else:
    print("Wegen de jaren zwaar?")
```

!!! info "Hoe werkt `elif`?"
    Python test de condities **van boven naar beneden**. Zodra er één `True` is, wordt dat blok uitgevoerd en worden **alle andere blokken overgeslagen** — ook als een latere conditie ook `True` zou zijn.

    Daardoor is bij de eerste `elif` (`leeftijd < 18`) geen extra check nodig voor `leeftijd >= 12`. Als `leeftijd` kleiner dan 12 was, had de `if` al getriggerd.

!!! note "`else` is optioneel"
    Je mag de `else` weglaten als er geen "standaard" actie nodig is. Maar het is een goede gewoonte om hem toch te zetten, zeker als vangnet voor onverwachte waarden.

---

### 6.2.6 Geneste condities

Je kunt `if` statements **binnen andere `if` blokken** plaatsen. Dit heet *nesten*:

```python
x = 41

if x % 7 == 0:
    if x % 11 == 0:
        print(x, "is deelbaar door 7 en 11.")
    else:
        print(x, "is deelbaar door 7, maar niet door 11.")
elif x % 11 == 0:
    print(x, "is deelbaar door 11, maar niet door 7.")
else:
    print(x, "is niet deelbaar door 7 of 11.")
```

!!! tip "Vermijd diep nesten"
    Geneste condities worden snel moeilijk leesbaar. Gebruik waar mogelijk `elif` in plaats van geneste `if`/`else`. Vergelijk:

    === "Met nesten (minder leesbaar)"
        ```python
        if leeftijd < 12:
            print("kind")
        else:
            if leeftijd < 18:
                print("teenager")
            else:
                if leeftijd < 30:
                    print("jong")
        ```

    === "Met elif (leesbaarder)"
        ```python
        if leeftijd < 12:
            print("kind")
        elif leeftijd < 18:
            print("teenager")
        elif leeftijd < 30:
            print("jong")
        ```

---

## 6.3 Vroegtijdig afbreken met `exit()`

Soms wil je een programma meteen stoppen als een fout optreedt. Dat doe je met `exit()` uit de `sys` module:

```python
from pcinput import getInteger
from sys import exit

num = getInteger("Geef een positief geheel getal: ")
if num < 0:
    print("Je had een positief geheel getal moeten geven!")
    exit()

print("Ik handel je getal", num, "af")
print("Nog meer code...")
```

!!! note "Waarom `exit()`?"
    Zonder `exit()` zou je de rest van het programma in een `else` tak moeten zetten, wat diepere inspringing veroorzaakt voor alle volgende code. Met `exit()` stopt het programma onmiddellijk en blijft de rest van de code op het hoofdniveau staan.

!!! warning "SystemExit melding"
    In sommige editors (niet in IDLE) zie je bij `exit()` een `SystemExit` melding. Dat ziet er uit als een fout, maar is het niet — het is gewoon de bevestiging dat het programma gestopt is. Je mag dit negeren.

---

## 6.4 `match`/`case` (Python 3.10+)

Python 3.10 introduceerde een alternatief voor lange `if`/`elif` ketens waarbij je één variabele tegen meerdere vaste waarden test: het `match`/`case` statement.

```python
match <expressie>:
    case <waarde>:
        <acties>
    case <waarde>:
        <acties>
    case other:
        <acties>
```

Voorbeeld:

```python
a = 2
match a:
    case 1:
        print("a is 1")
    case 2:
        print("a is 2")
    case 3:
        print("a is 3")
    case other:
        print("a is niet 1, 2, of 3")
```

Dit is equivalent aan:

```python
if a == 1:
    print("a is 1")
elif a == 2:
    print("a is 2")
elif a == 3:
    print("a is 3")
else:
    print("a is niet 1, 2, of 3")
```

#### Wildcards met `_`

Je kunt ook **wildcards** gebruiken met het underscore symbool `_`, dat elke waarde matcht:

```python
x = 5
y = 0

match (x, y):
    case (0, 0):
        print("x en y zijn allebei 0")
    case (0, _):
        print("x is 0")
    case (_, 0):
        print("y is 0")
    case other:
        print("noch x noch y is 0")
```

!!! tip "`case _` als alternatief voor `case other`"
    Je kunt `case other` ook schrijven als `case _` — beide fungeren als de "vangnet" tak die altijd matcht als niets anders past.

---

## Wat je geleerd hebt

In dit hoofdstuk heb je het volgende gezien:

- Boolean waarden `True` en `False`, en welke waarden als `False` worden beschouwd
- Vergelijkingsoperatoren: `<`, `<=`, `==`, `>=`, `>`, `!=`
- De `in` en `not in` operator
- Logische operatoren: `and`, `or`, `not`
- Kortsluitingsevaluatie
- Het `if` statement en blokken code
- Inspringing — verplicht en essentieel in Python
- Twee-weg beslissingen met `if`/`else`
- Meer-weg beslissingen met `elif`
- Stroomdiagrammen als hulpmiddel
- Geneste condities
- Vroegtijdig afbreken met `exit()`
- Het `match`/`case` statement (Python 3.10+)

---

## Opgaven

### Opgave 6.1 — Cijfers naar letters

!!! example "Opgave 6.1"
    Schrijf een programma dat de gebruiker vraagt om een **cijfer tussen 0 en 10** (afgerond op halve punten). Het programma vertaalt het cijfer naar een Amerikaans lettercijfer:

    | Cijfer | Letter |
    |--------|--------|
    | 8.5 – 10 | A |
    | 7.5 – 8 | B |
    | 6.5 – 7 | C |
    | 5.5 – 6 | D |
    | 0 – 5 | F |

    Als de gebruiker een cijfer buiten het bereik 0–10 ingeeft, geef je een foutmelding.

### Opgave 6.2 — Fout in de logica

!!! example "Opgave 6.2"
    Snap je welke logische fout er zit in de volgende code? Wat is het probleem, en hoe los je het op?

    ```python
    score = 98.0

    if score >= 60.0:
        oordeel = 'D'
    elif score >= 70.0:
        oordeel = 'C'
    elif score >= 80.0:
        oordeel = 'B'
    elif score >= 90.0:
        oordeel = 'A'
    else:
        oordeel = 'F'

    print(oordeel)
    ```

??? note "Antwoord (klik om te openen)"
    De volgorde van de condities klopt niet. De eerste `if` test of `score >= 60`. Voor `score = 98.0` is dat `True` — dus het oordeel wordt meteen `D`, en de rest wordt nooit getest. De condities moeten van **hoog naar laag** staan.

### Opgave 6.3 — Klinkers tellen

!!! example "Opgave 6.3"
    Vraag de gebruiker om een string. Druk af hoeveel **verschillende** klinkers er in de string zitten (a, e, i, o, u — hoofdletters tellen mee als dezelfde klinker).

    Maak de uitvoer netjes: zeg niet "Er zitten 1 verschillende klinkers" maar "Er zit 1 verschillende klinker."

    Voorbeeld: voor de string `"De Heilige Handgranaat van Antioch"` is het antwoord **4 verschillende klinkers** (a, e, i, o — de u komt niet voor).

### Opgave 6.4 — Kwadratische vergelijking

!!! example "Opgave 6.4"
    Schrijf een programma dat de gebruiker vraagt om de coëfficiënten **A**, **B** en **C** van een kwadratische vergelijking: `Ax² + Bx + C = 0`.

    Gebruik de **wortelformule** om de oplossingen te berekenen:

    - Oplossing 1: `(-B + √(B² - 4AC)) / (2A)`
    - Oplossing 2: `(-B - √(B² - 4AC)) / (2A)`

    Het programma moet rapporteren:

    - **Geen oplossingen** als `B² - 4AC < 0`
    - **Één oplossing** als `B² - 4AC == 0`
    - **Twee oplossingen** als `B² - 4AC > 0`

    Vergeet ook de randgevallen niet:

    - Als `A == 0` en `B != 0`: één oplossing, namelijk `-C / B`
    - Als `A == 0` en `B == 0`: geen oplossing (of oneindig veel als ook `C == 0`)

!!! tip "Hint"
    Bereken eerst `discriminant = B * B - 4 * A * C`. Gebruik dat om te bepalen hoeveel oplossingen er zijn. Gebruik `sqrt()` uit de `math` module.

---

*Volgende: [Hoofdstuk 7 – Iteraties](h07-iteraties.md)*
