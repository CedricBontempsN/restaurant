<template>
  <div class="resto">
    <v-container>
      <form v-on:submit="ajouterRestaurant">
        <v-row>
          <v-col>
            <v-text-field type="text" placeholder="Nom" required v-model="nom">
            </v-text-field>
          </v-col>
          <v-col>
              <v-text-field type="text" placeholder="Cuisine" required v-model="cuisine">
              </v-text-field>
          </v-col>
          <v-col>
            <v-btn>Ajouter <v-icon> mdi-check-circle</v-icon></v-btn>
          </v-col>
        </v-row>
      </form>
    </v-container>

    <h1 style="text-align: center">Nombre de restaurants : {{ restaurants.length * page }}/{{ total }}</h1>
    <v-container>
      <v-row>
        <v-col>
          <div class="slidecontainer">
            <v-slider
              max="100"
              step="5"
              min="0"
              value="10"
              class="slider"
              id="resAfficher"
              v-on:keyup="updateAffichage"
              v-on:click="updateAffichage"
            >
            </v-slider>
            <p><span id="valRes">10</span></p>
          </div>
        </v-col>
        <v-col>
          <div>
            <v-btn
              color="primary"
              elevation="3"
              v-on:click="pagePrecedente"
              v-bind:disabled="page == 1"
              >Precedent</v-btn
            >
            <v-btn
              eleveation="3"
              v-on:click="pageSuivante"
              v-bind:disabled="restaurants.length * page == total"
              >Suivant</v-btn
            >
          </div>
        </v-col>
        <v-col>
            <v-text-field
              type="text"
              id="recherche"
              value=""
              v-on:input="chercherRestaurant"
            >
            </v-text-field>
        </v-col>
        <v-col>
           <v-btn v-on:click="chercherRestaurant">Rechercher <v-icon> mdi-check-circle</v-icon></v-btn>
        </v-col>
      </v-row>
    </v-container>

    <v-simple-table>
      <tr>
        <th>Restaurants</th>
        <th>Cuisine</th>
        <th>Quartier</th>
        <th>Supprimer</th>
        <th>Modifier</th>
        <th>Détails</th>
      </tr>
      <tbody>
        <tr
          v-for="(r, index) in restaurants"
          :key="index"
        >
          <td style="text-align: center">{{ r.name }}</td>
          <td style="text-align: center">{{ r.cuisine }}</td>
          <td style="text-align: center">{{ r.borough }}</td>
          <td style="text-align: center">
            <v-btn-fab elevation="2" fab
              ><v-icon
                alt="Supprimer restaurant"
                v-on:click="supprimerRestaurant(r._id)"
              >
                mdi-delete</v-icon
              ></v-btn-fab
            >
          </td>
          <td style="text-align: center">
            <v-icon
              class="delete"
              id="modifImg"
              alt="Modifier restaurant"
              v-on:click="updRestaurant(r)"
            >
              mdi-update</v-icon
            >
          </td>
          <td style="text-align: center"><v-btn><v-icon>mdi-help</v-icon></v-btn></td>
        </tr>
      </tbody>
    </v-simple-table>
    <div>
      <v-container>
        <form v-on:submit="changerRestaurant" id="formModif" class="modif">
          <v-row>
            <v-col>
                <v-text-field type="text" placeholder="Nom" required v-model="oldName">
                </v-text-field>
            </v-col>
            <v-col>
                <v-text-field type="text" placeholder="Cuisine" required v-model="oldCuisine">
                </v-text-field>
            </v-col>
            <v-col>
              <v-btn>Modifier<v-icon> mdi-check-circle </v-icon></v-btn>
            </v-col>
          </v-row>
        </form>
      </v-container>
    </div>
  </div>
</template>

<script>
export default {
  name: "ListeRestaurants",
  data: function () {
    return {
      restaurants: [
        {
          nom: "café de Paris",
          cuisine: "Française",
        },
        {
          nom: "Sun City Café",
          cuisine: "Américaine",
        },
      ],
      nom: "",
      cuisine: "",
      total: "",
      page: 1,
      pageSize: 10,
      elemRecherche: "",
      oldName: "",
      oldCuisine: "",
      borough: "",
    };
  },
  mounted() {
    console.log("Before HTML");
    this.getRestaurantsFromServer();
    this.getRestaurantsNb();
  },
  methods: {
    supprimerRestaurant(id) {
      fetch("http://localhost:8080/api/restaurants/" + id, { method: "DELETE" })
        .then((responseJS) => {
          console.log("ok" + responseJS);
        })
        .catch(function (err) {
          console.log(err);
        });
      this.getRestaurantsFromServer();
    },
    ajouterRestaurant(event) {
      // eviter le comportement par defaut
      event.preventDefault();
      var newNom = event.target[0].value;
      var newCuisine = event.target[1].value;
      fetch("http://localhost:8080/api/restaurants", {
        method: "POST",
        headers: {
          Accept: "application/json",
          "Content-Type": "application/json",
        },
        body: JSON.stringify({
          nom: newNom,
          cuisine: newCuisine,
        }),
      })
        .then((responseJSON) => {
          return responseJSON.json();
        })
        .then((responseJS) => {
          console.log("resto ajouté" + responseJS);
        })
        .catch(function (err) {
          console.log(err);
        });
      this.getRestaurantsFromServer();
      this.nom = "";
      this.cuisine = "";
      this.borough = "";
    },
    pageSuivante(event) {
      event.preventDefault();
      console.log("page suivante");
      this.page++;
      fetch(
        "http://localhost:8080/api/restaurants?page=" +
          this.page +
          "&pagesize=" +
          this.pageSize
      )
        .then((responseJSON) => {
          return responseJSON.json();
        })
        .then((responseJS) => {
          this.restaurants = responseJS.data;
        })
        .catch(function (err) {
          console.log(err);
        });
    },
    pagePrecedente(event) {
      event.preventDefault();
      console.log("page precedente");
      this.page--;
      fetch(
        "http://localhost:8080/api/restaurants?page=" +
          this.page +
          "&pagesize=" +
          this.pageSize
      )
        .then((responseJSON) => {
          return responseJSON.json();
        })
        .then((responseJS) => {
          this.restaurants = responseJS.data;
        })
        .catch(function (err) {
          console.log(err);
        });
    },
    getColor(index) {
      return index % 2 ? "lightBlue" : "pink";
    },
    getRestaurantsFromServer() {
      fetch(" http://localhost:8080/api/restaurants")
        .then((responseJSON) => {
          return responseJSON.json();
        })
        .then((responseJS) => {
          this.restaurants = responseJS.data;
        })
        .catch(function (err) {
          console.log(err);
        });
    },
    getRestaurantsNb() {
      fetch("http://localhost:8080/api/restaurants/count")
        .then((responseJSON) => {
          return responseJSON.json();
        })
        .then((responseJS) => {
          this.total = responseJS.data;
        })
        .catch(function (err) {
          console.log(err);
        });
    },
    updateAffichage() {
      var rangeslider = document.getElementById("resAfficher");
      var output = document.getElementById("valRes");
      output.innerHTML = rangeslider.value;

      rangeslider.oninput = function () {
        output.innerHTML = this.value;
      };
      this.pageSize = output.innerHTML;
      fetch(
        "http://localhost:8080/api/restaurants?page=" +
          this.page +
          "&pagesize=" +
          this.pageSize +
          "&name=" +
          this.elemRecherche
      )
        .then((responseJSON) => {
          return responseJSON.json();
        })
        .then((responseJS) => {
          this.restaurants = responseJS.data;
        })
        .catch(function (err) {
          console.log(err);
        });
    },
    chercherRestaurant() {
      this.elemRecherche = document.getElementById("recherche").value;
      fetch(
        "http://localhost:8080/api/restaurants?page=" +
          this.page +
          "&pagesize=" +
          this.pageSizename +
          "&name=" +
          this.elemRecherche
      )
        .then((responseJSON) => {
          return responseJSON.json();
        })
        .then((responseJS) => {
          this.restaurants = responseJS.data;
        })
        .catch(function (err) {
          console.log(err);
        });
    },
    updRestaurant(r) {
      this.oldName = r.name;
      this.oldCuisine = r.cuisine;
      this.oldId = r._id;
      console.log("OLD name..." + this.oldName + "CUISIE..." + this.oldCuisine);
      document.getElementById("formModif").className = "modifView";
    },
    changerRestaurant(event) {
      //event.preventDefault();
      var newNom = event.target[0].value;
      var newCuisine = event.target[1].value;

      fetch("http://localhost:8080/api/restaurants/" + this.oldId, {
        method: "PUT",
        headers: {
          Accept: "application/json",
          "Content-Type": "application/json",
        },
        body: JSON.stringify({
          nom: newNom,
          cuisine: newCuisine,
        }),
      })
        .then((responseJSON) => {
          return responseJSON.json();
        })
        .then((responseJS) => {
          this.restaurants = responseJS.data;
        })
        .catch(function (err) {
          console.log(err);
        });
    },
  },
};
</script>

<style scoped>
</style>