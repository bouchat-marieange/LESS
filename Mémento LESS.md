# Mémento LESS

Le CSS est un langage déclaratif simple composé d'une propriété auquel on correpondent une ou plusieurs valeurs.

```CSS
.text_gras
{
font-weight: bold;
color: #000000;
border: solid #000 1px;
}
```

Cela s'est compliqué avec l'appartion des attributs propriétaires pour assurer la compatibilité sur les différents navigateurs (*-moz-*, *-webkit-*), ce qui oblige à dupliquer le code plusieurs fois pour éviter les problèmes d'affichage

```css
#foo {
  -moz-border-radius: 10px;
  -webkit-border-radius: 10px;
  border-radius: 10px;
  /* etc. */
}
```

De plus on ne peut pas imbriquer les selecteurs CSS.

```css
/* Impossible d’écrire ceci : */
#foo {
  bar {
    color: #150;
  }
  baz::before {
    content: '[';
  }
  baz::after {
    content: ']';
  }
}

/* Il vous faudra écrire ceci : */
#foo bar{
  color: #150;
}
#foo baz::before {
  content: '[';
}
#foo baz::after {
  content: ']';
}
```

Les limitations de CSS interviennent également si par exemple, on doit définir une palette de couleur pour un site. En cas de changement d'une couleur, il faudra rechercher toutes les endroits où la couleur à été utilisée pour ensuite la remplacé par la nouvelle couleur ce qui engendre un immense perte de temps.

Pour palier à ces différents problèmes et simplifier le développement avec CSS, on a donc inventer LESS, un langage dynamique capable de générer des feuilles de style que les navigateurs pourraient comprendre.

LESS étend le CSS avec un comportement dynamique, utilisant des variables, des classes abstraites, des opérations et des fonctions. LESS fonctionne aussi bien côté client (IE version 6 et +, Webkit [Ndt : Safari, Chrome, Midori], Firefox) que côté serveur, avec Node.js.

Il existe également des implémentations non officielles en PHP, Ruby, Python, etc...


## Installation

Les navigateurs ne reconnaissent pas LESS, ils ne connaissent que CSS, il faut donc un interpréteur pour transformer LESS en CSS.

On peut installer cet interpréteur de 2 façons différentes:

* coté client (le fichier LESS est traduit par votre visiteur)
* coté serveur (vous envoyer directement un fichier CSS traduit à votre visiteur)

On ne peut pas installer LESS coté client ET coté serveur. Il faut choisir!

### Installation côté client

C'est la méthode la plus simple.

Il faut :

* Ajouter un fichier sur votre serveur

* Modifier la balise <head> de votre html.

Attention cette technique requière l'activation de JavaScript par le client (le visiteur de votre site). Si JavaScript n'est pas activé sur la machine de votre visiteur, il verra s'afficher une page non stylisée puisque LESS n'aura pas été traduit en CSS. Il faut cependant garder à l'esprit que le nombre de clients n'utilisant pas JavaSCript se fait rare et que ceux qui le désactive intentionnelement savent à quoi s'en tenir. C'est donc un solution que j'utilise personnellement.

On commence par télécharger la dernière version de less.js sur le site officiel: http://lesscss.org/#download-options et on le place dans le dossier de notre projet.

On remplace dans le fichier la ligne 1 par la ligne 2:


```html
<!-- Utilise CSS -->
<link rel="stylesheet" type="text/css" href="styles.css">

<!-- Utilise LESS -->
<link rel="stylesheet/less" type="text/css" href="styles.less">

On peut ensuite soit faire un lien vers la version en ligne du fichier less.min.js comme conseiller dans le site officiel de less (http://lesscss.org/)

<script src="//cdnjs.cloudflare.com/ajax/libs/less.js/3.0.0/less.min.js" ></script>

Soit télécharger le fichier de less (less.js ou less.min.js) le placer dans le dossier de notre projet et effectuer un lien vers celui)ci dans le head de notre page html

<!-- Changez la source par la position réelle du fichier chez vous -->
<script src="less.js" type="text/javascript"></script>
```

Attention veillez à adapter le chemin du fichier less.js ou less.min.js selon l'endroit où vous l'avez placer par rapport à votre fichier index.html dans le dossier de votre projet.

Et voilà , on peut passer à la pratique!

### Installation coté serveur

Ne réaliser l'installation côté serveur que si vous n'avez pas réalisé l'installation coté client. Il faut choisir entre l'une est l'autre installation.

Si vous décidez d'installer LESS côté server, vous pourrez permettre à vos visiteurs de visualiser votre page correctement même si JavaScript est désactivé. Cependant cette installation est dépendante du serveur utilisé. Il n'existe à ce jour d'interpréteur officiel que pour le framework Node.js.

Soit plusieurs choix:

* Utiliser Node.js et suivre l'installation décrite plus loin.
* Utiliser un autre serveur et générer des fichiers CSS que vous servirez statiquement (voir explications plus bas)
* Utiliser un autre serveur en vous référant à un moteur de recherche pour son installation.

#### Installation avec Node.js

Commencer par installer le paquet LESS avec npm

```
$ npm install less
```

Utiliser LESSS dans votre code en important le module LESS pouis en utilisant son compilateur

```
var less = require('less');
less.render(style, callback);
```

#### Compilation des fichiers pour les servir statiquement

Vous n'avez plus qu'à compiler vos fichiers .less en .css grâce à la commande suivante

```
lessc style.less > style.css
```
vous pourrez ensuite envoyer directement les fichier généré

## Utilisation de LESSS

CSS est totalement compatible avec LESS, vous pouvez donc utiliser du CSS pur dans votre code LESS si vous le souhaitez

### Constantes

LESS vous permet d'utiliser des contantes. Les constantes seront modifiées par leur contenu lors de la compilation.

Exemple:

```LESS
/* Déclaration */
@blue: #17B6FF;
@magenta: #FF17B6;
@lime: #B6FF17;
@green: #1BFF17;

/* Utilisation */
#header { background: @blue; }
#footer { background: @magenta; }
```

LESS générera le code CSS suivant:

```LESS
#header { background: #17B6FF; }
#footer { background: #FF17B6; }
```

On peut utiliser des noms de variables plus ciblées:

```LESS
/* Déclaration */
@bg_header: #17B6FF;
@bg_footer: #FF17B6;

/* Utilisation */
#header { background: @bg_header; }
#footer { background: @bg_footer; }
```

On peut utiliser des variables de différents type (couleur, dimensions, pourcentage, chaines de caractères, etc...)

### Classes abstraites

Les classes abstraites peuvent être comparée à des fonctions.

```LESS
/* Définition de la classe .border-radius */
.border-radius {
  -moz-border-radius: 10px;
  -webkit-border-radius: 10px;
  border-radius: 10px;
}

/* Utilisation de .border-radius comme d’une classe abstraite */
#foo {
  .border-radius;
  /* etc. */
}

/* CSS généré : */
#foo {
  -moz-border-radius: 10px;
  -webkit-border-radius: 10px;
  border-radius: 10px;
  /* etc. */
}
```

Cette transformation sur un seul élément peut paraitre sans intérêt mais si on doit appliquer des bordures arrondies sur 50 éléments, cette fonction de LESS sera très utile et nous fera gagner beaucoup de temps.

L'exemple précédent propose une valeur de 10px stockée en dur dans notre classe abstraite. Nous pouvons avec LESS faire encore mieux en transmettant des paramètres à notre classe abstraite.

```LESS
/* Définition de la classe .border-radius avec un paramètre */
.border-radius(@radius) {
  -moz-border-radius: @radius;
  -webkit-border-radius: @radius;
  border-radius: @radius;
}

/* Utilisation de .border-radius comme une classe abstraite */
#foo {
  .border-radius(10px);
  /* etc. */
}

/* CSS généré : */
#foo {
  -moz-border-radius: 10px;
  -webkit-border-radius: 10px;
  border-radius: 10px;
  /* etc. */
}
```

Vous pouvez également utilisé plusieurs paramètres, séparés par des virgules.

On peut également avec LESS, utiliser des valeurs par défaut pour les paramètres:

```LESS
/* Définition de la classe .border-radius avec un paramètre optionnel */
.border-radius(@radius: 10px) {
  -moz-border-radius: @radius;
  -webkit-border-radius: @radius;
  border-radius: @radius;
}

/* Utilisation de .border-radius comme une classe abstraite */
#foo {
  .border-radius(10px);
  /* – ou bien – */
  .border-radius;
  /* etc. */
}

/* CSS généré : */
#foo {
  -moz-border-radius: 10px;
  -webkit-border-radius: 10px;
  border-radius: 10px;
  /* etc. */
}
```
