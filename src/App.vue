<template>
  <main>
    <div v-if="showModal" class="overlay">
      <div class="modal">
        <textarea v-model.trim="newNote" name="note" id="note" cols="30" rows="10"></textarea>
        <p v-if="errorMessage">{{errorMessage}}</p>
        <button @click="addNote">Add Note</button>
        <button @click="showModal = false" class="close">Close</button>
      </div>
    </div>
    <div class="container">
      <header>
        <h1>Notes</h1>
        <button @click="showModal = true">+</button>
      </header>
      <div class="cards-container">
        <div v-for="note in notes"
             :key="note.id"
             :style="{backgroundColor: note.backgroundColor}"
             class="card">
          <p class="main-text">
            {{ note.text }}
          </p>
          <p class="date">{{ note.date }}</p>
        </div>
      </div>
    </div>
  </main>
</template>

<script setup>
import {ref} from "vue";

const showModal = ref(false);
const newNote = ref("");
const errorMessage = ref("");
const notes = ref([]);

const randomLightColor = () => "hsl(" + Math.random() * 360 + ", 100%, 75%)";
const addNote = () => {
  if(newNote.value.length < 10) {
    return errorMessage.value = "Note needs to be at least 10 characters"
  }
  notes.value.push({
    id: Math.floor(Math.random() * 1000000),
    text: newNote.value,
    date: new Date().toLocaleDateString(),
    backgroundColor: randomLightColor(),
  });
  showModal.value = false;
  newNote.value = "";
  errorMessage.value = "";
}

</script>

<style scoped>

main {
  width: 100vw;
  min-height: 100vh;
  overflow-y: scroll;
}

.container {
  max-width: 1000px;
  padding: 20px;
  margin: 0 auto;
}

header {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

h1 {
  font-weight: bold;
  margin-bottom: 75px;
  font-size: 75px;
  color: var(--text-color);
}

header button {
  border: none;
  padding: 10px;
  width: 50px;
  height: 50px;
  cursor: pointer;
  background-color: var(--background-button);
  border-radius: 100%;
  color: var(--text-color);
  font-size: 20px;
  transition: background-color var(--transition-duration), transform var(--transition-transform), box-shadow var(--transition-box-shadow);
}
header button:hover {
  background-color: var(--background-button-hover);
  transform: scale(1.05);
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.3);
}

.card {
  width: 225px;
  height: 225px;
  background-color: var(--background-gradient);
  padding: 10px;
  border-radius: 15px;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  margin-right: 20px;
  margin-bottom: 20px;
  overflow: hidden;
}
.main-text {
  line-height: 1.25;
  font-size: 17px;
  font-weight: bold;
  overflow: hidden;
  white-space: normal;
}

.date {
  font-size: 13px;
  font-weight: bold;
}

.cards-container {
  display: flex;
  flex-wrap: wrap;
}

.overlay {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: var(--background-overlay);
  z-index: 10;
  display: flex;
  align-items: center;
  justify-content: center;
}

.modal {
  width: 750px;
  background-color: var(--background-modal);
  border-radius: 10px;
  padding: 30px;
  position: relative;
  display: flex;
  flex-direction: column;
}

.modal textarea {
  background-color: var(--background-textarea);
}

.modal button {
  padding: 10px 20px;
  font-size: 20px;
  width: 100%;
  background-color: var(--background-button);
  border: none;
  color: var(--text-color);
  cursor: pointer;
  margin-top: 15px;
  transition: background-color var(--transition-duration), transform var(--transition-transform), box-shadow var(--transition-box-shadow);
}
.modal button:hover {
  background-color: var(--background-button-hover);
  transform: scale(1.05);
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.3);
}

.modal .close {
  background-color: var(--background-close-button);
  transition: background-color var(--transition-duration), transform var(--transition-transform), box-shadow var(--transition-box-shadow);
}
.modal .close:hover {
  background-color: var(--background-close-button-hover);
  transform: scale(1.05);
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.3);
}
.modal p {
  color: var(--errorText-color);
}
</style>
