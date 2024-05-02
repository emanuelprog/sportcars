<template>
  <v-container>
    <v-navigation-drawer v-model="drawer" temporary fixed :width="drawerWidth">
      <v-list-item>
        <v-list-item-content>
          <v-list-item-title class="text-h6"> Menu </v-list-item-title>
        </v-list-item-content>
        <template v-if="isMobile">
          <v-btn icon @click="closeDrawer" class="close-button">
            <v-icon>mdi-close</v-icon>
          </v-btn>
        </template>
      </v-list-item>

      <v-divider></v-divider>

      <v-list dense nav>
        <v-list-item v-for="item in items" :key="item.title" link :to="item.to">
          <v-list-item-icon>
            <v-icon>{{ item.icon }}</v-icon>
          </v-list-item-icon>

          <v-list-item-content>
            <v-list-item-title>{{ item.title }}</v-list-item-title>
          </v-list-item-content>
        </v-list-item>
      </v-list>
    </v-navigation-drawer>

    <v-app-bar app>
      <v-app-bar-nav-icon @click="drawer = !drawer"></v-app-bar-nav-icon>
      <v-spacer class="d-lg-none"></v-spacer>
      <router-link to="/" style="text-decoration: none; color: red;">
        <v-app-bar-title class="logo-text">SPORTCARS</v-app-bar-title>
      </router-link>
      <v-spacer></v-spacer>
      <div class="mr-1">
        <input
          @click="onThemeChanged()"
          type="checkbox"
          class="checkbox"
          id="checkbox"
        />
        <label for="checkbox" class="checkbox-label">
          <i class="fas fa-moon"></i>
          <i class="fas fa-sun"></i>
          <span class="ball"></span>
        </label>
      </div>
    </v-app-bar>
  </v-container>
</template>

<script>
import { eventBus } from "@/event-bus.js";
import globals from "@/globals";

export default {
  data: () => ({
    drawer: null,
    drawerWidth: 300,
    items: [
      { title: "Home", icon: "mdi-view-dashboard", to: "/" },
      { title: "Sell Car", icon: "mdi-car", to: "/sell-car" }
    ],
    dark: false,
  }),
  created() {
    this.checkMobile();
    window.addEventListener("resize", this.checkMobile);
    eventBus.$on("language-changed", this.onLanguageChanged);
  },
  destroyed() {
    window.removeEventListener("resize", this.checkMobile);
    eventBus.$off("language-changed", this.onLanguageChanged);
  },
  methods: {
    checkMobile() {
      this.isMobile = window.innerWidth <= 600;
      this.drawerWidth = this.isMobile ? "100%" : 300;
    },
    closeDrawer() {
      this.drawer = false;
    },
    onThemeChanged() {
      this.dark = !this.dark;
      globals.dark = this.dark;
      eventBus.$emit("themeChanged", this.dark);
    },
  },
};
</script>

<style>
.logo-text {
  font-weight: 900;
}
.close-button {
  position: absolute;
  top: 0;
  right: 0;
  margin: 16px;
}

.checkbox {
  opacity: 0;
  position: absolute;
}

.checkbox-label {
  background-color: #111;
  width: 50px;
  height: 26px;
  border-radius: 50px;
  position: relative;
  padding: 5px;
  cursor: pointer;
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.fa-moon {
  color: #f1c40f;
}

.fa-sun {
  color: #f39c12;
}

.checkbox-label .ball {
  background-color: #fff;
  width: 22px;
  height: 22px;
  position: absolute;
  left: 2px;
  top: 2px;
  border-radius: 50%;
  transition: transform 0.2s linear;
}

.checkbox:checked + .checkbox-label .ball {
  transform: translateX(24px);
}
</style>