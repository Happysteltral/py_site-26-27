---
title: Hoofdstuk 15 – Besturingssysteem
description: De os module, bestandssystemen, command prompt en bestandspaden.
---

# Hoofdstuk 15 – Besturingssysteem

Python programma's draaien op een computer met een besturingssysteem. Soms moet je vanuit je programma interageren met dat systeem — bestanden vinden, directories beheren, omgevingsinfo opvragen. Python biedt hiervoor de **`os` module**.

---

## 15.1 Computers en besturingssystemen

Een **besturingssysteem** (Engels: *operating system*, OS) is een laag tussen programma's en hardware. Het biedt standaardfuncties om hardware aan te sturen, ongeacht het merk of model van de computer.

Bekende besturingssystemen: **Windows**, **macOS**, **Linux**.

!!! info "Waarom de `os` module?"
    Elk OS heeft andere commando's en conventies. De `os` module biedt een **uniforme interface** die werkt op alle besturingssystemen. Je programma hoeft niet te weten op welk OS het draait — de `os` module regelt de verschillen.

---

## 15.2 Command prompt

Naast de muisgestuurde interface heeft elk OS ook een **command shell** — een tekstinterface voor het typen van commando's:

| OS | Command shell |
|----|--------------|
| Windows | Command Prompt (`cmd`) of PowerShell |
| macOS | Terminal |
| Linux | Terminal / Bash |

Je kunt Python programma's uitvoeren via de command shell:

```bash
python programma.py
```

!!! example "Opdracht"
    Zoek op jouw systeem de command shell en start hem op. Typ `dir` (Windows) of `ls` (macOS/Linux) om bestanden in de huidige directory te zien.

---

## 15.3 Bestandssysteem

Het **bestandssysteem** organiseert bestanden en directories in een boomstructuur met een **root directory** aan de top.

Een **pad** (path) beschrijft de locatie van een bestand:

| OS | Voorbeeld |
|----|-----------|
| Windows | `C:/Python/programma.py` |
| macOS/Linux | `/home/gebruiker/programma.py` |

Speciale directory-aanwijzers:

| Symbool | Betekenis |
|---------|-----------|
| `.` | Huidige directory |
| `..` | Een niveau hoger |

!!! tip "Gebruik voorwaartse slash"
    In Python strings heeft de backslash `\` een speciale betekenis (escape teken). Gebruik liever de voorwaartse slash `/` in paden — die werkt op alle besturingssystemen, ook op Windows.

---

## 15.4 `os` functies

### 15.4.1 `getcwd()` — huidige directory

Retourneert de huidige working directory als string:

```python
from os import getcwd
print(getcwd())
```

### 15.4.2 `chdir()` — directory wisselen

Wijzigt de huidige directory:

```python
from os import getcwd, chdir

home = getcwd()
print(home)

chdir("..")          # één niveau hoger
print(getcwd())

chdir(home)          # terug naar het begin
print(getcwd())
```

### 15.4.3 `listdir()` — inhoud van een directory

Retourneert een list van alle bestanden en subdirectories in een directory (willekeurige volgorde, zonder volledig pad):

```python
from os import listdir

flist = listdir(".")    # "." = huidige directory
for naam in flist:
    print(naam)
```

### 15.4.4 `system()` — systeemcommando's uitvoeren

Voert een systeemcommando uit alsof het in de command shell getypt is:

```python
from os import system
system("dir")    # Windows
system("ls")     # macOS/Linux
```

!!! warning "Wees voorzichtig met `system()`"
    Systeemcommando's kunnen bestanden verwijderen of andere onomkeerbare acties uitvoeren. Gebruik `system()` alleen als je precies weet wat je doet.

---

## Wat je geleerd hebt

In dit hoofdstuk heb je het volgende gezien:

- Wat een besturingssysteem is en wat de `os` module oplost
- De command shell en hoe je Python programma's erin uitvoert
- Bestandssystemen, paden, en de huidige directory
- `os` functies: `getcwd()`, `chdir()`, `listdir()`, `system()`

---

## Opgaven

### Opgave 15.1 — Bestanden met volledig pad

!!! example "Opgave 15.1"
    Schrijf een programma dat alle bestanden en directories in de huidige directory toont, inclusief het **volledige pad**.

    **Hint:** Combineer `getcwd()` en `listdir()`. Je kunt paden samenvoegen met `os.path.join()` (zie hoofdstuk 16).

---

*Volgende: [Hoofdstuk 16 – Tekstbestanden](h16-tekstbestanden.md)*
