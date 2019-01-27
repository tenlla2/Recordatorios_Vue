<template>
  <div class="container">
    <div class="form-group">
      <input type="text" class="form-control" v-model="nombre" @keyup.enter="anadir">
    </div>
    <hr>
    <small
      class="textopeque"
    >{{tareasPendientes}} Tareas pendientes de un total de {{this.tareas.length}}</small> ||
    <small @click="borraCompletadas">Borrar tareas completadas</small>

    <hr>
    <ul class="list-group">
      <li class="list-group-item" v-for="(tarea,index) in tareasPorPrioridad" :key="tarea.id">
        <div class="row">
          <input class="checkbox-circle" type="checkbox" v-model="tarea.completada">
          <h1
            v-bind:class="[{completado : tarea.completada},'col-11','d-flex justify-content-start']"
          >{{tarea.nombre}}</h1>
          <button class="btn btn-danger" @click="borrar(index)">Borrar</button>
        </div>
        <div class="row">
          <small>Prioridad:</small>
          <button
            type="button"
            :class="['btn btn-xd', tarea.prioridad==1 ? 'b1' : 'desactivado',]"
            @click="prioridad(index,'1')"
          >Low</button>
          <button
            :class="['btn', tarea.prioridad==2 ? 'b3' : 'desactivado']"
            @click="prioridad(index,'2')"
          >Normal</button>
          <button
            :class="['btn', tarea.prioridad==3 ? 'b2' : 'desactivado']"
            @click="prioridad(index,'3')"
          >Hihg</button>
          
          <small>Añadido hace {{tarea.fecha}}</small>
        </div>
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  name: "HelloWorld",
  props: {
    msg: String
  },
  data() {
    return {
      tareas: [],
      nombre: ""
    };
  },
  methods: {
    anadir: function() {
      this.tareas.push({
        nombre: this.nombre,
        completada: false,
        prioridad: "1",
        fecha: 0,
        tiene: false
      });
      for (var i = 0; i < this.tareas.length; i++) {
        if (this.tareas[i].tiene == false) {
          this.tareas[i].fecha++;
          this.tareas[i].tiene = true;
        }
      }
      this.nombre = "";
    },
    borrar: function(index) {
      this.tareas.splice(index, 1);
    },
    borraCompletadas: function() {
      this.tareas = this.tareas.filter(tarea => !tarea.completada);
    },
    prioridad: function(index, n) {
      this.tareas[index].prioridad = n;
    },
  },

  updated: function() {
    localStorage.setItem("recordatorios-vue", JSON.stringify(this.tareas));
  },

  computed: {
    tareasPendientes: function() {
      return this.tareas.filter(tarea => !tarea.completada).length;
    },
    tareasPorPrioridad: function() {
      return this.tareas.sort((a, b) => b.prioridad - a.prioridad);
    }
  },

  mounted: function() {
    let datosDB = JSON.parse(localStorage.getItem("recordatorios-vue"));
    if (datosDB === null) {
      this.tareas = [];
    } else {
      this.tareas = datosDB;
    }
  }
};
</script>

<style scoped>
li {
  cursor: pointer;
}
.completado {
  text-decoration: line-through;
  color: rgb(17, 185, 129);
}
.desactivado {
  background: #646363;
  margin-right: 10px;
}
.pequeño {
  height: 20px;
  width: 30px;
  font-size: 9px;
}
small {
  color: grey;
  margin-left: 10px;
  margin-right: 10px;
  margin-top: 10px;
}
li {
  color: white;
  background: #3a3939;
}

.b1 {
  margin-right: 10px;
  background: rgb(83, 83, 216);
}

.b2 {
  margin-right: 10px;
  background: rgb(216, 29, 29);
}
.b3 {
  margin-right: 10px;
  background: rgb(41, 41, 248);
}
.textopeque {
  color: white;
}
</style>

