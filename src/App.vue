<template>
  <v-app>
    <v-app-bar
      app
      absolute
      color="rgb(138 191 234)"
      shrink-on-scoroll
      prominent
      dark
      src="https://cdn.pixabay.com/photo/2012/03/04/00/43/architecture-22039_1280.jpg"
    >
      <div class="d-flex align-center">
        <v-img
          alt="Vuetify Name"
          class="shrink mt-1 hidden-sm-and-down"
          contain
          min-width="100"
          src="https://ahcv.com/wp-content/themes/AHCV/assets/images/ahcv-white-shadow.png"
          width="100"
        />
      </div>

      <v-app-bar-title> <h1 style="width: 500px">Customer Portal</h1></v-app-bar-title>

      <v-spacer></v-spacer>

      <v-btn
        href="https://github.com/vuetifyjs/vuetify/releases/latest"
        target="_blank"
        text
      >
        <v-icon>mdi-menu</v-icon>
      </v-btn>
    </v-app-bar>

    <v-main>
      <HelloWorld />
    </v-main>
  </v-app>
</template>

<script lang="ts">
import Vue from "vue";
import HelloWorld from "./components/HelloWorld.vue";

export default Vue.extend({
  name: "App",

  components: {
    HelloWorld,
  },

  data: () => ({
    //
  }),
});
</script>
