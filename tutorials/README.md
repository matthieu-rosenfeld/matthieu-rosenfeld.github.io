---
lang: fr
---

# ![](ressources/logo.jpeg) JavaScript  -- Vue.js

### IUT Montpellier-Sète – Département Informatique

## TD1
#### _Thème : Le réactif en JavaScript_

Commencez par faire un fork du TD puis de le cloner sur votre machine.

# INTRODUCTION



Des parties de ce TDs sont directement inspirée de la documentation de Vue et en partie du [tutoriel Vue](https://vuejs.org/tutorial/#step-1).

# Création de notre première application vue grâce à npm
Jusqu'à maintenant nous n'avons utilisé que du JavaScript "simple". Nous pouvions donc simplement écrire notre fichier `js`, l'inclure dans le fichier `HTML` et le navigateur web faisait le reste du travail sans même avoir besoin d'un serveur web. Beaucoup de Frameworks JS coexiste avec tout un paquet d'utilitaires (compilateur, linter, serveur de développement...). Ces utilitaires sont très simples à installer grâce à Node.JS et [npm](https://fr.wikipedia.org/wiki/Npm) (officieusement l'acronyme de "Node Package Manager", officiellement )
  
  

 Cependant, pour utiliser vue.js (comme pour beaucoup d'autres frameworks JS)
Comme pour beaucoup d'autre framework JS, Node.JS [npm](https://fr.wikipedia.org/wiki/Npm)


```
npm init vue@latest
```

L'utilitaire vous demande d'abord le nom du projet à créer : vous pouvez choisir `todo_list` (puisque nous allons commencer par réaliser une liste de tâches). Ensuite, il vous propose d'inclure plusieurs fonctionnalités supplémentaires dès la création du projet. Nous allons toutes les refuser sauf TypeSript (nous ignorerons les erreurs de type au début, mais nous y reviendront ensuite).

![](ressources/output_vite.png)

Enfin, on nous propose d'entrer les 3 lignes suivantes pour commencer à travailler sur le projet, faites-le, 

```
  cd todo_list
  npm install
  npm run dev
  ```
  La seconde commande permet d'installer les différentes dépendances du projet. Si tout va bien, la dernière commande donne la sortie suivante

```
 test@0.0.0 dev
 vite


  VITE v4.4.2  ready in 420 ms

  ➜  Local:   http://localhost:5173/
  ➜  Network: use --host to expose
  ➜  press h to show help
```
On peut alors ouvrir le lien `http://localhost:5173/` dans le navigateur web de notre choix (Firefox ou Chrome/Chromium). Notre page est disponible sur ce lien et celle-ci se met à jour dès que l'on modifie les fichiers ce qui est plutôt pratique pour développer. Actuellement, vous devez avoir la page d'accueil d'un projet vue de base. 


## Petit point IDE et navigateur

La documentation Vue semble recommander l'usage de VS code. De même, TypeScript que nous allons utiliser est développé par le même éditeur que VScode (MicroSoft). Tous ces outils sont libres et open source. Pour ces différentes raisons, c'est un bon choix d'utiliser VS code (ou l'alternative VScodium qui est une version de VScode sans la télémétrie de Microsoft) et d'installer les plugins `Vue Language Features (Volar)` et `TypeScript Vue Plugin (Volar)`. Webstorm propose à priori aussi un très bon support pour TypeScript et Vue sans installation préalable de plugin.

Installation de vue dans le navigateur??

## La première page


Ouvrez dans votre IDE le dossier `todo_list/src`. Commençons par regarder les deux fichiers `main.ts` et `App.vue`. Le fichier `main.ts` est le point d'entrée principal du site et pour l'instant, il se contente d'importer un composant définit dans le fichier `App` et de le déclarer comme composant principal avec la dernière ligne. Ouvrez le fichier `index.HTML` pour voir à quoi le `#app` fait référence : on pourrait le remplacer par n'importe quel sélecteur valide.

 Nous allons pour l'instant surtout nous concentrer sur le fichier `App.vue`. La première chose à constater c'est qu'il contient du JS entre les balises `<script>`, du `HTML` entre les balises `<template>` et enfin du CSS entre les balises `<style>`. Ce découpage contredit le découpage auquel nous sommes habitués. Nous allons tout de même effectué un découpage, mais il sera composant par composant. C'est la méthode adoptée par de nombreux framework JS. Nous verrons les détails des composants plus tard, mais pour l'instant vous pouvez constater que le JS importe deux composants qui sont ensuite utilisés dans le HTML (`HelloWorld` et `The_ welcome`). 

 Remplacez le contenu de `App.vue` par 
 
```js
	<script setup lang="ts">
		let texte =  "un texte qui s'affiche";
	</script>


	<template>
		<div id="wrapper">
			<h1>TODO liste</h1>
			<p>{{texte}}</p>
		</div>
	</template>

	<style scoped>

	</style>
```
Vérifiez dans le navigateur que l'affichage a bien été mis-à-jour. Vous pouvez aussi supprimer le dossier `src/component` que nous n'utiliserons pas dans le premier exercice.

# Base du réactive, moustache, v-bind, v-on et v-model
Vous avez probablement remarqué dans l'exemple précédent que dans la page HTML `{{texte}}` a été remplacé par le contenu de la variable. C'est la syntaxe "moustache" (double accolades) qui permet l'interpolation de texte. On peut utiliser des expressions plus compliquées. Par exemple, on pourrait écrire `{{texte1+" "+texte2}}` pour afficher la concaténation des deux variables séparés par une espace.

La moustache ne fonctionne que pour du texte. Si l'on veut utiliser une variable dans un attribut il va falloir utiliser `v-bind:monAttribut="maVariable"`. Par exemple, on pourrait ajouter au JS la définition `let url = "https://vuejs.org/";` et au HTML la ligne `<a v-bind:href="url"> Site de vue</a>`. Essayez. Vue fourni plusieurs de ces attributs spéciaux en `v-` qui s'appellent des directives.

On aimerait ensuite rajouter un compteur simple en dessous du texte de notre page. On va ajouter au HTML le bouton suivant : `<button v-on:click="incremente">+</button>`. Cette fois nous utilisons la directive `v-on:` qui indique que l'on cherche à détecter un événement qui est précisé ensuite (donc `v-on:click` détecte un clic alors que `v-on:keyup` détecte une keyup). La valeur donnée est le nom de la fonction JS à appeler (on peut aussi écrire du code directement, mais sauf s'il est particulièrement court, c'est une mauvaise pratique d'écrire le JS au milieu du HTML). 

Vous l'avez compris il faut donc écrire la fonction `incremente`. Voilà ce qu'on peut ajouter dans la partie script :
``` js 
	let compteur = 7;
	function incremente(){
		compteur++;
		console.log(compteur);
	}
```
Vérifiez que cela fonctionne (ouvrez la console pour vérifier l'affichage de la valeur). Vous pouvez maintenant utiliser la syntaxe moustache pour afficher la variable compteur dans votre page. Que se passe-t-il ? 

On peut constater dans la console que la valeur de la variable change, mais l'affichage n'est pas mis à jour. C'est normal, l'interface n'est mis-à-jour que lorsqu'une variable "réactive" change, hors pour l'instant nous n'avons pas défini de variable réactive. Pour rendre la variable compteur réactive, vous allez devoir faire les trois choses suivantes :

1. Importer la fonction `ref` en ajoutant la ligne suivante en haut du script `import {ref} from 'vue';`. Cette ligne indique simplement que nous importons la fonction `ref` depuis le module `vue`. La notion de module permet de séparer le code JS en "blocs logiques" et donc d'avoir des projets mieux organisés. En particulier, plutôt que de devoir inclure tous nos fichiers JS dans l'entête du HTML, on peut ainsi inclure le module JS principale qui importe les modules JS dont il a besoin qui vont eux même importer les modules JS dont ils ont besoin (on a donc des dépendances beaucoup plus claires pour l'humain, pour l'IDE et pour le navigateur). 

2. Modifier la définition de compteur pour utiliser `ref` `ainsi let compteur = ref(7);`.

3. Remplacer toutes les utilisations de compteur dans le script par `compteur.value` (par contre, gardez compteur dans la partie `HTML`).

Faites-le et vérifiez que tout fonctionne. La fonction `ref` retourne un objet réactif qui encapsule l'objet donné en argument. Il faut donc maintenant utiliser `monobjet.value` pour accéder à sa valeur dans le JS, mais en contrepartie dès qu'il change l'interface se met-à-jour correctement (cette fonction utilise l'objet proxy de JavaScript que nous avons utilisé dans un contexte similaire l'an dernier). Il faudra donc définir et utiliser tous les objets réactif de cette manière.

Maintenant, ajouter un bouton `<input type="input">` et utiliser `v-bind` pour que `value` de cet input soit toujours la variable `compteur`. Vérifiez que si l'on change la valeur en cliquant sur le bouton précédent l'affichage se met à jour dans notre nouvel input. Ensuite, utilisez `v-on:input` et la fonction suivante pour que la modification du contenu de l'input mette la variable à jour.

```js
function onInput(e) {
  text.value = e.target.value
}
```

On a réussi à associer une variable à l'input dans les deux directions. En fait, pour faire cela on aurait simplement pu utiliser une troisième directive `v-model`. Remplacez l'input par 
`<input type="input" v-model="compteur">` et supprimez la fonction onInput. Vérifiez que tout fonctionne. De manière générale, `v-model` permet de relier une variable à un input de manière cohérente (par exemple, si `type="checkbox"` alors la variable sera un booléen).


Les directives ont un rôle assez important et nous en verrons d'autres. En particulier, `v-bind:monattribut="mavariable"` et `v-on:event="mavariable"` sont très utilisées et il existe une syntaxe plus courte: `:monattribut="mavariable"` et `@event="mavariable"` que nous allons privilégier à partir de maintenant.


# Réalisation d'une liste de tâches
## Afficher la liste de tâches
Nous allons maintenant réaliser la liste de tâches. Nous allons utiliser une nouvelle directive `v-for` qui permet de faire une boucle. Remplacez le contenu de `App.vue` par le code suivant

```js
	<script setup lang="ts">
		import {ref} from 'vue'; 
		let id = 0;
		const taches = ref([{id:id++, description:"apprendre vue", faite:false},
			{id:id++, description:"finir la SAÉ", faite:false},
			{id:id++, description:"réviser pour l'interro", faite:false}]);
	</script>

	<template>
		<div id="wrapper">
			<ul>
				<li v-for="tache in taches" :key="tache.id">
					<span>{{tache.description}}</span>
				</li>
			</ul>
		</div>
	</template>

	<style scoped>
	</style>
```

Dans la partie JS, nous définissions le tableau `taches` qui contient les éléments de la liste de tâches. Chaque élément de la liste de tâches, possède un texte qui décrit la tâche, un booléen qui décrit si la tâche est effectuée ou non et une id dont nous allons voir l'utilité. L'autre partie intéressante est la ligne `<li v-for="tache in taches" :key="tache.id">` qui permet de faire une boucle sur tous les éléments de `taches` et de créer un `<li>` pour chacun d'entre eux. La partie `:key="todo.id"` permet d'associer une clef différente à chaque élément. Pour l'instant le code pourrait fonctionner sans, mais le fait d'associer une clef à chaque élément permet à Vue de comprendre quel élément correspond à quel élément quand le tableau change (par exemple, si l'on supprime une case au milieu du tableau). Il faut donc toujours associer une clef unique à chaque élément.


## Ajouter et retirer un élément à la liste de tâches

Nous allons maintenant ajouter la possibilité d'ajouter un élément à la liste de tâches.
Pour cela, commencez par ajouter une input texte associé à une variable réactive qu'on pourra appeler `nouvelleTache` (il faut donc utiliser `ref` et `v-model`). On ajoutera un `placeholder` pertinent à l'input. 


Écrivez une nouvelle fonction `ajouterTache` qui ajoute un nouvel élément à `taches` (on pourra utiliser [Array.prototype.push](https://developer.mozilla.org/fr/docs/Web/JavaScript/Reference/Global_Objects/Array/push)). Ce nouvel élément doit posséder une nouvelle id (on peut utiliser `id++`), un texte qui correspond au contenu de la variable `nouvelleTache` et le booléen `faite:false`. La fonction `ajouterTache` devra aussi vérifier que `nouvelleTache` est différent de `""` avant de l'ajouter à `taches` puis réinitialiser la valeur de `nouvelleTache` à `""`.
On va ensuite ajouter un bouton qui appelle `ajouterTache` (on utilisera `@click`). Vérifiez que tout fonctionne.

On aimerait bien que l'ajout d'une tâche ignore les espaces inutiles en début ou fin de chaine (en particulier on aimerait que la chaine "   " soit ignorée tout comme la chaine vide). On a une solution très simple pour ça: le modifier `.trim`. Pour l'utiliser, à la place d'écrire `v-model="nouvelleTache"` on doit écrire `v-model.trim="nouvelleTache"` et le contenu de `nouvelleTache` serra alors le même que celui de l'input, mais en ignorant les espaces de début et de fin. Vérifiez que tout fonctionne.

De manière générale la syntaxe complète des directives est la suivante `name:argument.modifiers="value"`. Par exemple, `v-on:click.prevent="gestionDuclick"` appelle la fonction `gestionDuclick` dès que l'élément est cliqué tout en empêchant la propagation de l'événement (même si en l'occurrence on préfère écrire `@click.prevent="gestionDuclick"`).

On va maintenant ajouter la possibilité de retirer un élément de la liste de tâche. Commençons par ajouter un bouton `<button @click="retirerTache(tache)">retirer</button>` à chaque élément, puis coder la fonction `retirerTache`. On pourra utiliser la méthode [filter](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/filter) en ne gardant que les éléments qui sont différents de `tache`. 
Attention, votre IDE devrait normalement vous dire qu'il y a une erreur (`Parameter 'tache' implicitly has an 'any' type.`). Ignorez cette erreur pour l'instant, nous verrons plus tard ce qu'elle veut dire.
 Vérifiez que tout fonctionne.


On peut ajouter un CSS minimal pour un résultat un peu plus joli. Par exemple,

 ```css
  #wrapper{
    border-radius: 5px;
    border:solid black 2px;
    padding: 10px;
  }
  ul,span{
    padding:10px;
  }
  li{
    list-style: none;
    padding: 2px 0px;
  }
```
![](ressources/todo_liste_mitd.png)


## Sélectionner les taches faites

Ajoutez devant chaque tâche, un input de type checkbox reliée à la valeur `tache.faite` correspondante. 

Pour que chaque tâche faite apparaisse raturée, nous allons ajouter les lignes suivantes au CSS

```css
.fait{
  text-decoration: line-through;
}
```

Il faut ensuite associer la classe `fait` à toutes les tâches qui sont faites. La directive `v-bind:class` fonctionne de manière un peu particulière. L'un des manières de l'utiliser est de lui donner un objet dont les noms des attributs sont les noms classes à ajouter et la valeur de chaque attribut est un booléen qui indique si la classe correspondante doit être ajoutée (ces classes se cumulent avec celles définies en HTML classique avec `class ="..."`). Par exemple, un div avec 

```js
	<div class="blip blop" :class="{ class1: true, class2:false, toto:true }">
```
 aurait donc comme classes `"blip blop class1 toto"`. On peut utiliser la valeur d'une variable pour désigner le booléen (et le nom de la classe). Ajouter au `span` associé à chaque `tache` la classe `fait` si son booléen `tache.faite` est `true`. Vérifiez que tout fonctionne. Notez qu'il est aussi possible d'utiliser `:class` avec une variable qui contient le tableau des classes à ajouter.


## Cacher les todos faits

Nous voulons ajouter un bouton qui permet de cacher/afficher les tâches qui sont déjà faites.
Commençons par ajouter le bouton, dans le JS

```js
  const cacheFaits = ref(false);
```

dans le HTML
```HTML
    <button @click="cacheFaits = !cacheFaits">
      {{ cacheFaits ? 'Tout montrer' : 'cacher les tâche terminées' }}
    </button>
```
Prenez le temps de comprendre le fonctionnement du bouton.

Pour sélectionner les tâches qu'on affiche nous allons modifier la ligne qui boucle sur `taches` ainsi

```html
	<li v-for="tache in tacheFiltrees()" :key="tache.id">
```
Écrivez la fonction `tacheFiltrees` qui utiliser [filter](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/filter) pour renvoyer un tableau qui contient les tâches à afficher (ça dépend donc de `cacheFaits` et de `tache.faite`). Vérifiez que tout fonctionne.

Plutôt que d'utiliser la fonction, nous allons utiliser une propriété calculée `tacheFiltrees`. Il faut commencer par importer la fonction `computed` en changeant l'import au début.

```js
  import {ref, computed} from 'vue'; 
```
On importe maintenant deux fonctions du module `vue`. On aurait pu ajouter une deuxième ligne et faire une ligne par import plutôt que d'importer plusieurs fonctions en une seule ligne.

Ensuite, au lieu de `function tacheFiltrees(){ ... return ...}` on écrira 
`const tacheFiltrees = computed(() => {... return ...})` (on peut invoquer une fonction définie ailleurs au lieu dé définir une fonction sur place). On peut ensuite utiliser `tacheFiltrees` comme si c'était une variable (il faut donc mettre à jour la ligne du `v-for`). Vérifiez que tout fonctionne.

La variable `tacheFiltrees` qu'on a définie est ce qu'on appelle une propriété calculée. Le résultat est calculé et mis à jour automatiquement dès qu'une des variables réactive utilisée est modifiée. Si `tacheFiltrees` est utilisée plusieurs fois entre chaque mise-à-jour cela évite de la recalculer à chaque fois et on peut donc gagner en performance. Il faut faire attention d'éviter le problème inverse qui serait de définir une propriété calculée qu'on doit tout le temps calculer alors qu'on l'utilise rarement. Il faut donc réfléchir au choix le plus pertinent entre méthode et propriété calculée en fonction de la situation. Comme toujours, il se peut aussi qu'un choix soit plus clair et lisible et donc préférable dans certains contextes.


Notre liste de tâche est terminée. Vous pouvez améliorer le CSS avant de passer à la suite.

## Et TypeScript dans tout ça ?
Avant d'expliquer l'usage de TypeScript considérez les deux instructions suivantes `let a="4"; a=a+1;`. Quel est le résultat ? Vérifiez dans la console du navigateur. Une connaissance suffisante du langage permet de répondre correctement ici, mais dans un projet de plusieurs milliers de lignes de code, le typage permet d'éviter ce genre de difficultés. 

TypeScript est simplement une légère surcouche à JavaScript qui permet de typer son code. Il va donc permettre de déclarer des types dans les déclarations de variables et de fonctions. Cela permet ensuite à l'IDE et à divers outils de développement de détecter automatiquement différents bugs et de proposer de la complétion automatique plus efficacement ce qui est crucial pour de gros projets. 

Le fonctionnement de TypeScript très simple : il fournit quelques manières d'annoter le code avec des types et pour produire un code JavaScript valide il suffit d'effacer toutes ces annotations. Normalement, il faut donc un outil spécial (qu'on peut facilement installer avec `npm`) pour transformer le code TypeScript en JavaScript, mais dans notre cas vite se charge de gérer Vue et TypeScript en même temps.

La fonctionnalité principale de TypeScript est de pouvoir définir le type d'une variable avec la syntaxe `mavariable:montype`. Ça n'est pas forcément utile quand on déclare une variable puisque TypeScript associe automatique le type string à cette affectation par exemple 

```js
	let helloWorld = "Hello World"
```
mais on pourrait écrire

```js
	let helloWorld:String = "Hello World"
```
Par contre, on peut l'utiliser dans les définitions de fonctions ` function maFonction(param1:type1):typeDeRetour` et TypeScript pourra ainsi vérifier que tout se passe correctement lors de l'appel de cette fonction. L'autre fonctionnalité principale est qu'il permet de définir des types en particulier les interfaces et les unions.

Remplacer temporairement votre fonction `ajouterTache` par celle-ci
 
```js
  function ajouterTache(){
    if(nouvelleTache.value === "") return;
    taches.value.push({id: id++, description: nouvelleTache.value, faite: "false"});
    nouvelleTache.value = "";
  }
```
Normalement votre IDE devrait vous indiquer qu'il s'attendrait à voir un booléen à la place d'une String ici et c'est TypeScript qui lui permet cela. Annulez ce changement.


Normalement, votre IDE soulève déjà une première erreur TypeScript que nous avons ignoré jusqu'à présent

```
Parameter 'tache' implicitly has an 'any' type.
```

Le type `any` est un type spécial qui correspond à "n'importe quel type". Ici l'IDE nous dit qu'il a été obligé de deviner que c'était le type attendu pour cette fonction. On peut dans un premier temps corriger l'erreur en corrigeant la déclaration de `function retirerTache(tache:any)`. Ce n'est évidemment pas très utile de mettre `any` partout puisqu'il y a alors trop peu d'information pour que les types permettent de détecter une erreur. Nous allons donc définir une interface. Ajoutez le code suivant au début du JS.

```js
  interface Tache {
    id: number;
    description:string;
    faite:boolean;
  }
```
Remplacez `tache:any` par `tache:Tache`, normalement votre IDE arrive à vérifier que tout va bien. Essayer `tache:String` ou `tache:number` pour voir ce que votre IDE vous indique. En passant notre souris sur la définition de `Tache` on voit que le type est plutôt compliqué. On peut le forcer à identifier qu'il contient un tableau de `Tache`. Pour cela il faut importer le type `Ref` avec la ligne `import type { Ref } from 'vue';` et on peut ensuite lui assigner le type `const taches:Ref<Tache[]>` et tout va bien (`Tache[]` est le type d'un tableau de `Tache`, mais ici on a une `Ref` vers un tableau de `Tache` à cause de la fonction `ref()`). 

À partir de maintenant nous essayerons de donner des types corrects aux objets, mais vous savez déjà presque tout ce qu'il faut savoir pour utiliser TypeScript.

# Tableau de bord de listes de tâches 
Nous allons repartir de notre exemple de liste de taches, pour le faire évoluer en une application qui permet de créer plusieurs listes de taches avec des noms différents (on pourrait imaginer les listes de taches "administratif", "ménage" et "jardinage"). C'est surtout l'occasion de parler de la dernière notion centrale de Vue que nous n'avons pas encore évoquée : les composants.

## Premiers composants et props


 BLABLABLA idée illustration screenshot truc final ?


 Pour définir notre premier composant ajouter dans le dossier src un dossier `composants` et à l'intérieur de ce dossier créer un fichier `ListeDeTaches.vue`. Copiez le code de `App.vue` dans `ListeDeTaches.vue`. Et voilà nous avons créé un composant (en fait, `App.vue` définissait déjà un composant). Pour utiliser ce composant remplacez le code de `App.vue` par le suivant.

 ```ts
  <script setup lang="ts">
      import ListeDeTaches from './composants/ListeDeTaches.vue'; 
  </script>

  <template>
      <ListeDeTaches />
  </template>

  <style>
  </style>
```
Normalement, votre site fonctionne toujours, mais nous avons utilisé notre premier composant. La ligne `import ...` permet de rendre le composant accessible et on voit qu'on peut alors l'utiliser dans le html. Une grosse partie du travail de Vue est de faire fonctionner tout ça. Notez que le nom de l'import à faire `ListeDeTaches` a été déduit automatiquement du nom du fichier qui le contient.

Trouvez comment afficher trois listes de taches dans la page (indice: c'est très simple). Notez que ces listes sont bien trois listes différentes et que modifier l'une d'entre elle n'impacte pas les autres.  

On aimerait bien donner des noms à nos différentes listes de tâches. Pour que ce soit utilisable, il faut que le nom de la liste se comporte comme un attribut ou un paramètre de la liste de tâche. C'est ce qu'on appelle les "props" du composant (les "accessoires" en français). Pour définir, les props d'un composant on va utiliser la fonction `defineProps`. Ajoutez la ligne suivant en haut du composant `ListeDeTaches`:

```ts
  const props = defineProps({titre:String});
```
Cette ligne indique que ce composant possède un prop `titre` qui est une String. Pour utiliser le prop, on peut simplement écrire props.titre. Pour donner sa valeur au prop on utiliser la même syntaxe que pour un attribut html

```ts
  <ListeDeTaches titre="Menage"/>
```

Donnez un titre aux trois listes de tâches et modifier le composant `ListeDeTaches` pour qu'il affiche son titre dans une balise `<h2>` en haut de la liste. Vous devriez obtenir quelque chose qui ressemble à ça:

![](ressources/listes_etape2.png)

Vous pouvez prendre le temps de personnaliser un peu votre CSS.

Nous voulons maintenant ajouter la possibilité d'ajouter des listes de tâches. Voilà ce qu'il va falloir faire (inspirez vous du fonctionnement de `ListeDeTâche`):

1. Créer un tableau `listes` qui contient les ids et les titres des listes

2. Utiliser la directive `v-for` pour créer une liste de tâche pour chaque élément du tableau `listes`. Pour donner les titres en props, nous avons vu en début de TD comment passer la valeur d'une variable dans un attribut.

3. Ajouter un input textuel et un bouton qui permettent d'ajouter un élément au tableau `listes` quand on clique sur le bouton.

4. Vérifier que tout fonctionne.

## Emit and emitted events
Nous aimerions rajouter un bouton dans chaque liste qui permet de supprimer la liste. Une nouvelle difficulté se présente à nous: si le bouton est dans le composant ListeDeTache alors c'est ce composant qui peut détecter que le bouton de suppression a été activé il faut donc faire remonter l'information à son parent (`App`) pour qu'elle puisse le supprimer. Vue offre une manière très simple de surmonter ce problème: la fonction `$emit`. Quand elle est appelée la fonction `$émit("nomDevenement")` déclenche un événement qui peut être écouté par le parent du composant.

Ajoutons le bouton suivant dans ListeDeTache (vous pouvez styliser un peu votre CSS pour qu'il soit mieux positionné et moins laid) :

```ts 
  <button @click="$emit('supprime')">x</button>
```
Cette ligne de code indique que quand on clique sur ce bouton cela déclenche l'événement supprime. On peut maintenant détecter l'événement en ajoutant la directive  `@supprime="gestionDeLaSuppression"` dans un composant ListeDeTache. Dans notre cas ça donne

```ts
  <ListeDeTaches v-for="liste in listes" :key="liste.id" :titre="liste.titre" @supprime="retirerListe(liste)"/>
```
Il ne reste plus qu'à coder la fonction `retirerListe` (qui fonctionne comme la fonction `retirerTache`). Vérifier que tout fonctionne. N'oubliez pas de donner le type de l'argument de la fonction retirer liste vous pouvez soit définir une interface soit utiliser:
```ts
  function retirerListe(liste:{id:number,titre:string})
```
Notez qu'on peut ajouter des arguments à un événement. 


## Écrire le composant tâche

Nous avons écrit notre premier composant, nous allons maintenant extraire du code de ListeDeTaches un composant pour améliorer son code. Plus précisément nous allons essayer d'extraire la partie qui correspond au HTML suivant:

```html
        <input type ="checkbox" v-model="tache.faite"> 
        <span :class="{fait: tache.faite}">{{tache.description}}</span>
        <button @click="retirerTache(tache)">Retire</button>
```

Vous devez être capable d'imaginer comment le faire en utilisant des `props`, `emmit`, et `emmited event`. Commençons par définir notre composant `TacheDeListe`:

```vue
  <script setup lang="ts">
    import {ref } from 'vue';
  </script>

  <template>
      <input type ="checkbox" v-model="tache.faite"> 
      <span :class="{fait: tache.faite}">{{tache.description}}</span>
      <button @click="retirerTache(tache)">Retirer</button>
  </template>

  <style scoped>
    .fait{
      text-decoration: line-through;
    }
  </style>
```

Il y a plusieurs problèmes à régler pour rendre de composant fonctionnel. Premièrement, l'objet JavaScript `tache` n'est pas accessible ici. Une mauvaise manière de régler le problème serait de le passer dans les `props`. En faisant cela, on passerait dans les props des valeurs qui n'ont aucun sens pour `TacheDeListe`. L'`id` n'a rien à y faire, mais en fait même le booléen `faite`. En effet, le booléen `faite` est crée et géré par `TacheDeListe` et la seule raison de vouloir le passer en prop serait de vouloir le modifier depuis un composant. Cependant, un composant ne doit pas modifier ses props, si un composant doit modifier quelque chose chez son parent on utilise `$emit`. 

Nous allons donc déclarer un seul prop et la variable faite serra déclarée localement dans le composant.

```js
  const props = defineProps({nomTache: String});
  const faite = ref(false); 
```
Faites les trois changements correspondants dans le `template`. Ensuite plutôt que d'appeler une fonction `retirerTache` on veut que le clique sur le bouton émet un signal `$emit('retirerTache')` qu'on pourra donc détecter dans le composant `ListeDeTache`. 
Le composant n'est pas encore parfaitement fonctionnel, mais il ne cause plus d'erreur nous pouvons donc l'inclure dans notre page pour le tester au fur et à mesure. Nous allons donc l'importer dans `ListeDeTaches.vue` avec la ligne :

```js
  import TacheDeListe from '@/components/TacheDeListe.vue'; 
```
Notez l'usage de `@` qui fait référence au dossier racine du site, on va toujours utiliser ce type de chemin à partir de maintenant pour éviter des problèmes de résolution de chemin. Pour utiliser le composant remplacez les trois lignes à l'intérieur du `<li>` par :

```vue
  <TacheDeListe :nom-tache="tache.description" />
```

Vérifiez que le site s'affiche correctement. Il reste deux problèmes : les boutons "retirer" et le bouton "cacher les tâches" ne fonctionnent plus. 

Pour le bouton retirer, il suffit d'appeler la fonction `retirerTache` quand on détecte l'événement `supprimerTache`. Cela devrait suffire à faire fonctionner le bouton "Retirer". Cependant, si vous ouvrez le terminal de votre navigateur vous devriez apercevoir des warnings qui concerne l'événement `supprimerTache`. Le contenu de ce warning est assez compliqué, cependant il nous indique une manière de le régler qui est de déclarer explicitement l'événement. Pour ce faire, on utilise la fonction `defineEmits` dans le fichier du composant. Il y a plusieurs syntaxes, nous allons utiliser la syntaxe suivante (qui n'est valide qu'en TypeScript) :
 
```js
  const emit = defineEmits<{
    premierEvenementAEnregistrer: [premierArgument: sonType, deuxiemeArgument:sonType]
    premierEvenementAEnregistrer: [premierArgument: sonType]
  }>();
```
Dans notre cas nous définissons un seul événement et celui ci n'attend pas d'argument donc on écrira en haut de `ListeDeTaches.vue` :

```js
  const emit = defineEmits<{supprimerTache:[]}>();
```
Il existe d'autres syntaxes qui ne demandent pas de définir les types, mais puisque nous utilisons TypeScript c'est une bonne habitude d'utiliser les types autant que possible.

Il reste à réparer le bouton "Cacher les tâches terminées / Montrer tout". Le problème est que le booléen `tache.faite`, n'est pas mis à jour par un clic sur la case à cocher. La solution est de déclencher un événement dès que ce bouton est cliqué qui indique au parent de changer la valeur du booléen correspondant. Plutôt que de le faire directement nous allons définir un `v-model` pour notre composant. Revenons dd'abord sur le fonctionnement de `v-model`. Étant donné une variable de type `string` on peut associer son contenu avec un input avec la ligne suivante 

```html
<input v-model="variableTexte" />
```
En fait, on peut obtenir le même comportement que `v-model` en utilisant `v-bind` et `v-on`:

```js
<input
  :value="variableTexte"
  @input="variableTexte = $event.target.value"
/>
```
De la même manière, la ligne `<monComposantPersonalise v-model="maVariable">` est automatiquement transformé en :

```vue
<monComposantPersonalise
  :modelValue="maVariable"
  @update:modelValue="(nouvelleValeur) => maVariable = nouvelleValeur"
/>
```

Il suffit alors de définir le `prop` et l'évennement correspondant dans le composant `monComposantPersonalise`:

```vue
  <script setup>
    defineProps({modelValue:monType});
    defineEmits<{
    update:modelValue: [premierArgument: sonType, deuxiemeArgument:sonType]
    }>();
  </script>

  <template>
    <input
      :value="modelValue"
      @input="$emit('update:modelValue', $event.target.value)"
    />
  </template>
```



# Déployer le site
Pour l'instant, nous avons utilisé Vite pour faire tourner notre site sur un serveur de développement. Nous allons maintenant discuter les étapes à suivre pour pouvoir déployer notre site sur un serveur en production. Vite permet de facilement "construire" ("build" en anglais, cette étape transforme le code et ressemble un peu à une étape de compilation) le site qui sera déployé. Cette étape de construction transforme notre code Vue en un code JS beaucoup plus court et donc plus léger à envoyer sur le web, mais beaucoup moins lisible pour un humain.
 
Nous allons d'abord devoir ouvrir le fichier de configuration `vite.config.ts` qu'on peut utiliser pour configurer un grand nombre d'options. Nous allons devoir indiquer le chemin de base du site web. En effet, Vite part du principe que le site que nous construisons sera le seul site servi par le serveur web et sera donc à la racine du site. C'est habituellement, ce que l'on fait pour un site professionnel, mais puisque nous avons un seul serveur web pour tous nos TDs à l'IUT nous ne pouvons pas procéder ainsi. Commencer par choisir là où vous allez ranger votre site, par exemple, vous pouvez créer les dossiers `TD_vueJS/todo_list` dans votre `public_html`. Ensuite, ajoutez le champ `base: '/le_chemin_vers_là_où_le_site_sera_déployé'` qui permet d'indiquer le chemin vers la base du site (là où le `index.html` sera accessible) à la fin du `export default defineConfig`. Si vous avez suivi le nommage proposé la ligne devrait ressembler à `base: '******todo*****/TD_vueJS/todo_list` ou `base: 'http://******todo*****/TD_vueJS/todo_list` (les deux sont possibles). Nous reparlerons de ce fichier plus tard pour pouvoir installer un proxy qui nous permettra d'utiliser notre API.

Pour déployer le site, il ne reste plus qu'à "construire" son code. Allez dans votre terminal tapez "q" pour couper temporairement le serveur, puis entrez 

```
npm run build
```

Voilà, le code de notre site est disponible dans le dossier "todo_list/dist". Il ne reste plus qu'à le copier dans le dossier `public_html/le_chemin_vers_là_où_le_site_sera_déployé` et à vérifier qu'il est effectivement disponible à l'adresse `http://******todo*****/...`. N'oubliez pas de relancer le serveur de développement avec `npm run dev` avant de reprendre le TD.
`	


# Conclusion

