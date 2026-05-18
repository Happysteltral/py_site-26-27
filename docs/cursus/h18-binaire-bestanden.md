---
title: Hoofdstuk 18 – Binaire Bestanden
description: Binaire bestanden lezen en schrijven, byte strings, en pointer positionering.
---

# Hoofdstuk 18 – Binaire Bestanden

**Binaire bestanden** is de term voor alle bestanden die geen platte tekstbestanden zijn: uitvoerbare programma's, afbeeldingen, video's, Word documenten... In dit hoofdstuk leer je hoe Python omgaat met binaire bestanden.

---

## 18.1 Openen en sluiten van binaire bestanden

Je opent een binair bestand met de letter `"b"` toegevoegd aan de modus:

| Modus | Betekenis |
|-------|----------|
| `"rb"` | Lezen in binaire modus |
| `"wb"` | Schrijven in binaire modus (leegmaken!) |
| `"r+b"` | Lezen én schrijven in binaire modus |
| `"w+b"` | Lezen én schrijven (leegmaken!) |

```python
fp = open("bestand.bin", "rb")
fp.close()
```

---

## 18.2 Lezen uit een binair bestand

### 18.2.1 Byte strings

Het lezen uit een binair bestand retourneert geen gewone string, maar een **byte string** — aangeduid met een `b` prefix:

```python
hw1 = "Hello, world!"     # reguliere string
hw2 = b"Hello, world!"    # byte string
print(hw1)
print(hw2)
```

Het verschil: als je een teken via een index uit een byte string haalt, krijg je een **getal** (de byte-waarde), niet een teken:

```python
hw1 = "Hello, world!"
hw2 = b"Hello, world!"
print(hw1[0])    # "H"
print(hw2[0])    # 72  (ASCII code van 'H')
```

#### Byte string aanmaken uit een list van getallen

```python
bs = bytes([72, 101, 108, 108, 111, 44, 32, 119, 111, 114, 108, 100, 33])
print(bs)   # b'Hello, world!'
```

#### Byte string omzetten naar string (en omgekeerd)

```python
hw_bytes = b"Hello, world!"
hw_str = hw_bytes.decode("utf-8")    # byte string → string
print(hw_str)

hw_bytes2 = hw_str.encode("utf-8")   # string → byte string
print(hw_bytes2)
```

---

### 18.2.2 Lezen met `read(n)`

`read()` met een integer argument leest precies `n` bytes:

```python
fp = open("bestand.bin", "rb")
for i in range(10):
    buffer = fp.read(10)    # lees 10 bytes per keer
    print(buffer)
fp.close()
```

---

## 18.3 Schrijven in een binair bestand

Schrijf een **byte string** naar het bestand:

```python
from os.path import getsize

NAAM = "testbestand.bin"
fp = open(NAAM, "wb")
fp.write(b"And now for something completely different...\x0A")
fp.close()
print(getsize(NAAM), "bytes geschreven")
```

---

## 18.4 Positioneren van de pointer

Met `seek()` verplaats je de pointer naar een specifieke positie:

```python
fp.seek(positie)        # verplaats naar byte-positie vanaf begin
fp.seek(positie, 0)     # idem — 0 = vanaf begin
fp.seek(positie, 1)     # relatief t.o.v. huidige positie
fp.seek(positie, 2)     # relatief t.o.v. einde (-n bytes voor einde)
```

`tell()` retourneert de huidige pointer-positie:

```python
fp = open("bestand.bin", "rb")
print("Positie:", fp.tell())    # 0 (begin)
fp.seek(50)
print("Positie:", fp.tell())    # 50
buffer = fp.read(10)
print("Positie:", fp.tell())    # 60
fp.close()
```

!!! note "seek() voor tekstbestanden"
    `seek()` en `tell()` werken ook op tekstbestanden, maar zijn daar zelden nuttig. Ze zijn primair bedoeld voor binaire bestanden.

---

## Wat je geleerd hebt

In dit hoofdstuk heb je het volgende gezien:

- Binaire bestanden openen in `"rb"`, `"wb"`, `"r+b"` modus
- Byte strings — het verschil met gewone strings
- Byte strings aanmaken met `bytes([...])`
- Conversie: `decode()` (bytes → string) en `encode()` (string → bytes)
- `read(n)` — een vast aantal bytes lezen
- `write()` — een byte string schrijven
- `seek()` en `tell()` — pointer positionering

---

## Opgaven

### Opgave 18.1 — Eenvoudige encryptie

!!! example "Opgave 18.1"
    Schrijf een programma dat een tekstbestand versleutelt door elke byte-waarde te manipuleren:

    - Als de byte-waarde kleiner dan 128 is: tel 128 op
    - Als de byte-waarde groter dan 128 is: trek 128 af

    Test op een **kopie** van een tekstbestand. De gewijzigde versie moet onleesbaar zijn. Als je het programma opnieuw uitvoert op de versleutelde versie, moet je het origineel terugkrijgen.

---

*Volgende: [Hoofdstuk 19 – Bitsgewijze Operatoren](h19-bitsgewijze-operatoren.md)*
