
<template>
    <div>
      <!-- Formulario para agregar usuario -->
      <form @submit.prevent="addUsuario">
        <input v-model="nuevoUsuario" placeholder="Nuevo Usuario" />
        <input v-model="correo" placeholder="Correo" />
        <button type="submit">Agregar</button>
      </form>
  
      <!-- Lista de usuarios -->
      <ul>
        <li v-for="usuario in usuarios" :key="usuario.id">
          {{ usuario.id }} - {{ usuario.nombre }} - {{ usuario.correo }}
          <button @click="deleteUsuario(usuario.id)">Eliminar</button>
        </li>
      </ul>
    </div>
  </template>
  
  <script>
  import { getFirestore, collection, onSnapshot, addDoc, deleteDoc, doc } from 'firebase/firestore';
  import firebaseApp from '../firebaseConfig';
  
  export default {
    data() {
      return {
        nuevoUsuario: '',
        usuarios: []
      };
    },
    mounted() {
      
      const db = getFirestore(firebaseApp);
      const usuariosRef = collection(db, 'usuarios');
      onSnapshot(usuariosRef, (snapshot) => {
        this.usuarios = snapshot.docs.map((doc) => ({
          id: doc.id,
          ...doc.data()
        }));
      });
    },
    methods: {
      async addUsuario() {
        if (this.nuevoUsuario.trim() === '') return;
  
        const db = getFirestore(firebaseApp);
        const usuariosRef = collection(db, 'usuarios');
  
        try {
          
          await addDoc(usuariosRef, {
            nombre: this.nuevoUsuario,  
            correo: this.correo,  
          });
  
        
          this.nuevoUsuario = '';
          this.correo = '';
        } catch (error) {
          console.error('Error al agregar usuario:', error);
        }
      },
  
      async deleteUsuario(id) {
        const db = getFirestore(firebaseApp);
        const usuarioRef = doc(db, 'usuarios', id);
  
        try {
          // Eliminar usuario de Firestore
          await deleteDoc(usuarioRef);
        } catch (error) {
          console.error('Error al eliminar usuario:', error);
        }
      }
    }
  };
  </script>
  
  