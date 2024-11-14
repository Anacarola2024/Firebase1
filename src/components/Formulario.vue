<template>
    <li v-if="data">
        {{ data.nombre }} - {{ data.correo }}
        <button @click="deleteProducto( data.id )"> Eliminar </button>
    </li>
</template>

<script>
    import { mapActions, mapState } from "vuex";
    import  app  from '../firebaseConfig';
    import { getFirestore, collection, deleteDoc, doc } from 'firebase/firestore/lite';

    export default {
        name: 'ListarUsuarios',
        props: {
            data: Object
        },
        computed: {
            ...mapState(["usuarios"]),
        },
        methods: {
            ...mapActions(["change"]),
            async deleteProducto( id ) {
                try {
                    const db = getFirestore( app );
                    await deleteDoc( doc( collection( db, 'usuarios' ), id  ) );
                    this.change(this.usuarios.filter( usuario => usuario.id !== id ))
                } catch (error) {
                    console.log(error);
                }
            }
        }
    }
</script>
  <style scoped>
  /* Estilos opcionales para el formulario */
  form {
    display: flex;
    flex-direction: column;
    width: 300px;
  }
  
  div {
    margin-bottom: 10px;
  }
  
  button {
    padding: 5px;
  }
  </style>