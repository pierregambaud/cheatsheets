# Cheatsheet

| Model method | Description |
| --- | --- |
| create | Create document |
| insertMany | Create many documents |
| find | Finds documents |
| findOne | Finds one document (arg must be an object {"_id": 32} |
| findById | Finds a single document by its _id field |
| updateOne | Updates one document |
| updateMany | Updates many documents |
| findByIdAndUpdate | Updates a single document based on its _id field |
| deleteOne | Deletes one document |
| deleteMany | Deletes many documents |


# Initialisaton
## Installation via console
```
$ npm init --yes // créer le fichier package.json
$ npm install mongoose
```
## Connexion à MongoDB
```
const mongoose = require('mongoose');

mongoose
  .connect('mongodb://localhost/exampleApp', {useNewUrlParser: true, useUnifiedTopology: true})
  .then(x => console.log(`Connected to Mongo! Database name: "${x.connections[0].name}"`))
  .catch(err => console.error('Error connecting to mongo', err));
```


# Fondamentaux
## Définition
Mongoose est une configuration de type *Object Document Mapper* (ODM) à l'opposée du type Object Relation Mapping (ORM). 

## Model
Mongoose introduit la notion de Model : le modèle va décrire ce à quoi nos documents finaux de la base de données ressembleront.

### Create
Equivaut à new + save()

```
User.create({ name: 'Alice', password:"ironhack2018", job: 'Architect' }, function (err, user) {
  if (err) {
      console.log('An error happened:', err);
  } else {
      console.log('The user is saved and its value is: ', user);
  }
});

// The same code as above but with a Promise version
User.create({ name: 'Alice', password:"ironhack2018", job: 'Architect' })
  .then(user => { console.log('The user is saved and its value is: ', user) })
  .catch(err => { console.log('An error happened:', err) });
```

### Read
find all
```
let callback = (err, users) => {});

// Find all users and execute the callback
User.find({}, callback);
```

find // find one // find by id
```
// Promise version
User.find({ name: 'Alice' })
  .then(successCallback)
  .catch(errorCallback);

User.findOne({ name: "Alice" })
  .then(successCallback)
  .catch(errorCallback);

User.findById("someMongoIdGoesHere129")
  .then(successCallback)
  .catch(errorCallback);
```

### update
```
// update for one 
User.update({ query }, { $set : { key: value, key: value }})
  .then()
  .catch()

// For all users that as "@ironhack\.com" in its email, change the company to "Ironhack"
User.updateMany({ email: /@ironhack\.com/}, { company: "Ironhack" })
  .then(successCallback)
  .catch(errorCallback);

// For the first "Alice" found, change the company to "Ironhack"
User.updateOne({ name: "Alice"}, { company: "Ironhack" })
  .then(successCallback)
  .catch(errorCallback);

// For the user with _id "5a3a7ecbc6ca8b9ce68bd41b", increment the salary by 4200
User.findByIdAndUpdate("5a3a7ecbc6ca8b9ce68bd41b", { $inc: {salary: 4200} })
  .then(successCallback)
  .catch(errorCallback);
```

Attention, les données retournées par défaut sont celles **avant l'update**. Il faut utiliser ```{ new: true }``` pour récupérer les nouvelles valeurs.
```
Model.update({ query },
 { 
   $set : { 
    key: value,
    key: value
   }
 },{
  new: true
 })
  .then()
  .catch()
```

### delete
```
// Delete all the users that have "@ironhack.com" in their email
User.deleteMany({ email: /@ironhack\.com/})
  .then(successCallback)
  .catch(errorCallback);

// Delete the first "Alice" found
User.deleteOne({ name: "Alice"})
  .then(successCallback)
  .catch(errorCallback);

// Delete the user with _id "5a3a7ecbc6ca8b9ce68bd41b"
User.findByIdAndRemove("5a3a7ecbc6ca8b9ce68bd41b")
  .then(successCallback)
  .catch(errorCallback);
```

### count
```
User.countDocuments({ type: 'student' })
  .then(count => { console.log(`There are ${count} students`) })
  .catch(err => { console.log(err) });
```

### misc
module.exports retourne la valeur définie lorsque le fichier .js est include ailleurs
```
module.exports = Recipe;
```

## Schemas
Le 2e paramètre est ce que l’on appelle le schéma. Il nous permet de donner de la structure à nos documents et évite :

* d’ajouter un champ non-défini dans le schéma
* d’oublier un champ obligatoire (nous verrons comment)
* d’utiliser le mauvais type pour un champ

```
const mongoose = require("mongoose");
const Schema = mongoose.Schema;

const catSchema = new Schema({
  name: { type: String, required: true },
  age: { type: Number, min: 0, max: 30 },
  color: { type: String, enum: ['white', 'black', 'brown'] },
  avatarUrl: { type: String, default: 'images/default-avatar.png' },
  location: {
    address: String,
    city: String
  },
  countryCode: { type: String, match: /^[A-Z]{2}$/ }
}, {
  timestamps: true
});

const Cat = mongoose.model("Cat", catSchema);

module.exports = Cat;
```

##  Connection events 

| Connection event	| Description |
| --- | --- |
| ```mongoose.connection.on('connected', callback)```	| Call callback when Mongoose is connected. |
| ```mongoose.connection.on('error', callback)```	| Call callback when an error happened on connection. |
| ```mongoose.connection.on('disconnected', callback)```	| Call callback when Mongoose is disconnected. |
| ```process.on('SIGINT', callback)```	| Call callback just before stopping Node (can be simulated with <Ctrl>-C) |


## Data Models
La référence à un document d'une autre collection se fait grâce à son ```ObjectifId```. Ex : **ObjectId("593e7b2b4299c306d656299d")**

```
// Users
{
 _id: '1',
 email: "foo@bar.fr",
 pass: "shhht",
 followers: [
  '2', '3'
 ]
}

// Tweets
{
 _id: '10',
 user_id: '1', // ObjectifId de Users
 text: "Lorem ipsum...",
 favorites: [
  '4', '7'
 ]
}
```

### Réflexion sur les models One-to-N

#### One-to-Few
exemple : les adresses pour une personne
```
// db.person.findOne()
{
  name: 'Kate Monster',
  ssn: '123-456-7890',
  addresses : [
     { street: '123 Sesame St', city: 'Anytown', cc: 'USA' },
     { street: '123 Avenue Q', city: 'New York', cc: 'USA' }
  ]
}
```
* avantage : 1 seule requête de faite
* inconvénient : difficile d'obtenir "toutes les tâches pour demain" si elles sont embedding

#### One-to-Many
exemple : un système de référencement de pièces détachées
```
// db.parts.findOne()
{
    _id : ObjectID('AAAA'),
    partno : '123-aff-456',
    name : '#4 grommet',
    qty: 94,
    cost: 0.94,
    price: 3.99


// db.products.findOne()
{
    name : 'left-handed smoke shifter',
    manufacturer : 'Acme Corp',
    catalog_number: 1234,
    parts : [     // array of references to Part documents
        ObjectID('AAAA'),    // reference to the #4 grommet above
        ObjectID('F17C'),    // reference to a different Part
        ObjectID('D2AA'),
        // etc
    ]
}
```

Pour retrouver les pièces détachées d'un produit spécifique.
```
// Fetch the Product document identified by this catalog number
> product = db.products.findOne({catalog_number: 1234});

// Fetch all the Parts that are linked to this Product
> product_parts = db.parts.find({_id: { $in : product.parts } } ).toArray() ;
```
* avantage : chaque pièce est indépendante, plus facile pour les chercher et les mettre à jour indépendamment 
* inconvénient : besoin d'une seconde requête

#### One-to-Squillions
exemple : un système de logs
```
// db.hosts.findOne()
{
    _id : ObjectID('AAAB'),
    name : 'goofy.example.com',
    ipaddr : '127.66.66.66'
}

// db.logmsg.findOne()
{
    time : ISODate("2014-03-28T09:42:41.382Z"),
    message : 'cpu is on fire!',
    host: ObjectID('AAAB')       // Reference to the Host document
}
```

Pour obtenir les 5000 derniers messages :
```
// find the parent ‘host’ document
> host = db.hosts.findOne({ipaddr : '127.66.66.66'});  // assumes unique index

// find the most recent 5000 log message documents linked to that host
> last_5k_msg = db.logmsg.find({host: host._id}).sort({time : -1}).limit(5000).toArray()
```