# Beer Project ;-)
Blabla blabla blabla blabla blabla blabla blabla blabla blabla blabla blabla blabla blabla blabla blabla blabla blabla blabla blabla blabla blabla blabla blabla blabla blabla blabla blabla blabla blabla blabla blabla blabla blabla blabla blabla blabla blabla blabla blabla blabla blabla


## 1. Initialisation
Ouvrez le terminal et rendez-vous dans votre dossier contenant vos projets.
Lancez la commande suivante:

    vue init webpack NOM_DU_DOSSIER
>  Permet d'initialiser vuejs avec le template webpack dans NOM_DU_DOSSIER

Il vous sera demandé de remplir vos informations et de choisir si vous souhaitez installer certains modules.

Commencez par remplir vos informations, ensuite accepter d'installer vue-router, refuser eslint et les 2 modules de testing. Terminez en précisant que vous utiliser npm.

> touche enter pour valider par defaut ou pour accepter, n pour refuser

Une fois que NPM a terminé, faites les commandes afficher sur le terminal

    cd NOM_DU_DOSSIER
    npm run dev
    #ctrl + c pour couper le server meme sur 
    # terminer par la ligne suivante pour ouvrir votre code dans le dossier courant 
    code ./

> npm run dev, permet de lancer un script qui lancera un server
> la liste des scripts exécutables se trouvent dans votre fichier package.json à la ligne script

Pour ce projet il va nous falloir quelques packages supplémentaires: 

 - Axios - https://github.com/axios/axios
 - vuex - https://vuex.vuejs.org/
 - bootstrap
 - sass-loader - Ces 2 là permettent d'utiliser du sass dans votre projet
 - node-sass 

l

    npm i axios vuex bootstrap sass-loader node-sass --save

> le flag --save permet d'enregistrer les packages téléchargés au niveau des dépendances.

L'avant dernière étape de l'initialisation est d'intégrer les fichiers qui se trouvent dans ce zip à la racine de votre projet.
[Lien du zip](www.google.be)

Il contient des images, des templates html et un serveru nodejs.

Pour terminer allez dans le dossier server/ et installez les packages

	cd server
	npm i

## 2. Composants

Avant de commencer avec les composants, on va lier notre bootstrap à la totalité du projet. Pour cela créez-vous un fichier **main.scss** dans le dossier src/assets/scss et importer bootstrap.scss.

> Si vous utilisez sass, remplacer les scss par sass

Rendez-vous dans votre src/main.js et importer votre main.scss en dessous des autres import

```javascript
import  './assets/scss/main.scss'
```

Encore un peu de patience, on nettoie d'abord les fichiers avant de créer ses composants. Pour cela allez dans le fichier src/App.vue. Virez la partie css et la balise img qui se trouve dans le \<template>. 
Ensuite supprimez le fichier src/components/HelloWorld.vue.
Et enfin, rendez-vous dans le fichier src/router/index.js, supprimer l'import concernant le HelloWorld.vue et la route concernée (en gros retirez tout ce qu'il y entre `{ path:  '/', name:  'HelloWorld', component: HelloWorld }`.

Voilà, vous êtes paré !

### Menu.vue
Créons-nous un premier composant nommé Menu.vue
Remplissez le avec les 3 balises de base (template, script, style).
Ce composant sera notre racine. Pour se faire, allez dans le src/route/index.js et créez une route racine à partir de ce composant.
On commence par importer le composant
```javascript
import Menu from '@/components/Menu'
```
Et dans le tableau routes insérez un objet comme celui-ci
```javascript
{
	path:  '/',
	name:  'Menu',
	component: Menu
}
``` 

> Path est pour le chemin, donc www.site.com**/**
> name est pour appeler plus facilement la route dans nos liens route (on y reviendra)
> component est le composant qui sera appelé quand on sera à la racine / du site

On terminera par remplir le template du Menu.vue avec le static/template.html
Placez dans le template de Menu.vue ce qui se trouve entre \<!-- MENU --> et \<!-- END MENU -->

> Si il y a une erreur, enlever les src des images, ou remplacez le chemin par ../../static/images/****.jpg


 ### Navigation.vue

Ce composant servira de nav pour notre application. Comme précédemment créez un composant nommé Navigation.vue.
Dans la partie template insérez le code html déjà fait (de la nav) se trouvant dans static/template.html.

Une fois fait, vous allez importer ce composant dans votre src/App.vue et le mettre juste avant le \router link/

### Basket.vue

Créez le composant Basket.vue comme les autres, et insérez-y le contenu container de static/basket.html

Une fois fait, vous devez créer une route pour pouvoir charger ce composant.
Rendez-vous dans index.js gérant les routes. Ajoutez-y la route /basket qui chargera le composant Basket.vue

> Si vous avez des problèmes en appelant site.ddd/basket. Insérez avant routes: [] ceci: mode: 'history',
> Cela enlèvera le # présent dans l'url 

Pour l'instant vous pouvez seulement naviguer dans votre application en changeant l'url à la main.
Pour changer cela, allez dans Navigation et vous allez créer des router-link.
Remplacer les balises a par router-link, le href="" se tranforme en to=""
```html
<router-link to="/basket" class="mes-classes"> mon panier </router-link>
``` 
   
# 3. Le serveur

Le serveur est déjà créer, il suffit juste de le lancer.

    cd server
    npm run start
   
Le serveur permet de récupérer et de poster des données.

    http://localhost:1337/api/v1/beers
pour récupérer les données sur les bières 

    http://localhost:1337/api/v1/basket
pour récupérer le pannier et pour ajouter une bière au panier.
Laissez-le tourner en fond.

# 4. VueX et Axios

### Vuex config
*Vuex sur google pour savoir ce que c'est.*

Comme chaque 'plugin' de vue, vous devez l'importer dans le main.js en dessous de vue et le charger avec un vue.use()
```js
import Vue from 'vue'
import Vuex from 'vuex'

Vue.use(Vuex)
``` 
A présent nous allons créer le store à proprement parlé.
Pour cela créez un fichier BasketStore.js dans votre dossier components. Et le code qui suit est le code basique pour un store.
```js
import Vuex from 'vuex'

const store =  new Vuex.Store({
	state: {},
	mutations: {}
})
export default store
``` 

> Ici on utilise vuex pour créer un objet 'global' qui contiendra des données et celles-ci seront accessible dans vos différents composants.

Maintenant vous devez dire a Vue que vous comptez utiliser ce fichier en tant que store. Pour cela il faut que vous importer dans main.js votre store et il faut que vous l'initialisez au sein de l'objet vue (comme le router)
```js
import store from './components/BasketStore'
// ( ... ) //
new  Vue({
	el:  '#app',
	store: store,
	router,
	components: { App },
	template:  '<App/>'
})
``` 

> Stocker ce store ici vous permet d'accéder à la variable $store globalement, on y reviendra

A ce niveau ci vous devriez avoir une erreur dans la console de votre application. Cette erreur dit:

    Uncaught Error: [vuex] must call Vue.use(Vuex) before creating a store instance.
        at assert (vuex.esm.js?358c:97)
        at new Store (vuex.esm.js?358c:302)
        at eval (BasketStore.js?3b30:3)
        at Object../src/components/BasketStore.js (app.js:1136)
        at __webpack_require__ (app.js:679)
        at fn (app.js:89)
        at eval (main.js?1c90:1)
        at Object../src/main.js (app.js:1160)
        at __webpack_require__ (app.js:679)
        at fn (app.js:89)

Cette erreur signifie que vous créez un store avant que vue js utilise vuex. Cette erreur est due à webpack qui reformate le code derrière vous. Il existe ce pendant un petit package qui règle ce soucis. Eteignez votre serveur vuejs, et lancez cette commande:

    npm i vue-use-vuex --save
    

> https://www.npmjs.com/package/vue-use-vuex

Vous allez donc retirer dans main.js la ligne qui importe vuex et le Vue.use()
Importez à la place vue-use-vuex

```js
import 'vue-use-vuex'
```

### Axios config

Comme vuex, vous allez lier axios à vue et créer un variable 'globale' ($http) pour y accéder plus facilement.
main.js
```js
import axios from 'axios'
// ( ... ) //
Vue.prototype.$http = axios
```

Vous voilà avec vos 2 plugins correctement implémentés et configurés. 

### Stocker les données

Dans votre Menu.vue vous voudriez bien stocker les bières qui se trouvent sur votre serveur dans un fichier json. Comme cité plus haut, le serveur est déjà fait et configuré. Rejoignez cette url pour voir ce que vous obtenez:  http://localhost:1337/api/v1/beers

Vous devriez voir un tableau, qui contient plusieurs bières, leurs noms, leurs descriptions, ...

Ce que vous devez faire est de récupérer ces données grâce à axios et les stocker dans vos data de Menu.vue

```js
export  default  {
	data ()  {
		return  {
			products: [],
			api:  'http://localhost:1337/api/v1'
		}
	},
	methods:  {
		getProducts ()  {
			this.$http.get(this.api  +  '/beers').then(r  =>  {
				this.products  =  r.data
			})
		}
	},
	created ()  {
		this.getProducts()
	}
}
```

> Vous créez la data products et une méthode pour récupérer la data ensuite vous lancez cette méthode au moment de la création du composant

Maintenant que vous avez une data vous pouvez déjà dynamiser votre partie template. Bouclez sur le tableau produit et afficher pour chacun:
 - son nom
 - sa description
 - nombre d'étoiles
 - son prix
 - son image
 - son stock

Si le stock est égale à 1, le fond de la card doit être jaune
Si le stock est vide, la bière ne s'affiche pas

```html
<template>
  <div  class="container mt-2">
    <div  class="row">
      <div  class="col-4"  v-for="item  in  products" v-if="item.stock">
        <div  class="card"  :class="{'bg-warning' : item.stock  <=  1}">
          <img  class="card-img-top"  :src="item.image"  height="100%"  alt="Card image cap">
          <div  class="card-body">
            <h5  class="card-title">{{  item.label }}</h5>
            <div  class="text-primary">
              <span  v-for="i  in  item.stars">★</span>
            </div>
            <p  class="card-text">{{  item.description }}</p>
            <div  class="d-flex justify-content-between">
              <span  class="text-primary"><strong>{{  item.price  }}</strong></span>
              <a  href="#"  class="btn btn-primary">Ajouter</a>
              <span>{{  item.stock  }} en stock</span>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>
```

Créez maintenant un filtre qui affiche les bières par prix et un autre qui affiche sans ordre précis.
Il vous faut donc une data filtered qui est un booléen. Ensuite il vous faut une propriété computed qui retourne si la filtered est vrai un tableau contenant les bières par prix croissant et si non il retourne le tableau de base
```js
data () {
  return {
    filtered: false,
  //(...) //
  }
}
// ( ... ) //
computed:  {
  sortedProducts  ()  {
    if(this.filtered){
      return  this.products.concat().sort((a,b)  =>  {
        return  parseFloat(a.price) -  parseFloat(b.price)
      })
    }
    return  this.products
  }
}
```

```html
<!-- ( ... ) -->
<a  href="#"  class="btn btn-primary"  @click.prevent="filtered  =  false">Tout</a>
<a  href="#"  class="btn btn-primary"  @click.prevent="filtered  =  true">Prix</a>
<!-- ( ... ) -->
<div  class="col-4"  v-for="item  in  sortedProducts"  v-if="item.stock">
<!-- ( ... ) -->
```

## filtres

Vous allez devoir créer des filtres vuejs.
> https://fr.vuejs.org/v2/guide/filters.html

 - Un qui sert à rajouter le signe € à la fin des prix et doit obligatoirement avoir 2 chiffres après la virgule
 - Un qui sert à mettre le titre des bières en majuscule


```js
filters:  {
  currency  (value)  {
    return  value.toFixed(2) +  ' €'
  },
  capitalize  (value)  {
    return  value.toUpperCase()
  }
}
```
```html
<h5  class="card-title">{{  item.label |  capitalize}}</h5>
<span  class="text-primary"><strong>{{  item.price  |  currency  }}</strong></span>
```

# 5. Le store en pronfondeur

Vous allez tout dabord devoir créer le state basket et des mutations pour jouer sur le state de votre store. Il vous en faut une pour créer le panier et une qui push une bière dans le panier
```js
state: {
  basket: []
},
mutations: {
  CREATE_BASKET (state, basket) {
    state.basket = basket
  },
  ADD_TO_BASKET (state, el) {
    state.basket.push(el)
  }
}
```
Maintenant que vous pouvez intéragir avec votre store, vous devez créer une méthode qui ajoutera une bière au panier (celui du store ET celui du serveur).
Pour cela allez dans votre Menu.vue et ajoutez une méthode addBeerToBasket. 
```js
addBeerToBasket (beer) {
  this.$http.post(this.api + '/basket', beer).then(response => {
    if (response.status === 201) {
      this.$store.commit('ADD_TO_BASKET', beer)
      this.getProducts()
    }
  })
}
```
>Si jamais vous voulez vider le panier du serveur, relancez-le tout simplement. (Les données sont stocker dans un tableau qui se crée lors du lancement du serveur)

### L'affichage

Rendez-vous dans Navigation.vue et vous allez devoir affecter les valeurs de votre panier. 
Si votre panier contient rien, il affiche `Accéder à votre panier (vide)`
Si non il vous affiche `Accéder à votre panier (xx articles - xx.xx €)`
```js
computed: {
  basketTotal () {
    let total = 0
    this.$store.state.basket.map((beer) => {
      total += parseFloat(beer.price)
    })
    return total
  }
}
```
```html
<router-link v-if="$store.state.basket.length > 0" to="/basket" class="nav-link">Accéder à votre panier ({{ $store.state.basket.length }} articles - {{ basketTotal.toFixed(2) }} €)</router-link>
<router-link v-else to="/basket" class="nav-link">Accéder à votre panier (vide)</router-link>
```

Maintenant rendez-vous dans Basket.vue et vous allez presque faire la même chose.
Ici vous devez afficher toutes les bières présentes dans le store et leurs prix dans la bulle bleue

```html
<li v-for="item in $store.state.basket" class="list-group-item d-flex justify-content-between align-items-center">
  {{ item.label }}
  <span class="badge badge-primary badge-pill">{{ item.price.toFixed(2) }} €</span>
</li>
```

Peut-être que vous avez déjà observer cela, mais si vous actualisez votre page celle-ci redéfinis le store panier à rien malgré que le panier du serveur soit rempli.
Pour remedier à ça il faut qu'au lancement de la page une méthode se lance et défini le store au panier du serveur.
>Exceptionnelement, vous devez faire ceci dans votre App.vue. Comme ça quoi qu'il arrive votre panier sera chargé

```js
methods: {
	getBasket () {
    this.$http.get('http://localhost:1337/api/v1' + '/basket').then(r => {
      this.$store.commit('CREATE_BASKET', r.data)
    })
  }
},
created () {
  this.getBasket()
}
```