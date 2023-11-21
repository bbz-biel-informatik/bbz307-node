# Login

## Formular

Füge in der `index.js`-Datei eine URL für dein Loginformular ein:

```js
app.get("/login", (req, res) => {
  res.render("login");
});
```

Erstelle dann das Formular:

```html
Login
<form action="/login" enctype="multipart/form-data" method="post">
  Benutzername: <input type="text" name="name"><br>
  Passwort: <input type="password" name="password"><br>
  <input type="submit">
</form>
```

Wichtig! Die Namen der Formularfelder müssen den Spalten deiner Datenbank entsprechen. Sonst wird der Code nicht funktionieren.

Über die URL `/login` kannst du nun auf das Loginformular zugreifen.

## Logincode

Wenn das Formular abgeschickt wird, müssen wir nun überprüfen, ob die User das richtige Passwort verwendet haben. Du kannst dafür den folgenden Code
in die Datei `index.js` einfügen.

```js
app.post("/login", upload.none(), async (req, res) => {
  const user = await login.loginUser(req);
  if (!user) {
    res.redirect("/login");
    return;
  } else {
    res.redirect("/intern");
    return;
  }
});
```

Mit den beiden Zeilen, die `res.redirect` enthalten, kannst du steuern, wohin deine Besucher/innen geleitet werden sollen,
wenn das Login erfolgreich oder nicht erfolgreich war.

## Login-Check

Du kannst auf deinen internen Seiten nun überprüfen, ob die Besucher eingeloggt sind.

Wenn du beispielsweise eine interne Seite namens `intern.mustache` hast, kannst du in der Datei `index.js` die mit einem Pfeil markierten
Zeilen einfügen, um zu überprüfen, ob die Besucher eingeloggt sind. Beachte, dass du unter Umständen auch in der obersten Zeile noch `async` einfügen musst.

```js
app.get("/intern", async (req, res) => {
  const user = await login.loggedInUser(req);       // <--
  if (!user) {                                      // <--
    res.redirect("/login");                         // <--
    return;                                         // <--
  }                                                 // <--
  res.render("intern", { user: user });
});
```

So werden die User, die nicht angemeldet sind, automatisch auf das Login weitergeleitet.

Damit ist das Login abgeschlossen.

## Einträge des eingeloggten Users laden

```js
const user = await login.loggedInUser(req);
if (!user) {
  res.redirect("/login");
  return;
}
const posts = await pool.query("SELECT * FROM posts WHERE user_id = $1", [user.id]);
```

&rarr; [Zurück zur Startseite](README.md)
