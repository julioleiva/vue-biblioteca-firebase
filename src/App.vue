<template>
  <div id="app">
    <div class="container">
      <div class="page-header">
        <h1>VueJs2 y Firebase</h1>
      </div>

      <div class="panel panel-default">
        <div class="panel-heading">
          <h3>Añadir libro</h3>
        </div>
        <div class="panel-body">
          <form  id="form" class="form-inline" v-on:submit.prevent="anadirLibro">
            <div class="form-group">
              <label for="tituloLibro">Titulo:</label>
              <input type="text" id="tituloLibro" class="form-control" v-model="nuevoLibro.titulo">
            </div>
            <div class="form-group">
              <label for="anioLibro">Año:</label>
              <input type="text" id="anioLibro" class="form-control" v-model="nuevoLibro.AnioEdicion">
            </div>
            <div class="form-group">
              <label for="urlLibro">URL:</label>
              <input type="text" id="urlLibro" class="form-control" v-model="nuevoLibro.url">
            </div>
            <input type="submit" class="btn btn-primary" name="" value="Añadir">
          </form>
        </div>

      </div>


      <div class="panel panel-default">
        <div class="panel-heading">
          <h3>Listado de libros de Enrique Vila-Matas</h3>
        </div>
        <div class="panel-body">
          <table class="table table-striped">
            <thead>
              <tr>
                <th>Titulo</th>
                <th>Año</th>
                <th>Eliminar</th>
              </tr>
            </thead>
            <tbody>
              <tr v-for="libro in libros">
                <td><a :href="libro.url" target="_blank">{{ libro.titulo}}</a></td>
                <td>{{ libro.AnioEdicion}}</td>
                <td><span class="glyphicon glyphicon-trash" @click="eliminarLibro"></span></td>
              </tr>
            </tbody>
          </table>

        </div>
      </div>

    </div>
  </div>
</template>

<script>

import Firebase from 'firebase'

let config = {
    apiKey: "AIzaSyAfKCAY6PLPYkBnG3rftQH6GJ_C-Vu-Q5o",
    authDomain: "vue-firebase-good.firebaseapp.com",
    databaseURL: "https://vue-firebase-good.firebaseio.com",
    projectId: "vue-firebase-good",
    storageBucket: "vue-firebase-good.appspot.com",
    messagingSenderId: "832477989079"
}

let app = Firebase.initializeApp(config);
let db = app.database();
let refDbLibro = db.ref('libros');

export default {
  name: 'app',
  firebase:{
    libros: refDbLibro
  },
  data(){
    return {
      nuevoLibro: {
        titulo:null,
        AnioEdicion:null,
        url:null
      }
    }
  },
  methods:{
    anadirLibro(){
      refDbLibro.push(this.nuevoLibro);
      this.nuevoLibro.titulo =null;
      this.nuevoLibro.AnioEdicion =null;
      this.nuevoLibro.url =null;
      console.log('Hola');
    },
    eliminarLibro(libro){
      console.log(refDbLibro.child(libro['.key']));
      // refDbLibro.child(libro['.key']).remove();
    }
  }

}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  margin-top: 10px;
}

h1, h2 {
  font-weight: normal;
}

ul {
  list-style-type: none;
  padding: 0;
}

li {
  display: inline-block;
  margin: 0 10px;
}

a {
  color: #42b983;
}
</style>
