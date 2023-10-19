
<script setup>

import { ref } from 'vue';

// Retrieve the data from localStorage and assign it to the ref
const storedNotes = JSON.parse(localStorage.getItem('notes'));
    
const showModal = ref(false);
const newNote = ref("");
const notes = ref(storedNotes || []);

function randomColor() {
return "hsl("+ Math.random() * 360 +", 100%, 75% )";
}

const addNote = () => {
  if (!newNote.value) {
    alert('Please type something.')
  } else {
    if (newNote.value.length < 240) {
      notes.value.push(
    {
     id: Math.floor(Math.random() * 10), 
     text: newNote.value,
     date: new Date(),
     backgroundColor: randomColor()
  });
  localStorage.setItem('notes', JSON.stringify(notes.value));
  newNote.value = "";
  showModal.value = false;
    } else {
      alert('Your note should be less than 240 characters.')
    }
    
  }
  
}


</script>

<template>
  <main>
    <div v-if="showModal" class="overlay">
      <div class="modal">
        <textarea v-model="newNote" name="note" id="note" cols="30" rows="10"></textarea>
        <button @click="addNote">Add Note</button>
        <button class="close" @click="showModal = false">Close</button>
      </div>
    </div>

    <div class="container">
     <header>
      <div class="notes-btn">
        <h1>My Notes</h1>
      <button @click="showModal = true">+</button>
      </div>
      

      <div class="cards-container">
        <div v-for="note in notes" :key="note.id" class="card" :style="{backgroundColor: note.backgroundColor}">
        
          <p class="main-text">{{ note.text }}</p>
          <p class="date">{{ note.date }}</p>
        </div>
        
      </div>

     </header>
    </div>
  </main>
</template>

<style scoped>
main {
  height: 100vh;
  width: 100vw;
  background-image: url("src/assets/background.jpg");
  background-attachment: fixed;
}
.container {
  max-width: 1000px;
  padding: 10px;
  margin: 0 auto;
}
.notes-btn {
  display: flex;
  justify-content: space-around;
  align-items: center;
}
header {
  display: flex;
  justify-content: space-between;
  flex-direction: column;

}
h1 {
  font-weight: bold;
  font-size: 65px;
  font-family: Arial, Helvetica, sans-serif;
  color: white;
  background-color: black;
}
header button {
  border: none;
  padding: 10px;
  width: 50px;
  height: 50px;
  cursor: pointer;
  background-color: rgb(21,20,20);
  border-radius: 100%;
  color: white;
  font-size: 30px;
}
.card {
  height: 225px;
  width: 225px;
  background-color: rgb(237, 182, 44);
  padding: 10px;
  border-radius: 15px;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  margin-right: 20px;
  margin-left: 20px;
  margin-bottom: 20px;
  font-family: Arial, Helvetica, sans-serif;
}
.date {
  font-size: 12px;
  font-weight: bold;
}
.cards-container {
  display: flex;
  flex-wrap: wrap;
}
.overlay {
  position: absolute;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.674);
  display: flex;
  align-items: center;
  justify-content: center;
}
.modal {
  width: 750px;
  background-color:black;
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
  border: none;
  background-color: darkcyan;
  color:white;
  cursor: pointer;
  margin-top: 15px;
}
.modal .close {
  background-color: crimson;

}
</style>