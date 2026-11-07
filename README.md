# SVWS-API-Change

Dieses Repository dokumentiert Änderungen an der API und den Katalogen des
SVWS-Servers. Entwicklerinnen und Entwickler können damit nachvollziehen, was
sich zwischen Versionen geändert hat und welche Anpassungen an ihren
Integrationen möglicherweise erforderlich sind.

## Aufbau

- [`openAPI/`](openAPI/) enthält die abgelegten Beschreibungen, jeweils in
  einem Ordner pro Version:
  - `server.json`: OpenAPI-Beschreibung der Server-API.
  - `allinone.json`: zusammengefasste Kataloge.
- [`docs/`](docs/) enthält die Dokumentation der Änderungen für GitHub Pages.
  Dort sind Vergleiche für [1.5.1](docs/1.5.1/index.md) und den
  [Entwicklungsstand für 1.6.1](docs/1.6.1-SNAPSHOT/index.md) zu finden.

Die Dateien in `openAPI/` sind Momentaufnahmen. SNAPSHOT-Versionen bilden den
jeweiligen Entwicklungsstand ab und können sich bis zum Release ändern. Der
Entwicklungsstand für das für Januar 2027 geplante Release 1.6.1 wird
regelmäßig verfolgt. Der Ordner `1.6.1-SNAPSHOT` bezeichnet dabei das
Zielrelease; die aktuell abgelegte Serverbeschreibung meldet intern
`1.6.0-SNAPSHOT`. Ein Ordnername allein ist daher kein Nachweis für die
interne Version der enthaltenen Daten.

## Neue Version dokumentieren

1. API-Beschreibung und Katalogdatei des gewünschten SVWS-Servers abrufen und
   unter `openAPI/<Version>/` als `server.json` und `allinone.json` ablegen.
   Die interne Version der Serverbeschreibung mit der gewünschten Version
   abgleichen.
2. Beide Dateien mit der vorherigen Version vergleichen. Dabei insbesondere
   entfernte und geänderte Operationen, Request- und Response-Schemas sowie
   Katalogänderungen und mögliche Auswirkungen auf Clients berücksichtigen.
3. Unter `docs/<Version>/index.md` eine Seite für den Versionsübergang
   anlegen und sie in [`docs/index.md`](docs/index.md) verlinken. Quelle,
   Vergleichsgrundlage und Grenzen der Analyse angeben.

Bei einer SNAPSHOT-Aktualisierung die Unterschiede erneut gegen das letzte
Release prüfen und die zugehörige Dokumentationsseite mit Stand,
Versionsangabe und Prüfsummen der verglichenen Dateien aktualisieren. Die
Git-Historie kann frühere dokumentierte Stände nachvollziehbar machen,
sobald die Dateien eingecheckt sind.

Eine lokale `.env` mit Serveradressen wird nicht eingecheckt. Der Vergleich
und die Dokumentation erfolgen derzeit nicht automatisch.

## GitHub Pages

Die Dokumentation ist für GitHub Pages mit Jekyll vorbereitet. In den
Repository-Einstellungen unter **Settings → Pages** als Quelle
**Deploy from a branch** und den gewünschten Branch mit dem Ordner `/docs`
auswählen. Nach der Veröffentlichung ist die Seite über die in den
Pages-Einstellungen angezeigte Adresse erreichbar.