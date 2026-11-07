---
layout: default
title: API- und Katalogänderungen
---

# API- und Katalogänderungen des SVWS-Servers

Diese Dokumentation unterstützt Entwicklerinnen und Entwickler bei der
Migration zwischen SVWS-Server-Versionen. Sie beschreibt Änderungen an
API-Endpunkten, Datenmodellen und Katalogen anhand der im Repository
abgelegten JSON-Dateien.

## Versionsvergleiche

| Zielversion | Ausgangsversion | Dokumentation |
|---|---|---|
| 1.5.1 | 1.4.1 | [Änderungen in 1.5.1](./1.5.1/) |
| 1.6.1 (Entwicklungsstand) | 1.5.1 | [Änderungen im Snapshot](./1.6.1-SNAPSHOT/) |

SNAPSHOT-Versionen zeigen den jeweiligen Entwicklungsstand und können sich
bis zum Release ändern. Das Release 1.6.1 ist für Januar 2027 geplant; die
Entwicklung wird bis dahin regelmäßig verfolgt. Die aktuell abgelegte
API-Beschreibung im Ordner `1.6.1-SNAPSHOT` meldet intern
`1.6.0-SNAPSHOT`.

## Hinweise zur Nutzung

Die Vergleiche beruhen auf den gespeicherten Beschreibungen, nicht auf
Laufzeittests. Eine geänderte oder entfernte Beschreibung beweist für sich
genommen nicht, ob alte Aufrufe vom Server noch unterstützt werden.
Insbesondere bei möglichen Breaking Changes sollten Integrationen gegen die
jeweilige Serverversion getestet werden.

Die Quelldateien und Hinweise zum Ergänzen weiterer Versionen stehen im
[Repository auf GitHub](https://github.com/SVWS-NRW/SVWS-API-Change).