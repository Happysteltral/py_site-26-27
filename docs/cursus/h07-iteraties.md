---
title: Hoofdstuk 7 – Iteraties
description: While loops, for loops en loop controle met break, continue en else.
---

# Hoofdstuk 7 – Iteraties

Computers raken niet verveeld. Als een computer een taak honderdduizenden malen moet herhalen, protesteert hij niet. Mensen daarentegen houden niet van teveel herhaling — daarom moeten herhalende taken aan een computer worden overgelaten. De programmeerconstructies die dit mogelijk maken heten **iteraties**, of in gewoon programmeursjargon: **loops**.

!!! warning "Dit is een sleutelhoofdstuk!"
    Als loops helemaal nieuw voor je zijn, neem dan extra de tijd. Elk hoofdstuk dat hierna komt maakt gebruik van loops. Je moet ze echt goed begrijpen voordat je verdergaat.

---

## 7.1 De `while` loop

Stel dat je de gebruiker moet vragen om vijf getallen en ze moet optellen. Zonder loop zou je dat zo coderen:

```python
from pcinput import getInteger

num1 = getInteger("Nummer 1: ")
num2 = getInteger("Nummer 2: ")
num3 = getInteger("Nummer 3: ")
num4 = getInteger("Nummer 4: ")
num5 = getInteger("Nummer 5: ")
print("Totaal is", num1 + num2 + num3 + num4 + num5)
```

Maar wat als je om **500 getallen** moet vragen? Dan heb je een loop nodig.

De syntax van een `while` loop:

```python
while <boolean expressie>:
    <acties>
```

Een `while` loop lijkt sterk op een `if` statement — maar met één cruciaal verschil: nadat het blok code uitgevoerd is, gaat Python **terug naar de boolean expressie** en test hem opnieuw. Dit herhaalt zich totdat de expressie `False` oplevert.

---

### 7.1.1 Eerste voorbeeld: tellen van 1 tot 5

```python
num = 1
while num <= 5:
    print(num)
    num += 1
print("Klaar")
```

Laten we dit stap voor stap doorlopen:

| Cyclus | Waarde van `num` | `num <= 5`? | Actie |
|--------|-----------------|-------------|-------|
| Start  | 1 | True | Print 1, num wordt 2 |
| 2 | 2 | True | Print 2, num wordt 3 |
| 3 | 3 | True | Print 3, num wordt 4 |
| 4 | 4 | True | Print 4, num wordt 5 |
| 5 | 5 | True | Print 5, num wordt 6 |
| 6 | 6 | **False** | Loop eindigt, print "Klaar" |

!!! tip "Stroomdiagram van een while loop"
    ```
         ┌─────────┐
         │  Start  │
         └────┬────┘
              ▼
        ◇ num <= 5? ◇
       True ↓       ↓ False
     ┌──────────┐   └──→ print("Klaar")
     │ print(num│             ↓
     │ num += 1 │          ┌──────┐
     └────┬─────┘          │ Stop │
          └────────────────└──────┘
    ```

---

### 7.1.2 Tweede voorbeeld: vijf getallen optellen

```python
from pcinput import getInteger

totaal = 0
teller = 0
while teller < 5:
    totaal += getInteger("Geef een nummer: ")
    teller += 1
print("Totaal is", totaal)
```

Er zijn hier twee variabelen:

- `totaal` — verzamelt de som van de ingegeven getallen, start op `0`
- `teller` — telt hoe vaak de loop al gedraaid heeft, start op `0`

!!! info "Waarom starten bij 0 en tellen tot maar niet inclusief 5?"
    Programmeurs starten indices vrijwel altijd bij `0` en tellen "tot maar niet inclusief" de bovengrens. Zo werken ook de meeste Python-functies (zoals `range()`). Went jezelf hieraan — het maakt code consistenter en leesbaarder.

!!! note "Wegwerpvariabele"
    `teller` is een **wegwerpvariabele** — hij dient alleen om bij te houden hoe vaak de loop doorlopen is. Voor zulke variabelen gebruiken programmeurs traditioneel de namen `i` of `j`. In dit voorbeeld is `teller` gekozen voor duidelijkheid, maar `i` is even correct.

---

### 7.1.3 De while loop onder controle van de gebruiker

Wat als je de gebruiker niet wilt beperken tot exact vijf getallen? Dan bepaalt de gebruiker zelf wanneer hij stopt — door bijvoorbeeld een `0` in te geven:

=== "Versie 1 (werkt, maar niet mooi)"
    ```python
    from pcinput import getInteger

    num = -1      # willekeurige beginwaarde om de loop te starten
    totaal = 0
    while num != 0:
        num = getInteger("Geef een nummer: ")
        totaal += num
    print("Totaal is", totaal)
    ```

=== "Versie 2 (schoner)"
    ```python
    from pcinput import getInteger

    num = getInteger("Geef een nummer: ")
    totaal = 0
    while num != 0:
        totaal += num
        num = getInteger("Geef een nummer: ")
    print("Totaal is", totaal)
    ```

!!! note "Welke versie is beter?"
    Versie 1 heeft twee problemen: de beginwaarde `-1` is betekenisloos, en als de stopwaarde niet `0` maar bijv. een negatief getal was, zou dat getal ook bij `totaal` opgeteld worden. Versie 2 lost dit op, maar herhaalt de `getInteger()` aanroep. Hoe dat netter kan volgt later in dit hoofdstuk (met `break`).

---

### 7.1.4 Eindeloze loops

Bekijk de volgende code (voer hem **niet** uit!):

```python
nummer = 1
totaal = 0
while (nummer * nummer) % 1000 != 0:
    totaal += nummer
    print("Totaal is", totaal)
```

!!! danger "Eindeloze loop!"
    `nummer` wordt **nooit gewijzigd** in het blok code. De boolean expressie blijft dus altijd `True`. Dit programma stopt nooit — dit is een **eindeloze loop**, het grootste gevaar bij `while` loops.

    Als je dit per ongeluk uitvoert in IDLE: druk op **Ctrl+C** om de uitvoering te onderbreken.

!!! tip "Preventie: schrijf de teller-update meteen"
    Zodra je een `while` loop begint, schrijf dan **onmiddellijk** de regel die de loopvariabele aanpast — voordat je de rest invult. Zo vergeet je hem nooit:

    ```python
    while i < 10:
        i += 1      # schrijf dit eerst!
        # voeg hier de rest van de code toe
    ```

---

### 7.1.5 While loop oefeningen

!!! example "Oefening: aftellen"
    Schrijf code die aftelt vanaf een specifiek getal (bijv. 10). Ieder getal wordt afgedrukt (10, 9, 8, ...). In plaats van 0 drukt het programma "Start!" af.

!!! example "Oefening: faculteit"
    De **faculteit** van een positief geheel getal is dat getal vermenigvuldigd met alle positieve gehele getallen kleiner dan het (maar groter dan 0).

    Voorbeelden:
    - `5! = 5 × 4 × 3 × 2 × 1 = 120`
    - `3! = 3 × 2 × 1 = 6`

    Schrijf code die de faculteit van een getal berekent met een `while` loop.

    **Hint:** Gebruik twee variabelen: één voor het lopende resultaat (start op 1) en één voor de huidige factor (start op het ingegeven getal). Vermenigvuldig het resultaat met de factor, en trek dan 1 af van de factor.

---

## 7.2 De `for` loop

De `for` loop is een alternatief dat **gemakkelijker en veiliger** te gebruiken is dan de `while` loop — maar niet voor alle situaties geschikt. De `while` loop is altijd bruikbaar; de `for` loop alleen als je een collectie hebt om over te lopen.

```python
for <variabele> in <collectie>:
    <acties>
```

De `for` loop neemt de items uit de collectie **één voor één**, stopt ze in de variabele, en voert het blok code uit. Hij herhaalt dit voor elk item, en stopt vanzelf als alle items verwerkt zijn.

!!! success "For loops kunnen niet eindeloos zijn!"
    Omdat de collectie vastliggende afmetingen heeft, eindigt een `for` loop altijd. Dit maakt ze veiliger dan `while` loops.

---

### 7.2.1 For loop over een string

Een string is een collectie van tekens. Je kunt er letter voor letter over lopen:

```python
for letter in "banaan":
    print(letter)
print("Klaar")
```

Uitvoer:
```
b
a
n
a
a
n
Klaar
```

!!! note "De variabele hoeft niet vooraf te bestaan"
    De variabele (`letter` in dit geval) wordt automatisch aangemaakt door de `for` loop. Na afloop van de loop bestaat hij nog — met de waarde van het **laatste** verwerkte item.

!!! info "Wijzigen van de collectie in de loop"
    Wat als je de variabele die de string bevat aanpast **binnen** de loop?

    ```python
    fruit = "banaan"
    for letter in fruit:
        print(letter)
        if letter == "n":
            fruit = "mango"
    print("Klaar")
    ```

    De collectie wordt slechts **eenmalig bepaald** bij aanvang van de loop. Het aanpassen van `fruit` daarna heeft geen effect — de loop blijft "banaan" verwerken.

---

### 7.2.2 For loop met `range()`

`range()` genereert een reeks opeenvolgende gehele getallen. Hiermee kun je een loop een **exact aantal keren** laten draaien.

| Aanroep | Genereert |
|---------|----------|
| `range(5)` | 0, 1, 2, 3, 4 |
| `range(1, 6)` | 1, 2, 3, 4, 5 |
| `range(1, 11, 2)` | 1, 3, 5, 7, 9 |
| `range(10, 0, -1)` | 10, 9, 8, ..., 1 |

```python
for x in range(10):
    print(x)
```

```python
for x in range(1, 11, 2):
    print(x)    # 1, 3, 5, 7, 9
```

!!! tip "Aftellen met range()"
    Voor aftellen geef je een **negatieve stapgrootte**. Zorg dat het startgetal groter is dan het eindgetal:

    ```python
    for x in range(10, 0, -1):
        print(x)    # 10, 9, 8, ... 1
    ```

---

### 7.2.3 For loop met een handmatige collectie (tuple)

Als je een vaste reeks waarden hebt, zet je ze tussen ronde haakjes — dit heet een **tuple** (meer hierover in hoofdstuk 11):

```python
for x in (10, 100, 1000, 10000):
    print(x)

for fruit in ("appel", "peer", "druif", "banaan", "mango", "kers"):
    print(fruit)
```

Een tuple mag zelfs **gemengde data types** bevatten:

```python
for item in (42, "hallo", 3.14, True):
    print(item)
```

---

### 7.2.4 For loop oefeningen

!!! example "Oefening: veelvouden van 3"
    Gebruik een `for` loop en `range()` om de **veelvouden van 3** af te drukken, beginnend bij 21, aftellend tot en met 3 — in slechts **twee regels code**.

!!! example "Oefening: vijf getallen optellen (opnieuw)"
    Je hebt eerder een `while` loop geschreven die vijf getallen opvraagt en optelt. Schrijf dit nu met een `for` loop.

!!! example "Oefening: aftellen (opnieuw)"
    Je hebt eerder een `while` loop geschreven die aftelt en dan "Start!" drukt. Schrijf dit nu met een `for` loop.

!!! question "Denkopgave"
    Waarom wordt er **geen** oefening gegeven om met een `for` loop de gebruiker getallen te laten ingeven totdat hij `0` ingeeft?

??? note "Antwoord (klik om te openen)"
    Een `for` loop loopt over een **vaste collectie**. Je kunt niet van tevoren weten hoeveel getallen de gebruiker zal ingeven — dat aantal is onbekend en hangt af van de gebruikersinput. Dat is precies het soort situatie waarvoor een `while` loop bedoeld is.

---

## 7.3 Loop controle

Drie statements geven je extra controle over loops: `else`, `break` en `continue`. Ze werken bij zowel `while` als `for` loops.

---

### 7.3.1 `else`

Net als bij `if`, kun je aan een loop een `else` toevoegen. Het blok bij de `else` wordt uitgevoerd **wanneer de loop normaal eindigt**:

```python
i = 0
while i < 5:
    print(i)
    i += 1
else:
    print("De loop eindigt, i is nu", i)
print("Klaar")
```

Bij een `for` loop:

```python
for fruit in ("appel", "mango", "aardbei"):
    print(fruit)
else:
    print("De loop eindigt, fruit is nu", fruit)
print("Klaar")
```

!!! note "Wanneer is `else` nuttig?"
    Op zichzelf voegt `else` weinig toe. Het wordt pas echt nuttig in combinatie met `break` (zie hieronder).

---

### 7.3.2 `break`

Met `break` kun je een loop **onmiddellijk afbreken**. Python springt dan naar de eerste regel na de loop.

**Voorbeeld:** Zoek het kleinste getal dat begint met `1` waarbij de `1` naar het einde verplaatsen het getal verdrievoudigt:

```python
i = 1
while i <= 1000000:
    num1 = int("1" + str(i))
    num2 = int(str(i) + "1")
    if num2 == 3 * num1:
        print(num2, "is drie keer", num1)
        break
    i += 1
else:
    print("Geen antwoord gevonden")
```

!!! info "Waarom `break` hier?"
    We weten niet vooraf hoe groot het antwoord is — misschien wel heel groot, misschien bestaat het niet. We testen tot 1.000.000, maar zodra we een antwoord vinden, is verder zoeken zinloos. `break` stopt de loop onmiddellijk.

!!! warning "`break` annuleert de `else`!"
    Als een loop verlaten wordt via `break`, wordt de code bij de `else` **niet** uitgevoerd. In het voorbeeld hierboven zorgt dit ervoor dat "Geen antwoord gevonden" alleen verschijnt als er echt geen antwoord is.

**Ander voorbeeld:** beoordeel een cijferlijst van een student:

```python
for cijfer in (8, 7.5, 9, 6, 6, 6, 5.5, 7, 5, 8, 7, 7.5):
    if cijfer < 5.5:
        print("De student zakt!")
        break
else:
    print("De student slaagt!")
```

!!! tip "Gebruik `break` om je code leesbaarder te maken"
    Je kunt `break` altijd vermijden door de boolean expressie complexer te maken. Maar een `break` maakt de code vaak **begrijpelijker** dan een lange, ingewikkelde conditie.

    `break` mag alleen binnen een loop gebruikt worden — niet in een losse `if`.

---

### 7.3.3 `continue`

`continue` beëindigt de **huidige cyclus** van de loop onmiddellijk en gaat terug naar het begin van de volgende cyclus. Bij een `while` wordt de boolean expressie opnieuw geëvalueerd; bij een `for` wordt het volgende item gepakt.

**Voorbeeld:** druk alle getallen van 1 tot 100 af die niet deelbaar zijn door 2 of 3, en niet eindigen op 7 of 9:

```python
num = 0
while num < 100:
    num += 1
    if num % 2 == 0:
        continue
    if num % 3 == 0:
        continue
    if num % 10 == 7:
        continue
    if num % 10 == 9:
        continue
    print(num)
```

!!! danger "Gevaar van `continue` in een `while` loop!"
    Als `continue` uitgevoerd wordt **vóór** de regel die de loopvariabele verhoogt, wordt die verhoging overgeslagen. Dat leidt tot een eindeloze loop!

    ```python
    i = 0
    while i < 10:
        if i == 5:
            continue   # ❌ i wordt nooit verhoogd → eindeloze loop!
        i += 1
    ```

    Zorg altijd dat de teller-update **vóór** het `continue` staat, of gebruik een `for` loop.

---

### 7.3.4 Geneste loops

Je kunt loops **in andere loops** plaatsen. Dit heet nesten. De binnenste loop wordt volledig doorlopen bij elke cyclus van de buitenste:

```python
for i in range(1, 4):
    for j in range(1, 4):
        print(i * j, end="\t")
    print()
```

Uitvoer:
```
1	2	3	
2	4	6	
3	6	9	
```

!!! warning "Vermijd diep nesten"
    Meer dan twee niveaus van geneste loops maken code snel onleesbaar. Als je dieper moet nesten, overweeg dan om de binnenste loop in een aparte functie te zetten (zie hoofdstuk 8).

---

## Wat je geleerd hebt

In dit hoofdstuk heb je het volgende gezien:

- De `while` loop — herhaal zolang een boolean expressie `True` is
- Eindeloze loops en hoe je ze vermijdt
- De `for` loop — loop over een collectie
- `range()` — genereer een reeks getallen voor een `for` loop
- Tuples als handmatige collecties
- Loop controle met `else`, `break` en `continue`
- Geneste loops

---

## Opgaven

### Opgave 7.1 — Tekens tellen

!!! example "Opgave 7.1"
    Schrijf een programma dat de gebruiker om een string vraagt. Tel daarna met een `for` loop hoeveel tekens in de string **geen spatie** zijn. Druk het resultaat af.

    Voorbeeld: voor `"Hallo wereld"` is het antwoord `10`.

### Opgave 7.2 — Tafelreeksen

!!! example "Opgave 7.2"
    Schrijf een programma dat de **vermenigvuldigingstabel** van een door de gebruiker ingegeven getal afdrukt, van 1 tot en met 10.

    Voorbeeld voor het getal `7`:
    ```
    7 x 1 = 7
    7 x 2 = 14
    ...
    7 x 10 = 70
    ```

### Opgave 7.3 — Priemgetal testen

!!! example "Opgave 7.3"
    Schrijf een programma dat test of een door de gebruiker ingegeven positief geheel getal een **priemgetal** is. Een priemgetal is een getal groter dan 1 dat alleen deelbaar is door 1 en zichzelf.

    Gebruik een `for` loop met `range()` om alle mogelijke delers te testen. Gebruik `break` als je een deler vindt.

!!! tip "Hint"
    Je hoeft alleen te testen tot de **vierkantswortel** van het getal — maar een eenvoudiger aanpak waarbij je test van 2 tot het getal zelf is ook goed voor nu.

### Opgave 7.4 — Fibonacci reeks

!!! example "Opgave 7.4"
    De **Fibonacci reeks** begint met 0 en 1. Elk volgend getal is de som van de twee voorgaande:

    `0, 1, 1, 2, 3, 5, 8, 13, 21, 34, ...`

    Schrijf een programma dat de eerste **15 Fibonacci getallen** afdrukt met een `while` loop. Gebruik twee variabelen `a` en `b` om de laatste twee getallen bij te houden.

### Opgave 7.5 — Nul in een reeks

!!! example "Opgave 7.5"
    Schrijf een programma dat de volgende reeks getallen doorloopt via een `for` loop:

    ```python
    (5, 4, 0, 3, 1, 0, 2, 7, 6)
    ```

    - Als er een `0` wordt aangetroffen: stop onmiddellijk en druk alleen `"Klaar"` af (gebruik `break`)
    - Negatieve getallen worden overgeslagen (gebruik `continue`)
    - Andere getallen worden afgedrukt

    Test je code ook met de reeks `(5, 4, -2, 3, -1, 7, 6)` (geen nul) — wat drukt het programma dan af?

---

*Volgende: [Hoofdstuk 8 – Functies](h08-functies.md)*
