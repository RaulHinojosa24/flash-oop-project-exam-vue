<script>
import AddQuestion from './components/AddQuestion.vue';
import QuestionCard from './components/QuestionCard.vue';

export default {
  components: {
    AddQuestion, QuestionCard
  },
  data() {
    return {
      nextId: 3,
      notes: [{
        id: 1,
        question: "¿Cómo hago para iterar un array con Vue?",
        answer: "Usa la directiva v-for"
      }, {
        id: 2,
        question: "¿Cómo hago para relacionar una variable de estado con el campo <em>value</em>' de input?",
        answer: "La directiva v-model nos ayudará en este caso"
      }]
    }
  },
  methods: {
    addNewNote(note) {
      this.notes.push({ id: this.nextId++, ...note });
      this.updateLocalStorage();
    },
    deleteNote(id) {
      this.notes = this.notes.filter(note => note.id != id);
      this.updateLocalStorage();
    },
    editNote(data) {
      const target = this.notes.find(note => note.id == data.id);

      target.question = data.newData.question;
      target.answer = data.newData.answer;

      this.updateLocalStorage();
    },
    updateLocalStorage() {
      localStorage.notes = JSON.stringify(this.notes);
    }
  },
  mounted() {
    if (localStorage.notes) {
      this.notes = JSON.parse(localStorage.notes);
    }
  }
}

</script>

<template>
  <div class="container">
    <div class="row">
      <div class="col-11 col-lg-6 my-3">
        <AddQuestion @newNote="(note) => addNewNote(note)" />
      </div>
    </div>

    <div class="row px-2" id="questions-list">
      <QuestionCard v-for="note in notes" :key="note.id" :note="note" :noteQuestion="note.question"
        @delete="(id) => deleteNote(id)" @edit="(data) => editNote(data)" />
    </div>
  </div>
</template>

<style>
@import './assets/main.css';
@import './assets/all.css';
</style>