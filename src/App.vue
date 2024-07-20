<script setup>
import { ref } from 'vue'
const showForm = ref(false)
const newMemo = ref('')
const memos = ref([])
const errorMessage = ref('')

function getRandomColor() {
  return `#${Math.floor(Math.random() * 16777215).toString(16)}`
}

function addmemo() {
  if (!newMemo.value) {
    errorMessage.value = 'Please enter a memo'
    return
  }
  errorMessage.value = ''
  memos.value.push({
    id: Date.now(),
    memo: newMemo.value,
    date: new Date().toLocaleDateString("en-GB"),
    backgroundColor: getRandomColor()
  })
  newMemo.value = ''
  showForm.value = false
}

function deleteMemo(id) {
  memos.value = memos.value.filter((memo) => memo.id !== id)
}
</script>

<template>
  <main>
    <div class="container">
      <header>
        <h1>Memo</h1>
        <button @click="showForm = true">+</button>
      </header>
      <div class="card-container">
        <div :key="memo.id" :style='{ backgroundColor: memo.backgroundColor }' v-for="memo in memos" class="card">
          <p class="card-content">{{ memo.memo }}</p>
          <div class="card-footer">
            <p class="card-date">{{ memo.date }}</p>
            <button @click="deleteMemo(memo.id)" class="card-button">X</button>
          </div>
        </div>
      </div>
    </div>
    <div v-if="showForm" class="form-overlay">
      <div class="form-modal">
        <button @click="showForm = false" class="form-close-button">&times;</button>
        <textarea v-model="newMemo" name="memo" id="memo" cols="30" rows="10"></textarea>
        <button @click="addmemo" class="form-save-button">Save</button>
        <p class="error">{{ errorMessage }}</p>
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
  max-width: 900px;
  padding: 10px;
  margin: 0 auto;
}

header {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

header h1 {
  font-size: 48px;
  font-weight: bold;
  margin-bottom: 25px;
  color: #495a7d;
}

header button {
  border: none;
  padding: 10px;
  width: 50px;
  height: 50px;
  cursor: pointer;
  border-radius: 100%;
  color: white;
  background-color: #495a7d;
}

.card-container {
  display: flex;
  flex-wrap: wrap;
  gap: 20px;
}

.card {
  width: 233px;
  height: 225px;
  padding: 10px;
  margin-bottom: 20px;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
}

.card-footer {
  display: flex;
  justify-content: space-between;
  align-items: center
}

.card-button {
  width: 2rem;
  border: none;
  cursor: pointer;
  height: 2rem;
  color: white;
  background-color: red;
  padding: 3px;
  border-radius: 100%;
}

.form-overlay {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, .77);
  z-index: 10;
  display: flex;
  align-items: center;
  justify-content: center;
}

.form-modal {
  width: 420px;
  background-color: white;
  border-radius: 10px;
  padding: 30px;
  position: relative;
  display: flex;
  flex-direction: column;
}

.form-close-button {
  position: absolute;
  top: 5px;
  right: 10px;
  width: 20px;
  height: 30px;
  background-color: transparent;
  border: none;
  font-size: 25px;
  cursor: pointer;
}

.form-save-button {
  padding: 10px 20px;
  font-size: 20px;
  width: 100%;
  background-color: #495a7d;
  border: none;
  cursor: pointer;
  border-radius: 5px;
  margin-top: 12px;
  color: white;
}

.error {
  color: red;
  display: flex;
  justify-content: center;
}
</style>