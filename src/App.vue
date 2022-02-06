<template>
  <v-app>
    <v-app-bar
      class="d-flex justify-center"
      app
      color="white"
      elevation="0"
    >
      <div style="width: 500px;" >
        <span style="width:100%;" class="text-h3 font-weight-bold d-flex justify-center"> WORDLE </span>
        <v-divider ></v-divider>
        <!-- <v-img
          alt="Vuetify Logo"
          class="shrink mr-2"
          contain
          src="https://cdn.vuetifyjs.com/images/logos/vuetify-logo-dark.png"
          transition="scale-transition"
          width="40"
        />

        <v-img
          alt="Vuetify Name"
          class="shrink mt-1 hidden-sm-and-down"
          contain
          min-width="100"
          src="https://cdn.vuetifyjs.com/images/logos/vuetify-name-dark.png"
          width="100"
        /> -->
      </div>

      <!-- <v-spacer></v-spacer>

      <v-btn
        href="https://github.com/vuetifyjs/vuetify/releases/latest"
        target="_blank"
        text
      >
        <span class="mr-2">Latest Release</span>
        <v-icon>mdi-open-in-new</v-icon>
      </v-btn> -->
    </v-app-bar>

    <v-main>
      <Wordle/>
    </v-main>
  </v-app>
</template>

<script>
import Wordle from './components/Wordle';
export default {
  name: 'App',

  components: {
    Wordle
  },

  data: () => ({
    //
  }),
};
</script>
