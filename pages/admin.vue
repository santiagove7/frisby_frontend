<template>
  <div>
    <v-main>
      <v-container class="fill-height" fluid>
        <v-row align="center" justify="center">
          <v-col cols="12" sm="8" md="4">
            <v-card>
              <v-toolbar color="#FFD700">
                <v-toolbar-title>Frisby - Admon (clave=1234)</v-toolbar-title>
              </v-toolbar>
              <v-card-text>
                <v-form>
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
                <v-btn @click="login()" color="#FFD700">Ingresar</v-btn>
                <v-spacer></v-spacer>
              </v-card-actions>
              <div align="center">
                <p><a href="/">Soy Usuario </a></p>
              </div>
            </v-card>
          </v-col>
        </v-row>
      </v-container>
    </v-main>
    <v-dialog v-model="dialog" max-width="290">
      <v-card>
        <v-card-title class="headline"> Error </v-card-title>

        <v-card-text> Clave Incorrecta </v-card-text>

        <v-card-actions>
          <v-spacer></v-spacer>

          <v-btn color="green darken-1" text @click="dialog = false">
            Close
          </v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
  </div>
</template>

<script>
export default {
  layout: "blank",
  data() {
    return {
      password: null,
      dialog: false,
    };
  },

  methods: {
    login() {
      console.log("Login");
      console.log(this.email, this.password);
      if (this.password == "1234") {
        this.$router.push("/homeadmin");
      } else {
        this.dialog = true;
      }
    },
  },
};
</script>