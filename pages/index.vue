<template>
  <v-main>
    <v-container class="fill-height" fluid>
      <v-row align="center" justify="center">
        <v-col cols="12" sm="8" md="4">
          <v-card>
            <v-toolbar color="#FFD700">
              <v-toolbar-title>Frisby - User (Entra con cualquier dato)</v-toolbar-title>
            </v-toolbar>
            <v-card-text>
              <v-form>
                <v-text-field
                  label="Correo"
                  prepend-icon="mdi-email-open"
                  type="email"
                  v-model="email"
                ></v-text-field>

                <v-text-field
                  label="Clave"
                  prepend-icon="mdi-lock"
                  type="password"
                  v-model="password"
                ></v-text-field>
              </v-form>
            </v-card-text>
            <v-card-actions>
              <v-spacer></v-spacer>
              <v-btn href="/registro" color="#FFD700">Registro</v-btn>
              <v-spacer></v-spacer>
              <v-btn @click="login()" color="#FFD700">Ingresar</v-btn>
              <v-spacer></v-spacer>
            </v-card-actions>
            <div align="center">
              <p><a href="/admin">Soy Administrador </a></p>
            </div>
          </v-card>
        </v-col>
      </v-row>
    </v-container>
  </v-main>
</template>

<script>
export default {
  layout: "blank",
  data() {
    return {
      email: null,
      password: null,
    };
  },
  methods: {
    login() {
      console.log("Login");
      console.log(this.email, this.password);
      if (this.email && this.password) {
        this.$router.push("/home");
      }
    },
  },
};
</script>
