---
title: TD2 -- Les composants en Vue.js
lang: fr
---

<!-- # ![](../assets/logo.jpeg) JavaScript -- Vue.js

### IUT Montpellier-Sète – Département Informatique

## TD2 -->
{% raw %}



## Tableau de bord de listes de tâches

Nous allons repartir de notre exemple de liste de tâches du TD précédent, pour le faire évoluer en une application qui permet de créer plusieurs listes de tâches avec des noms différents (on pourrait imaginer les listes de tâches "administratif", "ménage" et "jardinage"). C'est surtout l'occasion de parler de la dernière notion centrale de Vue que nous n'avons pas encore évoquée : les composants.

### Premier composant et *props*

Vous êtes normalement familier avec la structure arborescente d'une page web. Une balise contient des balises qui contiennent des balises qui contiennent des balises... L'idée derrière les composants est de définir nos propres "balises" avec des comportements bien définis et de pouvoir les utiliser à l'intérieur d'autres composants.
Cela permet donc de découper le code d'un site web en fonction des blocs de fonctionnalité : on va pouvoir définir dans des fichiers séparés le composant *header*, le composant *footer*, le composant menu et dans un autre le composant *page principale* (probablement lui-même constitué de plusieurs composants) et on écrira un composant qui assemble tous ces composants ensemble. Vous connaissez la notion de composition en programmation objet et ici l'idée est exactement la même. 
Si plusieurs morceaux de notre page utilisent des fonctionnalités similaires, on va pouvoir réutiliser plusieurs fois le même composant. On peut d'ailleurs aussi très facilement utiliser les composants écrits par d'autres utilisateurs.





<div class="exercice" markdown="1" >

**Exercice :** Pour définir notre premier composant, ajouter dans le dossier src un dossier `composants` et à l'intérieur de ce dossier créer un fichier `ListeDeTaches.vue`. Copiez le code de `App.vue` dans `ListeDeTaches.vue`. Et voilà nous avons créé un composant (en fait, `App.vue` définissait déjà un composant). Pour utiliser ce composant, remplacez le code de `App.vue` par le suivant :


 ```vue
<script setup lang="ts">
    import ListeDeTaches from '@/composants/ListeDeTaches.vue';
</script>

<template>
    <ListeDeTaches />
</template>

<style scoped>
</style>
```

</div>

Normalement, votre site fonctionne toujours, mais nous avons utilisé notre premier composant. Le `@` dans le chemin correspond à la racine du site. Utiliser ce symbole permet d'éviter des problèmes de chemins. 

Il se peut que votre IDE signale que le fichier du composant n'existe pas, relancer l'IDE à la racine du projet semble régler le problème. 

La ligne `import ...` permet de rendre le composant accessible et on peut alors l'utiliser dans `<template>` comme si c'était une balise HTML. Une grosse partie du travail de *Vue* est de faire fonctionner tout ça. 

>Notez que l'import est légèrement différent de ceux qu'on a fait jusqu'à maintenant qui ressemblaient à
>
>```js
>   import {ref} from 'vue';
>```
>En fait, un module JS peut exporter un certain nombre de fonctions, variables et autres par leur nom et quand on utilise la syntaxe `import {nom1, nom2} from 'module'` on liste précisément les imports qu'on fait. Les modules JS peuvent aussi définir un export principal et quand on utilise la commande sans accolades `import nomLocal from 'module'` on demande à importer l'export principal (et il sera accessible sous le `nomLocal` que j'ai choisi). À partir du code de `ListeDeTache.vue` vue définit en fait un composant dont l'export principal correspond au composant.



<div class="exercice" markdown="1" >

**Exercice :** Trouvez comment afficher trois listes de tâches dans la page (indice : c'est très simple). Vérifiez que ces listes sont bien trois listes différentes et que modifier l'une d'entre elle n'impacte pas les autres.

</div>

On aimerait bien donner des noms à nos différentes listes de tâches. Pour que ce soit utilisable, il faut que le nom de la liste se comporte comme un attribut ou un paramètre de la liste de tâche. C'est ce qu'on appelle les "props" du composant (les "accessoires" en français). Pour définir, les props d'un composant, on va utiliser la fonction `defineProps` comme ceci :

```ts
  const props = defineProps<{prop1: type1, prop2: type2, prop3: type3}>();
```
Cette ligne indique que le composant possède 3 props appelés `prop1`, `prop2` et `prop3` de type respectifs `type1`, `type2` et `type3`. La syntaxe utilisée est celle des "generics" et elle devrait vous rappeler celle des types paramétrés en Java (qui sont essentiellement la même chose). Nous ne rentrerons pas dans les détails de leur fonctionnement. Sachez qu'il existe deux autres syntaxes dont une en JS pure qui n'utilise donc pas de type. Pour indiquer que notre composant possède un prop `titre` qui est de type `string`, il faut donc écrire :

```ts
  const props = defineProps<{titre: string}>();
```

<div class="exercice" markdown="1" >

**Exercice :** Ajoutez cette ligne en haut du composant `ListeDeTaches`.

</div>

 Pour utiliser le prop, on peut simplement écrire `props.titre`. Pour donner sa valeur au prop quand on utilise le composant, on utilise la même syntaxe que pour un attribut HTML

```vue
<ListeDeTaches titre="Menage"/>
```
Pour l'associer à une variable, on peut utiliser `v-bind` comme pour n'importe quel attribut

```vue
<ListeDeTaches v-bind:titre="maVariable"/>
ou
<ListeDeTaches :titre="maVariable"/>
```

<div class="exercice" markdown="1" >

**Exercice :** Donnez un titre aux trois listes de tâches et modifier le composant `ListeDeTaches` pour qu'il affiche son titre dans une balise `<h2>` en haut de la liste.

</div>

Vous devriez obtenir quelque chose qui ressemble à ça:

![](../assets/listes_etape2.png)

Vous pouvez prendre le temps de personnaliser un peu votre CSS.

### Ajouter des listes de tâches

Nous voulons maintenant ajouter la possibilité d'ajouter des listes de tâches. Voilà ce qu'il va falloir faire :

1. Créer un tableau `listes` qui contient les `ids` et les titres des listes

2. Utiliser la directive `v-for` pour créer une liste de tâche pour chaque élément du tableau `listes`. Pour donner les titres en props, nous avons vu en début de TD comment passer la valeur d'une variable dans un attribut.

3. Ajouter un input textuel et un bouton qui permettent d'ajouter un élément au tableau `listes` quand on clique sur le bouton.

<div class="exercice" markdown="1" >

**Exercice :**

1. Faites-le et vérifiez que tout fonctionne. Vous pouvez vous inspirer du fonctionnement de `ListeDeTâche`.
2. Si vous ne l'avez pas fait depuis un moment, ouvrez l'onglet *Vue* des outils de développement du navigateur. Constatez qu'on peut voir l'arborescence des composants qui évolue en direct. Prenez le temps d'expérimenter un peu.


</div>

### Emit and emitted events

Nous aimerions rajouter un bouton dans chaque liste qui permet de supprimer la liste. Une nouvelle difficulté se présente à nous : si le bouton est dans le composant `ListeDeTache` alors c'est ce composant qui peut détecter que le bouton de suppression a été activé. Il faut donc faire remonter l'information à son parent (`App.vue`) pour qu'elle puisse le supprimer. *Vue* offre une manière très simple de surmonter ce problème : la fonction `$emit`. Quand elle est appelée, la fonction `$emit("nomEvenement")` déclenche un événement qui peut être écouté par le parent du composant.

<div class="exercice" markdown="1" >

**Exercice :** Ajoutons le bouton suivant dans `ListeDeTache` (vous pouvez styliser un peu votre CSS pour qu'il soit mieux positionné et moins laid) :

```vue
<button @click="$emit('supprime')">x</button>
```

</div>

Cette ligne de code indique que quand on clique sur ce bouton, cela déclenche l'événement *supprime*. On peut maintenant détecter l'événement en ajoutant la directive  `@supprime="truc à faire"` dans un composant ListeDeTache. Dans notre cas ça donne

```vue
<ListeDeTaches v-for="liste in listes" :key="liste.id"
  :titre="liste.titre"
  @supprime="retirerListe(liste)"
/>
```

Il ne reste plus qu'à coder la fonction `retirerListe` (qui fonctionne comme la fonction `retirerTache`).

<div class="exercice" markdown="1" >

**Exercice :** Faites le et vérifier que tout fonctionne. N'oubliez pas de donner le type de l'argument de la fonction `retirerListe`. Son type est `{id:number,titre:string}`, mais nous pouvons définir une interface pour le type `Liste`. On peut ensuite préciser manuellement le type de notre tableau de listes comme nous avions fait dans `ListeDeTaches.vue`.

</div>

## Écrire le composant tâche

Nous avons écrit notre premier composant, nous allons extraire du code de `ListeDeTaches` et définir un composant pour améliorer le découpage du code. Plus précisément, nous allons essayer d'extraire la partie qui correspond au HTML suivant :

```vue
<input type ="checkbox" v-model="tache.faite">
<span :class="{fait: tache.faite}">{{tache.description}}</span>
<button @click="retirerTache(tache)">Retire</button>
```

Nous allons donc créer un composant `TacheDeListe.vue` qui contient ce HTML et que nous utiliserons dans `ListeDeTaches.vue`. Vous devez être capable d'imaginer comment le faire en utilisant des `props`, `emit`, et `emited event`.

<div class="exercice" markdown="1" >

**Exercice :**
Commençons par définir notre composant `TacheDeListe.vue`:

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

</div>

Il y a plusieurs problèmes à régler pour rendre de composant fonctionnel. 
Premièrement, l'objet JavaScript `tache` n'est pas accessible ici. Une mauvaise manière de régler le problème serait de le passer en entier dans les `props`. En faisant cela, on passerait dans les *props* des valeurs qui n'ont aucun sens pour `TacheDeListe` et il serait plus difficile de rendre le composant "générique" pour qu'il soit réutilisable dans un autre contexte. Par ailleurs, un composant n'a pas le droit de modifier ses *props* (cela pourrait avoir des effets de bords difficilement contrôlables chez le composant parent). Il va donc falloir réfléchir un peu plus pour gérer le booléen `faite`.


### Déclarer les props

Nous allons déclarer deux *props*: le nom de la tâche et le booléen `cochee`.

```js
const props = defineProps<{descriptionTache: string, cochee:boolean}>();
```

<div class="exercice" markdown="1" >

**Exercice :**

1. Ajoutez ces lignes et faites les trois changements correspondants dans le `template`. Attention, nous allons remplacer `v-model="tache.faite"` par `:checked="props.cochee"`, car mettre `cochee` dans `v-model` signifierait que cocher la case peut modifier `props.cochee`, mais un composant n'a pas le droit de modifier ses props !

2. Ensuite, plutôt que d'appeler une fonction `retirerTache`, on veut que le clic sur le bouton émette un signal `$emit('supprimerTache')` (qu'on pourra ensuite détecter dans le composant `ListeDeTache`.)

</div>

### Utiliser le composant dans la liste de tâche

Le composant n'est pas encore parfaitement fonctionnel, mais il ne cause plus d'erreur. Nous pouvons donc l'inclure dans notre page pour le tester au fur et à mesure. Nous allons l'importer dans `ListeDeTaches.vue` avec la ligne :

```js
import TacheDeListe from '@/composants/TacheDeListe.vue';
```
Pour utiliser le composant, remplacez les trois lignes à l'intérieur du `<li>` par :

```vue
<TacheDeListe
    :description-tache="tache.description"
    :cochee="tache.faite"
/>
```
Notez que `descriptionTache` est devenu `description-tache`. En HTML, les noms d'attributs ne sont généralement pas en CamelCase mais en kebab-case, ce qui n'est normalement pas possible en JS (vous avez normalement déjà vu ce problème notamment pour les styles en CSS: `background-color` devient `backgroundColor`). Ici, on pourrait utiliser `descriptionTache`, mais on peut aussi utiliser `description-tache`. Le plus important est d'être cohérent tout le long d'un projet.

<div class="exercice" markdown="1" >

**Exercice :** Faites l'import et utilisez le composant `TacheDeListe` dans `ListeDeTaches`.
Vérifiez que le site s'affiche correctement.

</div>

Il reste trois problèmes : les boutons "retirer" ne fonctionnent plus, le bouton "cacher les tâches" ne fonctionne plus et cocher une case ne barre pas la tâche.

### Réparer le bouton retirer et déclarer les emits
Pour le bouton retirer, il suffit d'appeler la fonction `retirerTache` quand on détecte l'événement `supprimerTache`.


<div class="exercice" markdown="1" >

**Exercice :**  Faites-le.

</div>

Cela devrait suffire à faire fonctionner le bouton "Retirer". Cependant, si vous ouvrez le terminal de votre navigateur, vous devriez apercevoir des warnings qui concernent l'événement `supprimerTache`. Le contenu de ce warning est assez compliqué, heureusement il nous indique une manière de le régler qui est de déclarer explicitement l'événement. Pour ce faire, on utilise la fonction `defineEmits` dans le fichier du composant. Il y a plusieurs syntaxes, nous allons utiliser la syntaxe suivante (qui n'est valide qu'en TypeScript) :
 
```js
const emit = defineEmits<{
  premierEvenementAEnregistrer: [premierArgument: sonType, deuxiemeArgument:sonType]
  deuxiemeEvenementAEnregistrer: [premierArgument: sonType]
}>();
```

<div class="exercice" markdown="1" >

**Exercice :**
Dans notre cas, nous déclarons pour l'instant un seul événement et celui-ci n'attend pas d'argument donc on écrira en haut de `TacheDeListe.vue` :

```js
const emit = defineEmits<{supprimerTache:[]}>();
```

</div>

Il existe d'autres syntaxes qui ne demandent pas de définir les types, mais puisque nous utilisons TypeScript c'est une bonne habitude d'utiliser les types autant que possible. De même, il n'est pas obligatoire de déclarer les `emit`, mais c'est une bonne pratique qui peut permettre de régler de nombreux bugs. Déclarer les événements qu'un composant peut émettre et leurs types améliore aussi l'autocomplétion de l'IDE et la vérification des types par TypeScript.

<div class="exercice" markdown="1" >

**Exercice :**
Dans `ListeDeTaches.vue`, déclarez l'événement `supprime` puisque ce composant peut émettre cet événement.

</div>

### Réparer le booléen "faite" et définir le `v-model`

Il reste à réparer le bouton "Cacher les tâches terminées / Montrer tout" et le raturage des tâches terminées. Le problème est que le booléen `tache.faite`, n'est pas mis à jour par un clic sur la case à cocher. Il pourrait être tentant de modifier le *prop*, mais un composant n'a pas le droit de modifier ses *props* ! (C'est techniquement possible dans certains cas, mais c'est fortement déconseillé même dans ces cas-là !) La solution est de déclencher un événement dès que ce bouton est cliqué qui indique au parent du composant de changer la valeur du booléen correspondant. Nous allons donc devoir définir un événement qui prend un argument (pour indiquer la valeur actuelle de la case `true`/`false` si elle est cochée ou non).

<div class="exercice" markdown="1" >

**Exercice :**
Commencez par définir un nouvel événement en modifiant la définition de `emit`

```ts
const emit = defineEmits<{
  supprimerTache:[],
  checkedChange:[boolean]}
>();
```
La nouvelle ligne définie un nouvel événement qui produit une valeur booléenne. Maintenant, nous pouvons ajouter l'attribut suivant au bouton de `TacheDeListe`:

```vue
@change="$emit('checkedChange', ($event.target as HTMLInputElement).checked)"
```

</div>

L'expression `($event.target as HTMLInputElement).checked` est équivalente à `$event.target.checked` (qui vaut `true` si la case est cochée), mais elle indique à TypeScript que ici `$event.target` est forcément un `HTMLInputElement` ce qu'il ne sait pas déduire tout seul. Sans cela, il détecte une erreur qui n'en est pas une (il existe d'autres manières de contourner ce problème).

Il suffit maintenant de détecter l'événement `checkedChange` dans `ListeDeTaches` et de mettre à jour le booléen quand il change avec sa nouvelle valeur. Ce qu'on peut faire en ajoutant le code suivant au bon endroit :
```vue
@checkedChange="(v) => tache.faite=v"
```

<div class="exercice" markdown="1" >

**Exercice :** Faites-le, vérifiez que tout fonctionne à nouveau et prenez le temps de comprendre tout ce que nous venons de faire.

</div>

Plutôt que de définir un `prop` et un événement, nous aurions pu définir un `v-model` pour notre composant. Revenons d'abord sur le fonctionnement de `v-model`. Étant donné une variable de type `string` on peut associer son contenu avec un `<input>` avec la ligne suivante

```vue
<input v-model="variableTexte" />
```

En fait, on peut obtenir le même comportement que `v-model` en utilisant `v-bind` et `v-on`:

```vue
<input
  :value="variableTexte"
  @input="variableTexte = $event.target.value"
/>
```

Pour une input checkbox ce serait plutôt

```vue
<input type="checkbox" v-model="variableBool" />
```
est équivalent à :

```vue
<input
  type="checkbox"
  :checked="variableBool"
  @change="variableBool = $event.target.checked"
/>
```

De la même manière, la ligne `<monComposantPersonalise v-model="maVariable">` est automatiquement transformé en :

```vue
<monComposantPersonalise
  :modelValue="maVariable"
  @update:modelValue="(nouvelleValeur) => maVariable = nouvelleValeur"
/>
```

Il suffit alors de définir le `prop` et l'événement correspondant dans le composant `monComposantPersonalise`:

```vue
<script setup>
  defineProps({modelValue:monType});
  defineEmits<{
  "update:modelValue": [premierArgument: sonType, deuxiemeArgument:sonType]
  }>();
</script>

<template>
<input type="checkbox" :checked="modelValue"
  @change="$emit('update:modelValue', ($event.target as HTMLInputElement).checked)"
/>
</template>
```

<div class="exercice" markdown="1" >

**Exercice :**

1. Faites les modifications nécessaires en remplaçant `cochee` par `modelValue` et `checkedChange` par `update:modelValue` dans `TacheDeListe.vue`. (Attention dans `defineEmits` à bien mettre `"update:modelValue"` sinon JavaScript est gêné par les `:`).

2. Modifiez `ListeDeTache` pour utiliser `v-model` ainsi

    ```vue
    <TacheDeListe
      :description-tache="tache.description"
      v-model="tache.faite"
      @supprimer-tache="retirerTache(tache)"
    />
    ```

3. Vérifiez que tout fonctionne à nouveau.

4. Profitez de l'instant.

</div>

Retenez qu'on utilise les *props* pour faire descendre de l'information d'un composant vers son enfant et les événements pour faire remonter l'information. La directive `v-model` permet de faire descendre et remonter l’information, mais c’est simplement un raccourci qui utilise un *prop* et un évènement. Nous n'en parlerons pas ici, mais on peut "nommer" les `v-model` ce qui permet d'associer plusieurs `v-model` au même composant.

## Utiliser le slot

Jusqu'à maintenant nous avons utilisé nos composants comme des balises HTML autofermantes `<monComposant />`, mais il est aussi possible de les utiliser avec la syntaxe `<monComposant>...</monComposant>`. Le composant pourra utiliser le code HTML écrit entre les 2 balises. Supposons que j'utilise mon composant ainsi

```vue
<monComposant><span class="toto">du texte</span></monComposant>
```
et que dans le `<template>` de mon composant est

```vue
<template>
  du html
  <slot></slot>
  du html
</template>
```

alors la balise `<slot>` est automatiquement remplacée par le HTML contenu entre les balises `<monComposant>` : `<span class="toto"> du texte</span>`. On peut voir cela un peu comme une autre manière d'envoyer de l'information à un composant enfant. La différence majeure avec les *props* est qu'on passe ici du HTML qui sera bien interprété comme du HTML au niveau du composant enfant. Ce HTML est généré par le parent, peut contenir des variables du parent et utilisera le CSS défini par le parent. L'autre différence importante est que le sens sémantique est assez différent : intuitivement, on comprend la différence entre le contenu d'une balise et les attributs d'une balise. Nous n'en parlerons pas, mais il est possible de définir plusieurs `slot` pour la même balise.


<div class="exercice" markdown="1" >

**Exercice :** Retirer le prop `description-tache` de `TacheDeVue` et utiliser `<slot></slot>` à sa place dans le `<template>`. 

Modifier ensuite son utilisation dans `ListeDeTaches`: au lieu de passer `tache.description` dans un attribut, utiliser la syntaxe moustache à l'intérieur de la balise. Vérifiez que tout fonctionne.

</div>

Ici l'utilisation que nous avons fait du slot n'est pas impressionnante, mais elle permettrait par exemple d'utiliser facilement une image au lieu d'un texte pour certaines tâches.

C'était la dernière modification au site de gestionnaire de listes de tâches. Il nous reste maintenant à apprendre à déployer le site.

<div class="exercice" markdown="1" >

**Exercice :** Avant de passer au déploiement le site, refaites un appel au linter (`npm run lint`). Et explorez un peu l'onglet *Vue* du navigateur pour voir les informations qu'il peut fournir.

</div>

## Déployer le site

Pour l'instant, nous avons utilisé Vite pour faire tourner notre site sur un serveur de développement. Nous allons discuter les étapes à suivre pour pouvoir déployer notre site sur un serveur en production. Vite permet de facilement "construire" le site qui sera déployé ("build" en anglais, cette étape transforme le code et ressemble un peu à une étape de compilation). Cette étape de "construction" transforme notre code Vue en un code JS optimisé et beaucoup plus court et donc plus léger à envoyer sur le web, mais beaucoup moins lisible pour un humain.

<div class="exercice" markdown="1" >

**Exercice :**

1. Lancez la compilation de votre site : Allez dans votre terminal, tapez `q` pour couper temporairement le serveur, puis entrez

   ```
   npm run build
   ```

2. Le résultat de la compilation du site est stocké dans le répertoire
   `todo_list/dist`. Nous allons maintenant déployer le site sur un serveur Web.
   Nous donnons les instructions dans l'exemple où vous êtes à l'IUT.

   Nous voudrions faire comme si ce répertoire `dist` était stocké dans `public_html`. Nous allons donc créer un lien symbolique dans `public_html` qui pointe vers `dist`.

   ```bash
   ln -s todo_list/dist ~/public_html/TD_vueJS/TD1 
   ``` 

   Vous pouvez donc désormais accéder à votre site à l'URL
   `https://webinfo.iutmontp.univ-montp2.fr/~votre_login/TD_vueJS/TD1`.
   Cependant, la page Web est vide et l'onglet *Console* indique des problèmes
   de chargement des fichiers de votre site. 

</div>
 
En effet, Vite part du principe que le site que nous construisons sera le seul
site servi par le serveur web et sera donc à la racine du site, c'est-à-dire à
l'URL `https://webinfo.iutmontp.univ-montp2.fr/` au lieu de
`https://webinfo.iutmontp.univ-montp2.fr/~votre_login/TD_vueJS/TD1`. C'est
habituellement, ce que l'on fait pour un site professionnel. Cependant, nous
avons un seul serveur web pour tous nos TDs à l'IUT et nous ne pouvons pas
procéder ainsi. Nous touchons ici au “coût” d’utilisation des frameworks : Ils
offrent des manières plus simples de faire beaucoup de choses standards, mais en
contrepartie, il peut devenir un peu plus compliqué de faire des choses plus
exotiques. Dans la plupart des projets professionnels, les avantages d’utiliser
un framework restent largement rentables.

Nous allons d'abord devoir ouvrir le fichier de configuration `vite.config.ts`
pour indiquer à Vite le chemin de base du site Web.

<div class="exercice" markdown="1" >

**Exercice :**

1. Dans le fichier `vite.config.ts`, ajoutez à la fin du `export default defineConfig` le champ `base: '/~votre_login/TD_vueJS/TD1'`.
   
   Notez que `base` permet aussi d'indiquer l'URL complète (avec le nom de domaine) ou d'utiliser un chemin relatif, mais ça pourrait poser des difficultés plus tard.

2. Relancez la compilation avec `npm run build` et le site doit maintenant 
   être accessible à l'URL `https://webinfo.iutmontp.univ-montp2.fr/~votre_login/TD_vueJS/TD1`.

</div>

Nous reparlerons de ce fichier plus tard pour pouvoir installer un proxy qui nous permettra d'utiliser notre API.

N'oubliez pas de relancer le serveur de développement avec `npm run dev` si vous devez continuer à coder sur le TD.

# Remarques finales

Jusqu'à maintenant quand nous avons utilisé les props dans la partie template nous avons fait `props.titre`. En fait, dans la partie template, la bonne pratique est d'utiliser directement le nom du prop (donc `titre` au lieu de `prop.titre`). D'ailleurs, si on utilise les props que dans le template, la ligne 

```vue
  const props = defineProps<{titre: string}>();
```

peut être remplacée par

```vue
  defineProps<{titre: string}>();
```
En effet, c'est la fonction `defineProps` qui rend les props accessible dans le `<template>`, donc enregistrer le résultat dans la variable `props` sert à pouvoir accéder aux props dans la partie `<script>`.



# Conclusion

Vous avez découvert Vue et TypeScript. Nous avons mentionné la plupart des notions les plus importantes (mais nous avons laissé beaucoup de détails sous le tapis).
Les trucs à retenir :
- le rôle de TypeScript (définir les types et les interfaces),
- notion de réactif et de composant dans Vue,
- `v-bind:attribut` (équivalent à `:attribut`),
- `v-on:event` (équivalent à `@event`),
- `v-model` pour synchroniser une variable à un `<input>`,
- `v-for` permet de lister des éléments,
- comment définir un composant avec ses *props* et ses *events*,
- comment construire et déployer le site.

Pour pouvoir connecter notre façade Vue avec une API PHP, nous verrons dans le prochain TD comment utiliser le proxy de Vite pour pouvoir connecter le serveur de développement à l'API et comment utiliser le composant KeepAlive (un composant spécial fourni par Vue).

<!-- Une liste non exhaustives des notions que nous ne discuterons pas composables, v-if, v-show, lifeCycle, watchers. -->

{% endraw %}


