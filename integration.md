# Installation
## in HTML
```<meta name="viewport" content="width=device-width, initial-scale=1">```

## in CSS
```
*, :before, :after {box-sizing:border-box;}

:root {
    --white: #F7F9F9;
    --light-blue: #2081C3;
    --dark-blue: #0A2C42;
    --grey: rgba(145, 145, 145, 0.481);

    --gutter:1.5rem;
}

body {
    margin:0;
    font-family: Arial, Helvetica, sans-serif;
}

img {max-width: 100%;}
```

# Tips
* https://placeholder.com/
* Animations JS : https://animista.net/


## CSS variables
```
:root {
 --pad:1em;
 --gutter:1.5em;
}
```

```
.footer {
 padding-left:var(--pad);
}
```

```
// fallback value
.component .header {
  color: var(--header-color, blue); // header-color isn’t set, and so remains blue, the fallback value
}

// ex : https://css-irl.info/7-uses-for-css-custom-properties/
```


## CSS calculation
```
// mettre une goutière sur les 4 cards 
.card {
 width:calc(25%- 3*var(--gutter)/4);
}
```


## Responsive grids
```
:root {
  --noOfColumns: 8;
}

@media (min-width: 60em) {
  :root {
    --noOfColumns: 12;
  }
}

.grid {
  display: grid;
  grid-template-columns: repeat(var(--noOfColumns), 1fr);
}
```


## CSS limiter le nombre d'éléments à afficher (ex : pour mobile)
```li:nth-child(3)``` gets the 3rd li

```li:nth-child(n)``` (where ```n``` starts at 0) gets all li

```li:nth-child(3+n)``` gets all the li starting at 3


## Formulaire
Tips en vrac :
* ```<button>``` n'a pas besoin du ```type="submit"```
* mettre le ```<input type="number" name="price" min="0" max="5" step=".5">``` directement dans ```<label>```

```
// formulaire d'upload de fichier
<form action="/movie/add" method="post" enctype="multipart/form-data">
<input type="file" name="photo">
</form>
```