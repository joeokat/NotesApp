<script setup>
import { ref } from 'vue';

const showModal = ref(false)
const newNote = ref('')
const notes = ref([])
const errorMsg = ref('')

function randomColors() {
  return "hsl(" + Math.random() * 360 + ", 100%, 75%)";
}

const addNote = () => {
  if(newNote.value.length < 10) {
    return errorMsg.value = 'Note needs to be more than 10 characters'
  }
  notes.value.push({
    id: Math.floor(Math.random() * 1000000),
    text: newNote.value,
    date: new Date(),
    backgroundColor: randomColors()
  })
  showModal.value = false
  newNote.value = ''
  errorMsg.value = ''
}


</script>


<template>
  <main>
    <div v-show="showModal" class="overlay">
      <div class="modal">
        <button class="close" @click="showModal = false">x</button>
        <textarea v-model="newNote" id="note" name="note" cols="30" rows="10"></textarea>
        <p> {{ errorMsg }} </p>
        <button @click="addNote">Add Note</button>
      </div>
    </div>

    <div class="container">

      <header>
        <h1>Notes</h1>
        <button @click="showModal = true">+</button>
      </header>

      <div class="cards-container">
        <div v-for="note in notes"
        class="card" 
        :key="note.id"
        :style="{backgroundColor: note.backgroundColor}">
          <p class="main-text">{{ note.text }}</p>
          <p class="date">{{ note.date.toLocaleDateString('en-US') }}</p>
        </div>
      </div>

    </div>
  </main>
</template>


<style scoped>
main {
  height: 100vh;
  width: 100vw;
}

.container {
max-width: 1000px;
padding: 10px;
margin: 0 auto;
}

header {
display: flex;
justify-content: space-between;
align-items: center;
}

h1 {
  font-weight: bold;
  margin-bottom: 25px;
  font-size: 75px;
  color: #fafafa;
}

header button {
  border: none;
  padding: 10px;
  height: 50px;
  width: 50px;
  cursor: pointer;
  background-color: #fafafa;
  border-radius: 100%;
  color: black;
  font-size: 20px;
}

.card {
  width: 225px;
  height: 225px;
  background-color: #ffbb1c;
  padding: 10px;
  border-radius: 15px;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  margin-right: 20px;
  margin-bottom: 20px;
}

.main-text {
  color: #101010;
  font-weight: 500;
  font-size: 16;
}

.date {
  font-size: 12;
  font-weight: bold;
  color: #101010
}

.cards-container {
  display: flex;
  flex-wrap: wrap;
}

.overlay {
  position: absolute;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.77);
  z-index: 10;
  display: flex;
  align-items: center;
  justify-content: center;
}

.modal {
  width: 500px;
  background-color: #fafafa;
  border-radius: 10px;
  padding: 30px;
  position: relative;
  display: flex;
  flex-direction: column;
}

.modal button {
  padding: 10px 20px;
  font-size: 20px;
  width: 100%;
  color: #fafafa;
  background-color: #101010;
  border: none;
  cursor: pointer;
  margin-top: 16px;
}

.modal .close {
  background-color: red;
  margin-bottom: 16px;
  border-radius: 50px;
  width: 50px;
  height: 50px;
  
}

.modal p {
  font-weight: 500;
  color: red;
}
</style>