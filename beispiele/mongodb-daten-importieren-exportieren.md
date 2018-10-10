---
description: >-
  Anleitung um die lokale Datenbank zwischen Geräten austauschen um Fehler von
  anderen leichter reproduzieren zu können.
---

# MongoDB Daten importieren/exportieren



{% hint style="info" %}
Für die folgenden Operationen muss die Datenbank gestartet sein.   
`mongod --dbpath "path\to\database"`
{% endhint %}

## Export

```text
mongodump --db=schulcloud --gzip --archive=name.archive
```

## Import

```bash
mongorestore --db=schulcloud --gzip --archive=name.archive
```



