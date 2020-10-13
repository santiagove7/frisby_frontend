<template>
  <div>
    <h2 v-if="!editing" align="center">Nueva oferta </h2>
    <h2 v-else align="center">Editar oferta </h2>
    <v-form v-model="valid" ref="valid">
      <v-container>
        <v-row>
          <v-col cols="12" md="4">
            <v-text-field
              v-model="oferta.idoferta"
              :rules="fieldRequired"
              :counter="10"
              label="Código"
              :disabled="editing"
              required
            ></v-text-field>
          </v-col>
          <v-col cols="12" md="4">
            <v-text-field
              v-model="oferta.nombre"
              :rules="fieldRequired"
              label="Nombre"
              required
            ></v-text-field>
          </v-col>

          <v-col cols="12" md="4">
            <v-text-field
              v-model="oferta.cargo"
              :rules="fieldRequired"
              label="Cargo"
              required
            ></v-text-field>
          </v-col>

          <v-col cols="12" md="4">
            <v-text-field
              v-model="oferta.ciudad"
              :rules="fieldRequired"
              label="Ciudad"
              required
            ></v-text-field>
          </v-col>

          <v-col cols="12" md="4">
            <v-textarea
              v-model="oferta.descripcion"
              required
              :rules="fieldRequired"
              label="Descripción de la oferta"
              :counter="500"
            ></v-textarea>
          </v-col>
          <v-col cols="12" md="4">
            <v-textarea
              v-model="oferta.requisitos"
              required
              :rules="fieldRequired"
              label="Requisitos"
              :counter="500"
            ></v-textarea>
          </v-col>

          <v-col cols="12" md="4">
            <v-checkbox
      v-model="oferta.activa"
      :label="`¿Oferta Activa?`"
    ></v-checkbox>
          </v-col>
        </v-row>

        <v-btn @click="save" v-if="!editing" block color="#FFd700"> Guardar </v-btn>
        <v-btn @click="editarOferta" v-else block color="#FFd700"> Editar </v-btn>
      </v-container>
    </v-form>
    <br>
    <br>

    <h2 align="center">Ofertas</h2>

    <v-data-table
    :headers="headers"
    :items="ofertas"
    :items-per-page="5"
    class="elevation-1"
    
  >
  <template v-slot:item.acciones="{ item }">
      <v-icon
        small
        class="mr-2"
        @click="editOferta(item)"
      >
        mdi-pencil
      </v-icon>
      <v-icon
        small
        @click="deleteOferta(item)"
      >
        mdi-delete
      </v-icon>
    </template>
  
  </v-data-table>

    <v-dialog v-model="dialog" max-width="290">
      <v-card>
        <v-card-title class="headline"> Error </v-card-title>

        <v-card-text> Form required some files </v-card-text>

        <v-card-actions>
          <v-spacer></v-spacer>

          <v-btn color="green darken-1" text @click="dialog = false">
            Close
          </v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
    <v-dialog v-model="dialog1" max-width="290">
      <v-card>
        <v-card-title class="headline"> Registro </v-card-title>

        <v-card-text> Registro exitoso </v-card-text>

        <v-card-actions>
          <v-spacer></v-spacer>

          <v-btn color="green darken-1" text @click="dialog1 = false">
            Close
          </v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
  </div>
</template>

<script>
export default {
  layout:"defaultadmon",
  name:"gestoroferta",
  beforeMount(){
    this.loadPage();
  },
  data() {
    return {
      valid: null,
      headers:[
        {text:"Código", value:"idoferta"},
        {text:"Nombre", value:"nombre"},
        {text:"Cargo", value:"cargo"},
        {text:"Activa?", value:"activa"},
        {text:"Acciones", value:"acciones"},


      ],
      ofertas:[],
      oferta: {
        idoferta: null,
        nombre: null,
        cargo: null,
        requisitos: null,
        descripcion: null,
        ciudad: null,
        activa: false,
      },

      fieldRequired: [(v) => !!v || "Field is required"],
      emailRules: [
        (v) => !!v || "E-mail is required",
        (v) => /.+@.+/.test(v) || "E-mail must be valid",
      ],
      dialog: false,
      dialog1: false,
      editing:false,
    };
  },
  methods: {
    loadPage(){
      //captura info del localst
      let ofertas = localStorage.getItem("ofertas");
      this.ofertas = JSON.parse(ofertas);

    },

    save() {
      if (this.$refs.valid.validate() && this.valid == true) {
        //save
      let exist = this.ofertas.find((x) => x.idoferta == this.oferta.idoferta);
      // Validación si la identificación de una persona ya existe en el array
      if (exist == undefined) {
        this.ofertas.push(this.oferta);
        localStorage.setItem("ofertas", JSON.stringify(this.ofertas));
        this.oferta={};
        alert("La oferta ha sido creada correctamente.");
        this.dialog1 = true;
      } else {
        alert("La oferta ya existe en la tabla.");
      }
    
        
      } else {
        this.dialog = true;
      }
    },
    editOferta(oferta){
      this.oferta= Object.assign({},oferta);
      this.editing=true;

    },

    editarOferta(){
      
    if (this.$refs.valid.validate() && this.valid == true){
      let existIndex = this.ofertas.findIndex(
        (x) => x.idoferta == this.oferta.idoferta
      );
      if (existIndex > -1) {
        
        
        this.ofertas.splice(existIndex, 1, this.oferta);
        
        this.user={};
        this.editing=false;
        localStorage.setItem("ofertas", JSON.stringify(this.ofertas));
        this.oferta={};


        alert("La oferta ha sido modificada correctamente.");
      } else {
        alert("La oferta NO existe en la tabla.");
      }
    
    }else {
        console.log("Hay campos sin llenar");
        this.dialog = true;
      }
    },


    deleteOferta(oferta){
          let existIndex = this.ofertas.findIndex((x) => x.idoferta == oferta.idoferta);
    if (existIndex > -1) {
      let confirmacion = confirm("Desea eliminar la oferta??");
      if (confirmacion) {
        this.ofertas.splice(existIndex, 1);
        localStorage.setItem("ofertas", JSON.stringify(this.ofertas));
      
        alert("Oferta eliminada");
      }
    } else {
      alert("La oferta NO existe en la tabla.");
    }

    },
  },
};
</script>