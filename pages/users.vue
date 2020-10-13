<template>
<div>
    <h1 align="center">Usuarios registrados</h1>

    <v-data-table
    :headers="headers"
    :items="users"
    :items-per-page="5"
    class="elevation-1"
    
  >
  <template v-slot:item.acciones="{ item }">
    
     <v-icon 
        small
        @click="eliminarUsuario(item)"
      > mdi-delete
      </v-icon>
    </template>
    </v-data-table>

   <br>

   <h1 align="center">Aplicaciones registradas</h1> 

   <v-data-table
    :headers="headers1"
    :items="aplicaciones"
    :items-per-page="5"
    class="elevation-1"
    
  >
    </v-data-table>
</div>
</template>
<script>
export default {
  layout:"defaultadmon",
     beforeMount(){
    this.loadPage();
    this.loadPage2();
  },
    data(){
        return{
          users:[],
          aplicaciones:[],
            headers:[
        {text:"Identificacion", value:"id"},
        {text:"Nombre", value:"name"},
        {text:"Apellido", value:"lastname"},
        {text:"Correo", value:"email"},
        {text:"Telefono", value:"telefono"},
        {text:"Hoja de Vida", value:"hdv"},
        {text:"Acciones", value:"acciones"},
      ],
      headers1:[
        {text:"Código Oferta", value:"idaplicacion"},
        {text:"ID Usuario", value:"iduser"},
        {text:"IF Oferta", value:"idof"},
      ],
      editing:false,
        };

    },
    methods:{
        loadPage(){
      //captura info del localst
      let users = localStorage.getItem("users");
      this.users = JSON.parse(users);
    },

    loadPage2(){
      let aplicaciones = localStorage.getItem("aplicaciones");
      this.aplicaciones=JSON.parse(aplicaciones);
    },

    eliminarUsuario(user){
         let existIndex = this.users.findIndex((x) => x.id == user.id);
    if (existIndex > -1) {
      let confirmacion = confirm("Desea eliminar el usuario?");
      if (confirmacion) {
        this.users.splice(existIndex, 1);
        localStorage.setItem("users", JSON.stringify(this.users));
      
        alert("Usuario eliminado");
        
        
      }
    } else {
      alert("El usuario NO existe en la tabla.");
    }

    },

    editUsuario(user){
      this.user= Object.assign({},user);
      this.editing=true;
    },

    editarUsuario(){
      
    if (registro.$refs.valid.validate() && this.valid == true){
      let existIndex = this.users.findIndex(
        (x) => x.id == this.user.id
      );
      if (existIndex > -1) {
        
        
        this.users.splice(existIndex, 1, this.user);
        
        this.user={};
        this.editing=false;
        localStorage.setItem("users", JSON.stringify(this.users));
        this.user={};


        alert("El usuario ha sido modificada correctamente.");
      } else {
        alert("La usuario NO existe en la tabla.");
      }
    
    }else {
        console.log("Hay campos sin llenar");
        this.dialog = true;
      }
    },

   

      },
}
</script>