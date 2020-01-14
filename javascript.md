# Javascript Cheatsheet

## tips

``` 
// pour convertir un JSON en objet
JSON.parse(string)
```

## Day1

### Operators
``` === ``` compare (à utiliser plutôt que ``` == ```) 

``` || ``` signifie "ou"

``` && ``` signifie "et"

``` !a ``` donne l'opposé de a 

### Others
```prompt()``` ouvre une fenêtre de dialogue à l'utilisateur

``` str1.localCompare(str2) ``` compare les deux strings et retourne 1 si str2 est alphabétiquement 1er, -1 s'il est dernier ou 0 si égal

``` str.indexOf() ``` recherche un élément dans une chaine et retourne sa position (si pas présent, la valeur retournée est ```-1```)

```array.includes()``` retourne true ou false si la valeur est contenue dans le array

``` str.toUpperCase() ``` met toute la string en majuscules

``` str.slice(a, b) ``` retourne une nouvelle string de a à b

``` Math.sign() ``` donne le signe du nombre en retournant -1, 0, 1

``` int.toFixed(x) ``` arrondit à x décimales

``` Math.random() ``` retourne un nombre aléatoire entre 0 et 1

``` Math.min(a, b, c) ``` retourne le plus petit nombre par les 3

``` false, null, undefined, "", 0, NaN ``` sont les falsies (= false)

#### Randomize
```Math.floor```

```Math.ceil```

```Math.rand```

##### Number random
```javascript
function randomSelector(array) {
  var randomIndex = Math.floor(Math.random() * array.length);
  return array[randomIndex];
}
```

### Structures de controle

#### if
``` if(){} else{} ```

##### meilleure façon de l'écrire
``` ? "WIN" : "LOSE" ;```

##### 
```var test = false && 3``` returns false

```var test = false || 3``` returns 3

#### switch
``` 
switch(){
  case "":
    break;
  default:
    break;
}
```

#### while
```
let i = 0;

while (i < 100) {
  console.log("i vaut", i);
  i += 1;
}
```

#### for
```
for (let i = 0; i <= 100; i++) {
  console.log(i);
}
```

#### .forEach()
````
tablo.forEach(function (el, i) {
  console.log(el, i);
})
`````
_le 2ème argument ```i``` est facultatif_

_si on appelle tablo, ```undefined``` est retourné (si on veut retourner une valeur, il faut utiliser ```.map()```_

##### pour tester toutes les valeurs
```array.every(el => )``` exécute tout le contenu d'array

```array.every() ? "win" : "lose";``` est un exemple d'utilisation

##### pour tester toutes les valeurs VRAIES
```array.some(el => )``` exécute jusqu'à la première valeur fausse

#### .map()
````
tablo.map(function (el, i) {
  return el.toUpperCase();
})
`````
_```map()``` **crée une copie de tablo**, la modifie et retourne un tableau en résultat_

#### .reduce()
```
array.reduce(function (accumulator, currentValue) {
  return accumulator + currentValue;
}, initialValue);
```

_```reduce()``` **crée une copie de tablo**, la modifie et retourne un tableau en résultat_

_**accumulator** est une **variable accumulatrice** dans le reduce et peut être un objet, un tableau, une string, etc._

_**initialValue** peut être utile dans le cas où on a un tableau d'objects et qu'on veut changer le type de valeur finale retournée. Valeur qu'elle peut prendre : ```{}```, ```[]```, ```0```, etc._

##### Or in 2 times
```
array.reduce(function(total, currentValue, currentIndex, arr), initialValue)
``` 
```
array.reduce(mafonction);
```

#### .filter()
```
var numbers = [1, 2, 3, 4, 5, 6];

var evenNumbers = numbers.filter(function(number){
  return number % 2 === 0;
});

console.log(evenNumbers); // <== [ 2, 4, 6 ]
```

_```.filter()``` copie le tableau initial, test la condition (filtre) et retourne un nouveau tableau avec les valeurs qui ont passé le filtre_

#### .sort()
les valeurs attendues dans .sort() sont soient 0, soient 1, soient -1

##### with chains
```
const words = ["Hello", "Goodbye", "First", "A", "a", "Second", "Third"];

words.sort();

console.log(words);
```

_```.sort()``` ordonne par **défaut les chaines**._

##### with numbers (litteral)
```
const nums = [100, 3, 8, 12];

nums.sort(function compare(a, b) {
  if (a < b) return -1;  // a is less than b
  if (a > b) return 1;   // a is greater than b
  if (a === b) return 0; // a equals b
});

console.log(nums);
```

##### with numbers (shortcut)
```javascript
array.sort((a, b) => a - b)
```

#### .reverse()
_```.reverse()``` modifie la **chaine** en inversant ses éléments._

##### si besoin de faire une copie
```
const nums = [100, 3, 8, 12];

const numsCopy = nums.slice();

console.log(numsCopy.reverse());
```

#### shallow-copy
##### .Object.assign()
_If we want to create independent copy of an object, we should use Object.assign(), for ... in loop combined with initialized empty object or JSON.parse(JSON.stringify())_

```
const book1 = {
  author: "Charlotte Bronte"
}

const book2 = Object.assign({}, book1);

console.log(book2); // <== { author: "Charlotte Bronte" }
console.log(book1 === book2); // <== false
```
_attention, *assign() est une shallow-copy, ça ne copie pas que le 1er niveau* : pas la valeur du tableau, mais son adresse_

##### spread operator ```...```
```
const book2 = { ...book1, author: "Julie" }
```

#### deep copy
```
JSON.parse(JSON.stringify())
```

```
const book1 = {
  author: "Charlotte Bronte"
}

const book4 = {}; // <== INITIALIZED EMPTY OBJECT

for(let prop in book1){
  book4[prop] = book1[prop]
}

book1.author = "William Shakespeare"; // <== changed the original

console.log(book1); // <== { author: 'William Shakespeare' } ==> changed
console.log(book4); // <== { author: 'Charlotte Bronte' } ==> DIDN'T CHANGE
```

#### To sumup memory addresses
```var book2 = book1``` keeps all memory addresses

```shallow-copy``` keeps memory addresses of minimum

```deep copy``` keeps no memory addresses

#### bonus
``` continue; ``` saute au tour d'après 

``` break; ``` arrête la boucle

## Day3
### Arrays
```const arrayNames = ["", "", ""]``` est un exemple de tableau

```.push("str")```insére à la fin du tableau

```.unshift("str")```insére au début du tableau

```.splice(start, deleteCount)```enlève des éléments du tableau

```str.split(``)``` découpe une chaîne et retourne un tableau

```array.join(``)``` réunit les composants d'un tableau en une chaine

### Functions
```
function <name> ([<argument_list>]) {
  <instructions>

  [return <expression>;]
}
```
_par défaut, la fonction retourne ```undefined```_

_après un ```return``` rien n'est exécuté dans la fonction_

_une *fonction callback* est une fonction qui retourne une fonction_

### Arrow functions

#### si plusieurs arguments
```
function power(nombre, puissance) {
  return nombre ** puissance;
}
```
équivaut à 
```
let power = (nombre, puissance) => {
  return nombre ** puissance;
}
```

#### si seulement 1 argument
```
let carre = nombre => {
  return nombre ** 2;
}
```
ou en abrégé
```
let power = (nombre, puissance) => nombre ** puissance;
```


### Scopes

#### functional
Les variables ```var```, ```let``` et ```const``` ne peuvent pas sortir du scope d'une fonction

#### block
La variable ```var``` peut sortir du scope des ```{}```

Les variables ```let``` et ```const``` ne peuvent pas sortir du scope des ```{}```

#### implied global
Si l’on oublie de déclarer une variable, celle-ci devient ```globale`` (attachée à window)

## Day4
### Object
```
let someObject = {
  key1: value,
  key2: value,
  key3: value
}
```

```{}``` équivaut à ```new Object()``` qui sert à initialiser un objet

```user["username"]```équivaut à ```user.username``` (qui est recommandé), mais exite pour les clés spéciales

```'facebook' in user``` retourne ```true``` si la propriété ```facebook```existe dans ```user```

```delete user.facebook``` supprime la propriété ```facebook``` dans ```user```

```Object.keys(user)``` retourne toute la liste des propriétés de l'objet dans un **tableau**

```Object.values(user)``` retourne toute  la liste des valeurs de l'objet dans un **tableau**

```for (let myKey in user) {}``` retourne chacune des valeurs les unes après les autres

## Day 6
### Primitive values
Primitive data types are stored and copied by value, which means two values are equal if they have the same value.

_ex : undefined, null, NaN, true/false, Infinity_

## Day 7
### Arrays and mutability

| Mutable methods | Immutable methods (copy) | What they do |
|---|---|---|
| .push() | .concat() | adding |
| .unshift() | ... spread operator | adding |
| .splice() | .slice() | removing |
| .pop() | .slice() and ES6 spread operator 	removing | removing |
| .shift() | .filter() | removing |

### test perf of our JS script
https://www.jsperf.com

### for with an object
#### old version (no returning anything)
``` 
var film = {
	title: "Titanic",
	year: "1996"
}

for (let clef in film) {
	console.log(clef, film[clef];
}
```

#### new version (returning an array)
``` 
Object.keys(film).forEach(function (el) {
	console.log(el, film[el]);
}
```
_[title, year]_

## Day 8
### Object method
It is possible to add a function in an object

```this.``` refers to the object its belongs

```player1.move()```

### class
A class is a **object factory**.
```
class Animal {
  constructor(name, mainColor, sound){
    this.name = name;
    this.mainColor = mainColor;
    this.sound = sound;
  }
  
  scream(intensity) {
    console.log(`${this.sound} ${'!'.repeat(intensity)}`);
  }
}

var garfield = new Animal(`garfield`, `orange`, meow`);

console.log(ferrari.roule());
```
A class always begins with a **Capital**.

```constructor()``` is the method triggered with the `new` keyword 

### inheritance with extends
```
class Cat extends Animal {
  constructor(name, mainColor, sound, nbOfLegs) {
    super(name, mainColor, sound);
    
    this.nbOfLegs = nbOfLegs; // <== a new attribute, just for cats
  }
}

```

## Day 9

### Debugging
the first one is ```console.log()```

#### Chrome dev tools
important to use ```breakpoints```on the line where there is a problem, it executes all the js before the breakpoint

##### step in button
important to use ```step into``` (zap navigation) on the ```breackpointed``` line

##### watch tab
needs to put a breakpoint on the line then add variable on watch tab

##### call stack tab
historic of the function

#### Try… Catch Statement
```
try {
    myroutine();
} catch (error) {
    ...
    logMyErrors(error);
} finally {
  ...
}
```
with this, the program continues (doesnt stop on the error) and returns the log or message we defined

#### throw
```throw new Error("Mon message d'erreur");``` stops the script

#### JS hint
https://jshint.com/

is present in the console

## Day 10

### Async & Callbacks
#### Callbacks function
Callbacks function is an anonymous function called as a parameter

#### setTimeout()
```setTimeout()``` sets a timer which executes a callback function once the timer expires.

```
setTimeout(function () {
  console.log('coucou');
}, 5000);
```

```clearTimeout(timeoutId);``` cancels the timeout

#### setInterval()
```setInterval()``` calls a function repeatedly with a fixed delayed time between each call.

```
const intervalId = setInterval(callbackFunction, delay);
```

```clearInterval(intervalId);``` cancels the interval

### DOM objects
#### Properties and Methods
```document.getElementById('id')``` gets id element and only return first element (if more than 1 ID)

```var mices = document.getElementsByClassName(`class`)``` gets all class elements in an "array"

```[...mices]``` to convert in a real js array

```document.querySelector('selectorCSS')``` gets element from the document

```document.querySelectorAll('selectorCSS')``` gets all elements from the document

```element.classList``` returns an array with the class name(s) of an element

```element.clientHeight``` returns the height of an element, including padding

```element.clientWidth``` returns the width of an element, including padding

```element.getAttribute()``` returns the *specified attribute value of an element node*

```element.parentNode``` returns the parent node of an element

```element.style``` returns the parent node of an element

```element.innerHTML``` returns the value of an ELEMENT

```input.value``` returns the value of an INPUT

```element.style.fontSize = "3em"``` where "-" style is replaced by camelCase

```element.style.visibility = "hidden"``` hides an element

```element.classList.add('class')``` adds a CSS class to an element

```$0``` in console returns the element selected when clicked

### DOM manipulation
#### Operating with attributes
```element.setAttribute(name, value);``` changes the value of an existing attribute on the specified element AND could create a new one

```document.createElement(tagName);``` creates a new element

```parent.appendChild(h2Tag);``` adds in the end of parent HTML the h2Tag

```parent.insertBefore(h1Tag, h2Tag);``` adds h1Tag just before h2Tag

```parent.removeChild(el);``` removes element

##### deleting an HTML element
```$el.parentNode.removeChild($el)```

##### adding an HTML element
```
var el = document.createElement('p'); // is a node
el.innerHTML = 'foo';
document.body.appendChild(el);
```

*var $nameOfHTMLVariable* is a convention

#### Events in JavaScript elements

##### .onclick
```javascript
let button = document.getElementById("add-item-button");

button.onclick = function(){
  console.log("adding an element to the list");
}
```
```
btn.onclick = function (event) {
  console.log('just clicked');
}
```
```event``` param is always returned for an event

```event.CurrentTarget``` returns the HTML value of where the click has been made 

## Day 11

### Cancas
```<canvas width="800" height="800"> </canvas>```

Value in cavas are its resolution. We can however *define a style* to adjust is screen size.

#### insert in js
```
// javascripts/intro.js
var canvas = document.getElementById('idOfCanvas');
var ctx = canvas.getContext('2d');
```
ctx is an unofficial convention

#### Grid
Top left is the origin

- Horizontal: x
- Vertical: y

##### Rectangles
```fillRect(x, y, width, height)``` draws a filled rectangle

```strokeRect(x, y, width, height)``` draws a rectangular outline

```clearRect(x, y, width, height)``` clears the specified rectangular area, making it fully transparent.

##### Paths
A path is a list of points, connected by segments of lines that can be of different shapes, curved or not, of different width and of different color.

```beginPath()``` creates a new path. Once created, future drawing commands are directed into the path and used to build the path up.

```closePath()``` closes the path so that future drawing commands are once again directed to the context. It is *optional*.

```stroke()``` draws the shape by stroking its outline.

```fill()``` draws a solid shape by filling the path's content area.

###### moveTo()
```moveTo(x, y)``` moves the pen to the coordinates specified by x and y.

###### lineTo()
```lineTo(x, y)``` draws a line from the current drawing position to the position specified by x and y.

###### stroke()
```stroke()``` draws the Path

###### fill()
```fill()``` draws the Path and fills it

###### drawImage
```ctx.drawImage(image, dx, dy, dLargeur, dHauteur);``` draws an image

##### arc
###### arc()
```
arc(x, y, radius, startAngle, endAngle, anticlockwise)
ctx.arc(150, 170, 75, 0, Math.PI * 2);
```

// Draws an arc which is centered at (x, y) position with 

// radius starting at startAngle (0 PI) and ending at endAngle going (2 PI)

// in the given direction indicated by anticlockwise (defaulting to clockwise).

###### arcTo()
```arcTo(x1, y1, x2, y2, radius)```

// radians = (Math.PI/180)*degrees.

#### Style
##### Transparency
```globalAlpha = transparencyValue``` where transparencyValue varies from 0 to 1

##### writing
```fillText(text, x, y, maxWidth)``` fills a given text at the given (x,y) position. Optionally with a maximum width to draw.

##### image
```var img = new Image();``` creates new <img> element

```img.src = 'myImage.png';``` sets source path

```drawImage(image, x, y, width, height);``` adds the width and height parameters, which indicate the size to which to scale the image when drawing it onto the canvas.

#### capture key
```document.onkeydown = (e) => console.log(e.which);``` returns the number of the key (letters are in [65;90])

source to get directly the code: https://keycode.info/

### Animations
```
function anim() {
  x += 1;
  drawFrame();
  
  requestAnimationFrame(anim);
}
requestAnimationFrame(anim);
```
```requestAnimationFrame()``` is more recommanded than ```setInterval``` or ```setTimeout```

### FIXME ADD TO CLASS PART
        // function updateFrame() {
        //     this.currentAnimationFrame = ++(this.currentAnimationFrame) % this.imageCols;
        //     this.spriteX = this.currentAnimationFrame * this.spriteW;
        //     this.spriteY = 0 * this.spriteH;
        // }
        // updateFrame = updateFrame.bind(this);

### FIXEME
```window.onresize = funcRef;``` to do smth when window is resized
```element.getBoundingClientRect``` returns size of an element accord to its location in the viewport

## Module 2
### Recap - Intro to ES6
#### hoisting
**Definition :** le hoisting est un mécanisme qui va rassembler les déclarations de variables tout en haut du scope. Si on appelle la variable avant qu'elle est une valeur, elle retourne **"undefined"**.

Comme pour ```var```, les variables ```let``` sont aussi hoistés (en haut de leur scope). Cependant, on ne pourra pas “utiliser” la variable avant dans le code. Sinon, on aura un **ReferenceError**.


|		|	hoisted/initialized	|	scope	|	updated	|	redeclared	|
|---|---|---|---|---|
|var		|	✅/✅			|	global or functional(local)	|	✅	|	✅	|
|let		|	✅/❌			|	block	|	✅	|	❌	|
|const	|	✅/❌			|	block	|	❌	|	❌	|

**toujours utiliser ```let``` dans une boucle**

#### nouvelles méthodes
##### .startsWith()
```str.startsWith(searchString[, position])``` retourne ```true``` ou ```false```

* **searchString** - the characters to search at the start of this string
* **position** (optional) - the position in this string at which to begin searching for searchString (the default is 0)

##### .endsWith()
```str.endsWith(searchString[, length])``` retourne ```true``` ou ```false```

* **searchString** - the characters to search at the start of this string,
* **length** (optional) - if provided, overwrites the considered length of the string to search in. If omitted, the default value is the length of the string.

##### .includes()
```str.includes(searchString[, position])``` retourne ```true``` ou ```false```

* **searchString** - the characters to search for at the start of this string,
* **position** (optional) - the position within the string at which to begin the search (defaults to 0).

### Object and Array Destructuring
**Definition** : Destructuring is an easy and convenient way of extracting data from arrays and objects.
#### Object destructuring
```
let person = {
  name: "Ironhacker",
  age: 25,
  favoriteMusic: "Metal"
};

let { name, age, favoriteMusic } = person;

console.log(`Hello, ${name}.`);
console.log(`You are ${age} years old.`);
console.log(`Your favorite music is ${favoriteMusic}.`);
```

#### Array destructuring
```
const [first, second, third] = numbers;
console.log(first, second, third); // <== one two three
```
```
const [, second] = numbers;
console.log(second); // <== two
```
```
let [a, b = 2, c, d = 1] = [3, 4];
console.log(a, b, c, d);
```

### Spread operator
#### to concat
```
const reptiles = ["snake", "lizard", "alligator"];
const mammals = ["puppy", "kitten", "bunny"];
const animals = [...mammals, ...reptiles];
console.log(animals);
// [ 'puppy', 'kitten', 'bunny', 'snake', 'lizard', 'alligator' ]
```

#### in a function -> Rest Parameters
**Why?** when we don't know the potential numbers of the elements of the function
```
function add(...numbers) {
  return numbers.reduce((acc, num) => acc + num);
}

console.log(add(1,2,3)); // 6
console.log(add(1,2,3,4,5,6,7)); // 28
```

##### default value
```
function puissance(x, i=2) {
  return x**i;
}

puissance(3) // 9
puissance(2) // 4
puissance(2,3) // 8
```

### ES6 Promises
#### Promise
**Why?** check in the futur something. By default, its value is `Pending`

```const pro = new Promise(function(resolve, reject) {}```

```pro.then(function(){});``` // if resolve

```pro.catch(function(){});``` // if reject

#### example
```
[1,2].forEach(function (el, index) {})


const honete = false;

var pret = new Promise(function (resolve, reject) {
  setTimeout(function () {
    // dans 2 semaines
    
    if (honete) {
      // tenir ma promesse
      resolve(110);
    } else {
      // rompre ma promesse
      reject();
    }
  }, 2000)
});

pret.then(function (amount) {
  if (amount === 100) {
    console.log('merci');
  } else if (amount > 100) {
    console.log('waouh merciiii');
  }
})
    .catch(function () {
  console.log('WTF');
})
```
#### Promise.all()
Il peut être utile d’attendre plusieurs promesses en même temps. Promise.all() retourne une promesse de toutes les promesses passées en tableau :

```
Promise.all([p1, p2, p3]).then(function (values) {
  // p1, p2 et p3 sont maintenant résolues
}).catch(function (reason) {
  // l'une d'elle vient d'échouer
})
```

### play audio in JS
```
const audio = document.createElement(`audio`);
audio.src = `https://www.eternaltournaments.com/sounds/alert_new.mp3`; // need to click on the windows to allow the sound to be played
```

and then ```audio.play();```