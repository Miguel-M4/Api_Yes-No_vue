<template>
  <div class="flex flex-col">
    <input
      v-model="userQuestion"
      type="text"
      placeholder="Escribe tu pregunta..."
      class="w-full p-3 border border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-indigo-400 mb-4 text-black"
      @keydown.enter="submitQuestion" 
    />
    <button
      @click="submitQuestion"
      class="bg-indigo-400 text-white px-6 py-3 rounded-lg hover:bg-indigo-600 transition-colors duration-200"
    >
      Enviar
    </button>
  </div>
</template>

<script>
export default {
  data() {
    return {
      userQuestion: "",
    };
  },
  methods: {
    submitQuestion() {
      if (this.userQuestion.trim() === "") {
        alert("Por favor, escribe una pregunta.");
        return;
      }
      this.$emit("question-submitted", this.userQuestion);
      this.userQuestion = ""; // Limpiar el input después de enviar la pregunta
    },
  },
};
</script>

<style scoped>
/* Asegúrate de que el botón y el input se alineen bien */
button {
  align-self: flex-end;
}
</style>
