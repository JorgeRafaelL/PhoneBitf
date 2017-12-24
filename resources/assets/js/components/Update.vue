<template>
  <div class="modal" :class = 'openModal'>
    <div class="modal-background"></div>
    <div class="modal-card">
      <header class="modal-card-head">
        <p class="modal-card-title">Actualizar detalles del usuario: {{ list.name }}</p>
        <button class="delete" aria-label="close" @click='close'></button>
      </header>
      <section class="modal-card-body">
        <!-- Content ... -->
        <div class="field">
          <label class="label">Nombre</label>
          <div class="control">
            <input v-model="list.name" :class="{'is-danger':errors.name}" class="input" type="text" placeholder="Nombre">
          </div>
          <small v-if="errors.name" class="has-text-danger"> {{ errors.name[0] }} </small>
        </div>

        <div class="field">
          <label class="label">Teléfono</label>
          <div class="control">
            <input v-model="list.phone" :class="{'is-danger':errors.phone}" class="input" type="number" placeholder="Telefono">
          </div>
          <small v-if="errors.phone" class="has-text-danger"> {{ errors.phone[0] }} </small>
        </div>

        <div class="field">
          <label class="label">Email</label>
          <div class="control has-icons-left has-icons-right">
            <input v-model="list.email" :class="{'is-danger':errors.email}" class="input" type="email" placeholder="Correo electrónico">
            <span class="icon is-small is-left">
              <i class="fa fa-envelope"></i>
            </span>
            
          </div>
          <small v-if="errors.email" class="has-text-danger"> {{ errors.email[0] }} </small>
          
        </div>

      </section>
      <footer class="modal-card-foot">
        <button class="button is-success" @click="update">Guardar cambios</button>
        <button class="button"  @click='close'>Cancelar</button>
      </footer>
    </div>
  </div>
</template>

<script>
export default{
  data(){
    return{
      list: {
      },
      errors: {

      }
    }
  },
  props:['openModal'],
  methods:{
    close(){
      this.$emit('closeRequest')
    },
    update(){
      axios.patch(`/phonebook/${this.list.id}`, this.$data.list)
      .then((response) => {
        this.close()

        this.$parent.lists.sort(function(a,b){
          if (a.name > b.name) {
            return 1;
          }
          else if (a.name < b.name) {
            return -1;
          }
        })   
        
      })
      .catch((error) => this.errors = error.response.data.errors)
    }
  }
}
</script>
