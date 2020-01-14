# Cheatsheet
```$ kill -9 $(lsof -ti tcp:3000)``` sert à kill tous les process du port 3000

## Raccourci pour lancer le serveur
```$ npm start``` ou ```$ npm run dev```

```
// dans package.json
  "scripts": {
    "start": "nodemon app.js",
    "test": "echo \"Error: no test specified\" && exit 1"
  },
```

## Requêtes
*Exemple d'URL : http://localhost:3000/products/1345?show=reviews*
| HTTP Request | Express req object | value |
| --- | --- | --- |
| Method | req.method | GET |
| URL Path | req.path | /products/1345 |
| URL Params | req.params.id | 1345 |
| URL Query String | req.query.show | reviews |
| All headers | req.headers['Accept-Language'] | "en-US,en;q=0.9" |

## Express Generator by IronHack
```
// installation
$ npm install -g ironhack_generator

// generation du projet
$ irongenerate library-project

// lancement
$ npm run dev
```

# Initialisaton
## Installation via console
```$ npm install express```

## Configuration dans app.js
```
// Require Express
const express = require('express');

// Express server handling requests and responses
const app = express();

// Make everything inside of public/ available
app.use(express.static('public'));

// our first Route:
app.get('/', (request, response, next) => {
  response.sendFile(__dirname + '/views/home-page.html');
});

// cat route:
app.get('/cat', (request, response, next) => {
  response.sendFile(__dirname + '/views/cat-page.html');
});

// Server Started
app.listen(3000, () => {
  console.log('My first app listening on port 3000!');
});
```

# Fondamentaux
## Send information
```
// send content (string, boolean, etc.)
response.send(``)

// send file
response.sendFile(__dirname + `/views/homepage.html`)

// render file
response.render(`index.html`);
```

## GET method
### route param
```
// 1 param
app.get('/users/:username', (req, res, next) => {
    console.log(`voici les infos de :` + req.params.username);

    res.send(`user`, {
        who: req.params.username;
    });
})

// 2 params
app.get('/users/:username/books/:bookId', (req, res, next) => {
  res.send(req.params)
})
```

### query string
```/search?city=Paris -> req.query.city```


## POST method
### route param
```
app.post('/login', (req, res) => {
  res.send("You've logged in!");
});
```

### body parser
Module permettant de voir le contenu du body du POST

```
$ npm install body-parser
```

```
// dans app.js
const bodyParser = require('body-parser');

app.use(bodyParser.urlencoded({ extended: true }));

// nouvelle route
app.post('/login', (req, res) => {
  // What ES6 feature could we use to clean these two lines up?
  let email    = req.body.email;
  let password = req.body.password;
  
  if (/* fill in this condition*/){
    // render "Welcome"
  } else {
    // render go away
  }

});
```

## Router
Le router sert à **structurer et répartir le trafic**. Les fichiers js sont ajoutés dans le dossier /routes

```
// dans app.js

const indexRouter = require('./routes/index.js');
app.use('/', indexRouter);

const celebritiesRouter = require('./routes/celebrities.js');
app.use('/celebrities', celebritiesRouter);
```

```
// dans routes/celebrities.js

const express = require('express');
const router  = express.Router();

router.get('/', function () {})
router.get('/add', function () {}) // /celebrities/add
router.get('/edit', function () {}) // /celebrities/edit

module.exports = router;
```

## Seed the DB
```
// dans bin/seeds.js

const mongoose = require('mongoose');
const Book = require('../models/book'); // we call our Book model from here

const dbName = 'library-project';
mongoose.connect(`mongodb://localhost/${dbName}`);

// le tableau des datas
// ...


// ajout des livres dans MongoDB

Book.create(books, (err) => {
  if (err) { throw(err) }
  console.log(`Created ${books.length} books`);
  
  // Once created, close the DB connection
  mongoose.connection.close();
});
```

Si le seed se fait avec des **Relations**
```
const books = [
  {
    title: "The Hunger Games",
    description: "Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.",
    author: "Suzanne Collins",
    rating: 10
  },
  {
    title: "Harry Potter",
    description: "Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.",
    author: "J.K. Rowling ",
    rating: 9
  }
]

//
// 1. Create authors (before)
//

// extract an array of authors from books
const authors = books.map(book => book.author);

Author.create(authors).then(authors => {
  console.log(`${authors.length} authors created.`);

  //
  // 2. create books (after with author's id)
  //

  const authorsIds = authors.map(author => author.id);
  const books = data.map((book, i) => {
    book.author = authorsIds[i]; // update with author id
    return book;
  });

  Book.create(books).then(books => {
    console.log(`${books.length} books created.`);

    mongoose.connection.close();
  }).catch(err => console.error(err));
}).catch(err => console.error(err));
```


## Utiliser des Models
```
// dans routes/index.js
const Book = require('../models/book.js');


router.get('/books', (req, res, next) => {
  Book.find() // 👈
    .then(allTheBooksFromDB => {
      res.render('books', { books: allTheBooksFromDB }); 
    })
    .catch(error => {
      console.log('Error while getting the books from the DB: ', error);
    })
});
```

```
// dans views/books.hbs
<h1>Books</h1>
{{#each books}}
    <p>{{this.title}}</p>
{{else}}
    <p>No books available</p>
{{/each}}
```

## Les middlewares
Utiliser ```use``` et ```next()``` permet de vérifier certains éléments avant les Routes
```
app.use("/controle-technique", function (req, res, next) {
  //
  // 1. vérification du niveau d'huile
  //

  next();
}, function (req, res, next) {
  //
  // 2. vérification de la pression des pneus
  //
  
  next();
}, function (req, res, next) {
  //
  // 3. nettoyage du pare-brise
  //
  
  res.send('voiture ok');
});
```

Il est recommandé d'utiliser next() pour partager les erreurs
```
// version 1
next(new Error("marteau cassé"));

// version 2
.catch(err => next(err);
```


## Les redirections
```res.redirect(`/books`);``` permet de rediriger


## Documents Relationships
```
// models/books.js

const bookSchema = new Schema({
 ...
 author: [ { type : Schema.Types.ObjectId, ref: 'Author' } ],  // 👈 référence au Model Author
 ...
});
```

Pour retourner le détail à la place d'un ID, il faut utiliser ```.populate``` après le ```.find()```
```
router.get('/book/:id', (req, res, next) => {
  let bookId = req.params.id;
  if (!/^[0-9a-fA-F]{24}$/.test(bookId)) { 
    return res.status(404).render('not-found');
  }
  
  Book.findOne({'_id': bookId})
    .populate('author') // 👈 populate the author field
    .then(book => {
      if (!book) {
          return res.status(404).render('not-found');
      }
      res.render("book-detail", { book })
    })
    .catch(next)
});
```


## Basic Authorization

```
// installation
$ npm install bcrypt
```

```
// app.js
...

const router = require('./routes/auth');
app.use('/', router);
```

```
// routes/auth.js
const bcrypt = require("bcrypt");
const bcryptSalt = 10;

const salt = bcrypt.genSaltSync(bcryptSalt);
const hashPass = bcrypt.hashSync(password, salt);

router.post("/signup", (req, res, next) => {
  ...

  if (username.length <= 0 || password.length <= 0) {
    res.render("auth/signup", {
      errorMessage: "Indicate a username and a password to sign up"
    });
    return;
  }

  ... 
```


## Basic Authentication & Sessions
```
// installation
$ npm install express-session connect-mongo
```

```
// dans app.js
const session = require("express-session");
const MongoStore = require("connect-mongo")(session); // pour faire persister la session malgré redémarrage serveur

...

app.use(session({
  secret: "basic-auth-secret",
  cookie: { maxAge: 60000 },
  store: new MongoStore({
    mongooseConnection: mongoose.connection,
    ttl: 24 * 60 * 60 // 1 day
  })
}));

// Middleware Setup
...
```

```
// routes/auth.js
router.post("/login", (req, res, next) => {
  const theUsername = req.body.username;
  const thePassword = req.body.password;

  if (theUsername === "" || thePassword === "") {
    res.render("auth/login", {
      errorMessage: "Please enter both, username and password to sign up."
    });
    return;
  }

  User.findOne({ "username": theUsername })
  .then(user => {
      if (!user) {
        res.render("auth/login", {
          errorMessage: "The username doesn't exist."
        });
        return; // STOP
      }
      if (bcrypt.compareSync(thePassword, user.password)) {
        req.session.currentUser = user; // Save the user in the session & create cookie
        res.redirect("/");
      } else {
        res.render("auth/login", {
          errorMessage: "Incorrect password"
        });
      }
  })
  .catch(err => next(err))
});
```

```
// routes/index.js

function ensureIsLogged(req, res, next) { // fonction Middleware vérifiant que l'user est connecté
  if (req.session.currentUser) {
    next();
  } else {
    res.redirect("/login");
  }
}

router.get("/secret", ensureIsLogged, (req, res, next) => {
  res.render("secret");
});

...
```

```
// routes/auth.js

router.get("/logout", (req, res, next) => { // détruit le cookie
  req.session.destroy((err) => {
    // cannot access session here
    res.redirect("/");
  });
});
```


## Authentication with Passport

```
// installer les modules nécessaires
$ npm install passport passport-local express-session connect-mongo
```

``` javascript
// dans app.js

...

const session    = require("express-session");
const MongoStore = require('connect-mongo')(session);

const bcrypt = require("bcrypt");
const passport = require("passport");
const LocalStrategy = require("passport-local").Strategy;

...

// pour configurer les sessions
app.use(session({
  secret: "our-passport-local-strategy-app",
  store: new MongoStore( { mongooseConnection: mongoose.connection }),
  resave: true,
  saveUninitialized: true,
}));

...

// pour configurer Passport
app.use(passport.initialize());
app.use(passport.session());

// pour Passport, à mettre avant les routes
passport.serializeUser((user, cb) => {
  cb(null, user._id);
});
passport.deserializeUser((id, cb) => {
  User.findById(id)
    .then(user => cb(null, user))
    .catch(err => cb(err))
  ;
});

// pour définir la "stratégie" (= méthode de connexion)
passport.use(new LocalStrategy(
  {passReqToCallback: true},
  (...args) => {
    const [req,,, done] = args;

    const {username, password} = req.body;

    User.findOne({username})
      .then(user => {
        if (!user) {
          return done(null, false, { message: "Incorrect username" });
        }
          
        if (!bcrypt.compareSync(password, user.password)) {
          return done(null, false, { message: "Incorrect password" });
        }
    
        done(null, user);
      })
      .catch(err => done(err))
    ;
  }
));
```

``` javascript
// dans routes/auth-route.js
...

const passport = require("passport");

...

router.get("/login", (req, res, next) => {
  res.render("auth/login");
});

router.post("/login", passport.authenticate("local", {
  successRedirect: "/",
  failureRedirect: "/login"
}));

// alternative au Router ci-dessus si on veut personnaliser le comportement
// router.post('/login', (req, res, next) => {
//   passport.authenticate("local", (err, theUser, failureDetails) => {
//     if (err) {
//       // Something went wrong authenticating user
//       return next(err);
//     }
//  
//     if (!theUser) {
//       // Unauthorized, `failureDetails` contains the error messages from our logic in "LocalStrategy" {message: '…'}.
//       res.render('auth/login', {errorMessage: 'Wrong password or username'}); 
//       return;
//     }
//
//     // save user in session: req.user
//     req.login(theUser, (err) => {
//       if (err) {
//         // Session save went bad
//         return next(err);
//       }
//
//       // All good, we are now logged in and `req.user` is now set
//       res.redirect('/')
//     });
//   })(req, res, next);
// });
```

``` javascript
// routes/auth-routes.js

...

router.get("/private-page", (req, res) => {
  if (!req.user) {
    res.redirect('/login'); // not logged-in
    return;
  }
  
  // ok, req.user is defined
  res.render("private", { user: req.user });
});


// LOGOUT
router.get("/logout", (req, res) => {
  req.logout();
  res.redirect("/login");
});
```

### Social oauth
Pour trouver les Stratégies : http://www.passportjs.org/packages/

```
// dans models/user.js
const userSchema = new Schema(
  {
    username: String,
    password: String,
    slackID: String, // 👈
    googleID: String // 👈
  },
  {
    timestamps: true
  }
);
```

```
// dans app.js

const SlackStrategy = require("passport-slack").Strategy;

...

passport.use(
  new SlackStrategy(
    {
      clientID: process.env.SLACK_CLIENT_ID,
      clientSecret: process.env.SLACK_CLIENT_SECRET,
      callbackURL: "/auth/slack/callback"
    },
...
```

```
// routes/auth-routes.js

...

router.get("/auth/google", passport.authenticate("google", {
  scope: [
    "https://www.googleapis.com/auth/userinfo.profile",
    "https://www.googleapis.com/auth/userinfo.email"
  ]
}));
```
***Source** : https://hackmd.io/@abernier/SypU_0eAH*

***Exemples** :*
* https://github.com/bradtraversy/node_passport_login/blob/master/routes/users.js
* https://github.com/eXtremeXR/APIAuthenticationWithNode/blob/master/server/passport.js


## Flash message

```
// installation
$ npm install connect-flash
```

``` javascript
// app.js

...

const flash = require("connect-flash");

...

app.use(flash());

...
```

``` javascript
// routes/auth-route.js

...

router.post("/login", passport.authenticate("local", {
  successRedirect: "/",
  failureRedirect: "/login",
  failureFlash: true // 👈
}));

...

router.get("/login", (req, res, next) => {
  res.render("auth/login", { "errorMessage": req.flash("error") });
  //                                                                              👆
});
```


## Ajax avec Axios

### Principales méthodes :
* axios.request(config)
* axios.get(url[, config])
* axios.post(url[, data[, config]])

### Installation
```
$ npm install axios
```
ou
```
<script src="https://unpkg.com/axios/dist/axios.min.js"></script>
```

### Exemple d'utilisation
```
const restCountriesApi = axios.create({
  baseURL: "https://restcountries.eu/rest/v2/name/"
});

function getCountryInfo(theName) {
  restCountriesApi
    .get(theName)
    .then(responseFromAPI => console.log("Response from API is: ", responseFromAPI.data))
    .catch(err => console.log("Error is: ", err));
}

document.getElementById("theButton").onclick = function() {
  const country = document.getElementById("theInput").value;
  getCountryInfo(country);
};
```

**responseType** : définit le type de données que va retourner le serveur : arraybuffer, blob, document, json, text, stream

***Documentation :** https://github.com/axios/axios#request-config*


## Uploader une image avec Multer & Cloudinary
```
$ npm install --save cloudinary multer-storage-cloudinary multer
```

```
// config/cloudinary.js

const cloudinary = require('cloudinary');
const cloudinaryStorage = require('multer-storage-cloudinary');
const multer = require('multer');

cloudinary.config({
  cloud_name: process.env.CLOUDINARY_NAME,
  api_key: process.env.CLOUDINARY_KEY,
  api_secret: process.env.CLOUDINARY_SECRET
});

var storage = cloudinaryStorage({
  cloudinary: cloudinary,
  folder: 'folder-name', // The name of the folder in cloudinary
  allowedFormats: ['jpg', 'png'],
  filename: function (req, file, cb) {
    cb(null, file.originalname); // The file on cloudinary would have the same name as the original file name
  }
});

const uploadCloud = multer({ storage: storage });

module.exports = uploadCloud;
```

```
// routes/index.js
const uploadCloud = require('../config/cloudinary.js');
...

router.post('/movie/add', uploadCloud.single('nameOfTheInputOfTheForm'), (req, res, next) => {
  const { title, description } = req.body;
  const imgPath = req.file.url;
  const imgName = req.file.originalname;
  
  const newMovie = new Movie({title, description, imgPath, imgName});
  
  newMovie.save()
    .then(movie => {
      res.redirect('/');
    })
    .catch(error => {
      next(error);
    })
  ;
});
```

Documentation : https://www.npmjs.com/package/multer-cloudinary


## dot env
```
$ npm install --save dotenv
```

```
// tout en haut de app.js
require("dotenv").config();

...

mongoose.connect(process.env.MONGODB_URI);
```


## Deploy sur Heroku
Installer en pré-requis Heroku Command Line Interface : https://devcenter.heroku.com/articles/heroku-cli#download-and-install

```
$ heroku login
```

```
// configuration
$ git init
$ heroku git:remote -a nomDuProjetSurHeroku
$ git remote -v
```

```
// push
$ git push heroku master
```

```
// ouvrir l'app
$ heroku open
```

```
// installer l'addon MongoDB : MongoLab
$ heroku addons:create mongolab:sandbox
```

```
// ouvrir MongoLab
$ heroku addons:open mongolab
```

```
// pour obtenir l'URL d'accés MongoDB hébergé du .env de chez Heroku
$ heroku config:get MONGODB_URI
```


## Express controllers / routes convention
| HTTP Verb | Path  | Controller#Action  | Used for |
| - | - | - | - |
| GET  | /photos  | photos#index  | display a list of all photos |
| GET  | /photos/new  | photos#new  | return an HTML form for creating a new photo |
| POST  | /photos  | photos#create  | create a new photo |
| GET  | /photos/:id  | photos#show  | display a specific photo |
| GET  | /photos/:id/edit | 	photos#edit  | return an HTML form for editing a photo |
| PATCH/PUT  | /photos/:id  | photos#update  | update a specific photo |
| DELETE  | /photos/:id  | photos#destroy  | delete a specific photo |

```
// app.js

const express = require('express')
const app = express()

app.use('/photos', require ('./routes/photos.js'))

app.listen(3000)
```

```
// controllers/photos.js
exports.index = (req, res, next) => {}

exports.new = (req, res, next) => {}
exports.create = (req, res, next) => {}

exports.show = (req, res, next) => {}

exports.edit = (req, res, next) => {}
exports.update = (req, res, next) => {}

exports.destroy = (req, res, next) => {}
```

```
// routes/photos.js
const express = require('express')
const router = express.Router()

const {
  index,
  new:neww, create, // renaming to neww since new is reserved var
  show,
  edit, update,
  destroy
} = require('../controllers/photos.js');

router.get('/', index);

router.get('/new', neww);
router.post('/', create);

router.get('/:id', show);

router.get('/:id/edit', edit);
router.put('/:id', update);

router.delete('/:id', destroy);

module.exports = router
```

Module npm à installer pour PUT & DELETE : https://github.com/expressjs/method-override/blob/master/README.md


## Handling errors
Référence : https://developer.github.com/v3/#client-errors