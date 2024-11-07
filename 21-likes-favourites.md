# Likes und Favoriten

Um Likes oder Favoriten in der Datenbank zu speichern, brauchen wir normalerweise zwei Informationen:

* die Entität, die gelikt werden soll (z.B. Blogpost oder Rezept)
* die Person, die liket

Als Person können wir jeweils den User verwenden, der eingeloggt ist. Wir müssen also
nur noch die zu likende Entität finden.

Am einfachsten geht das, indem wir den Like-Button auf der Seite einbauen, auf der
unser Blogpost, Rezept usw. angezeit wird. Dafür können wir folgenden Code in die Mustache-
Datei einbauen, in der der Like-Button angezeigt werden soll:

```html
<form action="/like/{{id}}" enctype="multipart/form-data" method="POST">
  <input type="submit" value="liken">
</form>
```

Um anstelle des Knopfs ein Bild anzuzeigen, kannst du den Submit-Knopf mit der folgenden Zeile
ersetzen:

```html
<input type="image" src="/like.png">
```

Nun müssen wir noch den Code schreiben, der das Like in der Datenbank speichert. Dafür
ergänzen wir die Datei `config.js` mit den folgenden Zeilen Code:

```js
app.post('/like/:id', upload.none(), async function(req, res) {
  const user = await login.loggedInUser(req);
  if(!user) {
    res.redirect('/login');
    return;
  }
  await pool.query('INSERT INTO likes (post_id, user_id) VALUES ($1, $2)', [req.params.id, user.id]);
  res.redirect('/');
});
```

Ändere je nach Bedarf den Tabellennamen (`likes`) und die Spaltennamen (`post_id`, `user_id`) ab.

Der Code überprüft zuerst, ob die Person eingeloggt ist, und leitet sie sonst auf die
Login-Seite weiter.

Falls sie eingeloggt ist, wird in der Datenbank ein Like gespeichert mit der ID des
Posts und der ID der eingeloggten Person.

Danach wird die Person auf die Startseite zurückgeleitet. Diese Adresse kannst du natürlich auch
ändern, und die Person z.B. auf die Post-Seite zurückleiten:

```js
res.redirect(`/posts/${req.params.id}`);
```

Wenn du möchtest, dass die Anzahl Likes auf der Detailseite des Posts angezeigt wird, benötigt es im `index.js` eine weitere SQL-Abfrage
in der bestehenden Route für die Detailseite. Dafür definieren wir die Variable `likes`. Ein Beispiel davon könnte folgendermassen aussehen:

```js
  app.get("/events/:id", async function (req, res) {
    const event = await app.locals.pool.query("SELECT * FROM events WHERE id = $1", [
      req.params.id,
    ]);
    const likes = await app.locals.pool.query("SELECT COUNT(user_id) FROM likes WHERE post_id = $1", [
      req.params.id
    ]);
    res.render("details", { event: event.rows[0], likes: likes.rows[0] });
  });
```
Solltest du schon den Code für die Detailseite haben, ergänze deinen Code nur mit den Zeilen für die likes Variable und mit dem Codeteil in der letzten Zeile `, likes: likes.rows[0]`. Ansonsten ändere je nach Bedarf den Tabellennamen und die Spaltennamen ab.

Im HTML-Code kannst du dann die neue likes Variable verwenden, um die gezählten Likes anzuzeigen:

```html
<p>{{likes.count}} Likes</p>
```
