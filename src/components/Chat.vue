<template>   
      <div v-for="message in messages" :key="message.id" 
           :class="message.align" class="message-container">
        <strong>{{ message.name }}</strong>
        <div class="message" :style="{ backgroundColor: message.color }">
          {{ message.text }}
        </div>
      </div>

  </template>
  
  <script>
  export default {
   //Escucha las peticiones del componente padre para los mensajes
    props: {
      messages: {
        type: Array,
        required: true,
      },
    },
  };
  </script>
  
  <style scoped>
  
  
  .message-container {
    margin-bottom: 10px;
    display: flex;
    flex-direction: column;
  }
  
  .align-left {
    align-items: flex-start;
  }
  
  .align-right {
    align-items: flex-end;
  }
  
  .message {
    padding: 10px;
    border-radius: 30px;
    max-width: 60%;
    word-wrap: break-word;
    color: black;
  }
  
  .message-container strong {
    margin-bottom: 5px;
  }
  </style>
  