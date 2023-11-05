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
<form action="/like/{{post.id}}" method="POST">
  <input type="submit" value="liken">
</form>
```

Wichtig: Der Platzhalter `{{post.id}}` muss angepasst werden, je nachdem, welche Entität auf
der Seite angezeigt wird!

Nun müssen wir noch den Code schreiben, der das Like in der Datenbank speichert. Dafür
ergänzen wir die Datei `app.js` mit den folgenden Zeilen Code:

```js
app.post('/like/:id', async function(req, res) {
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
