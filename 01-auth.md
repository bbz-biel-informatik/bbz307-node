# Login-System

## Installation

Füge zuerst in der Codesandbox im Menu links unter "Dependencies" die Bibliothek `bbz307` hinzu.

Falls die Codesandbox die Bibliothek nicht findet, kannst du auch am unteren Fensterrand das Panel `Terminal` öffnen, und dort den Text

```bash
npm install bbz307
```

eingeben. Damit wird die Bibliothek auch installiert und sollte unter "Dependencies" erscheinen.

Füge dann in deiner `index.js`-Datei, direkt unter dem Teil mit `const pool = new Pool(...)` die folgende Zeile ein:

```js
const bbz307 = require('bbz307');
const login = new bbz307.Login('users', ['benutzername', 'passwort', 'profilbild'], pool);
```

Der Teil vor dem ersten Komma, `'users'`, sagt dem Server, in welcher Tabelle deine Benutzerdaten gespeichert sind. Passe
ihn auf deine Datenbank an.

Der Teil in den eckigen Klammern, `['benutzername', 'passwort', 'profilbild']`, sagt dem Server, wie die Spalten deiner User-Tabelle
heissen. Passe auch sie an. **Wichtig: der Benutzername muss zuerst kommen, dann das Passwort, dann alle weiteren Spalten.**

Den letzten Teil, `pool`, kannst du so stehen lassen.

Damit ist die Bibliothek installiert und du kannst die eingebauten Funktionen verwenden.

&rarr; [Weiter zur Registrierung](registrierung.md)

&rarr; [Weiter zum Login](login.md)
