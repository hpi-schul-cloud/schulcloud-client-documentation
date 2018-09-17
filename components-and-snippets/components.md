# Handlebars Components

## Page Templates

Für das erstellen von Seiten stehen die folgenden Templates zur Verfügung, welche über `extend` erweitert werden können.

```markup
{{#extend "lib/loggedin"}}
    {{#content "page"}}
        <h1>Hi</h1>
    {{/content}}
{{/extend}}
```

{% tabs %}
{% tab title="lib/loggedin" %}
{% hint style="info" %}
Sollte für alle Seiten genutzt werden, welche der Nutzer in eingeloggtem Zustand sehen kann.
{% endhint %}

![views/lib/loggedin.hbs](../.gitbook/assets/loggedin.hbs.png)
{% endtab %}

{% tab title="lib/simple" %}
{% hint style="info" %}
Sollte für alle Seiten genutzt werden, für welche kein Login notwendig ist.
{% endhint %}

![views/lib/simple.hbs](../.gitbook/assets/simple.hbs.png)

### Parameter

| Parameter | Werte \(default\) | Effekt |
| :--- | :--- | :--- |
| hideMenu | true/\(false\) | Beeinflusst die Sichtbarkeit des Menüs oben rechts. |
| hideFooter | true/\(false\) | Blendet den Footer ein/aus |
| extendedFooter | true/\(false\) | Auswahl zwischen simplen und komplexem Footer |

![extendedFooter = false](../.gitbook/assets/simple_footer.hbs.png)

![extendedFooter = true](../.gitbook/assets/extended_footer.hbs.png)
{% endtab %}

{% tab title="lib/default" %}
{% hint style="warning" %}
So selten wie möglich verwenden und nur wenn es nicht anders geht. Dieses Template dient prinzipiell nur als Grundlage für weitere Templates.
{% endhint %}

Dieses Template ist so minimalistisch wie möglich gehalten und bindet lediglich Schul-Cloud spezifische Schriftarten, Meta-Tags und base-styles ein.

![views/lib/default.hbs](../.gitbook/assets/default.hbs.png)
{% endtab %}
{% endtabs %}

Alle Templates beinhalten die Blöcke scripts und styles welche jeweils um die Seitenspezifischen Tags erweitert werden können.

{% tabs %}
{% tab title="Styles" %}
```markup
{{#content "styles" mode="append"}}
    <link rel="stylesheet" href="/styles/authentication/home.css"/>
{{/content}}
```
{% endtab %}

{% tab title="Scripts" %}
{% hint style="warning" %}
Verwende defer an script-Tags um das rendering nicht zu blockieren. [Mehr Infos](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/script#attr-defer).
{% endhint %}

```markup
{{#content "scripts" mode="append"}}
    <script src="/scripts/loggedin.js" type="text/javascript" defer></script>
{{/content}}
```
{% endtab %}
{% endtabs %}

## Komponenten

### Tabellen

{% hint style="info" %}
TODO
{% endhint %}

### Pagination

{% hint style="info" %}
TODO
{% endhint %}

### Modals

{% hint style="info" %}
TODO
{% endhint %}

### MultiPage-Form

{% hint style="info" %}
TODO
{% endhint %}

### Pin Input

```markup
{{#embed "registration/pin" 
    digits=4 
    pattern="[0-9]"
    required="true" 
    name="email-pin" 
    class="mail-validation"
    }}{{/embed}}
```

![](../.gitbook/assets/pin-input.gif)

## Handlebars Helper

[GitHub Source File](https://github.com/schul-cloud/schulcloud-client/blob/master/helpers/handlebars/helpers/index.js)

