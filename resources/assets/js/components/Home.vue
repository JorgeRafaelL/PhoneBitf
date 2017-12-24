<template>
  <div>
    <nav class="panel column is-offset-2 is-8">
      <p class="panel-heading">
        Vuejs Phonebook
        <button class="button is-link is-outlined" @click="openAdd">
          Agregar nuevo
        </button>
        <span class="is-pulled-right" v-if="loading">
          <i class="fa fa-refresh fa-spin fa-1x fa-fw"></i>
        </span>
        
      </p>
      <div class="panel-block">
        <p class="control has-icons-left">
          <input class="input is-small" type="text" placeholder="search" v-model="searchQuery">
          <span class="icon is-small is-left">
            <i class="fa fa-search"></i>
          </span>
        </p>
      </div>


      <a class="panel-block" v-for="item, key in temp">
        <span class="column is-8">
          {{ item.name }}
        </span>


        <span class="panel-icon column is-1 icon is-large t-icono">
          <i class="fa fa-eye has-text-info" aria-hidden="true" @click="openShow(key)"></i>
          
        </span>
        <span class="panel-icon column is-1 icon is-large t-icono">
          <i class="fa fa-edit has-text-success" aria-hidden="true" @click="openUpdate(key)"></i>
        </span>
        <span class="panel-icon column is-1 icon is-large t-icono">
          <i class="fa fa-trash has-text-danger" aria-hidden="true" @click="deleteUser(key, item.id)"></i>
        </span>
      </a>

    </nav>
    <Add :openModal = 'addActive' @closeRequest='close'></Add>
    <Show :openModal = 'showActive' @closeRequest='close'></Show>
    <Update :openModal = 'updateActive' @closeRequest='close'></Update>
  </div>

</template>

<script>
let Add = require('./Add.vue');
let Show = require('./Show.vue');
let Update = require('./Update.vue');

export default{
  components: {Add, Show, Update},
  data(){
    return{
      addActive: '',
      showActive: '',
      updateActive: '',
      lists: {},
      errors: {},
      loading: false,
      searchQuery:'',
      temp: ''
    }
  },
  watch: {
    searchQuery(){
      if (this.searchQuery.length > 0) {
        this.temp = this.lists.filter((item) => {
          return Object.keys(item).some((key) => {
            let arre = String(item[key])
            return arre.toLowerCase().indexOf(this.searchQuery.toLowerCase()) > -1
          })
          
        });
      }
      else {
        this.temp = this.lists;
      }
    }
  },
  mounted(){
    axios.post('/getData')
    .then((response) => this.lists = this.temp = response.data)
    .catch((error) => this.errors = error.response.data.errors)
  },
  methods: {
    openAdd(){
      this.addActive = 'is-active';
    },
    openShow(key){
      this.$children[1].list = this.temp[key];
      this.showActive = 'is-active';
    },
    openUpdate(key){
      this.$children[2].list = this.temp[key];
      this.updateActive = 'is-active';
    },
    deleteUser(key, id){

      swal({
        title: "¿Estás seguro de eliminar al usuario " + this.temp[key].name + "?",
        text: "Una vez eliminado no podrás recuperarlo!",
        icon: "warning",
        buttons: true,
        dangerMode: true,
      })
      .then((willDelete) => {
        if (willDelete) {

          this.loading = !this.loading;
          axios.delete(`/phonebook/${id}`).then((response) => {this.lists.splice(key, 1);
            this.loading = !this.loading;})
          .catch((error) => this.errors = error.response.data.errors)

          swal("Eliminado con exito!", {
            icon: "success",
          });
        } 
        else {
          swal("No se eliminó el usuario!", {
            title: "Cancelado!",
            icon: "error"
          });
        }
      });  
      
    },
    close(){
      //this.addActive = '',
      //this.showActive = ''
      //Lo mismo de arriba
      this.addActive = this.showActive = this.updateActive = ''
    }
  }
}
</script>

<style>
.t-icono{
  font-size: 25px;
}
</style>
