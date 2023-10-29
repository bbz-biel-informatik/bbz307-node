# Formulare (mit Dateiupload)

Um über ein Formular Daten erfassen zu können brauchen wir zuerst eine Seite, in der das Formular angezeigt wird. Das kann entweder in einer bestehenden Mustache-Datei oder in einer neuen sein.
Wir gehen davon aus, dass wir eine neue Datei brauchen.

Registriere zuerst in der `index.js`-Datei eine neue Seite für das Formular:

```js
app.get('/rezept_formular', function(req, res) {
  res.render('rezept_formular');
});
```

Erstelle dann im Ordner `views` die Datei `rezept_formular.mustache` und füge das Formular ein:

```html
<form action="/rezept" enctype="multipart/form-data" method="post">
  Titel: <input type="text" name="titel"><br>
  Anleitung: <input type="text" name="anleitung"><br>
  Foto: <input type="file" name="headerfoto"><br>
  <input type="submit">
</form>
```

Passe die Formularfelder so an, dass die Eingabefelder, insbesondere die `name`-Attribute deiner Datenbankstruktur entsprechen. Das obenstehende Beispiel erfasst den Titel, die Anleitung und das Headerfoto eines Rezeptes. Passe es auf deine Applikation an.

Wenn das Formular abgesendet wird, wird es an die Adresse `/rezept` gesendet. Diese Adresse müssen wir noch registrieren und dann die Formulardaten in die Datenbank speichern.

Füge in der Datei `index.js` den folgenden Code ein:

```js
app.post('/rezept', upload.single('headerfoto'), async function (req, res) {
  await pool.query('INSERT INTO rezepte (titel, anleitung, headerfoto) VALUES ($1, $2, $3)', [req.body.titel, req.body.anleitung, req.file.filename]);
  res.redirect('/');
});
```

Damit werden die Formularfelder `titel` und `anleitung` in die Datenbank gespeichert. Das Headerfoto wird automatisch im Ordner `uploads` gespeichert und nur der Dateiname in der Datenbank gespeichert. Passe auch hier die folgenden Angaben an, damit sie deiner Applikation entsprechen:

* Den Namen des Datei-Formularfeldes (hier: `upload.single('headerfoto')`)
* Den Tabellennamen im SQL-Befehl (hier: `rezepte`)
* Die Spalten, die gefüllt werden sollen (hier: `titel, anleitung, headerfoto`)
* Die Formularfelder, die gespeichert werden sollen (hier: `req.body.titel, req.body.anleitung, req.file.filename`). Beachte, dass der letzte Teil (`req.file.filename`) immer gleich bleibt.
