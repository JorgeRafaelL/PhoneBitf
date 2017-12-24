<template>
  <div class="modal" :class = 'openModal'>
    <div class="modal-background"></div>
    <div class="modal-card">
      <header class="modal-card-head">
        <p class="modal-card-title">Detalles del usuario: {{ list.name }}</p>
        <button class="delete" aria-label="close" @click='close'></button>
      </header>
      <section class="modal-card-body">
        <!-- Content ... -->
        <li class="panel-block">
          <label class="column is-2"><strong>Nombre</strong></label> {{ list.name }}
        </li>

        <li class="panel-block">
          <label class="column is-2"><strong>Teléfono</strong></label> {{ list.phone }}
        </li>

        <li class="panel-block">
          <label class="column is-2"><span class="icon is-small is-left">
              <i class="fa fa-envelope"></i>
            </span> <strong>Email</strong></label> {{ list.email }}
        </li>

      </section>
      <footer class="modal-card-foot">
        
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

        }
      }
    },
    props: ['openModal'],
    methods:{
    close(){
      this.$emit('closeRequest')
    },
    
  }
  }
</script>