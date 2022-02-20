<template>
  <div>
    <h1 class="display-4 text-center">Listado de Maquinas</h1>
    <hr />
    <div class="row">
      <div class="col-lg-8 offset-lg-2">
        <div class="card mt-4">
          <div class="card-body">
            <div class="input-group">
              <input
                type="text"
                v-model="maquina"
                class="form-control form-control-lg"
                placeholder="Agregar Maquina "
              />
              <div class="input-group-append">
                <button
                  v-on:click="agregarMaquina()"
                  class="btn btn-success btn-lg"
                >
                  Agregar Maquina
                </button>
              </div>
            </div>
            <br />
            <div class="text-center">
              <div
                v-if="loading"
                class="spinner-border text-success"
                role="status"
              >
                <span class="sr-only">Loading...</span>
              </div>
            </div>

            <h5 v-if="listMaquinas.length == 0">No hay Maquinas Registradas</h5>
            <ul v-if="!loading" class="list-group">
              <li
                v-for="(maquina, index) of listMaquinas"
                :key="index"
                class="list-group-item d-flex justify-content-between"
              >
                <span
                  class="cursor"
                  v-bind:class="{ 'text-success': maquina.estado }"
                  v-on:click="editarMaquina(maquina, maquina.idMaquina)"
                >
                  <i
                    v-bind:class="[
                      maquina.estado ? 'fas fa-check-circle' : 'far fa-circle',
                    ]"
                  ></i>
                </span>
                <span
                  class="cursor"
                  v-bind:class="{ 'text-success': maquina.descMaquina }"
                  v-on:click="editarMaquina2(maquina, maquina.idMaquina)"
                >
                 {{maquina.descMaquina}}
                </span>
                <span
                  class="text-danger cursor"
                  v-on:click="eliminarMaquina(maquina.idMaquina)"
                >
                  <i class="fas fa-trash-alt"></i>
                </span>
              </li>
            </ul>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";

const URL = "https://localhost:7190/Maquina/";
const URL2="https://localhost:7190/Maquina2/";
export default {
  name: "Maquina",
  data() {
    return {
      maquina: "",
      listMaquinas: [],
      loading: false,
    };
  },
  methods: {
    agregarMaquina() {
      const maquina = {
        descMaquina:this.maquina,
        estado:true,
      };
      /*  this.listTareas.push(tarea); */
      this.loading = true;
      axios
        .post(URL, maquina)
        .then((response) => {
          console.log(response);
          this.loading = false;
          this.obtenerMaquinas();
        })
        .catch((error) => {
          console.error(error);
          this.loading = false;
        });
      this.maquina = "";
    },
    eliminarMaquina(idMaquina) {
       console.log("va a borrar un registro");
      /* this.listTareas.splice(index, 1) */
      this.loading = true;
      console.log(this.maquina, this.$data);
      console.log(URL + idMaquina);
      axios
              .delete(URL + idMaquina)
        .then((response) => {
          console.log(response);
          this.obtenerMaquinas();
          this.loading = false;
        })
        .catch((error) => {
          console.log(error);
          this.loading = false;
        });
    },
    editarMaquina(maquina, id) {
     /*  this.listTareas[index].estado = !tarea.estado; */
     this.loading = true;
     axios.put(URL + id, maquina).then(()=> {
       this.obtenerMaquinas();
       this.loading = false
     }).catch(() => this.loading = false);
    },
     editarMaquina2(maquina, id) {
     /*  this.listTareas[index].estado = !tarea.estado; */
    console.log(maquina)
    maquina.descMaquina=maquina.descMaquina+"   prueba";
     console.log(maquina)
     this.loading = true;
     axios.put(URL2 + id, maquina).then(()=> {
       this.obtenerMaquinas();
       this.loading = false
     }).catch(() => this.loading = false);
    },
    obtenerMaquinas() {
      this.loading = true;
      axios.get(URL).then((response) => {
        console.log("hasta aqui llego");
        console.log(response);
        this.listMaquinas = response.data;
        this.loading = false;
      }).catch(() => this.loading = false);
    },
  },
  created: function () {
    this.obtenerMaquinas();
  },
};
</script>

<style scoped>
.cursor {
  cursor: pointer;
}
</style>