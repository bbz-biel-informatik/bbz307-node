# Suche

Um Posts ungefiltert anzuzeigen, habt ihr bereits folgenden Code in der Datei `index.js`: 

```js
app.get("/posts", async (req, res) => {
  const posts = await pool.query("SELECT * FROM posts");
  res.render("posts", { posts: posts.rows });
});
```

Damit ihr die Posts über ein Suchfeld filtern könnt, müsst ihr zuerst in der Handlebars-Datei das Suchformular einbauen:

```html
<form action="/posts" enctype="multipart/form-data" method="get">
  Suchbegriff: <input type="text" name="suche"><br>
  <input type="submit">
</form>
```

Der Code zum Laden muss nun wie folgt angepasst werden:

```js
app.get("/posts", async (req, res) => {
  let posts = await pool.query("SELECT * FROM posts");
  if(req.query.suche) {
    posts = await pool.query("SELECT * FROM posts WHERE title LIKE '%' || $1 || '%'", [req.query.suche]);
  }
  res.render("posts", { posts: posts.rows });
});
```

Wichtig: Das Wort "const" auf der zweiten Zeile des Beispiels muss wie im Beispiel zu "let" abgeändert werden.
