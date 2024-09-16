<template>
  <div class="min-h-screen flex flex-col bg-gray-100">
    <nav class="bg-indigo-600 p-4">
      <h1 class="text-2xl font-bold text-white text-center">Chatbot Yes/No</h1>
    </nav>

    <div class="flex-grow flex flex-col p-4 sm:p-8 relative">


      <div class="chat-container" ref="chatContainer">
        <div v-for="(message, index) in messages" :key="index" :class="messageClass(message.type)">
          <div :class="messageBubbleClass(message.type)">
            <p>{{ message.text }}</p>
            <img v-if="message.image" :src="message.image" alt="Response image" class="message-image" />
          </div>
        </div>
      </div>
      
      <div class="chat-input-container">
        <ChatInput @question-submitted="handleQuestion" />
      </div>
    </div>
  </div>
</template>

<script>
import ChatInput from './components/ChatInput.vue';

export default {
  components: { ChatInput },
  data() {
    return { messages: [] };
  },
  methods: {
    async handleQuestion(question) {
      this.messages.push({ text: question, type: 'user' });

      try {
        const response = await fetch("https://yesno.wtf/api");
        const data = await response.json();
        this.messages.push({ text: data.answer, type: 'bot', image: data.image });
        this.$nextTick(() => {
          const container = this.$refs.chatContainer;
          container.scrollTop = container.scrollHeight;
        });
      } catch (error) {
        console.error("Error al consumir la API", error);
      }
    },
    messageClass(type) {
      return type === 'user' ? 'text-right' : 'text-left';
    },
    messageBubbleClass(type) {
      return `inline-block p-3 rounded-lg ${type === 'user' ? 'bg-blue-500 text-white' : 'bg-gray-200 text-gray-800'}`;
    }
  },
};
</script>

<style scoped>
/* Estilos globales básicos */
.min-h-screen {
  min-height: 100vh;
}

.flex-grow {
  flex-grow: 1;
}

.bg-gray-100 {
  background-color: #f7fafc;
}

/* Estilos del contenedor del chat */
.chat-container {
  background: #ffffff;
  box-shadow: 0 1px 4px rgba(0, 0, 0, 0.1);
  border-radius: 0.5rem;
  padding: 1rem;
  flex-grow: 1;
  overflow-y: auto;
  margin-bottom: 4rem;
  /* Espacio para el contenedor de entrada */
}

/* Estilos del contenedor de entrada */
.chat-input-container {
  position: sticky;
  bottom: 0;
  left: 0;
  right: 0;
  background-color: #f9fafb;
  padding: 0.5rem;
  border-top: 1px solid #e5e7eb;
}

/* Estilos para las imágenes dentro de los mensajes */
.message-image {
  width: 5rem;
  height: 5rem;
  margin-top: 0.5rem;
}

/* Estilos de los mensajes en el chat */
.text-right {
  text-align: right;
}

.text-left {
  text-align: left;
}
</style>
