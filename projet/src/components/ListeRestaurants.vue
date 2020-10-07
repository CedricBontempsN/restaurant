<template>
  <div class="resto">
    <form v-on:submit="ajouterRestaurant">
      <label>
        Nom : <input type="text" required v-model="nom">
      </label>
      <label>
        Cuisine : <input type="text" required v-model="cuisine">
      </label>

      <button>Ajouter</button>
    </form>

    <h1>Nombre de restaurants : {{restaurants.length*page}}/{{total}}</h1>

    <div class="slidecontainer">
      <input type="range" min="5" max="100" value="10" class="slider" id="resAfficher"  v-on:keyup="updateAffichage" v-on:click="updateAffichage">
      <p><span id="valRes">10</span></p>
    </div>


    <div>
      <button v-on:click="pagePrecedente" v-bind:disabled="page==1">Precedent</button>
      <button v-on:click="pageSuivante" v-bind:disabled="restaurants.length*page == total">Suivant</button>
    </div>

    <div>
      <input type="text" id="recherche" value="" v-on:input="chercherRestaurant">
      <button v-on:click="chercherRestaurant">Rechercher</button>
    </div>

    <table>
      <tr>
        <th>Nom</th>
        <th>Cuisine </th>
        <th>Supprimer </th>
        <th>Modifier </th>
      </tr>
      <tbody>
      <tr v-for="(r, index) in restaurants" :key="index"  v-bind:style="{backgroundColor:getColor(index)}"
          v-bind:class="{bordureRouge:(index === 2)}">
        <td>{{r.name}}</td>
        <td> {{r.cuisine}}</td>
        <td style="text-align: center">
          <img class="delete" src="../assets/croix.png" alt="Supprimer restaurant" width="20" height="20" v-on:click="supprimerRestaurant(r._id)">
        </td>
        <td style="text-align: center">
          <img class="delete" id="modifImg" src="../assets/refresh.png" alt="Modifier restaurant" width="15" height="15" v-on:click="updRestaurant(r)">
        </td>
      </tr>
      </tbody>
    </table>


    <form v-on:submit="changerRestaurant" id="formModif" class="modif">
      <label>
        Nom : <input type="text" required v-model="oldName" >
      </label>
      <label>
        Cuisine : <input type="text" required v-model="oldCuisine">
      </label>
      <button>Modifier</button>
    </form>
  </div>
</template>

<script>
export default {
  name: "ListeRestaurants",
  data: function() {
      return{
        restaurants: [
          {
            nom: 'café de Paris',
            cuisine: 'Française'
          },
          {
            nom: 'Sun City Café',
            cuisine: 'Américaine'
          }
        ],
        nom: '',
        cuisine: '',
        total: '',
        page:1,
        pageSize: 10,
        elemRecherche: '',
        oldName: '',
        oldCuisine: ''
    }
  },
  mounted(){
    console.log("Before HTML");
    this.getRestaurantsFromServer();
    this.getRestaurantsNb();
  },
  methods: {
    supprimerRestaurant(id) {
      fetch('http://localhost:8080/api/restaurants/'+id, {method: 'DELETE'})
          .then((responseJS) => {
        console.log('ok'+ responseJS)
      }).catch(function(err) {
        console.log(err);
      });
      this.getRestaurantsFromServer();
    },
    ajouterRestaurant(event) {
      // eviter le comportement par defaut
      event.preventDefault();
      var newNom = event.target[0].value;
      var newCuisine = event.target[1].value;
      fetch('http://localhost:8080/api/restaurants', {method: "POST",
        headers: {
          'Accept': 'application/json',
          'Content-Type': 'application/json'
        },
        body : JSON.stringify({
          nom : newNom,
          cuisine: newCuisine
        })})
          .then((responseJSON) => {
            return responseJSON.json();
          }).then((responseJS) => {
        console.log('resto ajouté'+ responseJS);
      }).catch(function(err) {
        console.log(err);
      });
      this.getRestaurantsFromServer();
      this.nom = "";
      this.cuisine = "";
    },
    pageSuivante(event){
      event.preventDefault();
      console.log("page suivante");
      this.page++;
      fetch('http://localhost:8080/api/restaurants?page='+this.page+'&pagesize='+this.pageSize)
          .then((responseJSON) => {
            return responseJSON.json();
          }).then((responseJS) => {
        this.restaurants = responseJS.data;
      }).catch(function(err) {
        console.log(err);
      });
    },
    pagePrecedente(event){
      event.preventDefault();
      console.log("page precedente");
      this.page--;
      fetch('http://localhost:8080/api/restaurants?page='+this.page+'&pagesize='+this.pageSize)
          .then((responseJSON) => {
            return responseJSON.json();
          }).then((responseJS) => {
        this.restaurants = responseJS.data;
      }).catch(function(err) {
        console.log(err);
      });
    },
    getColor(index) {
      return (index % 2) ? 'lightBlue' : 'pink';
    },
    getRestaurantsFromServer(){
      fetch(' http://10.154.123.224:8080/api/restaurants')
          .then((responseJSON) => {
            return responseJSON.json();
          }).then((responseJS) => {
        this.restaurants = responseJS.data;
      }).catch(function(err) {
        console.log(err);
      });
    },
    getRestaurantsNb(){
      fetch('http://localhost:8080/api/restaurants/count')
          .then((responseJSON) => {
            return responseJSON.json();
          }).then((responseJS) => {
        this.total = responseJS.data;
      }).catch(function(err) {
        console.log(err);
      });
    },
    updateAffichage(){
      var rangeslider = document.getElementById("resAfficher");
      var output = document.getElementById("valRes");
      output.innerHTML = rangeslider.value;

      rangeslider.oninput = function() {
        output.innerHTML = this.value;
      }
      this.pageSize= output.innerHTML;
      fetch('http://localhost:8080/api/restaurants?page='+this.page+'&pagesize='+this.pageSize+'&name='+this.elemRecherche)
          .then((responseJSON) => {
            return responseJSON.json();
          }).then((responseJS) => {
        this.restaurants = responseJS.data;
      }).catch(function(err) {
        console.log(err);
      });
    },
    chercherRestaurant(){
      this.elemRecherche = document.getElementById("recherche").value;
      fetch('http://localhost:8080/api/restaurants?page='+this.page+'&pagesize='+this.pageSizename+'&name='+this.elemRecherche)
          .then((responseJSON) => {
            return responseJSON.json();
          }).then((responseJS) => {
        this.restaurants = responseJS.data;
      }).catch(function(err) {
        console.log(err);
      });
    },
    updRestaurant(r){
      this.oldName=r.name;
      this.oldCuisine=r.cuisine;
      this.oldId=r._id;
      console.log('OLD name...'+ this.oldName + "CUISIE..."+ this.oldCuisine);
      document.getElementById("formModif").className ="modifView"
    },
    changerRestaurant(event){
      //event.preventDefault();
      var newNom = event.target[0].value;
      var newCuisine = event.target[1].value;

      fetch('http://localhost:8080/api/restaurants/'+this.oldId,
          { method:'PUT',
            headers: {
              'Accept': 'application/json',
              'Content-Type': 'application/json'
            },
            body : JSON.stringify({
              nom : newNom,
              cuisine: newCuisine
            })})
          .then((responseJSON) => {
            return responseJSON.json();
          }).then((responseJS) => {
        this.restaurants = responseJS.data;
      }).catch(function(err) {
        console.log(err);
      });
    }
  }
}
</script>

<style scoped>

</style>