---
layout: default
title: Änderungen in 1.5.1
---

[← Alle Versionsvergleiche](../)

# Änderungen von 1.4.1 auf 1.5.1

## Grundlage und Umfang

Verglichen wurden die Dateien `server.json` und `allinone.json` aus den
Ordnern `openAPI/1.4.1` und `openAPI/1.5.1`. Die Serverbeschreibungen weisen
intern die Versionen `1.4.1` beziehungsweise `1.5.1` aus. Beide verwenden
OpenAPI 3.0.1.

Die folgende Auswertung fasst wichtige Änderungen zusammen; sie ist keine
vollständige Liste aller betroffenen Felder und Endpunkte. Sie beschreibt
die Unterschiede in den JSON-Dateien, nicht das zur Laufzeit überprüfte
Verhalten des Servers.

## Überblick

| Merkmal | 1.4.1 | 1.5.1 | Veränderung |
|---|---:|---:|---|
| API-Pfade | 713 | 784 | +71 |
| HTTP-Operationen | 854 | 921 | 146 hinzugefügt, 79 entfernt |
| Schemas | 431 | 562 | 147 hinzugefügt, 16 entfernt |
| Kataloge | 74 | 74 | einer hinzugefügt, einer entfernt |

Eine Operation wird anhand von HTTP-Methode und vollständigem Pfadtext
verglichen. Auch die Umbenennung eines Pfadparameters zählt deshalb als
entfernte und hinzugefügte Operation. Von den 775 Operationen mit identischer
Methode und identischem Pfad weisen 81 Unterschiede in der Beschreibung auf;
darunter können reine Dokumentationsänderungen sein.

## API-Endpunkte

Alle folgenden Pfade beginnen, sofern nicht anders angegeben, mit
`/db/{schema}`.

### Unterrichtsverteilung

In diesem Bereich sind 77 Operationen hinzugekommen und 30 entfallen.
Betroffen sind unter anderem Planungsabschnitte, Lerngruppen, Lehrkräfte,
Räume, Zeitraster und Import/Export. Zahlreiche einzelne PATCH-Pfade mit
`{id}` sind nicht mehr beschrieben; stattdessen gibt es neue PATCH-Pfade auf
Sammlungsebene. Bei einer Migration müssen daher sowohl Pfade als auch
Request-Bodies geprüft werden.

### GOSt-Klausuren

Hier sind 29 Operationen hinzugekommen und 27 entfallen. Ein Beispiel für eine
Änderung von Pfad und Methode:

| 1.4.1 | 1.5.1 |
|---|---|
| `GET /gost/klausuren/kursklausuren/create/abiturjahrgang/{abiturjahr}/halbjahr/{halbjahr}/quartal/{quartal}` | `POST /gost/klausuren/kursklausuren/erzeugen/abiturjahrgang/{abiturjahr}/halbjahr/{halbjahr}/quartal/{quartal}` |

Weitere Änderungen betreffen Vorgaben, Termine und Raumzuweisungen. Auch
Antworten bestehender Operationen ändern sich: Bei
`DELETE /gost/klausuren/schuelerklausuren/termine/{id}` ist für HTTP 200
statt eines Boolean nun ein `GostSchuelerklausurtermin`-Objekt beschrieben.

### Weitere Bereiche

- **Schulwechsel:** 17 neue Operationen für Abgänge, Dokumente und
  erfolgreiche Bewerbungen, einschließlich Synchronisierung und Vorschau.
- **OAuth:** Die bisherigen `oauth/secrets`-Operationen entfallen; neue
  `oauth/credentials`-Operationen kommen hinzu. Ein entsprechender
  PATCH-Pfad ist in 1.5.1 nicht beschrieben.
- **Jahrgänge:** Vier GET- beziehungsweise DELETE-Operationen unter
  `jahrgaenge` entfallen.
- **Notenmodul:** Aus `GET /enm/v2/lehrer/{id}` wird ein Pfad mit
  zusätzlichem Segment `{funktion}`.
- **Schüler:** `GET /schueler/{id}/abschnitt/{abschnitt}/lernabschnittsdaten`
  entfällt. Neu beschrieben ist unter anderem
  `GET /abschluesse/schueler/{id}/abschnitt/{abschnitt}`. Ob dieser Pfad ein
  vollständiger fachlicher Ersatz ist, muss bei der Migration geprüft werden.
- **Operation IDs:** Bei 22 Operationen mit unveränderter Methode und
  unverändertem Pfad ändert sich die `operationId`. Das kann insbesondere
  neu generierte API-Clients betreffen.

## Datenmodelle

Bei Schemas, die in beiden Versionen vorkommen, werden 33 Eigenschaften
entfernt und 62 hinzugefügt. Beispiele mit möglichen Auswirkungen auf
Clients:

| Schema | Änderung |
|---|---|
| `GostKlausurenVorgabeCreateRequest` | `abiJahrgang` entfällt; das Pflichtfeld `abiturjahrgang` kommt hinzu. |
| `FachklasseEintragCreateRequest` | `bezeichnungWeiblich` und `idSchulgliederung` sind zusätzlich verpflichtend. |
| `ErzieherStammdaten` | `staatsangehoerigkeitID` entfällt; `idStaatsangehoerigkeit` kommt hinzu. |
| `OrtKatalogEintrag` | `kuerzelBundesland` entfällt; `idBundesland` kommt hinzu. |
| `UvLehrer` | `kuerzel` ist verpflichtend und nicht mehr als nullable ausgewiesen. |
| `Logo` | Unter anderem entfallen `mimeType` und die Größenangaben. |

## Kataloge

| Katalog | Änderung |
|---|---|
| `Hochschulabschluss` | Neu hinzugefügt. |
| `HerkunftBildungsgangTyp` | Entfernt. |
| `Fach` | 292 → 304 Einträge; zwölf neue und 20 geänderte Datensätze. |
| `Fachklasse` | 1.227 von 2.175 Datensätzen geändert. Beispielsweise wird bei `ebene1` teilweise ein ausgeschriebener Wert wie `Technik/Naturwissenschaften` statt `TN` verwendet. |
| `HerkunftBildungsgang` | Drei neue Einträge: `G02`, `K02` und `R02`. |
| `LehrerAnrechnungsgrund` | Ein neuer und 72 geänderte Datensätze. |
| `LehrerMehrleistungsarten` / `LehrerMinderleistungsarten` | Sechs beziehungsweise 18 Datensätze geändert, unter anderem bei den zulässigen Schulformen. |
| `Schulgliederung` | Sechs Datensätze geändert. |
| `ValidatorenFehlerartKontext` | 172 → 265 Einträge: 96 hinzugefügt, drei entfernt und neun geändert. |

Die Datensätze wurden anhand ihres `bezeichner`- beziehungsweise
`validator`-Werts verglichen. Änderungen an verschachtelten Listen werden
dabei auch dann gezählt, wenn sich deren Reihenfolge ändert.

## Auffälligkeiten in der OpenAPI-Beschreibung

- Bei `PATCH /unterrichtsverteilung/lerngruppen/lehrer/{id}` fehlt in 1.5.1
  die Definition des im Pfad enthaltenen Parameters `id`.
- Bei `POST /schule/logoverwaltung/logos` ändert sich das dokumentierte
  Request-Schema von einem `Logo`-Objekt zu einem Integer-Array.

Beides sollte gegen die Serverimplementierung geprüft werden. Generell
belegen entfernte oder geänderte Beschreibungen allein nicht, ob der Server
alte Aufrufe weiterhin unterstützt.
