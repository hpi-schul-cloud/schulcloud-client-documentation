# Naming

Die `HPI Schul-Cloud` wird auch unter dem Namen `Niedersächsische Bildungscloud` gehosted. Damit in dieser nicht überall Schul-Cloud steht gibt es ein paar Umgebungsvariablen, in welcher jeweilige Einstellungen gesetzt werden

| ENV-Variable | Default | Handlebars | Controller |
| :--- | :--- | :--- | :--- |
| SC\_TITLE | _HPI Schul-Cloud_ | `{{theme.title}}` | `res.locals.theme.title` |
| SC\_SHORT\_TITLE | _Schul-Cloud_ | `{{theme.short_title}}` | `res.locals.theme.short_title` |



