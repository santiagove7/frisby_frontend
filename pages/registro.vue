<template>
  
    <div>
      <v-btn @click="salir" block color="#FD0"> Salir </v-btn>
      <br>
      <v-btn @click="voyEditar" block color="#FD0"> Editar usuario </v-btn>
    
    <br>

    <h1 v-if="editing">Seleccionar Usuario</h1>
    

     <v-data-table v-if="editing"
    :headers="usuariosR"
    :items="users"
    :items-per-page="5"
    class="elevation-1"
    
  > <template v-slot:item.acciones="{ item }">
    <v-btn @click="editUser(item)" block color="#FD0"> Seleccionar </v-btn>
      
    </template></v-data-table>
    <br>

  <h1 v-if="!editing">Registro</h1>
  <h1 v-if="editing">Edición</h1>
  
    <v-form v-model="valid" ref="valid">
      <v-container>
        <v-row>
          <v-col cols="12" md="4">
            <v-text-field
              v-model="user.id"
              :rules="fieldRequired"
              :counter="10"
              label="Identificación"
              required
              :disabled="editing"
            ></v-text-field>
          </v-col>
          <v-col cols="12" md="4">
            <v-text-field
              v-model="user.name"
              :rules="fieldRequired"
              label="Nombre"
              required
            ></v-text-field>
          </v-col>

          <v-col cols="12" md="4">
            <v-text-field
              v-model="user.lastname"
              :rules="fieldRequired"
              label="Apellidos"
              required
            ></v-text-field>
          </v-col>

          <v-col cols="12" md="4">
            <v-text-field
              v-model="user.email"
              :rules="emailRules"
              label="E-mail"
              required
            ></v-text-field>
          </v-col>
          <v-col cols="12" md="4">
            <v-text-field
              v-model="user.password"
              :rules="fieldRequired"
              type="password"
              :counter="8"
              label="Contraseña"
              required
              
            ></v-text-field>
          </v-col>
          <v-col cols="12" md="4">
            <v-text-field
              v-model="user.telefono"
              :rules="fieldRequired"
              label="Telefono"
              required
            ></v-text-field>
          </v-col>
          <v-col cols="12" md="4">
            <v-text-field
              v-model="user.direccion"
              :rules="fieldRequired"
              label="Direccion"
              required
            ></v-text-field>
          </v-col>
          <v-col cols="12" md="4">
            <v-text-field
              v-model="user.ciudad"
              :rules="fieldRequired"
              label="Ciudad"
              required
            ></v-text-field>
          </v-col>
          <v-col cols="12" md="4">
            <v-checkbox
              required
              :rules="fieldRequired"
              v-model="user.accepted"
              :label="'Acepto tratamiento de datos'"
            ></v-checkbox>
          </v-col>
          <v-col cols="12" md="4">
            <v-textarea
              v-model="user.descripcion"
              required
              :rules="fieldRequired"
              label="Breve descripción/cualidades"
              :counter="200"
            ></v-textarea>
          </v-col>
        </v-row>
        <v-file-input
          required
          v-model="user.hdv"
          :rules="fieldRequired"
          accept=".doc,.docx,application/msword,application/vnd.openxmlformats-officedocument.wordprocessingml.document"
          label="Cargar hoja de vida"
        ></v-file-input>
        <v-btn v-if="!editing" @click="save" block color="#FD0"> Guardar </v-btn>
        <v-btn v-if="editing" @click="editarUsuario" block color="#FD0"> Guardar cambios </v-btn>
      </v-container>
    </v-form>

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
  </div>
</template>

<script>
export default {
  beforeMount(){
    this.loadPage();
  },
  layout: "blank",
  data() {
    return {
      usuariosR:[
        {text:"Identificacion", value:"id"},
        {text:"Nombre", value:"name"},
        {text:"Apellido", value:"lastname"},
        {text:"Correo", value:"email"},
        {text:"Telefono", value:"telefono"},
        {text:"Hoja de Vida", value:"hdv"},
        {text:"Acciones", value:"acciones"},
      ],
      valid: null,
      users:[],
      user: {
        id: null,
        name: null,
        lastname: null,
        email: null,
        password: null,
        direccion: null,
        telefono: null,
        descripcion: null,
        accepted: null,
        ciudad: null,
        hdv: null,
      },

      fieldRequired: [(v) => !!v || "Field is required"],
      emailRules: [
        (v) => !!v || "E-mail is required",
        (v) => /.+@.+/.test(v) || "E-mail must be valid",
      ],
      dialog: false,
      editing:false,
    };
  },
  methods: {

    loadPage(){
      //captura info del localst
      let users = localStorage.getItem("users");
      this.users = JSON.parse(users);
    },
     save() {
      if (this.$refs.valid.validate() && this.valid == true) {
      let exist = this.users.find((x) => x.id == this.user.id);
      // Validación si la identificación de una persona ya existe en el array
      if (exist == undefined) {
        this.users.push(this.user);
        localStorage.setItem("users", JSON.stringify(this.users));
        this.user={};
        alert("El usuario ha sido registrado correctamente.");
      } else {
        alert("El ID ya se encuentra registrado.");
      }
    
        
      } else {
        this.dialog = true;
      }
    },



    salir() {
      this.$router.push("/");
    },

    voyEditar(){
      this.editing=true;
    },

    editUser(user){
      this.user= Object.assign({},user);
    },

     editarUsuario(){
      
    if (this.$refs.valid.validate() && this.valid == true){
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
        alert("El usuario NO existe en registro.");
      }
    
    }else {
        console.log("Hay campos sin llenar");
        this.dialog = true;
      }
    },
  },
};
</script>