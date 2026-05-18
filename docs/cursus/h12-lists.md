---
title: Hoofdstuk 12 – Lists
description: Lists aanmaken, wijzigen, sorteren, kopiëren en list comprehensions.
---

# Hoofdstuk 12 – Lists

Een **list** is een geordende verzameling van elementen — net als een tuple, maar met één cruciaal verschil: lists zijn **veranderbaar**. Je kunt elementen toevoegen, verwijderen en overschrijven. Dit maakt lists tot de meest gebruikte data structuur in Python.

---

## 12.1 Basis van lists

Lists schrijf je met **vierkante haken** `[]`. Je mag data types mixen, en je hebt toegang tot dezelfde basisfuncties als bij tuples:

```python
fruitlist = ["appel", "banaan", "kers", 27, 3.14]
print(len(fruitlist))      # 5

for element in fruitlist:
    print(element)

print(fruitlist[2])        # "kers"

numlist = [314, 315, 642, 246, 129, 999]
print(max(numlist))        # 999
print(min(numlist))        # 129
print(sum(numlist))        # 2645
print(100 in numlist)      # False
print(999 in numlist)      # True
```

Afgezien van de vierkante haken lijken lists sterk op tuples. Maar er is een groot verschil...

---

## 12.2 Lists zijn veranderbaar

Je kunt een individueel element overschrijven via een assignment:

```python
fruitlist = ["appel", "banaan", "kers", "doerian", "mango"]
fruitlist[2] = "aardbei"
print(fruitlist)
```

Je kunt ook een **sub-list overschrijven** — de nieuwe sub-list hoeft niet even lang te zijn:

```python
fruitlist = ["appel", "banaan", "kers", "doerian", "mango"]
fruitlist[1:3] = ["framboos", "aardbei", "aalbes"]
print(fruitlist)   # 3 elementen vervangen door 3 nieuwe
```

Nieuwe elementen **invoegen** door toe te kennen aan een lege sub-list:

```python
fruitlist = ["appel", "banaan", "kers", "doerian", "mango"]
fruitlist[1:1] = ["framboos", "aardbei", "aalbes"]
print(fruitlist)
```

Elementen **verwijderen** door een lege list toe te kennen:

```python
fruitlist = ["appel", "banaan", "kers", "doerian", "mango"]
fruitlist[1:3] = []
print(fruitlist)
```

!!! tip "Methodes zijn leesbaarder"
    De sub-list syntaxis is krachtig maar kan verwarrend zijn. De list methodes (zie sectie 12.4) zijn veel leesbaarder en de aanbevolen aanpak.

!!! danger "Pas op: list wijzigen in een loop!"
    Als je een list aanpast **terwijl je er doorheen loopt**, krijg je onverwachte resultaten:

    ```python
    numlist = [1, 2, 0, 3, 4, 0, 0, 5, 6, 7]
    for num in numlist:
        if num == 0:
            numlist.remove(0)   # ❌ verwijdert elementen terwijl je loopt!
        else:
            print(num, end=" ")
    print(numlist)
    ```

    De `3` wordt overgeslagen en er blijft één `0` achter. Dit is een bekende valkuil. Loop **nooit** over een list terwijl je hem verwijdert. Maak in plaats daarvan een nieuwe list.

---

## 12.3 Lists en operatoren

Net als strings ondersteunen lists `+` (samenvoegen) en `*` (herhalen):

```python
fruitlist = ["appel", "banaan"] + ["kers", "doerian"]
print(fruitlist)

numlist = 10 * [0]   # snel een list van tien nullen
print(numlist)
```

!!! warning "Enkel element toevoegen via `+`"
    Je kunt **geen** los element optellen bij een list — je moet er vierkante haken omheen zetten:

    ```python
    fruitlist = ["appel", "banaan"]
    fruitlist += "kers"      # ❌ voegt losse letters toe!
    print(fruitlist)         # ['appel', 'banaan', 'k', 'e', 'r', 's']

    fruitlist = ["appel", "banaan"]
    fruitlist += ["kers"]    # ✅ correct
    print(fruitlist)
    ```

---

## 12.4 List methodes

!!! warning "Belangrijk verschil met string methodes!"
    String methodes **passen de originele string nooit aan** — ze retourneren een nieuwe string. List methodes daarentegen **wijzigen de list zelf** — ze hebben vaak geen retourwaarde. Dit is een veelgemaakte fout bij beginners:

    ```python
    fruitlist = ["kers", "appel", "banaan"]
    gesorteerd = fruitlist.sort()   # ❌ gesorteerd is None!
    print(gesorteerd)               # None

    fruitlist.sort()                # ✅ de list zelf is nu gesorteerd
    print(fruitlist)
    ```

### 12.4.1 `append()`

Voegt één element toe aan het **einde** van de list:

```python
fruitlist = ["appel", "banaan", "kers"]
fruitlist.append("mango")
print(fruitlist)
```

### 12.4.2 `extend()`

Voegt **alle elementen** van een tweede list toe aan het einde:

```python
fruitlist = ["appel", "banaan", "kers"]
fruitlist.extend(["framboos", "aardbei", "aalbes"])
print(fruitlist)
```

### 12.4.3 `insert()`

Voegt een element in op een **specifieke positie**:

```python
fruitlist = ["appel", "banaan", "kers", "doerian"]
fruitlist.insert(2, "mango")   # insert vóór index 2
print(fruitlist)
```

### 12.4.4 `remove()`

Verwijdert de **eerste instantie** van een waarde. Geeft een runtime error als de waarde niet bestaat:

```python
fruitlist = ["appel", "banaan", "kers", "doerian"]
fruitlist.remove("banaan")
print(fruitlist)
```

### 12.4.5 `pop()`

Verwijdert een element via **index** en **retourneert** het. Zonder argument verwijdert het het laatste element:

```python
fruitlist = ["appel", "banaan", "kers", "doerian"]
print(fruitlist.pop())     # "doerian" — verwijdert laatste
print(fruitlist.pop(0))    # "appel" — verwijdert eerste
print(fruitlist)
```

!!! tip "pop() vs remove()"
    Gebruik `remove()` als je een element **op waarde** wilt verwijderen, en `pop()` als je op **index** werkt en de verwijderde waarde nog nodig hebt.

### 12.4.6 `del`

`del` verwijdert een element of sub-list via index (geen retourwaarde):

```python
fruitlist = ["appel", "banaan", "kers", "banaan", "doerian"]
del fruitlist[3]        # verwijder index 3
print(fruitlist)

del fruitlist[1:3]      # verwijder sub-list
print(fruitlist)
```

### 12.4.7 `index()`

Retourneert de index van de **eerste instantie** van een waarde. Runtime error als niet gevonden:

```python
fruitlist = ["appel", "banaan", "kers", "banaan", "doerian"]
print(fruitlist.index("banaan"))   # 1
```

### 12.4.8 `count()`

Telt hoe vaak een waarde voorkomt in de list:

```python
fruitlist = ["appel", "banaan", "kers", "banaan", "doerian"]
print(fruitlist.count("banaan"))   # 2
```

### 12.4.9 `sort()`

Sorteert de list **op zijn plaats** (geen retourwaarde!):

```python
fruitlist = ["appel", "aardbei", "banaan", "framboos", "kers"]
fruitlist.sort()
print(fruitlist)    # alfabetisch

numlist = [314, 315, 642, 246, 129, 999]
numlist.sort()
print(numlist)      # numeriek

# Omgekeerde volgorde:
fruitlist.sort(reverse=True)
print(fruitlist)
```

#### Sorteren met een `key` functie

Je kunt een `key` functie meegeven — Python roept die functie aan voor elk element om de sorteringswaarde te bepalen:

```python
# Case-insensitief sorteren:
fruitlist = ["appel", "Aardbei", "banaan", "KERS", "Mango"]
fruitlist.sort(key=str.lower)
print(fruitlist)

# Sorteren op stringlengte, dan alfabetisch:
def len_alfabetisch(element):
    return len(element), element

fruitlist = ["appel", "aardbei", "banaan", "kers", "mango"]
fruitlist.sort(key=len_alfabetisch)
print(fruitlist)
```

!!! note "Geen haakjes bij de key functie"
    Schrijf `key=str.lower`, **niet** `key=str.lower()`. Je geeft de **functie zelf** mee als argument, niet de uitkomst van een aanroep.

#### Lambda functies als key

Je kunt ook een **anonieme functie** (lambda) gebruiken als key, zodat je geen aparte functie hoeft te definiëren:

```python
fruitlist = ["appel", "aardbei", "banaan", "kers", "mango"]
fruitlist.sort(key=lambda x: (len(x), x))
print(fruitlist)
```

### 12.4.10 `reverse()`

Keert de volgorde van de list om (op zijn plaats):

```python
fruitlist = ["appel", "aardbei", "banaan", "kers"]
fruitlist.reverse()
print(fruitlist)
```

---

## 12.5 Alias

Als je een list toewijst aan een nieuwe variabele, maak je een **alias** — geen kopie. Beide variabelen verwijzen naar dezelfde list in het geheugen:

```python
fruitlist = ["appel", "banaan", "kers", "doerian"]
fruitlist2 = fruitlist

fruitlist2[2] = "mango"
print(fruitlist)    # ook gewijzigd! ['appel', 'banaan', 'mango', 'doerian']
print(fruitlist2)   # ['appel', 'banaan', 'mango', 'doerian']
```

Je kunt dit verifiëren met `id()`:

```python
print(id(fruitlist))    # zelfde getal
print(id(fruitlist2))   # zelfde getal
```

Om een **echte kopie** te maken, gebruik je de slice-notatie `[:]`:

```python
fruitlist3 = fruitlist[:]   # echte kopie
print(id(fruitlist3))       # ander getal!
```

### 12.5.1 `is` — identiteitstest

Het gereserveerde woord `is` test of twee variabelen verwijzen naar **hetzelfde object** in het geheugen (niet of ze gelijke inhoud hebben):

```python
fruitlist = ["appel", "banaan", "kers"]
fruitlist2 = fruitlist      # alias
fruitlist3 = fruitlist[:]   # kopie

print(fruitlist is fruitlist2)    # True  — zelfde object
print(fruitlist is fruitlist3)    # False — ander object
print(fruitlist == fruitlist3)    # True  — gelijke inhoud
```

!!! note "== vs is"
    `==` vergelijkt **inhoud**. `is` vergelijkt **identiteit** (hetzelfde object in geheugen). Gebruik altijd `==` voor gewone vergelijkingen en `is` alleen als je echt wilt weten of twee variabelen naar hetzelfde object verwijzen.

### 12.5.2 Ondiepe vs. diepe kopieën

Een kopie via `[:]` is een **ondiepe kopie**. Als de list sub-lists bevat, worden die **niet** gekopieerd maar als alias opgeslagen:

```python
numlist = [1, 2, [3, 4]]
copylist = numlist[:]

numlist[0] = 5          # wijzigt alleen numlist
numlist[2][0] = 6       # wijzigt BEIDE! want de sub-list is een alias

print(numlist)          # [5, 2, [6, 4]]
print(copylist)         # [1, 2, [6, 4]]  ← ook gewijzigd!
```

Gebruik `deepcopy()` voor een echte diepe kopie:

```python
from copy import deepcopy

numlist = [1, 2, [3, 4]]
copylist = deepcopy(numlist)

numlist[2][0] = 6
print(numlist)          # [1, 2, [6, 4]]
print(copylist)         # [1, 2, [3, 4]]  ← ongewijzigd
```

### 12.5.3 Lists als functie-argumenten

Als je een list meegeeft aan een functie, krijgt die functie een **alias** — niet een kopie. De functie kan de originele list wijzigen:

```python
def wijzig_list(x):
    if len(x) > 0:
        x[0] = "GEWIJZIGD!"

fruitlist = ["appel", "banaan", "kers"]
wijzig_list(fruitlist)
print(fruitlist)   # ["GEWIJZIGD!", "banaan", "kers"]
```

!!! warning "Wil je de originele list beschermen?"
    Geef dan een **diepe kopie** mee:

    ```python
    wijzig_list(deepcopy(fruitlist))
    print(fruitlist)   # ongewijzigd
    ```

---

## 12.6 Geneste lists

Lists kunnen andere lists als elementen bevatten. Dit is handig voor **matrices**:

```python
def toon_bord(b):
    print("  1 2 3")
    for rij in range(3):
        print(rij + 1, end=" ")
        for kol in range(3):
            print(b[rij][kol], end=" ")
        print()

bord = [["-", "-", "-"], ["-", "-", "-"], ["-", "-", "-"]]
bord[1][1] = "X"   # midden
bord[0][2] = "O"   # rechterbovenhoek
toon_bord(bord)
```

Je benadert een cel via twee indices: `bord[rij][kolom]`.

---

## 12.7 List casting

Met `list()` zet je een andere collectie om naar een list:

```python
# Tuple naar list:
t1 = ("appel", "banaan", "kers")
fruitlist = list(t1)
print(type(fruitlist))    # <class 'list'>

# range() naar list:
numlist = list(range(1, 11))
print(numlist)    # [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]

# String naar list van tekens:
tekenlist = list("hallo")
print(tekenlist)  # ['h', 'a', 'l', 'l', 'o']
```

---

## 12.8 List comprehensions

Een **list comprehension** is een compacte manier om lists te maken. Python-specifiek en optioneel — maar je moet ze kunnen **herkennen** in andermans code.

Vergelijk de klassieke aanpak met een list comprehension:

=== "Klassiek (met functie)"
    ```python
    def kwadraatlist():
        k = []
        for i in range(1, 26):
            k.append(i * i)
        return k

    sl = kwadraatlist()
    print(sl)
    ```

=== "List comprehension"
    ```python
    sl = [x * x for x in range(1, 26)]
    print(sl)
    ```

Met een `if` filter:

```python
# Kwadraten zonder veelvouden van 5:
sl = [x * x for x in range(1, 26) if x % 10 != 5]
print(sl)
```

Complexe combinaties zijn ook mogelijk:

```python
# Tuples van drie verschillende integers tussen 1 en 4:
triolist = [(x, y, z) for x in range(1, 5)
            for y in range(1, 5) for z in range(1, 5)
            if x != y if x != z if y != z]
print(triolist)
```

!!! tip "Leesbaarheid boven compactheid"
    List comprehensions zijn elegant maar kunnen snel onleesbaar worden. Gebruik ze alleen als de code er duidelijker van wordt, niet om te laten zien hoe goed je Python kent.

---

## Wat je geleerd hebt

In dit hoofdstuk heb je het volgende gezien:

- Lists aanmaken en doorlopen
- Lists zijn veranderbaar — elementen overschrijven, invoegen, verwijderen
- Operatoren `+` en `*` met lists
- List methodes: `append()`, `extend()`, `insert()`, `remove()`, `pop()`, `del`, `index()`, `count()`, `sort()` (met `key`), `reverse()`
- Aliassen en het verschil met echte kopieën
- `is` — identiteitstest
- Ondiepe vs. diepe kopieën (`deepcopy()`)
- Lists als functie-argumenten: pass by reference
- Geneste lists (matrices)
- List casting met `list()`
- List comprehensions

---

## Opgaven

### Opgave 12.1 — Magische bol

!!! example "Opgave 12.1"
    Schrijf een magische-bol programma dat een willekeurig antwoord geeft op elke vraag. Gebruik de list hieronder:

    ```python
    antwoord = ["Dat is zeker", "Het is zeker zo", "Zonder twijfel",
    "Ja, zeker", "Je kunt erop vertrouwen", "Zoals ik het zie, ja",
    "Waarschijnlijk", "Ziet er goed uit", "Ja", "Lijkt van wel",
    "Vaag, probeer het nog eens", "Vraag later nog eens",
    "Kan ik beter niet zeggen", "Kan ik nu niet voorspellen",
    "Concentreer je en vraag nog eens", "Reken er maar niet op",
    "Ik zeg van niet", "Mijn bronnen zeggen van niet",
    "Lijkt er niet op", "Zeer twijfelachtig"]
    ```

    Vraag de gebruiker een vraag te stellen en geef een willekeurig antwoord. Herhaal dit totdat de gebruiker op Enter drukt zonder iets te typen.

### Opgave 12.2 — Stok kaarten schudden

!!! example "Opgave 12.2"
    Een speelkaart heeft een kleur (`"Harten"`, `"Schoppen"`, `"Klaveren"`, `"Ruiten"`) en een waarde (`2` t/m `10`, `"Boer"`, `"Vrouw"`, `"Heer"`, `"Aas"`). Maak een list met alle 52 speelkaarten. Schrijf dan een functie die de stok schudt (willekeurige volgorde).

    **Hint:** Gebruik de `shuffle()` functie uit de `random` module.

### Opgave 12.3 — FIFO Queue

!!! example "Opgave 12.3"
    Schrijf een programma dat een **FIFO queue** (first-in, first-out) implementeert. De gebruiker kan:

    - Iets intypen → voeg toe aan het einde van de queue
    - `?` intypen → verwijder en toon het eerste element van de queue (of een foutmelding als de queue leeg is)
    - Alleen Enter → stop het programma

### Opgave 12.4 — Letters tellen en sorteren

!!! example "Opgave 12.4"
    Tel hoe vaak elke letter voorkomt in een string (case-insensitief, niet-letters negeren). Druk de letters af gesorteerd van meest naar minst voorkomend.

    **Hint:** Gebruik `ord(letter) - ord("a")` als index in een list van 26 tellers.

### Opgave 12.5 — Zeef van Eratosthenes

!!! example "Opgave 12.5"
    Implementeer de **zeef van Eratosthenes** om alle priemgetallen tussen 1 en 100 te vinden:

    1. Maak een list van 1 t/m 100
    2. Zet de waarde van 1 op 0 (geen priemgetal)
    3. Zoek het eerste niet-nul getal (= 2) → priemgetal, zet alle veelvouden op 0
    4. Ga door met het volgende niet-nul getal...
    5. Druk alle overgebleven priemgetallen af

### Opgave 12.6 — Boter-kaas-eieren

!!! example "Opgave 12.6"
    Schrijf een volledig boter-kaas-eieren spel voor twee spelers. Het programma vraagt om beurt aan elke speler om een rij en kolom te kiezen. Controleer of de cel geldig en leeg is. Detecteer een winnaar (drie op een rij) of een gelijkspel (bord vol).

    Gebruik functies: `toon_bord()`, `neemRijKolom()`, `winnaar()`, `opponent()`.

---

*Volgende: [Hoofdstuk 13 – Dictionaries](h13-dictionaries.md)*
