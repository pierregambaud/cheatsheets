# CSS Cheatsheet

### Références
* Flexbox : https://flexbox.help/
* Shadow box : https://brumm.af/shadows
* Créer un dégradé de couleurs : https://cssgradient.io/
* Commenter en ASCI : http://patorjk.com/software/taag/#p=display&c=c%2B%2B&f=Big&t=destroy

### Ajuster la proportion d'une image
```align-items: flex-start;``` à ajouter sur le parent d'une image (qui a un width défini) pour ajuster la proportion d'une image (pas strech)

### Aligner 2 blocs par ligne
```flex-wrap: wrap;``` sur le parent avec sur les enfants un ```width: 50%```

### Prendre toute la largeur restante
```flex:1``` sur la div ciblée, et **surtout, mettre sur la div parent**```container: min-height: 100vh;```

### Tips en vrac
```width > margin > border > padding``` correspondent à l'ordre des éléments

```box-sizing: content-box``` définit la largeur totale d'un bloc (de ```width``` jusqu'à ```border```)

```background-size:cover``` couvre la surface du bloc avec l'image de ```background-img``` 

```margin-left, margin-right: auto; width: 60%``` technique pour centrer 

```margin-left: auto;``` permet de pousser l'élément vers le bord droit

```footer a:not(:last-child)``` tous les liens de footer sauf le dernier

```box-shadow: 5px 4px 3px rgba(0,0,0, .25)``` pour mettre une ombre avec un décalage de 5px vers le bas, 4px vers la droite, et une largeur de 3px

```box-shadow: 5px 4px 3px rgba(0,0,0, .25) inset``` permet de mettre une ombre interne

```rgba(0,0,0, .25)```pour faire du gris en modifiant l'opacité du noir

```a: currentColor;``` applique la couleur de la zone au lien

```
// centrer le child sur toute la page
div.parent {min-height: 100vh;}
div.child {padding: auto;}
```

```
// sélectionner un élément selon son attribut
a[alt="value"] {
  border-color: blue;
}

// plus de possibilités sur https://css-tricks.com/almanac/selectors/a/attribute/
```

```vw, vh, vmin, vmax``` se basent sur le viewport

```vmin```: la plus petite valeur entre vw et vh

```height: 100vh``` permet d'ajuster la taille en fonction de la taille de l'écran


# Fondamentaux

## Position

* static : defaut position
* relative :
```top:``` ```right:``` ```bottom:``` ```left:```

* fixed : basé sur le viewport (même avec un scroll)
* absolute : recherche la première position parent
	* par défaut, la page
	* sinon, **parent** le plus proche sur lequel on a mis
	
		```position: relative```


## Media queries

### Dans le CSS
```@media (min-width: 400px) {}``` pour matcher la vision **mobile first**

***Bonne pratique :** mettre le @media après son correspondant mobile*

### Dans le HTML
```<meta name="viewport" content="width=device-width, initial-scale=1">``` pour adapter la vue en fonction du device


## Transitions VS Animations

| | Transitions | Animations |
|-|-|-|
| Lancement | Besoin d'un trigger (ex : hover, js, etc.) | Pas besoin d'un trigger (même si l'animation peut être lancée par un trigger) |
| Points intermédiaires | Limitée à un point initial et un point final | Autant de points intermédiaires que nécessaires (keyframes) |
| Itération sur CSS | Ne change pas les propriétés CSS | Peut changer les valeurs des propriétés CSS dans leurs keyframes |
| Boucle | Pas pensé pour | Tout à fait possible |

### CSS3 Transitions
```
// transition au passage de la souris de .box vers .box:hover
.box {
  display: block;
  width: 100px;
  height: 100px;
  background-color: #3838FF;

  transition-property: width;
  transition-duration: 2s;
  transition-timing-function:linear;
  transition-delay: 1s;
}

.box:hover {
  background-color: #00D1AE;
  width: 200px;
  height: 200px;
}
```

```
// exemple sur une seule ligne
transition: width 2s ease-in 1s, height 4s, background-color 6s;
```

***Références :***
* https://cubic-bezier.com/#.88,.07,.64,.93
* https://hackmd.io/@abernier/ByQDH8sPB

### CSS3 Animations

```
// sur la class CSS
.circle {
 animation-name: nomAnimation;
 animation-duration: 3s;
 animation-iteration-count: infinite;
}
```

```
// à inclure dans le CSS
@keyframes nomAnimation {
  0% {
    margin-left: 100%;
    width: 300%;
  }

  100% {
    margin-left: 0%;
    width: 100%;
  }
}
```

***Exemples :***
* https://codepen.io/lastik0t/pen/gOYJgYq?editors=1100
* https://hackmd.io/@abernier/ByQDH8sPB
* Générateur puissant et complet : https://animista.net/