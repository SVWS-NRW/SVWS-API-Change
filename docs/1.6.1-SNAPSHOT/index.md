---
layout: default
title: Änderungen im Entwicklungsstand für 1.6.1
---

[← Alle Versionsvergleiche](../)

# Änderungen von 1.5.1 auf 1.6.0-SNAPSHOT

## Entwicklungsstand und Vergleichsgrundlage

**Stand der Auswertung: 7. November 2026.** Der Snapshot bildet einen
Entwicklungsstand ab, kein fertiges Release. Änderungen können bis zur
Veröffentlichung noch hinzukommen, angepasst oder zurückgenommen werden.
Das Release 1.6.1 ist für Januar 2027 geplant; der Entwicklungsstand soll
bis dahin regelmäßig verfolgt werden.

Verglichen wurden die abgelegten Dateien aus `openAPI/1.5.1` und
`openAPI/1.6.1-SNAPSHOT`. Der Snapshot-Ordner bezeichnet das angestrebte
Release; seine Serverbeschreibung gibt intern `1.6.0-SNAPSHOT` an. Die
Release-Datei meldet `1.5.1`. Beide Beschreibungen verwenden OpenAPI 3.0.1.
Die Auswertung bezieht sich auf die am 7. November 2026 abgelegten Dateien;
es wurde für diesen Vergleich kein neuer Snapshot abgerufen.

Zur eindeutigen Zuordnung des verglichenen Stands:

| Datei | SHA-256 |
|---|---|
| `openAPI/1.5.1/server.json` | `91e64f29862661d69818a6361a5cfc399c8a33c50a6d13127573526736a46552` |
| `openAPI/1.5.1/allinone.json` | `3be7ff0ad12e1afbe35ab3d394ec1e274d726ca075a3c4c8fbcdcfceae134d05` |
| `openAPI/1.6.1-SNAPSHOT/server.json` | `de29acf64114d168cd220409a25a20271b513d731249e055ce63fe53d65b6f82` |
| `openAPI/1.6.1-SNAPSHOT/allinone.json` | `89c99db404bcc05946f90a25f6da7975ba0f7a8220265204dca9afb09ac89679` |

Die Auswertung beschreibt Unterschiede in den JSON-Dateien, nicht das zur
Laufzeit überprüfte Verhalten des Servers.

## Überblick

| Merkmal | 1.5.1 | 1.6.0-SNAPSHOT | Veränderung |
|---|---:|---:|---|
| API-Pfade | 784 | 799 | +15 |
| HTTP-Operationen | 921 | 935 | 23 hinzugefügt, neun entfernt |
| Schemas | 562 | 578 | 17 hinzugefügt, eines entfernt |
| Kataloge | 74 | 76 | zwei hinzugefügt, keiner entfernt |

Eine Operation wird anhand von HTTP-Methode und vollständigem Pfadtext
verglichen. Umbenannte Pfadparameter würden daher als Entfernung und
Hinzufügung zählen. Von 912 Operationen mit identischer Methode und
identischem Pfad weisen sieben Unterschiede in der Beschreibung auf;
darunter sind auch reine Textänderungen.

## API-Endpunkte

Alle nachfolgenden Pfade beginnen mit `/db/{schema}`.

### Neue Operationen

**GOSt:**

| Methode | Pfad |
|---|---|
| PUT | `/gost/schueler/aktuelleplanung/gklwahl` |
| GET | `/gost/schueler/wiederholungen` |
| GET, POST | `/gost/schueler/{id}/aktuelleplanung/laufbahnplanung/v2/daten` |
| GET | `/gost/schueler/{id}/aktuelleplanung/laufbahnplanung/v2/export` |
| POST | `/gost/schueler/{id}/aktuelleplanung/laufbahnplanung/v2/import` |
| POST | `/gost/schueler/{schuelerid}/aktuelleplanung/fachwahl/delete` |
| POST | `/gost/schueler/{schuelerid}/aktuelleplanung/fachwahl/reset` |
| PATCH | `/gost/schueler/{schuelerid}/aktuelleplanung/fachwahl/{fachid}` |
| GET | `/gost/schueler/{schuelerid}/aktuelleplanung/gklwahl` |
| GET | `/gost/schueler/{schuelerid}/aktuelleplanung/laufbahnplanung` |
| DELETE, GET, POST | `/gost/schueler/{schuelerid}/wiederholung` |
| POST | `/gost/schueler/{schuelerid}/wiederholung/uebernehmen` |

**Schule, Klassen und Schüler:**

| Methode | Pfad |
|---|---|
| POST | `/klassen/multiple` |
| POST | `/schueler/lernabschnittsdaten` |
| GET | `/schule/schulinterne-funktionen` |
| POST | `/schule/schulinterne-funktionen/create` |
| DELETE | `/schule/schulinterne-funktionen/delete/multiple` |
| PATCH | `/schule/schulinterne-funktionen/{id}` |
| POST | `/schule/schuljahresabschnitte/nach-letztem` |
| POST | `/schule/schuljahresabschnitte/vor-erstem` |

### Nicht mehr beschriebene Operationen

| Methode | Pfad |
|---|---|
| DELETE, GET | `/schule/einwilligungsarten/{id}` |
| POST | `/schule/leitungsfunktion/create/multiple` |
| DELETE, GET | `/schule/leitungsfunktion/{id}` |
| GET | `/schule/leitungsfunktionen` |
| POST | `/schule/leitungsfunktionen/create` |
| DELETE | `/schule/leitungsfunktionen/delete/multiple` |
| PATCH | `/schule/leitungsfunktionen/{id}` |

Die neu beschriebenen `schulinterne-funktionen`-Pfade sind bei der Migration
zu prüfen; die Beschreibung allein belegt nicht, dass sie ein vollständiger
Ersatz für die entfernten `leitungsfunktionen`-Pfade sind. Andere
`leitungsfunktion`-Operationen bleiben in der Beschreibung enthalten.

### Geänderte Antworten bestehender Operationen

| Operation | Dokumentierte Änderung |
|---|---|
| `PATCH /schule/teilstandort/{id}` | Erfolg: statt HTTP 204 nun HTTP 200 mit `Teilstandort`; HTTP 409 ist nicht mehr aufgeführt. |
| `PATCH /schule/ankreuzkompetenzen/{id}` | HTTP 200 enthält nun ein `Ankreuzkompetenz`-Objekt. |
| `PATCH /schule/leitungsfunktion/{id}` | HTTP 200 enthält nun ein `Schulleitung`-Objekt. |
| `DELETE /schule/leitungsfunktion/delete/multiple` | HTTP 200 enthält statt eines Arrays von `Schulleitung` ein Array von `Einfache-Operation-Antwort`. |
| `POST /schule/ankreuzkompetenzen/create` | Zusätzlich sind HTTP 400, 404 und 409 beschrieben. |
| `DELETE /schule/ankreuzkompetenzen/delete/multiple` | HTTP 400 und 404 sind nicht mehr aufgeführt. Laut neuer Beschreibung werden bereits bei Schülern verwendete Ankreuzkompetenzen nicht gelöscht; die Ergebnisse werden je ID gemeldet. |
| `POST /schule/leitungsfunktion/create` | HTTP 400 ist zusätzlich beschrieben. |

Diese Angaben geben die Spezifikation wieder. Ob sich das tatsächliche
Laufzeitverhalten entsprechend geändert hat, wurde nicht getestet.

## Datenmodelle

Im Snapshot kommen 17 Schemas hinzu; `Leitungsfunktion` entfällt. Bei 22
Schemas mit unverändertem Namen gibt es Unterschiede. Über diese gemeinsamen
Schemas hinweg wurden sechs Eigenschaften entfernt und 29 hinzugefügt.
Insbesondere folgende Änderungen können Client-Anpassungen erfordern:

| Schema | Dokumentierte Änderung |
|---|---|
| `JahrgangCreateRequest`, `JahrgangPatchRequest`, `JahrgangsDaten` | `idJahrgang` entfällt; `idJahrgangStatistik` kommt hinzu. Im Create-Request ist das neue Feld verpflichtend. |
| `KursDaten`, `KursStatistikGesamt` | `lehrer` entfällt; `idLehrer` kommt hinzu. |
| `FoerderschwerpunktEintrag` | Das Pflichtfeld `kuerzel` entfällt; `bezeichnung` kommt als Pflichtfeld hinzu. |
| `StatistikGesamt` | Das Pflichtfeld `schulkatalog` mit dem neuen Schema `SchulKatalogStatistikGesamt` kommt hinzu. |
| `SchuleStatistikGesamt` | Das Format von `idGebundenerGanztag` ändert sich von `int32` auf `int64`. |
| `Einwilligungsart` | `referenziertInAnderenTabellen` ist nicht mehr als nullable ausgewiesen. |

Neue Eigenschaften betreffen außerdem unter anderem `idBetreuung` bei
Lernabschnittsdaten, `istJva` bei Klassen- und Schülerdaten sowie Angaben zu
Ganztag und Talentschule in `SchuleStammdaten`.

## Kataloge

| Katalog | Änderung |
|---|---|
| `FormGebundenerGanztag` | Neu; vier Einträge. |
| `Talentschule` | Neu; drei Einträge. |
| `HerkunftSchulform` | Version 6 → 7; drei von 20 Einträgen geändert. Bei `BK`, `SB` und `WB` ändern sich die zulässigen Schulformen. |
| `ZulaessigeKursart` | Version 5 → 6; 76 → 77 Einträge. Neu ist `AB5` („5. Abiturfach“), gültig ab 2028. |
| `ValidatorenFehlerartKontext` | 265 → 299 Einträge: 40 hinzugefügt, sechs entfernt und zwei geändert. |

Die Datensätze wurden anhand ihres `bezeichner`- beziehungsweise
`validator`-Werts verglichen. Änderungen an verschachtelten Listen werden
dabei auch dann gezählt, wenn sich deren Reihenfolge ändert.

## Hinweise zur Migration

Entfernte oder geänderte Beschreibungen beweisen allein nicht, ob der Server
alte Aufrufe noch unterstützt oder ob neue Pfade fachlich vollständige
Ersatzlösungen sind. Request- und Response-Formate sowie Fehlerbehandlung
sollten gegen den jeweils aktuellen Entwicklungsstand getestet werden.
Diese Seite wird bei späteren Snapshot-Aktualisierungen erneut gegen 1.5.1
ausgewertet.
