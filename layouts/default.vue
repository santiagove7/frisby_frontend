<template>
  <v-app>
    <v-navigation-drawer v-model="drawer" app color="#FFD700">
      <v-list-item>
        <v-list-item-content>
          <v-list-item-title>Menú Usuario </v-list-item-title>
        </v-list-item-content>
      </v-list-item>

      <v-divider></v-divider>

      <v-list dense>
        <!-- Menu items -->
        <v-list-item v-for="item in items_menu" :key="item.id" :to="item.route">
          <v-list-item-icon>
            <v-icon>{{ item.icon }}</v-icon>
          </v-list-item-icon>

          <v-list-item-content>
            <v-list-item-title style="padding: 5px">{{
              item.title
            }}</v-list-item-title>
          </v-list-item-content>
        </v-list-item>
      </v-list>
    </v-navigation-drawer>

    <v-app-bar app color="#FFD700">
      <v-app-bar-nav-icon @click="drawer = !drawer"></v-app-bar-nav-icon>
      <v-toolbar-title>Frisby</v-toolbar-title>
      <v-spacer></v-spacer>
      <v-btn @click="logout()"> Salir </v-btn>
    </v-app-bar>

    <v-main>
      <v-container> <nuxt /> </v-container>
    </v-main>
  </v-app>
</template>
<script>
export default {
  data() {
    return {
      drawer: null,
      items_menu: [
        { id: "01", title: "Home User", icon: "mdi-home-account", route: "/home" },
        { id: "02", title: "Editar Perfil", icon: "mdi-account-edit", route: "/registro" },
        { id: "03", title: "Ofertas", icon: "mdi-briefcase", route: "/ofertas" },
        
      ],
    };
  },
  methods: {
    logout() {
      this.$router.push("/");
    },
  },
};
</script>
