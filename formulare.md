---
description: >-
  Erläuterungen zu wichtigen Infos, wie wir Formulare aus Eingabefeldern
  konstruieren und welche Besonderheiten dabei zu beachten sind.
---

# Formulare

## `<input>`

Every `input`-Element needs a **connected** label!  
You can connect the label with 2 methods

```markup
// a) wrap the input inside the label
<label>
    cool description
    <input ...>
</label>
```

```markup
// b) link with id
<label for="input-1">cool description</label>
<input id="input-1" ...>
```

Damit die Eingabefelder sich in das Gesamtdesign der Schul-Cloud einordnen, müssen ihnen noch ein paar Bootstrap-Klassen gegeben werden:

```markup
<div class="form-group">
    <label for="email">E-Mail Adresse:</label>
    <input id="email" class="form-control" type="email" name="email" placeholder="user@schul-cloud.org">
</div>
```

### `<input type="email">`

Die E-Mail Validierung ist leider über verschiedene Browser hinweg inkonsistent, weshalb wir automatisch an alle E-Mail Eingabefelder die [offizielle Firefox Regex](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/input/email) als pattern setzen, welche somit required ist.

{% code-tabs %}
{% code-tabs-item title="/static/scripts/base.js" %}
```javascript
window.addEventListener('DOMContentLoaded', ()=>{
    document.querySelectorAll('input[type="email"]:not([pattern])').forEach((input) => {
        input.setAttribute('pattern', "^[a-zA-Z0-9.!#$%&'*+\/=?^_`{|}~-]+@[a-zA-Z0-9](?:[a-zA-Z0-9-]{0,61}[a-zA-Z0-9])?(?:\.[a-zA-Z0-9](?:[a-zA-Z0-9-]{0,61}[a-zA-Z0-9])?)*$");
    });
});
```
{% endcode-tabs-item %}
{% endcode-tabs %}

### `<input type="date">`

Der HTML-Input-Type `date` wird leider [nicht von allen Browsern unterstützt](https://caniuse.com/#search=type%3D%22date%22). Wir verwenden daher als Polyfill den [jQuery DateTimePicker](https://xdsoft.net/jqplugins/datetimepicker/). Dieser Polyfill  kann entweder manuell per JS initialisiert werden oder wie unten gezeigt durch den import der Datei `/scripts/jquery/datetimepicker-easy.js`. Diese bietet direkt ein paar Hilfsfunktionen, durch welche der Datetimepicker direkt im HTML konfiguriert werden kann und das native input-Event bei Änderungen getriggert wird.

```markup
<script src="/scripts/jquery/datetimepicker-easy.js" type="text/javascript" defer></script>
<link rel="stylesheet" href="/styles/lib/datetimepicker/jquery.datetimepicker.min.css"/>

<div class="form-group">
    <label for="birthday">Geburtsdatum</label>
    <input id="birthday" class="form-control" name="birthday" type="text" data-date data-min-date="1990/01/01" data-max-date="2015/01/01" data-start-date="2000/01/01" autocomplete="off" value="{{dateToPicker user.birthday}}"/>
</div>
```

| Attribut | Effekt |
| :--- | :--- |
| data-date, data-datetime | Je nachdem welches gewählt wird wird die Zeitauswahl angezeigt oder nicht |
| start-date | Initial angezeigtes Datum beim öffnen des Pickers |
| min-date | Minimal auswählbares Datum |
| max-date | Maximal auswählbares Datum |

#### Manuelle Initialisierung

Bei der manuellen Initialisierung entfallen logischerweise zunächst all die erwähnten Hilfsfunktionen.

```javascript
import 'jquery-datetimepicker';

$(document).ready(function () {
    $.datetimepicker.setLocale('de');
    $('input[data-datetime]').datetimepicker();
});
```

Mehr Details sind in der offiziellen Dokumentation zu finden: [https://xdsoft.net/jqplugins/datetimepicker/](https://xdsoft.net/jqplugins/datetimepicker/)

## `<select>...</select>`

Für Auswahlfelder \(`<select>...</select>`\) verwenden wir die [jQuery Bibliothek chosen](https://harvesthq.github.io/chosen/). Standardmäßig sehen die Eingabefelder ziemlich schrecklich aus und sind gerade mit dem Attribut `multiple="true"` sehr schlecht in das Design zu integrieren. Chosen löst dieses Problem, bringt jedoch auch ein paar Eigenheiten mit sich.

Grundsätzlich wird ein Auswahlfeld mit Bootstrap und Chosen so erstellt:

```markup
<div class="form-group">
    <label for="gender">Geschlecht: *</label>
    <select id="gender" class="form-control" multiple type="text" name="gender">
        <option value="male">männlich</option>
        <option value="female">weiblich</option>
        <option value="other">anderes</option>
        <option value="noinfo" selected>keine Angabe</option>
    </select>
</div>
```

Mit Javascript wird anschließend noch Chosen für das Eingabefeld aktiviert. Dies ist jedoch bereits in der base.js implementiert und in der all.js \(von gulp erstellt\) wird chosen auch bereits importiert. Der folgende Code wird also stets ausgeführt und wird hier nur der Vollständigkeit halber erklärt.

{% code-tabs %}
{% code-tabs-item title="static/scripts/base.js" %}
```javascript
// Initialize bootstrap-select
$('select:not(.no-bootstrap)').chosen({
    width: "100%",
    "disable_search": true
}).change(function() {
    this.dispatchEvent(new Event('input'));
});
```
{% endcode-tabs-item %}
{% endcode-tabs %}

Mit der Klasse `class="no-bootstrap"` an dem select Feld, kann verhindert werden, dass chosen für das input-Feld verwendet wird. Ebenso kann mit Klasse `class="search-enabled"` die Suche nach Auswahlmöglichkeiten aktiviert werden. Zu beachten ist, dass bei Eingaben mit `multiple="true"` die Suche immer aktiviert ist.

### Manipulation mit Javascript

```javascript
// Werte auslesen (jQuery)
$('#gender').val();
/* results:
    multiple="false" - "male"
    multiple="true"  - ["male"]
*/
```

{% hint style="info" %}
Wichtig ist, dass man mit JS ganz normal das `select` Feld manipulieren kann, jedoch anschließend auf dem `select` Feld unbedingt das Event `chosen:updated` getriggert werden muss, damit Chosen die Änderungen übernimmt und anzeigt.
{% endhint %}

```javascript
// Werte setzen (jQuery)
$('#gender').val("female").trigger("chosen:updated"); // mutliple="false"
$('#gender').val(["male", "female"]).trigger("chosen:updated"); // mutliple="true"
```



