---
title: Hoofdstuk 19 – Bitsgewijze Operatoren
description: Bits, bytes, binair tellen en bitsgewijze operatoren in Python.
---

# Hoofdstuk 19 – Bitsgewijze Operatoren

Bij het werken met binaire bestanden werk je niet meer met tekens en getallen, maar met **bytes**. Python biedt **bitsgewijze operatoren** om data op het niveau van individuele bits te manipuleren.

---

## 19.1 Bits en bytes

Een **bit** is de kleinste data-eenheid: waarde 0 of 1. Een **byte** bestaat uit 8 bits en kan waarden van 0 tot 255 bevatten.

### 19.1.1 Binair tellen

Een byte wordt weergegeven als 8 bits, bijv. `11010010`. De decimale waarde bereken je door elke bit te vermenigvuldigen met een stijgende macht van 2 van rechts naar links:

```
1  1  0  1  0  0  1  0
↓  ↓  ↓  ↓  ↓  ↓  ↓  ↓
128+64+ 0+16+ 0+ 0+ 2+ 0 = 210
```

Bits worden genummerd van rechts (bit 0) naar links (bit 7).

### 19.1.2 Codering van tekens

- **ASCII**: 7-bits codering, tekens 0–127
- **Latin-1**: 8-bits codering, tekens 0–255
- **UTF-8**: variabele lengte, ondersteunt vrijwel alle tekens wereldwijd

---

## 19.2 Manipulatie van bits

### 19.2.1 Shift operators: `<<` en `>>`

Verschuif alle bits naar links of rechts:

```python
# Links schuiven = vermenigvuldigen met macht van 2
print(345 << 2)    # 345 × 4 = 1380

# Rechts schuiven = delen door macht van 2
print(345 >> 3)    # 345 // 8 = 43

# Voorbeeld met tekens:
code = "!"         # ASCII 33 = 00100001
print(chr(ord(code) << 1))   # 01000010 = 66 = "B"
print(chr(ord("B") >> 1))    # 00100001 = 33 = "!"
```

### 19.2.2 Bitsgewijze and: `&`

Resultaat is `1` alleen waar **beide** bits `1` zijn:

```
11   = 00001011
 6   = 00000110
&    = 00000010  = 2
```

```python
print(11 & 6)    # 2
```

Toepassing: modulo van een macht van 2 berekenen:

```python
print(345 & 31)    # 345 % 32 = 345 & (32-1) = 345 & 31
```

### 19.2.3 Bitsgewijze or: `|`

Resultaat is `1` als **minstens één** bit `1` is:

```
11   = 00001011
 6   = 00000110
|    = 00001111  = 15
```

```python
print(11 | 6)    # 15
```

Toepassing: een specifieke bit op `1` zetten:

```python
getal = 100
bit_index = 7
getal = getal | (1 << bit_index)   # zet bit 7 op 1
print(getal)
```

### 19.2.4 Bitsgewijze not: `~`

Flipt alle bits (elke `0` wordt `1` en omgekeerd):

```python
print(~11)    # -12  (door het twee-complement systeem)
```

Toepassing: een specifieke bit op `0` zetten:

```python
getal = 255
bit_index = 3
getal = getal & ~(1 << bit_index)   # zet bit 3 op 0
print(getal)   # 255 - 8 = 247
```

### 19.2.5 Bitsgewijze xor: `^`

Resultaat is `1` als de bits **verschillend** zijn:

```
11   = 00001011
 6   = 00000110
^    = 00001101  = 13
```

```python
print(11 ^ 6)    # 13
```

Toepassing: eenvoudige encryptie (XOR met een masker, twee keer toepassen geeft origineel terug):

```python
masker = 0b00101010    # = 42
getal  = 65            # "A"

versleuteld = getal ^ masker
print(versleuteld)                  # 107

ontsleuteld = versleuteld ^ masker
print(chr(ontsleuteld))             # "A" — origineel terug
```

### 19.2.6 Volgorde van bitsgewijze operatoren

!!! danger "Gebruik haakjes!"
    De bitsgewijze operatoren worden **niet** vóór de rekenkundige operatoren uitgevoerd. Gebruik altijd haakjes:

    ```python
    print(1 << 1 + 2 << 1)     # ❌ = (1 << (1+2)) << 1 = 16, niet 6
    print((1 << 1) + (2 << 1)) # ✅ = 2 + 4 = 6
    ```

---

## 19.3 Praktijktoepassing: RGB kleuren

Kleuren zijn vaak gecodeerd als drie bytes (rood, groen, blauw) in één getal:

```python
def getRGB(color):
    blauw = color & 255
    groen = (color >> 8) & 255
    rood  = (color >> 16) & 255
    return rood, groen, blauw

r, g, b = getRGB(223567)
print("rood={}, groen={}, blauw={}".format(r, g, b))
```

---

## Wat je geleerd hebt

In dit hoofdstuk heb je het volgende gezien:

- Binair tellen: bits, bytes, en bit-nummering
- Tekens- en getalcodering: ASCII, Latin-1, UTF-8
- Bitsgewijze operatoren: `<<`, `>>`, `&`, `|`, `~`, `^`
- Toepassingen: modulo, bit zetten/wissen, XOR-encryptie, RGB-kleurextractie

---

## Opgaven

### Opgave 19.1 — XOR string encryptie

!!! example "Opgave 19.1"
    Versleutel een string met het masker `0b00101010` (= 42) via XOR. Toon de resulterende (versleutelde) string. Ontsleutel hem daarna en toon de ontsleutelde string, die gelijk moet zijn aan de originele.

### Opgave 19.2 — Bit opslaan in integer

!!! example "Opgave 19.2"
    Schrijf twee functies:

    1. `zet_bit(integer, boolean, index)` — als `boolean` True is, zet bit `index` op 1; als False, zet hem op 0. Retourneert de gewijzigde integer.
    2. `lees_bit(integer, index)` — retourneert True als bit `index` van de integer 1 is, anders False.

    Test beide functies door bits in te stellen en terug te lezen.

---

*Volgende: [Hoofdstuk 20 – Object Oriëntatie](h20-object-orientatie.md)*
