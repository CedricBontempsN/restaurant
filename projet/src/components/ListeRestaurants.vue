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
            <v-text-field
              type="text"
              placeholder="Cuisine"
              required
              v-model="cuisine"
            >
            </v-text-field>
          </v-col>
          <v-col>
            <v-btn type="submit"
              >Ajouter <v-icon style="margin-left: 5px"> fa-plus</v-icon></v-btn
            >
          </v-col>
        </v-row>
      </form>
    </v-container>

    <h1 style="text-align: center" font-family="serif">
      Nombre de restaurants : {{ restaurants.length * page }}/{{ total }}
    </h1>
    
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
          <v-btn v-on:click="chercherRestaurant"
            >Rechercher<v-icon style="margin-left: 5px">
              fa-search</v-icon
            ></v-btn
          >
        </v-col>
      </v-row>
    </v-container>
    <div id="resa">
      <v-table text-align="center" >
        <tr>
          <th>Restaurants</th>
          <th>Date</th>
          <th>Heure</th>
        </tr>
        <tbody>
        <tr v-for="(r, index) in reservations" :key=index>
        <td style="text-align: center; width:30%">{{ r.restaurant }}</td>
        <td style="text-align: center; width:30%">{{ r.dateResa }}</td>
        <td style="text-align: center; width:30%">{{ r.heureResa }}</td>
        </tr>
        </tbody>
      </v-table>
    </div>
    <!-- 
    <template>
      <v-card>
        <v-card-title>
          <v-text-field
            v-model="search"
            append-icon="mdi-magnify"
            label="Search"
            single-line
            hide-details
          ></v-text-field>
        </v-card-title>
        <v-data-table :headers="headers" :items="restaurants" :search="search">
          <template center v-slot:[`item.Modifier`]="{ item }">
            <v-icon small class="mr-2" @click="updRestaurant(item._id)">
              mdi-pencil
            </v-icon>
          </template>
          <template
            style="text-align: center"
            v-slot:[`item.supprimer`]="{ item }"
          >
            <v-icon small @click="supprimerRestaurant(item._id)"> fa-trash </v-icon>
          </template>
          <template
            style="text-align: center"
            v-slot:[`item.Details`]
          >
            <v-icon small @click="dialog = true"> fa-info </v-icon>
          </template>
        </v-data-table>
      </v-card>
      <v-dialog v-model="dialog" width="500">
        <v-card>
          <v-card-title class="headline grey lighten-2">
            Details du resaturant
          </v-card-title>

          <v-card-text> Go inserer la map avec le resto centré </v-card-text>
          <v-card-actions>
            <v-spacer></v-spacer>
            <v-btn color="primary" text @click="dialog = false">
              <v-icon style="margin-left: 5px">fa-undo</v-icon>
            </v-btn>
          </v-card-actions>
        </v-card>
      </v-dialog>
    </template>
-->
    <!-- table de base -->

      <v-table text-align="center" >
      <tr>
        <th>Restaurants</th>
        <th>Cuisine</th>
        <th>Quartier</th>
        <th>Supprimer</th>
        <th>Modifier</th>
        <th>Détails</th>
      </tr>
      <tbody>
        <tr v-for="(r, index) in restaurants" :key="index">
          <td style="text-align: center; width:30%">{{ r.name }}</td>
          <td style="text-align: center; width:30%">{{ r.cuisine }}</td>
          <td style="text-align: center; width:25%">{{ r.borough }}</td>
          <td style="text-align: center; width: 5%">
            <v-btn-fab elevation="2" fab
              ><v-icon
                style="margin-left:5px"
                alt="Supprimer restaurant"
                v-on:click="supprimerRestaurant(r._id)"
              >
              fas fa-trash</v-icon
              ></v-btn-fab
            >
          </td>
          <td style="text-align: center; width:5%">
            <v-icon
              style="margin-left:5px"
              class="delete"
              id="modifImg"
              alt="Modifier restaurant"
              v-on:click="updRestaurant(index)"
            >
              fa-redo</v-icon
            >
          </td>
          <td style="text-align: center; width:5%">
            <template>
              <div class="text-center">
                <v-btn color="grey" dark  v-on:click="showMap(r)" @click="dialog = true">
                  <v-icon style="margin-left:5px">
                    fa-info
                  </v-icon>
                </v-btn>
              </div>
            </template>
          </td>
        </tr>
      </tbody>

     </v-table>

    <div>
      <v-container>
        <form v-on:submit="changerRestaurant" id="formModif" class="modif">
          <v-row>
            <v-col>
              <v-text-field
                type="text"
                placeholder="Nom"
                required
                v-model="oldName"
              >
              </v-text-field>
            </v-col>
            <v-col>
              <v-text-field
                type="text"
                placeholder="Cuisine"
                required
                v-model="oldCuisine"
              >
              </v-text-field>
            </v-col>
            <v-col>
              <v-btn
                >Modifier<v-icon style="margin-left: 5px">
                  fab fa-jedi-order
                </v-icon></v-btn
              >
            </v-col>
          </v-row>
        </form>
      </v-container>
    </div>
      <v-dialog v-model="dialog" width="70%">
        <v-card>
          <v-card-title  class="headline grey lighten-2">
            Details du resaturant
          </v-card-title>
          <v-row>
            <v-col>
          <div id="restauMap">
          </div>
          <v-card-text>
            <div id="infoRestau"/>
          </v-card-text>
            </v-col>
            <v-col>
          <div>
            <v-date-picker  v-model="date" mode="dateTime" :minute-increment="5" />
          </div>
          </v-col>
            <v-col>
              <div>
                <v-time-picker  v-model="picker" elevation="15"></v-time-picker>
              </div>
            </v-col>
          </v-row>
          <v-card-actions>
            <v-spacer></v-spacer>
            <v-btn v-bind:disabled="picker === '' || date === ''" color="primary" text @click="reserver()" >Réserver
              <v-icon style="margin-left: 5px">fa-check</v-icon>
            </v-btn>
            <v-btn color="primary" text @click="dialog = false">Fermer
              <v-icon style="margin-left: 5px">fa-undo</v-icon>
            </v-btn>
          </v-card-actions>
        </v-card>
      </v-dialog>


    


  </div>
</template>

<script>
/// CHERCHE V-DATA Pour les info de la popup --> Info a mettre de le header, avant restaurant(data dans js)

export default {
  name: "ListeRestaurants",
  data: function () {
    return {
      search: "",
      headers: [
        {
          text: "Restaurant",
          align: "start",
          sortable: false,
          value: "name",
        },
        { text: "Cuisine", value: "cuisine" },
        { text: "Quartier", value: "borough" },
        { text: "Supprimer", align: "center", value: "supprimer" },
        { text: "Modifier", align: "center", value: "Modifier" },
        { text: "Details", align: "center", value: "Details" },
      ],
      restaurants: [
        {
          nom: "café de Paris",
          cuisine: "Française",
          address: "",
          grades: ""
        },
        {
          nom: "Sun City Café",
          cuisine: "Américaine",
          address: "",
          grades: ""
        },
      ],
      items: [
        {
          id: 1,
          name: "Applications :",
          children: [
            { id: 2, name: "Calendar : app" },
            { id: 3, name: "Chrome : app" },
            { id: 4, name: "Webstorm : app" }
          ]
        },
        {
          id: 5,
          name: "Documents :",
          children: [
            {
              id: 6,
              name: "vuetify :",
              children: [
                {
                  id: 7,
                  name: "src :",
                  restaurant: "",
                  dateResa: "",
                  heureResa: "",
                  children: [
                    {id: 8, name: "index : ts", restaurant: "resto", dateResa: "date", heureResa: "heure"},
                  ]
                }
              ]
            },
          ]
        }
        ],
      reservations: [
        {restaurant: "Nom resto", dateResa: "date", heureResa: "heure"},
      ],
      nextId: 1000,

      nom: "",
      cuisine: "",
      total: "",
      page: 1,
      pageSize: "",
      elemRecherche: "",
      oldName: "",
      oldCuisine: "",
      borough: "",
      dialog: false,
      supprimer: "",
      date: "",
      timezone: '',
      picker: "",
      currentRestaurant: "",
    };
  },
  mounted() {
    console.log("Before HTML");
    this.getRestaurantsFromServer();
    this.getRestaurantsNb();
    this.showMap();
  },
  methods: {
    addChild(item) {
      if (!item.children) {
        this.$set(item, "children", []);
      }

      //const name = `${item.name} (${item.children.length})`;
      const id = this.nextId++;

      const restaurant = "le reso";
      const dateResa = "la date";
      const heureResa = "l'heure";
      item.children.push({
        id,
        name,
        restaurant,
        dateResa,
        heureResa
      });
    },

    findItem(id, items = null) {
      if (!items) {
        items = this.items;
      }
      return items.reduce((acc, item) => {
        if (acc) {
          return acc;
        }
        if (item.id === id) {
          return item;
        }
        if (item.children) {
          return this.findItem(id, item.children);
        }
        return acc;
      }, null);
    },

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
          console.log(responseJS.data[0].address.coord[0]);
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
    showMap(restaurant) {
      if (restaurant != null && document.getElementById("restauMap") != null) {
        var coordX = restaurant.address.coord[0];
        var coordY = restaurant.address.coord[1];
        document.getElementById("restauMap").innerHTML =
            "<iframe width=\"100%\" height=\"400\" frameborder=\"0\" style=\"border:0\" src=\"https://www.google.com/maps/embed/v1/place?key=AIzaSyDVhNJO7h-3Sd5n9DNHMjQMWCxa4Is_684&q="+coordY+","+coordX+"&maptype=satellite\" allowfullscreen></iframe>" //+ coordY + "," + coordX + "&maptype=satellite+(mapResatu)&amp;t=&amp;z=14&amp;ie=UTF8&amp;iwloc=B&amp;output=embed\"></iframe>\n"
        document.getElementById("infoRestau").innerHTML =
            "Adresse complète : \n" + restaurant.address.street + ", " + restaurant.address.zipcode + ", " + restaurant.borough;
      }
      this.currentRestaurant = restaurant.name
    },
    reserver(){
      this.dialog=false;
      this.reservations.push({restaurant: this.currentRestaurant, dateResa: this.date, heureResa: this.picker});
    }
  },
};
</script>

<style scoped>
</style>