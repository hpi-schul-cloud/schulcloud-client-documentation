---
description: >-
  Wie können Daten aus der Datenbank über den schulcloud-server zugegriffen
  werden?
---

# API anfragen

## Overview

Damit Anfragen gestellt werden können muss die Datei `api.js` in den entsprechenden Controller importiert werden:

```javascript
const api = require('../api');
```

Querys werden in der Schul-Cloud mit Feathers ausgeführt.

Alle Datenbankabfragen returnen einen Promise welcher mit dem Result resolved. Dieses Result kann entweder mit [`.then()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Using_promises) oder mit [`await`](https://developer.mozilla.org/de/docs/Web/JavaScript/Reference/Operators/await) genutzt werden.

### Error Handling

{% code-tabs %}
{% code-tabs-item title="async/await" %}
```javascript
try{
    const result
 = await api(req).get('/[service]/[id]');
    // ...
} catch (error) {
    console.error(error);
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

{% code-tabs %}
{% code-tabs-item title="Promise.then\(\)" %}
```javascript
api(req).get('/[service]/[id]') // Anfrage
.then((result) => {
    // ...
})
.catch((error) => {
    console.error(error);
})
```
{% endcode-tabs-item %}
{% endcode-tabs %}

### Mehrere parallele Anfragen

Manchmal benötigt man Daten aus vielen verschiedenen Services. In diesem Fall bietet sich [`Promise.all()`](https://developer.mozilla.org/de/docs/Web/JavaScript/Reference/Global_Objects/Promise/all) an.

{% hint style="danger" %}
TODO - Beispiel hinzufügen
{% endhint %}

### Referenzierte Daten

Als Referenzierte Daten werden Daten bezeichnet, für die nur eine id eines anderen Datensatzes in einem Datensatz gespeichert wurde.

#### Beispiel

| document service | user service |
| :--- | :--- |
| `{_id: 'def'creator: 'abc'title: }` | `{  _id: 'abc'  name: 'firstName'}` |

Beim Laden eines Dokumentes vom document Service wird als ersteller nur die ID eines Nutzers angegeben, nicht die Nutzerdaten selbst. Diese Daten sind somit nur im Dokument _referenziert_.

#### Zugriff

Für den Zugriff auf referenzierte Daten kann das Attribut `$populate: ['attribute 1', ...]` zum Query hinzugefügt werden. Dies sorgt dafür, dass der Server direkt einen Lookup auf die referenzierten Daten macht und diese in die Antwort einfügt. Dies bietet sich beispielsweise beim Lesen eines Kurses an, wenn man die Namen der zugehörigen userIds herausfinden möchte.

{% hint style="warning" %}
Daten welche mit `$populate` aufgelöst wurden können nicht gefiltert werden.
{% endhint %}

```text
api(req).get('/courses/' + req.params.courseId, {
    qs: {
        $populate: ['userIds']
    }
})
```

## GET

Um einen Eintrag \(mit bekannter id\) von einem Service des Servers im Controller auszulesen genügt folgender Code:

```javascript
function async getSomething(service, id){
    const result = await api(req).get(`/${service}/${id}`);
    // ...
}

getSomething("link", "A3bc6");
```

`result` enthält dabei direkt das entsprechende JSON Datenobjekt.

## FIND \(Query\)

Um Einträge zu suchen, bzw. mehrere Datensätze eines Services zu erhalten kann ein Feathers Query gesendet werden. Welche Filtermöglichkeiten es gibt ist [detailliert hier nachzulesen](https://docs.feathersjs.com/api/databases/querying.html).

```javascript
function async findSomething(service, query){
    const result = await api(req).get(`/${service}/`, {
        qs: query
    });
    // ...
}

const query = {
    $limit: 10,
    $sort: {
      createdAt: -1
    }
};
findSomething("link", query);
```

Die Antwort des Servers enthält neben den Datensätzen noch weitere Metadaten.

```javascript
{ 
  total: 11,
  limit: 10,
  skip: 0,
  data: [ 
    { _id: '5b5ff94935e4363764e19cd6',
       updatedAt: '2018-08-11T18:28:54.366Z',
       ...
     },
     ...
  ]
}
```

| Attribut | Beschreibung |
| :--- | :--- |
| total | Anzahl an gefundenen Einträgen |
| limit | Anzahl an ausgegebenen Einträgen \(wie SQL\) |
| skip | Wie viele Einträge wurden übersprungen \([SQL `offset`](https://www.petefreitag.com/item/451.cfm)\) |

## CREATE

```javascript
function createSomething(service, data){
    api(req).post(`/${service}/`, {
        json: data
    }).then(result => {
        // result contains created database entry
    }).catch(err => {
        next(err);
    });
}

const data = {
    target: 'https://schul-cloud.org'
}
createSomething('link', data);
```

{% hint style="info" %}
Wenn ein HTML-Formular abgesendet wurde kann über`req.body` auf die Formulardaten im  JSON-Format zugrgriffen werden.
{% endhint %}

## UPDATE

{% hint style="warning" %}
Update ersetzt den gesamten Datensatz! Wenn du nur einzelne Attribute verändern möchtest, verwende PATCH.
{% endhint %}

## PATCH

{% hint style="warning" %}
Patch verändert nur die Attribute, welche gesendet werden. Alle anderen bleiben unverändert! Das löschen von Attributen muss daher explizit spezifiziert werden. [Read more](https://docs.mongodb.com/manual/reference/operator/update/unset/)
{% endhint %}

```javascript
function patchSomething(service, id, newData){
    api(req).patch(`/${service}/${id}`, {
        json: newData
    }}).then((result) => {
        // result contains patched database entry
    });
}

const newData = {
    target: 'https://schul-cloud.org'
}
patchSomething('link', 'abc', newData);
```

### Arrays manipulieren

Patch ersetzt normalerweise den gesamten, spezifizierten Eintrag. Wenn nun allerdings in der Datenbank ein Array gespeichert ist und man zu diesem nur einen Wert hinzufügen/entfernen möchte gibt es einige besondere Methoden

Eine ausführliche Dokumentation über alle Array Update Operatoren ist hier zu finden: [https://docs.mongodb.com/manual/reference/operator/update-array/](https://docs.mongodb.com/manual/reference/operator/update-array/)



#### Wert hinzufügen

Die Folgende Anfrage fügt den Wert `newId` zu dem Array `teacherIds` in der Datenbank hinzu.

```javascript
api(req).patch('/classes/[id]', { 
    json: { 
        $push: { 
            teacherIds: 'newId' 
        }
    }
});
```

#### Wert entfernen

Die Folgende Anfrage entfernt den Wert `newId` aus dem Array `teacherIds` in der Datenbank.

```javascript
api(req).patch('/classes/[id]', { 
    json: { 
        $pull: { 
            teacherIds: 'newId' 
        }
    }
});
```

## REMOVE

```javascript
function removeSomething(service, id){
    api(req).delete(`/${service}/${id}`)
    .then( () => {
        // ...
    });
}

removeSomething('link', 'abc');
```

