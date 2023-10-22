# Registrierung

## Formular

Füge in der `index.js`-Datei eine URL für dein Registrierformular ein:

```js
app.get("/register", (req, res) => {
  res.render("register");
});
```

Erstelle dann das Formular:

```html
Registrierung
<form action="/register" enctype="multipart/form-data" method="post">
  Benutzername: <input type="text" name="name"><br>
  Passwort: <input type="password" name="password"><br>
  Avatar: <input type="text" name="avatar"><br>
  <input type="submit">
</form>
```

Wichtig! Die Namen der Formularfelder müssen den Spalten deiner Datenbank entsprechen. Sonst wird der Code nicht funktionieren.

Über die URL `/register` kannst du nun auf das Registrierformular zugreifen.

## Registrierungscode

Wenn das Formular abgeschickt wird, müssen wir nun die User in die Datenbank schreiben. Du kannst dafür den folgenden Code
in die Datei `index.js` einfügen.

```js
app.post("/register", upload.none(), async (req, res) => {
  const user = await login.registerUser(req);
  if (user) {
    res.redirect("/login");
    return;
  } else {
    res.redirect("/register");
    return;
  }
});
```

Mit den beiden Zeilen, die `res.redirect` enthalten, kannst du steuern, wohin deine Besucher/innen geleitet werden sollen,
wenn die Registerierung erfolgreich oder nicht erfolgreich war.

Damit ist die Registrierung abgeschlossen.

&rarr; [Weiter zum Login](login.md)
