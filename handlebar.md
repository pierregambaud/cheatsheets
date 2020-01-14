# Initialisation
## Installation via console
```$ npm install hbs```

## Configuration dans app.js
```
app.set('views', __dirname + '/views');
app.set('view engine', 'hbs');
```

## Redémarrer le serveur automatiquement
```$ npx nodemon -e js,hbs app.js```

# Fondamentaux
## Créer une route en utilisant le template
```
// route simple
app.get(`/`, function (request, response, next) {
    response.render(`index`);
});

// route avec variable en dure transmise
app.get(`/`, function (request, response, next) {
    response.render(`index`, {
        h1: `Homepage`
    });
});
```

## Variables
```
// dans un fichier .hbs
<p>J'habite au {{address.numero}} {{address.street}}</p>
```

## Intégration

### Layout
Créer un fichier **layout.hbs** dans **views/** et hbs fera automatiquement le lien

```
// dans route/index.js
app.get('/teams', (req, res, next) => {
  let data = {
    layout: 'layout2'
  }
  res.render('teams', data);
});
```

### Partials
Créer un dossier **partials** dans **/views**

```
// dans app.js
hbs.registerPartials(__dirname + '/views/partials');
```

```
// dans le fichier .hbs
{{> playerCard}}
```

## Helpers

### if
```
{{#if lastname}}
  <h2>{{lastname}}</h2>
{{else}}
  oops
{{/if}}
```

### unless (opposite if)
```
{{#unless lastname}}
  <h2>Bonjour inconnu</h2>
{{/unless}}
```

### each (for table)
```
<ul>
    {{#each cities}}
       <li>{{@index}} - {{this}}</li>
       {{else}}
       <li>A définir</li>
   {{/each}}
</ul>
```

possible de conditionner avec ```{{#if @first}}```

```
app.get('/', (req, res, next) => {
  let data = {
    name: "Ironhacker",
    lastName: "Rocking it!",
    address: "Your heart",
    cities: ["Miami", "Madrid", "Barcelona", "Paris", "México", "Berlín"]
  };
  res.render('index', data);
});
```

### each (for table)
```
{{#each object}}
  {{@key}}: {{this}}
{{/each}}
```
```
<ul>
  {{#each cities}}
    {{#if @first}}
      <li><b>{{this}}</b></li>
    {{else if @last}}
      <li><i>{{this}}</i></li>
    {{else}}
      <li>{{this}}</li>
    {{/if}}
  {{/each}}
</ul>
```

### with
```
{{#with address}}
  <p>J'habite au {{numero}} {{street}}</p>
{{/with}}
```
au lieu de 
```
<p>J'habite au {{address.numero}} {{address.street}}</p>
```

## Commentaires
```{{!-- ceci est un commentaire --}}```