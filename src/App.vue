<template>
  <v-app>
    <Navbar />
    <v-main>
      <router-view/>
    </v-main>
    <Footer />
  </v-app>
</template>

<script>
import Navbar from './components/Navbar.vue';
import Footer from './components/Footer.vue';
import 'vuetify/dist/vuetify.min.css';
import Vuetify from 'vuetify';
import 'vuetify/dist/vuetify.min.css';
import globals from "./globals.js";
import { eventBus } from "@/event-bus.js";

export default {
  mounted() {
    eventBus.$on("themeChanged", (newTheme) => {
      this.darkMode = newTheme;
      this.updateVuetifyTheme();
    });
  },
  components: { Navbar, Footer },
  name: 'App',
  vuetify: new Vuetify({
    theme: {
      dark: false,
    },
  }),
  data: () => ({
    darkMode: globals.dark
  }),
  methods: {
    updateVuetifyTheme() {
      this.$vuetify.theme.dark = this.darkMode;
    }
  },
};
</script>
