<template>
  <div>
    <img
      src="http://wpa.aquarius.frisby.com.co/wp-content/uploads/2018/12/frase-frisby.jpg"
      width="800"
    />
    <img src="https://frisby.com.co/img/frisby-modal-2.png" />
    <h1 align="center">BIENVENIDO, ADMINISTRADOR</h1>
    <br>
    <br>
    <v-container>
        <v-row>
          <v-col cols="12" md="4">
            <h1 align="center">Gestionar ofertas</h1>
            <v-btn width="10px" href="/gestoroferta" block color="#FD0"> Gestor de Ofertas </v-btn>
          </v-col>
        
        
          <v-col cols="12" md="4">
            <h1 align="center">Gestionar Usuarios</h1>
            <v-btn width="10px" href="/users" block color="#FD0"> Usuarios/Aplicaciones </v-btn>
          </v-col>
           <v-col cols="12" md="4">
            <h1 align="center">Capacitaciones</h1>
          </v-col >
          
          </v-row>
    </v-container>
  </div>
</template>
<script>
export default {
  layout:"defaultadmon",
}
</script>