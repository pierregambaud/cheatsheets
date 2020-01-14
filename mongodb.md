# MongoDB

## Définition
MongoDB est une base de donnée de type NoSQL. Outre ses performances et sa simplicité d'utilisation et de compréhension, il s'adapte parfaitement à MEAN en stockant tout en JSON. C'est un vrai bonheur à utiliser. Tout est stocké sous la forme clé-valeur dans des collections (des objets JSON). Bien que très simple à utiliser, il existe un excellent module souvent proposé avec ce stack nommé Mongoose.js.

Mongoose est un ODM (Object Document Mapper). Peut-être que vous connaissez le terme ORM (Object Relational Mapper) qui est utilisé en SQL. Et bien c'est la même chose pour le NoSQL (du moins pour les base de données NoSQL de type documents). Mongoose va vous simplifier diverses tâches de MongoDB, et va aussi apporter des fonctionnalités comme la population ou les Schémas. Imaginons que vous souhaitiez gérer des utilisateurs. Vous allez créer un schéma utilisateur et indiquer ses propriétés et types qu'il doit recevoir (Nom, Prénom, etc). Par la suite, vous allez créer un Modèle basé sur ce schéma, que vous pourrez manipuler comme un objet. Cela permet une régularité dans l'architecture de vos données et vous donne une abstraction complète de la base de donnée. Vous ne manipulez que vos Modèles et tout le monde est content ! 

## Structure
database => collections => documents (BSON) => fields

## Start the database
```brew services start mongodb-community```

Connect the service to **MongoDB Compass**
* **hostname:** the hostname (or IP address) of the machine where the MongoDB instance is running. Remember you can always reference your computer from **127.0.0.1** or **localhost**
* **port:** on which mongod is running: **27017**

## Data type
| type	|	Examples |
|---|---|
| Double	|	3.141625 |
| String	|	“IronHack Coding Bootcamp” |
| Date	|	“Sun Dec 08 07:15:44 UTC 2013” |
| Boolean	|	true |
| Object	|	{ foo: ‘bar’, } |
| Array	|	[“apple”, “oranges”, “kiwis”] |
| ObjectID	|	ObjectId(“52cdef7c4bab8bd675297d8a”) |
| Null	|	null |

# Mongo Compass CRUD Operations
## CRUD
CRUD stands for ```create```, ```read```, ```update``` and ```delete```

## importing Data
```mongoimport --db users --collection contacts --file contacts.json```

## research with Filter
### basic
```{_id: ObjectId('5dd003d18c3cfae496840aa6')}```

### complex
```{ $and: [ {year: "2000"}, {rate: "8,5"} ] }``` équivaut à "ET"

```{ $or: [ {year: "2000"}, {rate: "8,5"} ] }``` équivaut à "OU"

```{ $nor: [ {year: "2000"}, {rate: "8,5"} ] }``` équivaut à "NI NI"

### projection (project)
in "options"
```{title: 1, _id: 0}``` only displays 1 not 0

### sort
in "options"
```{title: 1}``` displays ASC (1) or DESC (-1)

### skip and limit
```
limit: 100
skip: 200
```
Very useful for pagination

### Operators
#### Query operators
```{ field : { $eq: value } }``` means Equal

```{ field : { $ne: value } }``` means Not Equal

```{ field : { $gt: value } }``` means Greater Than

```{ field : { $gte: value } }``` means Greater Than Equal

```{ field:  { $lt: value } }``` means Less Than

```{ field : { $lte: value } }``` means Less Than Equal

#### Comparison operators
```{ field: { $in: [ value1, value2, ..... , valueN ] } }``` means OR

```{ field: { $nin: [ value1, value2, ..... , valueN ] } }``` means NONE OF THE VALUES

```{ field: { $all: [ value1, value2, ..... , valueN ] } }``` means AND

```{ grades: { $elemMatch: { grade: "A", score: { $gte: 15 } } } }``` is for DEEPER RESEARCH (not in an array, but in an object), it selects documents if element in the array field matches all the specified $elemMatch conditions

#### Element Query Operators
```{ field: { $exists: <boolean> } }```

```{ field: { $type: <BSON type> } }```

##### BSON type
| Type	| Number | Alias |
|---|---|---|
| Double	| 1 | “double” |
| String	| 2 | “string” |
| Object	| 3 | “object” |
| Array	| 4 | “array” |
| ObjectId| 7 | “objectId” |
| Boolean	| 8 | “bool” |
| Date	| 9 | “date” |
| Null	| 10 | “null” |