---
description: Was liegt wo und gehört in welchen Ordner?
---

# Ordnerstruktur

| Datei | Funktion/Beschreibung |
| :--- | :--- |
| gulpfile.js | Gulp kompiliert die Dateien aus dem Ordner /static in den Ordner /build. Mehr Details. |
| package.json | [Dokumentation](https://docs.npmjs.com/files/package.json) |
| diff.sh | Die Nightwatch Frontend Tests benötigen bei travis sehr lange zur Ausführung. Die Datei prüft daher, in welchen Bereichen der Schul-Cloud überhaupt Änderungen durchgeführt wurden und sorgt anschließend dafür, dass nur die entsprechenden Tests ausgeführt werden. Dazu werden die entsprechenden Tests in die Datei `frontend_tests.sh` eingetragen. |
|  |  |

## /build

{% hint style="danger" %}
Dateien in diesem Ordner sollten nicht bearbeitet werden!
{% endhint %}

In diesen Ordner kompiliert gulp die Dateien aus dem Ordner `/static` und Express liefert diese von dort an den Client aus.

## /controllers

Controller dienen als Datengrundlage und Router von den Views. Sie stellen Datenbankabfragen und bereiten Daten so vor, dass sie von den Views verwendet werden können. Zugleich werden hier die verschiedenen Routen definiert.

Pro Bereich der Schul-Cloud nutzen wir nur einen Controller.

## /data

## /helpers

## /static

### /fonts

Hier werden \(in einem jeweils eigenem Ordner\) verwendete Schriftarten abgelegt. Versuche stets möglichst viele Formate der Schriftart hier abzulegen, auch wenn wir aktuell lediglich dass `.woff` Format verwenden, welches [mit dem Großteil aller Browser kompatibel](https://caniuse.com/#search=woff) ist und eine ausreichende Komprimierung bietet. Falls wir dieses vorgehen allerdings einmal ändern ist es gut, bereits alle Formate hier vorliegen zu haben.

Füge bitte wenn möglich auch eine css Datei hinzu, welche die Schriftart bereitstellt.

```text
// Beispiel FontAwesome
@font-face {
	font-family: 'FontAwesome';
	src: local('fontawesome'),
		url('fontawesome-webfont.woff') format('woff');
	font-weight: normal;
	font-style: normal;
}
```

### /images

### /other

#### /pdf

### /scripts

### /styles

Die Style-Dateien \([scss](https://sass-lang.com)\) nutzen einen Ordner pro Controller. 

### /vendor

Als vendor-Dateien bezeichnen wir alle externen Tools, Polyfills und Bibliotheken welche unverändert eingebunden werden können.

## /test

Der Sinn dieses Ordners erschließt sich hoffentlich von selbst. Für Frontend Tests verwenden wir [Mocha](https://mochajs.org/) und [Nightwatch](http://nightwatchjs.org/). Mehr Details findest du im Abschnitt [Testing](testing.md).

## /views

Hier liegen die Views der Schul-Cloud. Also das eigentliche HTML. Als Template-Engine verwenden wir [Handlebars](https://handlebarsjs.com/). Erweiterungen von Handlebars \(Handlebars Helpter\) sind in der Datei`/helpers/handlebars/helpers/index.js` implementiert und können auch dort ergänzt werden.

