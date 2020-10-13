<template>
  <v-app>
    <v-main>
      <ListeRestaurants/>
    </v-main>
  </v-app>
</template>

<script>
import ListeRestaurants from './components/ListeRestaurants';


export default {
  name: 'App',

  components: {
    ListeRestaurants,
  },

  data: () => ({
    //
  }),
};
</script>
