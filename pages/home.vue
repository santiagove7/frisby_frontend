<template>
  <div>
    <img
      src="http://wpa.aquarius.frisby.com.co/wp-content/uploads/2018/12/frase-frisby.jpg"
      width="800"
    />
    <img src="https://frisby.com.co/img/frisby-modal-2.png" />
    <h1 align="center">BIENVENIDO, USUARIO</h1>
    <v-container>
        <v-row >
          <v-col cols="12" md="4">
            <h1 align="center">Gestionar Perfil</h1>
            <v-btn  href="/registro" width="10px" block color="#FD0"> Editar perfil </v-btn>
          </v-col>
           <v-col cols="12" md="4">
            <h1 align="center">Ofertas</h1>
            <v-btn  href="/ofertas" width="10px" block color="#FD0"> Ver ofertas </v-btn>
          </v-col>
          
          </v-row>
    </v-container>
  </div>
</template>
