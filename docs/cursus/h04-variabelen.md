---
title: Hoofdstuk 4 – Variabelen
description: Wat zijn variabelen, hoe gebruik je ze, en hoe kies je goede namen?
---

# Hoofdstuk 4 – Variabelen

Tot nu toe heb je berekeningen gemaakt en resultaten afgedrukt, maar de tussenliggende waardes verdwenen meteen weer. Met **variabelen** kun je waardes bewaren en later hergebruiken. Ze zijn onmisbaar voor elk zinvol programma.

---

## 4.1 Variabelen en waardes

Een **variabele** is een plek in het geheugen van de computer met een naam, waarin je een waarde kunt opslaan. Je maakt een variabele door er een waarde aan toe te kennen via het `=` teken:

```python
x = 5
print(x)
```

Hier gebeuren twee dingen:

1. Er wordt een variabele `x` aangemaakt en de waarde `5` wordt erin opgeslagen.
2. `print(x)` toont **de waarde** die in `x` zit — niet de letter `x`.

!!! tip "Denk aan een doos"
    Stel je een variabele voor als een doos met een naam op de zijkant. Je kunt iets in de doos stoppen, en je kunt in de doos kijken. Er past maar één ding tegelijk in.

    - **Variabele** = de naam op de doos (`x`)
    - **Waarde** = de inhoud van de doos (`5`)

Aan de rechterkant van `=` mag je alles zetten dat een waarde oplevert: een getal, een berekening, een string, of een functie-aanroep:

```python
x = 5
print(x)

x = 7 * 9 + 13        # overschrijft de vorige waarde
print(x)

x = "En nu iets heel anders..."
print(x)

x = int(15 / 4) - 27
print(x)
```

!!! note "Overschrijven"
    Elke keer dat je een nieuwe waarde toekent aan een bestaande variabele, wordt de oude waarde **overschreven**. Een variabele bevat altijd de **laatste** toegekende waarde.

### Variabelen gebruiken in berekeningen

Zodra een variabele bestaat, kun je hem overal gebruiken waar je een waarde zou schrijven:

```python
x = 2
y = 3
print("x =", x)
print("y =", y)
print("x * y =", x * y)
print("x + y =", x + y)
```

### Waardes kopiëren en verwisselen

Je kunt de inhoud van een variabele kopiëren naar een andere variabele:

```python
x = 2
y = 3
print("x =", x, "en y =", y)

# Verwissel de waardes via een tijdelijke hulpvariabele
z = x
x = y
y = z
print("x =", x, "en y =", y)
```

### Een variabele gebruiken in zijn eigen berekening

Dit is toegestaan — de rechterkant wordt altijd **volledig berekend** voordat de toekenning plaatsvindt:

```python
x = 2
print(x)
x = x + 3   # eerst x + 3 berekenen (= 5), dan opslaan in x
print(x)
```

!!! danger "Variabele moet bestaan vóór gebruik"
    Je kunt een variabele niet gebruiken voordat je er een waarde aan hebt toegekend:

    ```python
    print(dagen_per_jaar)   # ❌ fout! variabele bestaat nog niet
    dagen_per_jaar = 365
    ```

    Dit geeft een `NameError`. Zorg altijd dat de toekenning **vóór** het gebruik staat.

---

## 4.2 Variabele namen

Je mag variabelenamen zelf kiezen, zolang je je aan deze regels houdt:

- De naam mag alleen bestaan uit **letters, cijfers en underscores** (`_`)
- De naam moet **beginnen met een letter of underscore**
- De naam mag **geen gereserveerd woord** zijn

### Gereserveerde woorden (keywords)

Deze woorden heeft Python zelf al in gebruik en zijn niet beschikbaar als variabelenaam:

```
False    None     True     and      as       assert   break
case     class    continue def      del      elif     else
except   finally  for      from     global   if       import
in       is       lambda   match    nonlocal not      or
pass     raise    return   try      while    with     yield
```

!!! warning "Case sensitive"
    Python maakt onderscheid tussen hoofd- en kleine letters. `wereld` en `Wereld` zijn **twee verschillende variabelen**. Het keyword `class` is gereserveerd, maar `Class` is dat niet (al is het beter om hoofdletters in variabelenamen te vermijden).

---

### 4.2.1 Conventies

Goede programmeurs houden zich aan vaste afspraken:

!!! success "Goede gewoontes"
    - Kies **betekenisvolle namen** die beschrijven wat de variabele bevat
    - Gebruik **alleen kleine letters** om verwarring te vermijden
    - Gebruik **underscores** tussen woorden: `secs_per_week`
    - Gebruik voor **wegwerpvariabelen** (tijdelijk gebruik) korte namen zoals `i` of `j`
    - Gebruik **nooit** de naam van een bestaande functie als variabelenaam (`print`, `int`, `len`…)
    - Begin **nooit** met een underscore — dat is voorbehouden aan Python zelf

Vergelijk deze twee stukken code:

=== "Slechte namen"
    ```python
    a = 3.14159265
    b = 7.5
    c = 8.25
    d = a * b * b * c / 3
    print(d)
    ```

=== "Goede namen"
    ```python
    pi = 3.14159265
    straal = 7.5
    hoogte = 8.25
    volume_van_kegel = pi * straal * straal * hoogte / 3
    print(volume_van_kegel)
    ```

Beide berekenen hetzelfde — het volume van een kegel — maar alleen de tweede versie is begrijpelijk zonder verdere uitleg. Zulke code heet **zelf-documenterende code**.

!!! tip "Zelf-documenterende code"
    Als je variabelenamen goed kiest, heb je nauwelijks commentaar nodig om uit te leggen wat de code doet. Dat maakt je programma leesbaarder én makkelijker te onderhouden.

---

### 4.2.2 Oefening: variabele namen herkennen

Bekijk de volgende toekenningen en bedenk welke **incorrect** zijn en waarom:

```python
classificatie = 1     # 1
Classificatie = 1     # 2
cl@ssificatie = 1     # 3
class1f1cat1e = 1     # 4
1classificatie = 1    # 5
_classificatie = 1    # 6
class = 1             # 7
Class = 1             # 8
```

??? note "Antwoord (klik om te openen)"
    - **#3** — bevat een `@`, wat niet is toegestaan
    - **#5** — begint met een cijfer
    - **#7** — `class` is een gereserveerd woord

    De andere zijn technisch correct, maar:
    - **#2 en #8** — bevatten hoofdletters, wat beter vermeden wordt
    - **#6** — begint met een underscore, wat voorbehouden is aan Python-interne namen
    - **#8** — lijkt op een gereserveerd woord, wat verwarrend is

---

### 4.2.3 Constanten

In veel talen kun je een **constante** aanmaken: een variabele waarvan de waarde nooit mag veranderen. Python ondersteunt dit niet echt, maar de **conventie** is om variabelenamen die volledig in HOOFDLETTERS zijn geschreven te behandelen als constanten:

```python
BTW_FACTOR = 1.21
CENTEN = 100

totaal = 24.95
eind_totaal = int(CENTEN * totaal * BTW_FACTOR) / CENTEN
print(eind_totaal)
```

!!! info "Magische getallen"
    Getallen waarvan de betekenis niet direct duidelijk is — zoals `1.21` voor BTW — heten **magische getallen**. Het is beter om ze een naam te geven via een constante, zodat de code leesbaar blijft en je de waarde op één plek kunt aanpassen.

---

## 4.3 Debuggen met variabelen

Programmafouten waarbij variabelen **onverwachte waardes** bevatten, zijn lastig te vinden. Een bewezen techniek: voeg tijdelijk `print()` statements toe om te zien wat er in je variabelen zit.

Bekijk deze foutieve code:

```python
nr1 = 5
nr2 = 4
nr3 = 5
print(nr3 / (nr1 % nr2))
nr1 = nr1 + 1
print(nr3 / (nr1 % nr2))
nr1 = nr1 + 1
print(nr3 / (nr1 % nr2))
nr1 = nr1 + 1
print(nr3 / (nr1 % nr2))
```

Ergens gaat dit mis. Om te achterhalen waar, voeg je een debug-regel toe:

```python
nr1 = nr1 + 1
print("DEBUG - nr1:", nr1, "nr2:", nr2, "nr1%nr2:", nr1 % nr2)  # tijdelijk!
print(nr3 / (nr1 % nr2))
```

!!! tip "Debuggen met print()"
    `print()` statements veranderen niks aan je variabelen — je kunt ze veilig toevoegen en later weer verwijderen als het probleem is opgelost.

---

## 4.4 Soft typing

In Python hoef je het type van een variabele niet op voorhand te declareren. Python bepaalt het type automatisch op basis van de waarde die erin zit. Dit heet **soft typing** (ook wel *dynamisch typen*).

```python
a = 3
print(type(a))   # <class 'int'>

a = 3.0
print(type(a))   # <class 'float'>

a = "3.0"
print(type(a))   # <class 'str'>
```

Je kunt het type van een waarde of variabele opvragen met `type()`.

!!! note "Type verandert mee"
    Omdat Python soft typing gebruikt, kan het type van een variabele veranderen als je er een nieuwe waarde van een ander type aan toekent. Dit is handig, maar vraagt ook om aandacht:

    ```python
    a = 1
    b = 4
    c = "1"
    d = "4"
    print(a + b)   # 5   (numerieke optelling)
    print(c + d)   # 14  (string samenvoegen!)
    ```

### Type hints (Python 3.5+)

Vanaf Python 3.5 kun je **type hints** toevoegen als annotatie:

```python
a: int = 1
b: str = "hallo"
print(type(a), type(b))
```

!!! warning "Type hints zijn niet afdwingbaar"
    Python **negeert** type hints volledig tijdens uitvoering. Ze zijn puur documentatie:

    ```python
    a: int = "dit is een string"  # geen fout!
    print(type(a))                 # <class 'str'>
    ```

    In dit boek worden type hints niet gebruikt.

---

## 4.5 Verkorte operatoren

Het komt vaak voor dat je een variabele wilt aanpassen op basis van zijn huidige waarde. Python biedt daarvoor **verkorte notaties**:

| Lange notatie | Verkorte notatie | Betekenis |
|--------------|-----------------|-----------|
| `x = x + 5` | `x += 5` | Tel 5 op bij x |
| `x = x - 3` | `x -= 3` | Trek 3 af van x |
| `x = x * 2` | `x *= 2` | Vermenigvuldig x met 2 |
| `x = x / 4` | `x /= 4` | Deel x door 4 |
| `x = x ** 2` | `x **= 2` | Verhef x tot de macht 2 |

```python
aantal_bananen = 100
aantal_bananen += 1
print(aantal_bananen)   # 101
```

!!! tip "Gebruik van verkorte operatoren"
    `+=` wordt **heel vaak** gebruikt (je zult begrijpen waarom in hoofdstuk 7 over herhalingen). `-=` kom je regelmatig tegen. De rest is zeldzamer.

---

## 4.6 Commentaar

Commentaar zijn teksten in je code die Python **negeert** tijdens uitvoering. Ze zijn bedoeld voor menselijke lezers — inclusief jezelf, weken of maanden later.

### Eenregelig commentaar: `#`

Alles rechts van een `#` op een regel is commentaar:

```python
# Dit is een volledige commentaarregel
print(2 + 3)   # Dit is commentaar achter een statement
```

### Meerregelig commentaar: `"""` of `'''`

Voor langere toelichtingen gebruik je drievoudige aanhalingstekens:

```python
"""
Dit is een commentaarblok
dat meerdere regels beslaat.
"""
print("Klaar.")
```

!!! note "Wanneer commentaar toevoegen?"
    Voeg commentaar toe als de code niet voor zichzelf spreekt. Met goede variabelenamen heb je vaak weinig commentaar nodig — maar een korte uitleg van **waarom** iets gedaan wordt, is altijd welkom.

Voorbeeld van goed commentaar:

```python
pi = 3.14159265
straal = 7.5
hoogte = 8.25
# Berekening van het volume van een kegel: V = π * r² * h / 3
volume_van_kegel = pi * straal * straal * hoogte / 3
print(volume_van_kegel)
```

---

## Wat je geleerd hebt

In dit hoofdstuk heb je het volgende gezien:

- Wat variabelen zijn en hoe je ze aanmaakt
- Een waarde toekennen aan een variabele (de assignment operator `=`)
- Correcte en betekenisvolle variabelenamen kiezen
- Conventies rondom naamgeving
- Code debuggen door variabelewaardes af te drukken
- Soft typing en type hints
- Verkorte operatoren (`+=`, `-=`, enz.)
- Commentaar toevoegen met `#` en `"""`

---

## Opgaven

### Opgave 4.1 — Gemiddelde

!!! example "Opgave 4.1"
    Definieer drie variabelen `var1`, `var2` en `var3` met waarden naar keuze. Bereken het gemiddelde en sla het op in een variabele `gemiddelde`. Druk het gemiddelde af.

    Voeg ook **drie commentaarregels** toe die uitleggen wat de code doet.

### Opgave 4.2 — Oppervlakte van een cirkel

!!! example "Opgave 4.2"
    Schrijf een programma dat de **oppervlakte van een cirkel** berekent. Gebruik variabelen `straal` en `pi = 3.14159`.

    De formule is: `straal * straal * pi`

    De uitvoer moet er zo uitzien:
    ```
    De oppervlakte van een cirkel met straal 7.5 is 176.71...
    ```

    Gebruik string samenvoegen of `str()` om de variabelewaardes in de tekst te verwerken.

### Opgave 4.3 — Wisselgeld in munten

!!! example "Opgave 4.3"
    Schrijf een programma dat een bedrag in **centen** (opgeslagen in een variabele `bedrag`) omzet naar het **minimale aantal muntstukken**:

    | Munt | Waarde |
    |------|--------|
    | Dollar | 100 ct |
    | Kwartje | 25 ct |
    | Dubbeltje | 10 ct |
    | Stuiver | 5 ct |
    | Cent | 1 ct |

    Bepaal achtereenvolgens hoeveel dollars, kwartjes, dubbeltjes, stuivers en centen nodig zijn.

    **Hint:** Gebruik integer deling (`//`) en modulo (`%`).

    Voorbeeld: voor `bedrag = 188` zou de uitvoer zijn:
    ```
    Dollars: 1
    Kwartjes: 3
    Dubbeltjes: 1
    Stuivers: 0
    Centen: 3
    ```

### Opgave 4.4 — Verwisselen zonder hulpvariabele

!!! example "Opgave 4.4"
    Normaal verwissel je twee variabelen via een derde hulpvariabele. Maar dat kan ook **zonder** hulpvariabele — alleen met rekenkundige operatoren.

    Startcode:

    ```python
    a = 17
    b = 23
    print("a =", a, "en b =", b)
    a += b
    # Voeg hier twee regels toe om a en b te verwisselen
    print("a =", a, "en b =", b)
    ```

    De eerste stap (`a += b`) is al gegeven. Voeg **precies twee regels** toe om de verwisseling te voltooien.

    **Denktip:** Na `a += b` bevat `a` de som van beide. Hoe haal je dan de oorspronkelijke waarden terug?

!!! tip "Hint"
    Denk na over wat `a - b` oplevert nadat `a` de som is van de twee originele waarden.

---

*Volgende: [Hoofdstuk 5 – Eenvoudige Functies](h05-functies.md)*
