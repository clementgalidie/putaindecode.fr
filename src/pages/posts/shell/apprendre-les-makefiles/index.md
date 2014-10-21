À l'heure où tout le monde se rue sur des outils comme [Gulp][p!gulp],
[Grunt][p!grunt] ou autres [Rake][gem:rake], certains irréductibles
(dont je fais partie) ont fait le choix d'employer un des outils les plus
standard et emblématiques dont tout développeur a au moins entendu parler une
fois dans sa vie : *Make*.

Si de premier abord on a l'impression d'un système assez archaïque (ce qui n'est
pas toujours forcément faux), on se rend rapidement compte que _Make_ couplé à
une petite dose de scripting shell permet rapidement de mettre en place un moyen
de compiler ses fichiers et de lancer des tâches.

Les fichiers de configuration de _Make_ sont appellés _Makefiles_ (oui, ce sont
eux qui ont donné leurs noms aux `(Gulp|Grunt|Rake)files`). Les instructions
qu'ils contiennent sont exécutées grâce à la commande `make` dans votre
terminal.

# Hello World

Je vous propose de commencer en douceur par un classique _Hello World_. On va
simplement définir une tâche `hello-world` dont l'action va être d'afficher
_"Hello, world"_ à l'écran (boooring).

``` make
hello-world:
	echo "Hello, world"
```

Première remarque importante, les indentations dans un _Makefile_ **doivent**
être faites avec des tabulations. Oui, je sais, c'est moche, mais c'est comme
ça.

Pour exécuter notre commande, il nous suffit de lancer `make hello-world` dans
un terminal :

``` console
$ make hello-world
echo "Hello, world"
Hello, world
```

Décortiquons un peu cette exécution. Pour chaque commande qu'il exécute, _Make_
affiche la commande complète avant d'afficher la sortie standard de ladite
commande. C'est souvent très pratique car toutes les variables (on va revenir là dessus)
qu'on met dans la commande sont résolues, et on voit clairement ce que _Make_
exécute. Par contre, dans certains cas on s'en fout un peu, on peut alors
préfixer la ligne à rendre silencieuse par un `@`, comme ça :

``` make
hello-world:
	@ echo "Hello, world"
```

``` console
$ make hello-world
Hello, world
```

# Règles, cibles, recettes et pré-requis

OK, jusque là c'est génial, on a un super outil pour lancer des commandes qu'on
pourrait déjà stocker dans un bête script shell. Pas super utile finalement.

Bon, on va corser un peu les choses et commencer par définir un peu de
terminologie.

Un _Makefile_ est une collection de **règles**, chacune étant composée d'une
**cible**, de **pré-requis** (ou pas) et d'une **recette**. Dans notre exemple
précédent, `hello-world` est la cible et `@ echo "Hello, world"` est la recette
de la règle. Elle ne spécifie par contre pas de pré-requis.

Vous l'aurez compris, on invoque une règle depuis la ligne de commande en
spécifiant le nom de sa cible après la commande `make`. Si on ne précise rien,
c'est la première règle trouvée qui est exécutée (donc dans notre cas,
`hello-world`).

Les pré-requis sont déclarés après la cible. On pourrait par exemple ajouter un
`sauter-une-ligne` comme pré-requis à notre cible `hello-world` :

``` make
hello-world: sauter-une-ligne
	@ echo "Hello, world"

sauter-une-ligne:
	@ echo
```

``` console
$ make hello-world

Hello, world
```

Facile, non ?

# Construire des fichiers

Les pré-requis sont particulièrement pratiques quand on veut construire un
fichier depuis un autre, ce qui est la principale action d'à peu près tout
processus de compilation (paraît même que c'est grosso-modo la définition de la
compilation).

On peut par exemple écrire un _Makefile_ nous permettant de compiler un fichier
_Markdown_ en _HTML_ :

``` make
article.html: article.md
	@ marked article.html > article.md
```

Cette règle spécifie simplement que pour construire le fichier `article.html`
j'ai besoin du fichier `article.md` et que j'utilise la commande
[`marked`][npm:marked] pour construire le fichier. Essayez, vous verrez, c'est
magique.

Là où ça devient intéressant, c'est que si je lance de nouveau `make
article.html`, rien ne se passe. Hé oui, _Make_ vérifie les dates de
modification des pré-requis et les compare avec la date de modification de la
cible pour savoir s'il doit où non reconstruire la cible.

On peut bien sûr aller plus loin en ayant des fichiers qui dépendent de
fichiers, qui à leur tour dépendent de fichiers, ….

On peut aussi avoir une cible factice qui ne représente pas un fichier et qui
elle-même dépend de plusieurs fichiers (`website: index.html apropos.html
contact.html`).

# Variables et substitutions

# Variables spéciales

# La cible `.PHONY`

# Conventions et bonnes pratiques

[p!gulp]: /posts/js/introduction-gulp/
[p!grunt]: /posts/js/premiers-pas-avec-grunt/
[gem:rake]: https://rubygems.org/gems/rake
[npm:marked]: https://www.npmjs.org/package/marked
