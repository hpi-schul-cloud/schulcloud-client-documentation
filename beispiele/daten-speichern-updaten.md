---
description: >-
  Ein beispielhafter Workflow, wie Daten in der Oberfläche eingegeben, über den
  Server in der Datenbank gespeichert werden und anschließend in der Oberfläche
  wieder ausgegeben werden können.
---

# Daten aus Formular speichern

## 0. Überblick

Für dieses Beispiel erstellen wir eine Seite, auf welcher der Nutzer seinen Benutzernamen ändern kann.

{% hint style="info" %}
TODO
{% endhint %}

![TODO: replace image with page mockup](../.gitbook/assets/giphy-1.gif)

## 1. Seite mit Formular erstellen

### 1.1. Controller erstellen \(`/controller/*.js`\)

Wie genau ein Controller erstellt wird ist bereits im Quickstart erklärt. Mache dich bitte zunächst damit vertraut, da wir nun davon ausgehen, bereits eine Controller-Datei vorliegen zu haben.

{% page-ref page="../quickstart/" %}

Wir definieren uns also eine neue Route \(`/myname`\) im Beispielhaften Router `settings.js` welcher alle URL's welche mit `/settings` beginnen beinhaltet.

Möchte der Nutzer diese Adresse \(`GET /settings/myname`\) Aufrufen, so stellt er mit dem Browser eine GET-Anfrage an diese Route. Somit ergibt sich dieser Route:

{% code-tabs %}
{% code-tabs-item title="/controller/settings.js" %}
```javascript
router.get('/myname', function (req, res, next) {
    // TODO
});
```
{% endcode-tabs-item %}
{% endcode-tabs %}

Nun muss im Controller noch angegeben werden, welche Seite beim Aufruf der Route angezeigt werden soll. Dazu wird in der `res.render(...)` Methode ein Template \(`.hbs` Datei\) angegeben. In diesem Beispiel liegt dieses Template unter `/view/settings/name.hbs` . 

{% code-tabs %}
{% code-tabs-item title="/controller/settings.js" %}
```javascript
router.get('/myname', function (req, res, next) {
    res.render('settings/name', {
        title: 'Mein Name'
    });
});
```
{% endcode-tabs-item %}
{% endcode-tabs %}

### 1.2 View \(`*.hbs`\) erstellen

Nun müssen wir noch das entsprechende Template erstellen. Dazu erweitern wir das bestehende `loggedin`-Template. Welche Templates es gibt ist auf folgender Seite dokumentiert:

{% page-ref page="../components-and-snippets/components.md" %}

{% code-tabs %}
{% code-tabs-item title="/view/settings/name.hbs" %}
```markup
{{#extend "lib/loggedin"}}
    {{#content "page"}}
        <form method="post" action="/settings/myname">
            <div class="form-group">
                <label for="username">Benutzername:</label>
                <input id="username" type="text" name="username" required>
            </div>
            <button type="submit" class="btn btn-primary btn-submit">
                Speichern
            </button>
        </form>
    {{/content}}
{{/extend}}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

Mehr Infos, wie Formulare erstellt werden können und was es zu beachten gibt sind hier zu finden:

{% page-ref page="../formulare.md" %}

## 2. Submit Route erstellen

In dem Template haben wir für das Formular als Submit-Route `POST /settings/myname` angegeben. Diese Route müssen wir natürlich noch definieren. 

{% code-tabs %}
{% code-tabs-item title="/controller/settings.js" %}
```javascript
router.post('/myname', function (req, res, next) {
    // TODO
});
```
{% endcode-tabs-item %}
{% endcode-tabs %}

In dieser Route können wir auf die vom Nutzer eingegebenen Formulardaten ganz einfach über req.body zugreifen \(über das `input name`-Attribut\). Der eingegebene Nutzername steht also unter `req.body.username`. Dieser muss jetzt zum speichern an den Server gesendet werden. Wie Anfragen an den Server gestellt werden können ist in folgendem Artikel ausführlich erläutert: 

{% page-ref page="../quickstart/api-anfragen.md" %}

{% hint style="info" %}
Für dieses Beispiel nehmen wir an, dass wir den Nutzernamen eines Nutzers direkt im User-Objekt der Datenbank \(DB\) speichern wollen.
{% endhint %}

Da das Nutzerobjekt, in welches die Daten gespeichert werden sollen bereits existiert müssen wir dieses lediglich updaten und nicht komplett neu erstellen. Es wird daher `patch` genutzt.

{% code-tabs %}
{% code-tabs-item title="/controller/settings.js" %}
```javascript
const api = require('../api');

router.post('/myname', function (req, res, next) {
    // Anfrage an den Service "users" auf den Eintrag des aktuellen Nutzers
    api(req).patch(`/users/${res.locals.currentUser._id}`, {
        json: {
            username: req.body.username
        }
    }}).then((result) => {
        // result contains patched database entry
    });
});
```
{% endcode-tabs-item %}
{% endcode-tabs %}

### 2.1. Server anpassen

Angenommen das Feld `username` ist im Server für das User-Objekt noch nicht definiert, so müssen folgende Anpassungen gemacht werden:

{% hint style="info" %}
TODO
{% endhint %}

## 3. Daten ausgeben



{% hint style="info" %}
TODO
{% endhint %}

