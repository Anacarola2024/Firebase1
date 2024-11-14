<script >
import Usuarios from './components/Usuarios.vue'
import Formulario from './components/Formulario.vue'


import { mapActions, mapState } from 'vuex';



export default {
  components: {
    Usuarios,
    Formulario,
    
   
  },
  data() {
    return {
      listaUsuarios: []
    }
  },
  computed: {
    ...mapState(["usuarios"]),
  },
  methods: {
    ...mapActions(["getUsuarios"])
  },
  async mounted() {
    await this.getUsuarios()
    
  }
}
</script>

<template>
  
  <Usuarios />
  <Formulario />
  
 
  
</template>

<style>
/* #app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
} */
</style>
