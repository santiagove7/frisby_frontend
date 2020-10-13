<template>
<div>
    <h1 v-if="!editing" align="center">Ofertas</h1>
    <v-data-table v-if="!editing"
    :headers="headers"
    :items="ofertas"
    :items-per-page="5"
    class="elevation-1"
    
  >
  
    </v-data-table>
    <br>
    <h1 v-if="!editing" align="center">Aplicar a Oferta</h1>
    <v-form v-model="valid" ref="valid" v-if="!editing">
      <v-text-field
              v-model="aplicacion.idaplicacion"
              :rules="fieldRequired"
              :counter="15"
              label="Código de Aplicación"
              required
            ></v-text-field>
     <v-text-field
              v-model="aplicacion.iduser"
              :rules="fieldRequired"
              :counter="10"
              label="Ingrese su cédula"
              required
            ></v-text-field>
            <v-text-field
              v-model="aplicacion.idof"
              :rules="fieldRequired"
              :counter="10"
              label="Ingrese código oferta"
              required
            ></v-text-field>
            </v-form>
            <v-btn v-if="!editing" @click="aplicar"  block color="#FFd700"> Aplicar </v-btn>
            <h5 v-if="!editing" onMouseover="this.style.color='blue'" onMouseout="this.style.color='black'" @click="eliminarAplicacion" align="center">Deseo eliminar mi aplicación a una oferta</h5>
           <br>
           <h1 v-if="editing" align="center">Elimine solo una aplicación que le pertenezca:</h1>
            <v-data-table v-if="editing"
    :headers="headers2"
    :items="aplicaciones"
    :items-per-page="5"
    class="elevation-1"
    
  > <template v-slot:item.acciones="{ item }">
      <v-icon
        small
        @click="deleteAp(item)"
      >
        mdi-delete
      </v-icon>
    </template>
  
    </v-data-table>

    <v-btn v-if="editing" @click="back" block color="#FFd700"> Volver atrás </v-btn>

    

</div>
</template>
<script>
export default {
     beforeMount(){
    this.loadPage();
    
  },
    data(){
        return{
          valid: null,
          aplicaciones:[],

            aplicacion: {
              idaplicacion: null,
        idof: null,
        iduser: null,
      }, 
      fieldRequired: [(v) => !!v || "Field is required"],

            headers:[
        {text:"Código", value:"idoferta"},
        {text:"Nombre", value:"nombre"},
        {text:"Cargo", value:"cargo"},
        {text:"Descripcion", value:"descripcion"},
        {text:"Requisitos", value:"requisitos"},
      ],
      headers2:[
        {text:"Código Aplicación", value:"idaplicacion"},
        {text:"ID Usuario", value:"iduser"},
        {text:"ID Oferta", value:"idof"},
        {text:"Eliminar", value:"acciones"},
        
      ],

      editing:false,
        };

    },
    methods:{
        loadPage(){
      //captura info del localst
      let ofertas = localStorage.getItem("ofertas");
      this.ofertas = JSON.parse(ofertas);
    },

    loadPage2(){
      //captura info del localst
      let users = localStorage.getItem("users");
      this.users = JSON.parse(users);
    },

    loadPage3(){
      let aplicaciones = localStorage.getItem("aplicaciones");
      this.aplicaciones = JSON.parse(aplicaciones);

    },

    

    aplicar(){
      this.loadPage();
       this.loadPage2();
        if (this.$refs.valid.validate() && this.valid == true) {
         
      let exist = this.ofertas.find((x) => x.idoferta == this.aplicacion.idof);
      
      let exist1 = this.users.find((x)=> x.id == this.aplicacion.iduser);

      let existID = this.aplicaciones.find((x)=> x.idaplicacion == this.aplicacion.idaplicacion);

      console.log(exist);
      console.log(exist1);

      if (existID==undefined){
     
      if (exist == undefined || exist1==undefined) {
        alert("Verifique la información");
        
      } 
      else {
        this.aplicaciones.push(this.aplicacion);
        localStorage.setItem("aplicaciones", JSON.stringify(this.aplicaciones));
        this.aplicacion={};
        
        alert("Aplicación realizada con éxito, espere el proceso de selección");
      }
    
        
      } else {
        alert("Ingrese un código diferente")
      }
      } else {
        alert("Complete los campos /");
        
      }

    },

    eliminarAplicacion(){

      this.loadPage3();

      this.editing=true;

      
    },

    deleteAp(aplicacion){
      let existIndex = this.aplicaciones.findIndex((x) => x.idaplicacion == aplicacion.idaplicacion);
    if (existIndex > -1) {
      let confirmacion = confirm("Desea eliminar su aplicación?");
      if (confirmacion) {
        this.aplicaciones.splice(existIndex, 1);
        localStorage.setItem("aplicaciones", JSON.stringify(this.aplicaciones));
        this.editing=false;
      
        alert("Aplicacion eliminada");
        
      }
    } else {
      alert("La Aplicacion NO existe en la tabla.");
    }
    
    
    },

    back(){
      this.editing=false;
    }

      },
};
</script>