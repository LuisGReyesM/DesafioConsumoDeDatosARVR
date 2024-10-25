<template>
  <div id="app" class="container">
    <div class="row mt-5">
      <!-- Usuario 1 -->
      <div class="col-3">
        <div class="user-profile">
          <img :src="user1.picture.large" alt="User Photo" class="img-fluid rounded-circle mt-3">
          <p class="mt-3">{{ user1.name.first }} {{ user1.name.last }}</p>
          <input type="color" v-model="user1.color" class="form-control my-2 mt-3">
          <textarea v-model="user1.message" @keyup.enter= "enviarMensaje(user1, 'user1')" class="form-control my-2 mt-3" placeholder="Escribe tu mensaje"></textarea>
          <button @click="enviarMensaje(user1, 'user1')" class="btn btn-primary mt-3">Enviar</button>
        </div>
      </div>
      
      <!-- Chat (componente hijo) -->
      <div class="col-6 chat">
        <ChatMessages :messages="chat" />
      </div>
      
      <!-- Usuario 2 -->
      <div class="col-3">
        <div class="user-profile">
          <img :src="user2.picture.large" alt="User Photo" class="img-fluid rounded-circle mt-3">
          <p class="mt-3">{{ user2.name.first }} {{ user2.name.last }}</p>
          <input type="color" v-model="user2.color" class="form-control my-2 mt-3">
          <textarea v-model="user2.message" @keyup.enter= "enviarMensaje(user2, 'user2')"  class="form-control my-2 mt-3" placeholder="Escribe tu mensaje"></textarea>
          <button @click="enviarMensaje(user2, 'user2')" class="btn btn-primary mt-3">Enviar</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
// Importamos el componente hijo
import ChatMessages from './components/Chat.vue'; 
// Importamos el componente axios
import axios from 'axios'; 

export default {
  components: {
    // Lo registramos en el componente principal
    ChatMessages, 
  },
  
  data() {
    return {
      user1: {
        name: { first: '', last: '' },
        picture: { large: '' },
        color: "",
        message: "",
      },
      user2: {
        name: { first: '', last: '' },
        picture: { large: '' },
        color: "",
        message: "",
      },
      // Arreglo para almacenar los mensajes
      chat: [], 
    }
  },

  async mounted() {         
    //Se carga al inicio 
    await this.listarUsuario(); 
    
  },

  methods: {
    // Carga un nuevo usuario al iniciar la app o cada vez que se elimina un mensaje
    async cargarUsuario() {
      const { data } = await axios.get('https://randomuser.me/api/');
      return data.results[0];
    },

    // Carga los usuarios al iniciar la app y cada vez que se elimina un mensaje
    async listarUsuario() {
      this.user1 = await this.cargarUsuario();
      this.user2 = await this.cargarUsuario();
    },

    // Agrega un nuevo mensaje al chat y actualiza el color del usuario
    enviarMensaje(user, userKey) {
      if (user.message.trim() === "") return;
      const align = userKey === 'user1' ? 'align-left' : 'align-right';
      this.chat.push({
        name: `${user.name.first} ${user.name.last}`,
        text: user.message,
        color: user.color,
        align,
      });
      this[userKey].message = "";
    }
  },
}
</script>

<style scoped>

html, body, #app {
  height: 100%; 
}

.chat {
    overflow-y: auto;
    max-height: 400px;
    border: 1px solid #131212;
    padding: 10px;
    background-color: antiquewhite;
    border-radius: 10px;
  }
.user-profile {
  display: flex;
  flex-direction: column;
  align-items: center;
}

.user-profile img {
  width: 200px;
  height: 200px;
}
</style>
