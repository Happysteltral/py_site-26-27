---
title: Hoofdstuk 9 – Recursie
description: Wat is recursie, wanneer gebruik je het, en hoe implementeer je recursieve functies?
---

# Hoofdstuk 9 – Recursie

Recursie is een speciale techniek die je kunt gebruiken nu je functies beheerst. Het kan bepaalde problemen op een elegante en krachtige manier oplossen — maar studenten vinden het vaak een lastig onderwerp.

!!! tip "Mag je dit hoofdstuk overslaan?"
    Als je na bestudering van dit hoofdstuk het gevoel hebt dat het te moeilijk is, voel je dan vrij om het voorlopig over te slaan. De volgende hoofdstukken zijn een stuk toegankelijker. Je kunt hier later altijd op terugkomen.

---

## 9.1 Wat is recursie?

**Recursie** is een techniek waarbij een functie **zichzelf aanroept**. Iets algemener gesteld: een situatie waarbij een functie andere functies aanroept op zo'n manier dat de uitvoering van de eerste functie nog bezig is wanneer die functie zelf opnieuw wordt aangeroepen.

"Maar als een functie zichzelf aanroept, dan roept hij zichzelf nogmaals aan, en nogmaals... Wordt dat niet een eindeloos proces?" — Ja, dat gevaar bestaat. Maar goed ontworpen recursieve functies hebben een **stopconditie** die eindeloze aanroepen voorkomt.

Recursie is niet voor elk probleem de beste aanpak, maar voor sommige problemen is het de meest elegante oplossing. Het is belangrijk dat je weet wat de mogelijkheden én de beperkingen zijn.

---

## 9.2 Recursieve definities

### 9.2.1 De faculteit — een klassiek voorbeeld

De faculteit is een perfect voorbeeld van een recursieve definitie. Je kent de iteratieve omschrijving al:

> De faculteit van een positief geheel getal is dat getal, vermenigvuldigd met alle positieve gehele getallen die kleiner zijn (exclusief nul).

Wiskundigen definiëren de faculteit liever **recursief**:

- `1! = 1` (basisgeval)
- `n! = n × (n-1)!` voor `n > 1` (recursieve stap)

Deze definitie verwijst naar zichzelf — maar leidt niet tot eindeloze recursie, omdat `n` uiteindelijk `1` bereikt.

In Python:

```python
def faculteit(n):
    if n <= 1:
        return 1
    return n * faculteit(n - 1)

print(faculteit(5))   # 120
```

!!! info "Stap voor stap: `faculteit(5)`"
    Hier zie je precies wat er gebeurt bij de aanroep `faculteit(5)`:

    ```
    aanroep faculteit(5)
        aanroep faculteit(4)
            aanroep faculteit(3)
                aanroep faculteit(2)
                    aanroep faculteit(1)
                    return 1
                return 2 × 1 = 2
            return 3 × 2 = 6
        return 4 × 6 = 24
    return 5 × 24 = 120
    ```

    Elke aanroep wacht tot de dieper liggende aanroep klaar is, en gebruikt dan de geretourneerde waarde in zijn eigen berekening.

---

### 9.2.2 Wanneer gebruik je recursie?

De recursieve faculteit ziet er elegant uit, maar de **iteratieve versie is te verkiezen**. Waarom?

Bij `faculteit(5)` staan er vóór de eerste `return` al **4 aanroepen** tegelijkertijd in het geheugen. Bij `faculteit(100)` zijn dat er 100. Python kan bij te diepe recursie zelfs een `RecursionError` geven.

De iteratieve versie houdt slechts twee variabelen in het geheugen — dat is veel efficiënter.

!!! success "Gebruik recursie alleen als:"
    1. Recursie de **meest natuurlijke** manier is om het probleem te implementeren
    2. Het recursieve proces **gegarandeerd niet te diep** gaat

    Elke recursieve functie kan ook iteratief geschreven worden. Kies voor recursie als de elegantie en leesbaarheid duidelijk beter zijn.

---

### 9.2.3 Een doolhof doorzoeken

Een echt krachtige toepassing van recursie is het doorzoeken van een doolhof. De module `pcmaze` (te downloaden van de cursussite) implementeert een doolhof met genummerde cellen.

Beschikbare functies in `pcmaze`:

| Functie | Beschrijving |
|---------|-------------|
| `entrance()` | Retourneert het nummer van de ingangcel (laagst genummerd) |
| `exit()` | Retourneert het nummer van de uitgangscel (hoogst genummerd) |
| `connected(a, b)` | `True` als er een directe verbinding is tussen cel `a` en `b` |

Het doel: schrijf code die een pad vindt van ingang naar uitgang.

**De recursieve gedachte:**

> Een cel ligt op het pad naar de uitgang als:
> - De cel de uitgang zelf is, **of**
> - De cel verbonden is met een andere cel die op het pad naar de uitgang ligt

Dit is een recursieve definitie. In Python:

```python
from pcmaze import entrance, exit, connected

def leidt_naar_uitgang(komtvan, cel):
    if cel == exit():
        return True
    for i in range(entrance(), exit() + 1):
        if i == komtvan:
            continue
        if not connected(cel, i):
            continue
        if leidt_naar_uitgang(cel, i):
            print(cel, "->", i)
            return True
    return False

if leidt_naar_uitgang(0, entrance()):
    print("Pad gevonden!")
else:
    print("Pad niet gevonden")
```

!!! note "Hoe werkt dit?"
    - De functie krijgt twee parameters: de cel waar we vandaan komen (`komtvan`) en de cel die we nu controleren (`cel`)
    - `komtvan` voorkomt dat we terugkeren op ons spoor
    - Als `cel` de uitgang is: `True`
    - Anders: probeer alle verbonden cellen (behalve waar we vandaan komen) recursief
    - Als een pad gevonden wordt: druk het af en return `True`
    - Als niets werkt: return `False`

!!! warning "Het pad wordt omgekeerd afgedrukt"
    Omdat we het pad afdrukken *terwijl we terugkeren* uit de recursie, verschijnt het in omgekeerde volgorde. De verbeterde versie hieronder lost dat op.

---

### 9.2.4 Retourwaarden in recursieve functies

Een nettere aanpak: het pad niet afdrukken in de functie, maar **retourneren** als string. Dan kan de aanroeper het in de goede volgorde ontvangen:

```python
from pcmaze import entrance, exit, connected

def leidt_naar_uitgang(komtvan, cel):
    if cel == exit():
        return "{}".format(exit())
    for i in range(entrance(), exit() + 1):
        if i == komtvan:
            continue
        if not connected(cel, i):
            continue
        check = leidt_naar_uitgang(cel, i)
        if check != "":
            return "{} -> {}".format(cel, check)
    return ""

check = leidt_naar_uitgang(0, entrance())
if check != "":
    print("Pad gevonden!", check)
else:
    print("Pad niet gevonden")
```

!!! info "Hoe werkt dit anders?"
    - Als de uitgang gevonden wordt, retourneert de functie het uitgangsnummer als string
    - Elke hogere aanroep voegt zijn celnummer toe **voor** het ontvangen pad: `"cel -> rest_van_pad"`
    - Zo bouwt het pad zich op van begin naar einde — in de juiste volgorde!

    Dit is een typisch patroon voor recursieve functies waarbij informatie van een dieper niveau naar boven gecommuniceerd moet worden. Je hebt **geen globale variabele** nodig — de retourwaarde volstaat.

---

## 9.3 Structuur van een recursieve functie

Elke goed geschreven recursieve functie heeft twee essentiële onderdelen:

```python
def recursieve_functie(parameter):
    # 1. BASISGEVAL: stop de recursie
    if <stopconditie>:
        return <basiswaarde>

    # 2. RECURSIEVE STAP: roep zichzelf aan met een "kleiner" probleem
    return <bewerking> + recursieve_functie(<kleiner_probleem>)
```

!!! danger "Vergeet het basisgeval niet!"
    Zonder basisgeval eindigt de recursie nooit en krijg je een `RecursionError`:

    ```python
    def eindeloos(n):
        return n * eindeloos(n - 1)   # ❌ geen basisgeval!

    print(eindeloos(5))   # RecursionError: maximum recursion depth exceeded
    ```

De truc: zorg dat de parameter bij elke aanroep **dichter bij het basisgeval** komt.

---

## 9.4 Recursie versus iteratie

| | Recursie | Iteratie |
|--|----------|----------|
| **Leesbaarheid** | Vaak eleganter voor problemen met recursieve structuur | Soms langer maar begrijpelijker |
| **Geheugengebruik** | Elke aanroep gebruikt stack-geheugen | Slechts een paar variabelen |
| **Snelheid** | Trager door overhead van aanroepen | Sneller |
| **Risico** | `RecursionError` bij te diepe recursie | Eindeloze loop als je niet oplet |
| **Wanneer gebruiken** | Probleemstructuur is van nature recursief | In de meeste andere gevallen |

---

## Wat je geleerd hebt

In dit hoofdstuk heb je het volgende gezien:

- Wat recursie is: een functie die zichzelf aanroept
- Basisgeval en recursieve stap — de twee vereiste onderdelen
- De recursieve faculteit als klassiek voorbeeld
- Wanneer je recursie wel en niet gebruikt
- Het doolhofprobleem als krachtige toepassing
- Retourwaarden in recursieve functies — geen globale variabelen nodig

---

## Opgaven

### Opgave 9.1 — Fibonacci recursief

!!! example "Opgave 9.1"
    De Fibonacci reeks is recursief gedefinieerd:

    - `fib(1) = 1`
    - `fib(2) = 1`
    - `fib(n) = fib(n-1) + fib(n-2)` voor `n > 2`

    Schrijf een recursieve functie `fib(n)` die het n-de Fibonacci getal retourneert. Druk de eerste 10 Fibonacci getallen af.

### Opgave 9.2 — Recursie visualiseren

!!! example "Opgave 9.2"
    Pas de Fibonacci functie uit opgave 9.1 aan door een `diepte` parameter toe te voegen (start op `0`, verhoog met `1` bij elke recursieve aanroep).

    Bij binnenkomst van de functie druk je het argument `n` af, ingesprongen op basis van de diepte. Bij het retourneren druk je de retourwaarde ook af.

    Bestudeer de output: hoeveel aanroepen zijn er nodig voor `fib(6)`?

### Opgave 9.3 — Is recursie goed voor Fibonacci?

!!! example "Opgave 9.3"
    Denk na over de vraag: is het een goed idee om de Fibonacci reeks recursief te implementeren? Waarom wel of niet?

    Denk na over hoeveel keer `fib(3)` berekend wordt bij een aanroep van `fib(6)`.

??? note "Antwoord (klik om te openen)"
    Nee, het is geen goed idee. Dezelfde Fibonacci getallen worden **herhaaldelijk opnieuw berekend**. Bij `fib(6)` wordt `fib(3)` meerdere keren opnieuw berekend. Bij `fib(30)` loopt dit exponentieel op. De iteratieve versie is veel efficiënter.

### Opgave 9.4 — Grootste gemene deler (Euclidisch algoritme)

!!! example "Opgave 9.4"
    Het algoritme van Euclides berekent de grootste gemene deler (ggd) van twee getallen:

    - Als `a % b == 0`: de ggd is `b`
    - Anders: de ggd is `ggd(b, a % b)`

    Implementeer dit recursief in een functie `ggd(a, b)`.

    Test:
    - `ggd(14, 21)` → `7`
    - `ggd(48, 18)` → `6`
    - `ggd(100, 75)` → `25`

!!! tip "Hint"
    Deze functie is verrassend kort — slechts een paar regels!

### Opgave 9.5 — Slechte recursie herkennen

!!! example "Opgave 9.5"
    Bekijk de volgende code. Er is iets fundamenteel mis mee. Wat is het probleem?

    ```python
    def vraag_input(prompt):
        waarde = input(prompt)
        for letter in waarde:
            if letter < 'a' or letter > 'z':
                print(letter, "is niet toegestaan!")
                waarde = vraag_input(prompt)  # recursieve aanroep!
        return waarde

    s = vraag_input("Geef een string van kleine letters: ")
    print("Je gaf in:", s)
    ```

    **Hint:** Het gaat niet om de vergelijking `letter < 'a' or letter > 'z'` — die is correct.

??? note "Antwoord (klik om te openen)"
    Het probleem is dat de **diepte van de recursie afhankelijk is van de gebruiker**. Als een gebruiker heel vaak foute tekens ingeeft, groeit de recursie onbeperkt en krijg je uiteindelijk een `RecursionError`. Dit soort problemen moet altijd opgelost worden met een iteratieve aanpak (zoals een `while True` loop), nooit met recursie.

### Opgave 9.6 — Torens van Hanoi

!!! example "Opgave 9.6"
    De klassieke **Torens van Hanoi** puzzel: je hebt drie palen (A, B, C) en N schijven op paal A, gesorteerd van groot (onderaan) naar klein (bovenaan). Verplaats alle schijven naar paal C, met deze regels:

    1. Verplaats slechts één schijf per keer
    2. Een schijf mag alleen op een grotere schijf geplaatst worden
    3. Gebruik paal B als tussentijdse paal

    Schrijf een recursieve functie `hanoi(n, van, naar, via)` die het recept afdrukt.

    Voorbeeld voor `n=3`:
    ```
    Schijf 1 van A naar C
    Schijf 2 van A naar B
    Schijf 1 van C naar B
    Schijf 3 van A naar C
    Schijf 1 van B naar A
    Schijf 2 van B naar C
    Schijf 1 van A naar C
    ```

    Druk ook het totaal aantal stappen af.

!!! tip "Recursieve redenering"
    Om N schijven van A naar C te verplaatsen:

    1. Verplaats de bovenste `N-1` schijven van A naar **B** (met C als hulppaal)
    2. Verplaats de grootste schijf van A naar **C**
    3. Verplaats de `N-1` schijven van B naar **C** (met A als hulppaal)

    En hoe verplaats je `N-1` schijven? Met dezelfde aanpak voor `N-2`... totdat je bij `N=1` bent: één schijf direct verplaatsen.

---

*Volgende: [Hoofdstuk 10 – Strings](h10-strings.md)*
