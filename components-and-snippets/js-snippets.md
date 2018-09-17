# JS Snippets

## How-To

Worauf muss ich achten, wenn ich eigene Snippets erstellen möchte?

{% hint style="info" %}
TODO
{% endhint %}

## Snippets

### \(soft\) AJAX-Navigation

{% hint style="info" %}
TODO
{% endhint %}

### Linking Inputs

Du kannst Textfelder automatisch mit Texten aus Inputfeldern füllen lassen. Füge dazu einfach die `class="linked"` zu der Quelle hinzu und bei den jeweiligen Zielobjekten \(beliebig viele\) den entsprechenden Namen \(`data-from="[name]"`\) des input-Felds.

{% code-tabs %}
{% code-tabs-item title="script.js" %}
```javascript
import 'static/scripts/helpers/inputLinking';
```
{% endcode-tabs-item %}
{% endcode-tabs %}

{% code-tabs %}
{% code-tabs-item title="html" %}
```markup
<input class="linked" name="firstname" type="text" placeholder="Vorname"/>
<p data-from="firstname"></p>
```
{% endcode-tabs-item %}
{% endcode-tabs %}

![](../.gitbook/assets/giphy-1.gif)

### Konfetti Mode

{% code-tabs %}
{% code-tabs-item title="script.js" %}
```javascript
import 'static/scripts/confetti/confetti.js';
```
{% endcode-tabs-item %}
{% endcode-tabs %}

{% code-tabs %}
{% code-tabs-item title="html" %}
```markup
<div class="confetti-wrapper">
    ...
</div>
```
{% endcode-tabs-item %}
{% endcode-tabs %}

![.../firstLogin/existingGeb14](../.gitbook/assets/confetti.gif)

