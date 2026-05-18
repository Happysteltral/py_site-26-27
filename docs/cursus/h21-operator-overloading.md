---
title: Hoofdstuk 21 – Operator Overloading
description: Operatoren definiëren voor zelfgemaakte klassen via speciale methodes.
---

# Hoofdstuk 21 – Operator Overloading

**Operator overloading** stelt je in staat te definiëren wat operatoren als `+`, `==`, `<` doen voor jouw eigen klassen. Dit is een krachtige techniek die je klassen naadloos laat integreren in Python-code.

---

## 21.1 Het idee achter operator overloading

Standaard werken operatoren als `+` en `==` niet op zelfgemaakte klassen. Met operator overloading definieer je via **speciale methodes** (methodes met `__naam__`) precies wat er gebeurt.

!!! warning "Gebruik overloading alleen als het logisch is"
    Definieer nooit een operator voor een klasse waarbij die operator geen natuurlijke betekenis heeft. Twee studenten optellen heeft geen logische betekenis — vermijd het.

---

## 21.2 Vergelijkingen

Zonder operator overloading vergelijkt `==` de **identiteit** van objecten (net als `is`):

```python
class Punt:
    def __init__(self, x=0.0, y=0.0):
        self.x = x
        self.y = y

p1 = Punt(3, 4)
p2 = Punt(3, 4)
print(p1 == p2)    # False! (identiteitsvergelijking)
```

Met `__eq__()` definieer je een **waarde-vergelijking**:

```python
class Punt:
    def __init__(self, x=0.0, y=0.0):
        self.x = x
        self.y = y
    def __eq__(self, p):
        return self.x == p.x and self.y == p.y

p1 = Punt(3, 4)
p2 = Punt(3, 4)
print(p1 == p2)    # True!
```

### Alle vergelijkingsoperatoren

| Methode | Operator |
|---------|---------|
| `__eq__(self, other)` | `==` |
| `__ne__(self, other)` | `!=` |
| `__lt__(self, other)` | `<` |
| `__le__(self, other)` | `<=` |
| `__gt__(self, other)` | `>` |
| `__ge__(self, other)` | `>=` |

!!! tip "`__ne__()` is automatisch"
    Als je `__eq__()` definieert maar niet `__ne__()`, retourneert `__ne__()` automatisch het omgekeerde van `__eq__()`.

### `NotImplemented`

Als de vergelijking niet zinvol is voor het gegeven type, retourneer `NotImplemented`:

```python
def __eq__(self, n):
    if isinstance(n, Punt):
        return self.x == n.x and self.y == n.y
    return NotImplemented
```

### `__bool__()`

Definieer hoe je object zich gedraagt als conditie in een `if` statement:

```python
class Punt:
    def __init__(self, x=0.0, y=0.0):
        self.x = x
        self.y = y
    def __bool__(self):
        return self.x != 0.0 or self.y != 0.0  # False als het de oorsprong is
```

---

## 21.3 Berekeningen

Definieer wat er gebeurt bij rekenkundige operatoren:

| Methode | Operator |
|---------|---------|
| `__add__(self, other)` | `+` |
| `__sub__(self, other)` | `-` |
| `__mul__(self, other)` | `*` |
| `__truediv__(self, other)` | `/` |
| `__floordiv__(self, other)` | `//` |
| `__mod__(self, other)` | `%` |
| `__pow__(self, other)` | `**` |

```python
class Quaternion:
    def __init__(self, a, b, c, d):
        self.a, self.b, self.c, self.d = a, b, c, d
    def __repr__(self):
        return "({},{}i,{}j,{}k)".format(self.a, self.b, self.c, self.d)
    def __add__(self, n):
        if isinstance(n, (int, float)):
            return Quaternion(n + self.a, self.b, self.c, self.d)
        elif isinstance(n, Quaternion):
            return Quaternion(n.a + self.a, n.b + self.b,
                              n.c + self.c, n.d + self.d)
        return NotImplemented
    def __radd__(self, n):
        return self.__add__(n)    # optelling is commutatief

c1 = Quaternion(3, 4, 5, 6)
c2 = Quaternion(1, 2, 3, 4)
print(c1 + c2)
print(c1 + 10)
print(10 + c1)    # werkt door __radd__()
```

!!! note "Rechtshandige versies (`__r...__()`)"
    Als de linkeroperand de operator niet ondersteunt, probeert Python de rechteroperand via `__radd__()`, `__rsub__()`, enz. Implementeer deze als de operator commutatief is.

---

## 21.4 Eenwaardige operatoren

| Methode | Functie/Operator |
|---------|----------------|
| `__neg__(self)` | `-x` (negatie) |
| `__pos__(self)` | `+x` |
| `__abs__(self)` | `abs(x)` |
| `__int__(self)` | `int(x)` |
| `__float__(self)` | `float(x)` |
| `__round__(self, n)` | `round(x, n)` |

```python
class Quaternion:
    def __neg__(self):
        return Quaternion(-self.a, -self.b, -self.c, -self.d)
    def __abs__(self):
        return Quaternion(abs(self.a), abs(self.b), abs(self.c), abs(self.d))
```

---

## 21.5 Sequenties

Maak je eigen klasse bruikbaar als sequentie via deze methodes:

| Methode | Functie |
|---------|--------|
| `__len__(self)` | `len(x)` |
| `__getitem__(self, key)` | `x[key]` |
| `__setitem__(self, key, value)` | `x[key] = value` |
| `__delitem__(self, key)` | `del x[key]` |
| `__contains__(self, item)` | `item in x` |

```python
class Zin:
    def __init__(self, woorden):
        self.woorden = woorden
    def __repr__(self):
        return " ".join(self.woorden)
    def __len__(self):
        return len(self.woorden)
    def __getitem__(self, index):
        return self.woorden[index]
    def __setitem__(self, index, waarde):
        self.woorden[index] = waarde
    def __contains__(self, woord):
        return woord in self.woorden

s = Zin(["Hallo", "mooie", "wereld"])
print(len(s))          # 3
print(s[1])            # "mooie"
s[1] = "prachtige"
print("prachtige" in s)  # True
```

---

## Wat je geleerd hebt

In dit hoofdstuk heb je het volgende gezien:

- Wat operator overloading is en wanneer je het gebruikt
- Vergelijkingsoperatoren: `__eq__()`, `__ne__()`, `__lt__()`, enz.
- `NotImplemented` retourneren voor niet-ondersteunde types
- `__bool__()` voor conditie-evaluatie
- Rekenkundige operatoren: `__add__()`, `__sub__()`, enz.
- Rechtshandige versies: `__radd__()`, enz.
- Eenwaardige operatoren: `__neg__()`, `__abs__()`, enz.
- Sequentie methodes: `__len__()`, `__getitem__()`, enz.

---

## Opgaven

### Opgave 21.1 — Speelkaart klasse

!!! example "Opgave 21.1"
    Implementeer een klasse `Kaart` met kleur en waarde. Zorg dat twee kaarten gelijk zijn als ze dezelfde waarde hebben, en dat vergelijkingen de kaartwaarde gebruiken (2 = laagste, Aas = hoogste).

### Opgave 21.2 — Trekstapel

!!! example "Opgave 21.2"
    Maak een klasse `Trekstapel` die een sequentie van `Kaart` objecten bevat. Implementeer `__len__()` en `__getitem__()`. Voeg methodes `voegtoe()` en `trek()` toe.

### Opgave 21.3 — Oorlogje kaartspel

!!! example "Opgave 21.3"
    Gebruik `Kaart` en `Trekstapel` om het kaartspel "Oorlogje" te implementeren. Twee stapels spelen totdat één stapel leeg is.

---

*Volgende: [Hoofdstuk 22 – Overerving](h22-overerving.md)*
