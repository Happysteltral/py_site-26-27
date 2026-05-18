---
title: Hoofdstuk 8 – Functies
description: Zelf functies schrijven, parameters, retourwaardes, scope en modules.
---

# Hoofdstuk 8 – Functies

In hoofdstuk 5 leerde je hoe je bestaande functies gebruikt. Nu ga je **zelf functies schrijven** — een van de krachtigste hulpmiddelen in de programmeerwereld. Functies zijn de bouwstenen waarmee je complexe programma's overzichtelijk en herbruikbaar houdt.

!!! tip "Herhaling"
    Als je niet meer precies weet wat hoofdstuk 5 over functies zei, neem dat hoofdstuk dan even opnieuw door voordat je verdergaat.

---

## 8.1 Het nut van functies

Waarom zou je functies schrijven? Er zijn een hoop goede redenen:

- Je wilt een functionaliteit **onafhankelijk ontwikkelen en testen**, los van de rest van het programma
- Een stukje code is **op meerdere plekken nodig** — kopiëren leidt tot moeilijk te onderhouden code
- Je programma is **te lang geworden** om goed overzicht te bewaren
- Een probleem is te complex om in één keer op te lossen, dus je **splitst het op** in kleinere stukken
- Diep geneste code wordt **leesbaarder** als je de binnenste delen in functies plaatst
- Code die je wilt **delen met anderen** verpak je in gedocumenteerde functies

Samengevat bieden functies:

| Voordeel | Wat het betekent |
|----------|----------------|
| **Encapsulatie** | Nuttige code verpakt zodat je hem kunt gebruiken zonder de details te kennen |
| **Generalisatie** | Code geschikt gemaakt voor diverse situaties via parameters |
| **Beheersbaarheid** | Complex programma opgedeeld in begrijpbare stukken |
| **Onderhoudbaarheid** | Betekenisvolle namen en logische opdelingen maken code leesbaar |
| **Herbruikbaarheid** | Code overdraagbaar maken tussen programma's |
| **Recursie** | De basis voor de techniek die in hoofdstuk 9 aan bod komt |

---

## 8.2 Functies definiëren

De syntax voor een eigen functie:

```python
def <functie_naam>(<parameter_lijst>):
    <acties>
```

- Functienamen volgen dezelfde regels als variabelenamen
- De parameterlijst is een kommagescheiden reeks van nul of meer variabelenamen
- Het blok code **moet inspringen**
- Functiedefinities staan **bovenaan je programma**, net onder de `import` statements

!!! danger "Definitie vóór aanroep"
    Python moet een functiedefinitie gezien hebben **voordat** je de functie aanroept. Definieer functies altijd bovenaan je code.

---

### 8.2.1 Hoe Python met functies omgaat

Bekijk dit kleine programma:

```python
def tot_ziens():
    print("Tot ziens!")

print("Hallo!")
tot_ziens()
```

Uitvoer:
```
Hallo!
Tot ziens!
```

!!! info "Waarom eerst 'Hallo!' en dan 'Tot ziens!'?"
    Python voert de code van een functie **alleen uit als de functie wordt aangeroepen**. Als Python de `def` tegenkomt, registreert het alleen dat de functie bestaat — het voert hem niet uit. Pas bij de aanroep `tot_ziens()` wordt de code van de functie uitgevoerd.

---

### 8.2.2 Parameters en argumenten

Een functie kan **parameters** hebben — variabelen die hun waarde krijgen via de functie-aanroep. De waarde die je meegeeft bij de aanroep heet een **argument**:

```python
def hallo(naam):
    print("Hallo, {}!".format(naam))

hallo("Groucho")
hallo("Chico")
hallo("Harpo")
hallo("Zeppo")
```

Parameters zijn **lokaal** voor de functie — ze bestaan alleen binnen de functie, en kunnen variabelen buiten de functie niet beïnvloeden.

Met meerdere parameters:

```python
def vermenigvuldig(x, y):
    resultaat = x * y
    print(resultaat)

vermenigvuldig(2020, 5278238)
vermenigvuldig(2, 3)
```

!!! tip "De functie als machine"
    Stel je een functie voor als een pannenkoekenmachine. Aan de bovenkant zitten **invoersleuven** (de parameters): `melk`, `eieren`, `meel`. Als je de juiste ingrediënten erin stopt, ratelt de machine, en via de **uitvoeropening** (de `return`) komt de pannenkoek eruit. Het mooie: je hoeft niet te weten hoe de machine van binnen werkt — je weet alleen wat je erin stopt en wat eruit komt.

---

### 8.2.3 Parameter types controleren

Python controleert niet automatisch of de argumenten van het juiste type zijn. Wil je dat zelf doen, gebruik dan `isinstance()`:

```python
a = "Hallo"
if isinstance(a, int):
    print("integer")
elif isinstance(a, float):
    print("float")
elif isinstance(a, str):
    print("string")
else:
    print("ander type")
```

!!! note "Wanneer types controleren?"
    Zolang je je eigen functies alleen zelf aanroept, weet je wat je erin stopt. Typechecks zijn vooral nuttig als je functies schrijft voor andere programmeurs. De nette manier om fouten te signaleren (via exceptions) komt in hoofdstuk 17.

---

### 8.2.4 Default parameterwaarden

Je kunt parameters een **standaardwaarde** geven. Als de aanroeper geen waarde opgeeft voor die parameter, wordt de standaardwaarde gebruikt:

```python
def vermenigvuldig_xyz(x, y=1, z=7):
    print(x * y * z)

vermenigvuldig_xyz(2, 2, 2)   # x=2, y=2, z=2  → 8
vermenigvuldig_xyz(2, 5)       # x=2, y=5, z=7  → 70
vermenigvuldig_xyz(2, z=5)     # x=2, y=1, z=5  → 10
```

!!! tip "Parameters met default rechts"
    Parameters met een standaardwaarde moet je **rechts** plaatsen van parameters zonder standaardwaarde. Je kunt een specifieke parameter ook bij naam aanroepen: `vermenigvuldig_xyz(2, z=5)` stelt `z` in op `5` zonder `y` aan te raken.

---

### 8.2.5 `return` — retourwaarden

Met `return` geeft een functie een waarde terug aan de aanroeper. Na `return` stopt de functie onmiddellijk:

```python
from math import sqrt

def pythagoras(a, b):
    return sqrt(a * a + b * b)

c = pythagoras(3, 4)
print(c)    # 5.0
```

Je kunt `return` ook gebruiken om een functie **vroegtijdig te beëindigen** — met of zonder waarde:

```python
from math import sqrt

def pythagoras(a, b):
    if a <= 0 or b <= 0:
        return -1       # foutcode
    return sqrt(a * a + b * b)

resultaat = pythagoras(3, 4)
if resultaat < 0:
    print("Ongeldige invoer.")
else:
    print("De schuine zijde is", resultaat)
```

!!! danger "Zorg altijd voor een retourwaarde"
    Als een functie in sommige omstandigheden iets retourneert en in andere niet, geeft Python `None` terug voor het geval dat er geen `return` is. Dat leidt tot verwarrende fouten:

    ```python
    def pythagoras(a, b):
        if a > 0 and b > 0:
            return sqrt(a * a + b * b)
        # geen return hier → geeft None terug!

    print(pythagoras(-3, 4))   # None — verwarrend!
    ```

    Zorg altijd dat je functie **in alle omstandigheden** een waarde retourneert.

!!! warning "Code na `return` wordt nooit uitgevoerd"
    Alles op hetzelfde inspringniveau na een `return` wordt nooit bereikt:

    ```python
    def pythagoras(a, b):
        if a <= 0 or b <= 0:
            return -1
            print("Dit wordt nooit afgedrukt")  # nutteloos!
        return sqrt(a * a + b * b)
    ```

---

### 8.2.6 Het verschil tussen `return` en `print`

Dit is een veelgemaakte verwarring. Vergelijk:

=== "Functie met `print`"
    ```python
    def print3():
        print(3)

    print3()           # drukt 3 af
    x = 2 ** print3()  # ❌ runtime error! print3 geeft None terug
    ```

=== "Functie met `return`"
    ```python
    def return3():
        return 3

    print(return3())   # drukt 3 af
    x = 2 ** return3() # ✅ werkt! x = 8
    ```

!!! info "Wanneer gebruik je wat?"
    - **`return`** → als je de waarde **elders in de code** wilt gebruiken (in berekeningen, toekenningen, enz.)
    - **`print`** → als je alleen iets op het scherm wilt **tonen**, zonder de waarde verder te gebruiken

    Een functie die alleen `print` gebruikt en geen `return` heeft, geeft altijd `None` terug.

---

### 8.2.7 Meerdere retourwaarden

Een functie kan **meerdere waarden tegelijk** teruggeven, gescheiden door komma's. Je vangt ze op door ze toe te kennen aan meerdere variabelen:

```python
import datetime

def plus_dagen(jaar, maand, dag, increment):
    startdatum = datetime.datetime(jaar, maand, dag)
    einddatum = startdatum + datetime.timedelta(days=increment)
    return einddatum.year, einddatum.month, einddatum.day

y, m, d = plus_dagen(2015, 11, 13, 55)
print("{}/{}/{}".format(y, m, d))
```

!!! note "Je hoeft niet te weten hoe `plus_dagen` intern werkt"
    De `datetime` module is complex. Maar je hoeft dat niet te begrijpen om de functie te gebruiken — je weet wat erin gaat (jaar, maand, dag, aantal dagen) en wat eruit komt (nieuw jaar, maand, dag). Dat is de kracht van functies.

---

### 8.2.8 Functies aanroepen vanuit functies

Functies mogen andere functies aanroepen, zolang die **al gedefinieerd** zijn:

```python
from math import sqrt

def pythagoras(a, b):
    if a <= 0 or b <= 0:
        return -1
    return sqrt(a * a + b * b)

def afstand(x1, y1, x2, y2):
    return pythagoras(abs(x1 - x2), abs(y1 - y2))

print(afstand(1, 1, 4, 5))   # 5.0
```

Je kunt ook functies **binnen andere functies** definiëren (geneste functies). De binnenste functie is dan alleen zichtbaar binnen de buitenste:

```python
from math import sqrt

def afstand(x1, y1, x2, y2):
    def pythagoras_intern(a, b):
        return sqrt(a * a + b * b)
    return pythagoras_intern(abs(x1 - x2), abs(y1 - y2))

print(afstand(1, 1, 4, 5))
# print(pythagoras_intern(3, 4))  # ❌ bestaat niet buiten afstand()
```

---

### 8.2.9 Functienamen

Conventies voor functienamen:

!!! success "Goede gewoontes"
    - Gebruik **alleen kleine letters** (soms `camelCase` voor meerdere woorden)
    - Gebruik een **underscore** tussen woorden: `bereken_oppervlakte()`
    - Begin **niet** met een underscore (voorbehouden aan Python zelf)
    - Functies die iets **testen** beginnen traditioneel met `is`: `is_even()`, `is_priemgetal()`

---

## 8.3 Variabele scope

**Scope** bepaalt waar in je code een variabele zichtbaar en bruikbaar is. Dit is een van de belangrijkste concepten bij het werken met functies.

### 8.3.1 Lokale variabelen

Variabelen die je **inside** een functie aanmaakt, bestaan alleen binnen die functie. Ze zijn **lokaal**:

```python
def mijn_functie():
    lokaal = "Ik besta alleen in de functie"
    print(lokaal)

mijn_functie()
print(lokaal)   # ❌ NameError! lokaal bestaat hier niet
```

Parameters zijn ook lokale variabelen — ze worden aangemaakt bij de aanroep en vernietigd als de functie klaar is.

### 8.3.2 Globale variabelen

Variabelen die **buiten** alle functies aangemaakt worden, zijn **globaal**. Ze zijn zichtbaar in het hele programma, ook binnen functies:

```python
ELEPHANT = "Olifant"

def druk_elephant_af():
    print(ELEPHANT)   # ✅ globale variabele is zichtbaar

druk_elephant_af()
```

!!! warning "Functies kunnen globale variabelen niet wijzigen"
    Als je in een functie een waarde toekent aan een naam die ook globaal bestaat, maakt Python een **nieuwe lokale variabele** — de globale blijft ongewijzigd:

    ```python
    x = 1

    def probeer_te_wijzigen():
        x = 2           # dit is een NIEUWE lokale x, niet de globale!
        print("In functie:", x)

    probeer_te_wijzigen()
    print("Buiten functie:", x)   # nog steeds 1!
    ```

!!! danger "Vermijd globale variabelen in functies"
    Het **lezen** van globale variabelen in functies mag, maar het is beter om ze als parameter mee te geven. Het **wijzigen** van globale variabelen in functies (via het `global` keyword) is een slechte gewoonte en maakt code moeilijk te begrijpen. Vermijd het.

### 8.3.3 Samenvatting scope

```python
my_var = "globaal"

def scope_voorbeeld():
    my_var = "lokaal"       # nieuwe lokale variabele
    print(my_var)           # "lokaal"

scope_voorbeeld()
print(my_var)               # "globaal" — ongewijzigd
```

!!! tip "Ezelsbruggetje"
    Beschouw iedere functie als een **aparte ruimte**. Wat erin gebeurt, blijft erin. De buitenwereld ziet alleen wat de functie via `return` doorgeeft.

---

## 8.4 Modules zelf schrijven

Je kunt functies die je zelf schreef **beschikbaar stellen voor andere programma's** door ze in een apart `.py` bestand te plaatsen. Dat bestand is dan een module die je kunt importeren.

Maak een bestand `mijn_module.py`:

```python
# mijn_module.py

def zeg_hallo(naam):
    print("Hallo, {}!".format(naam))

def kwadraat(x):
    return x * x
```

En importeer het in een ander programma (dat in **dezelfde map** staat):

```python
from mijn_module import zeg_hallo, kwadraat

zeg_hallo("Python")
print(kwadraat(7))
```

!!! info "Zelfde map!"
    Het bestand dat je importeert moet in **dezelfde map** staan als het programma dat hem importeert, of in een map die Python kent. Begin met alles in dezelfde map te zetten.

---

## 8.5 `__name__` en `"__main__"`

Als je een module importeert, wordt alle code in die module uitgevoerd — inclusief eventuele test-aanroepen. Dat wil je niet altijd. De oplossing:

```python
# mijn_module.py

def kwadraat(x):
    return x * x

if __name__ == "__main__":
    # Dit wordt alleen uitgevoerd als je dit bestand DIRECT start
    # Niet als het geïmporteerd wordt
    print(kwadraat(5))
```

!!! info "Hoe werkt `__name__`?"
    Als je een Python bestand direct uitvoert, heeft `__name__` de waarde `"__main__"`. Als het bestand wordt geïmporteerd, heeft `__name__` de naam van de module. Door die check te doen, zorg je dat testcode alleen loopt als je het bestand direct start.

Dit is de **standaard patroon** voor Python bestanden die zowel als module als als zelfstandig programma gebruikt kunnen worden.

---

## Wat je geleerd hebt

In dit hoofdstuk heb je het volgende gezien:

- Het nut van functies: encapsulatie, generalisatie, beheersbaarheid, onderhoudbaarheid, herbruikbaarheid
- Functies definiëren met `def`
- Parameters en argumenten
- Default parameterwaarden
- `return` — waarden teruggeven aan de aanroeper
- Het verschil tussen `return` en `print`
- Meerdere retourwaarden
- Functies aanroepen vanuit andere functies
- Geneste functies
- Variabele scope: lokaal vs. globaal
- Zelf modules schrijven
- `__name__ == "__main__"`

---

## Opgaven

### Opgave 8.1 — Tafel van vermenigvuldiging als functie

!!! example "Opgave 8.1"
    Schrijf een functie `druk_tafel(n)` die de vermenigvuldigingstabel van `n` afdrukt voor 1 tot en met 10.

    Roep de functie aan voor de getallen 3, 7 en 12.

### Opgave 8.2 — `is_even()`

!!! example "Opgave 8.2"
    Schrijf een functie `is_even(n)` die `True` retourneert als `n` even is, en `False` als `n` oneven is.

    Test hem met een paar getallen:

    ```python
    print(is_even(4))    # True
    print(is_even(7))    # False
    print(is_even(0))    # True
    ```

### Opgave 8.3 — Maximum van drie

!!! example "Opgave 8.3"
    Schrijf een functie `maximum(a, b, c)` die het grootste van drie getallen retourneert. Gebruik **geen** ingebouwde `max()` functie — schrijf de logica zelf met condities.

### Opgave 8.4 — Priemgetal tester

!!! example "Opgave 8.4"
    Schrijf een functie `is_priemgetal(n)` die `True` retourneert als `n` een priemgetal is, en `False` als dat niet zo is. Gebruik de functie daarna in een programma dat alle priemgetallen tot en met 50 afdrukt.

    **Reminder:** Een priemgetal is een getal groter dan 1 dat alleen deelbaar is door 1 en zichzelf.

!!! tip "Hint"
    Gebruik een `for` loop met `range(2, n)` om alle mogelijke delers te testen. Gebruik `break` zodra je een deler vindt, en `else` om te detecteren dat er geen deler gevonden is.

### Opgave 8.5 — Faculteit als functie

!!! example "Opgave 8.5"
    Schrijf een functie `faculteit(n)` die de faculteit van `n` retourneert.

    Gebruik de functie om de faculteiten van 1 tot en met 10 netjes op te drukken:

    ```
     1! =       1
     2! =       2
     3! =       6
    ...
    10! = 3628800
    ```

    Opmaaktip: gebruik `format()` of een f-string voor nette uitlijning.

### Opgave 8.6 — `fizzbuzz()`

!!! example "Opgave 8.6"
    FizzBuzz is een klassieker uit sollicitatiegesprekken. Schrijf een functie `fizzbuzz(n)` die:

    - `"Fizz"` retourneert als `n` deelbaar is door 3
    - `"Buzz"` retourneert als `n` deelbaar is door 5
    - `"FizzBuzz"` retourneert als `n` deelbaar is door zowel 3 als 5
    - `n` zelf retourneert (als integer) in alle andere gevallen

    Gebruik de functie in een loop die de resultaten voor 1 tot en met 30 afdrukt.

### Opgave 8.7 — Eigen module

!!! example "Opgave 8.7"
    Maak een bestand `rekenen.py` met daarin de functies `is_even()`, `is_priemgetal()` en `faculteit()` die je hierboven hebt gemaakt.

    Voeg onderaan toe:

    ```python
    if __name__ == "__main__":
        print(is_even(7))
        print(is_priemgetal(13))
        print(faculteit(5))
    ```

    Importeer dan `rekenen` in een nieuw programma en gebruik de functies erin.

---

*Volgende: [Hoofdstuk 9 – Recursie](h09-recursie.md)*
