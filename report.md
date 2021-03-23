# Mini Challenge 4: Report

**wdb @ FHNW BSc Data Science** \
**Autor: Lukas Reber**

## Umsetzung

Für die Mini Challenge 4 der Kompetenz Web Datenbeschaffung (wdb) wurde ein Webscraper mittels [Selenium](https://www.selenium.dev/) umgesetzt. Hierfür wurde ein Script geschrieben, welches Inhalt der Webseite tutti.ch herunterlädt. Tutti.ch ist die grösste Kleinanzeigenplattform der Schweiz.

Um Inserate auf Tutti zu scrapen, kann mittels Parameter dem Script ein Suchbegriff mitgegeben werden, anschliessend wird tutti.ch aufgerufen und sämtliche für den Suchbegriff angezeigten Inserate heruntergeladen. Die folgenden Attribute werden gespeichert:

- ID des Inserats
- Titel
- Beschreibung
- Anbieter/in
- Postleitzahl des Anbieters / der Anbieterin
- Datum der Veröffentlichung
- Preis
- Anzahl Besuche

Für das Script wurden zwei Funktionen geschrieben. Die erste Funktion speichert sämtliche URLs der in der Suchanfrage zurückgegebenen Inserate. Die zweite Funktion ruft sämtliche URLs auf und speichert den angegeben Inhalt in ein Dateframe. Zudem wird der Inhalt an eine Datenbank übermittelt, mittels der in Mini Challenge 3 umgesetzten GraphQL API.

Folgenden Aspekte der Umsetzung sind besonders zu bemerken:

- Tutti verwendet in ihrem HTML Code nur verzeinzelt Divs mit fix definierten Namen oder IDs. So mussten in vielen Fällen mit absoluten XPATH Variablen gearbeitet werden, was das Scraping sehr anfällig auf Änderungen an der Website macht.
- Es gibt verschiedene Ausnahmen bei der Darstellung eines Inserats zu beachteten: Es gibt sowohl Tutti eigene wie auch Inserate anderer Websites (z.B. Ricardo angebote dargestellt als Tutti Inserat). Oder die Darstellung des Anbieternahmens, welcher je nach dem ob ein User ein "Pro" User ist oder nicht, leicht anders ist. Diese Punkte sind besonders mit den erwähnen absoluten XPATH Strings besonders fehleranfällig.
- Für Übermittlung der Daten mittels API mussten die Sonderzeichen aus Titel und Beschreibung des Inserats gefiltert werden, da dies ansonsten zu einem korrupten JSON Syntax führte.

## Data Science Aspekte

Da tutti.ch keine öffentlich zugängliche API anbietet besteht keine einfache Möglichkeit eine grössere Anzahl von Inseraten systematisch auszulesen und weiterzuverarbeiten. Durch das Webscraping, resp. die Ablage der Inserate in einer Datenbank eröffnen sich vielseitige Möglichkeiten:

- Die Daten können nach belieben durchsucht und aggregiert werden, ohne Limitation (Filtermöglichkeiten) der implementierten Suche auf tutti.ch.
- Inserate können historisiert werden um z.B. Preisanpassungen von einzelnen Inserate zu erkennen oder die Preisentwicklung einer bestimmten Artikelkategorie zu untersuchen.
- Zudem erlaubt das Speicher der Daten eine genaue Analyse von Angeboten z.B. aufgrund ihrer geographischen Lage (z.B. könnte eine Funktion "Angebote near me" erstellt werden, welche es aktuell auf Tutti nicht gibt)
- Erkennen von besonders günstigen Angeboten aufgrund Vergleiche mit vergangener Inseraten

Da die erwähnten Möglichkeiten nicht direkt Teil des Scrapings sind, wurde darauf verzichtet diese bereits umzusetzen.

## Weiterentwicklung

Um die oben genannten Use Cases umzusetzen kann auf dem bestehende Scraping Script aufgebaut werden. Sobald sich die Daten in der Datenbank befinden können diese nach belieben weiterverarbeitet werden. Sollten weitere Attribute eines Inserats benötigt werden, können die bestehenden Funktionen ohne grossen Aufwand erweitert werden. Ein Problem könnte sein, dass bei zu vielen Requests auf die Website dies als Angriff gewertet wird und der Zugang deshalb blockiert wird. Ob solch ein Limit besteht, wurde jedoch nicht näher untersucht.
