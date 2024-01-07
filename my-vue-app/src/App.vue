<script setup>
import { ref, onMounted } from 'vue'

const baseUrl = 'https://ominous-computing-machine-pjj6jjrvq9vvfqqg-8080.app.github.dev'
const expense = ref({ name: '', amount: '' })
const expenses = ref([])
const valid = ref(false)
const headers = [
  { text: 'Name', value: 'name' },
  { text: 'Amount', value: 'amount' },
  { text: 'Actions', value: 'action', sortable: false },
]

function addExpense() {
  fetch(baseUrl + '/api/expenses', {
    method: 'POST',
    headers: {
      'Content-Type': 'application/json',
    },
    body: JSON.stringify(expense.value),
  })
    .then(response => response.json())
    .then(data => {
      expenses.value.push(data)
      expense.value = { name: '', amount: '' }
    })
    .catch(error => {
      console.error('Error adding expense:', error)
    })
}

function deleteExpense(item) {
  fetch(baseUrl + `/api/expenses/${item.id}`, {
    method: 'DELETE',
  })
    .then(() => {
      const index = expenses.value.indexOf(item)
      if (index !== -1) expenses.value.splice(index, 1)
    })
    .catch(error => {
      console.error('Error deleting expense:', error)
    })
}

onMounted(() => {
  fetch(baseUrl + '/api/expenses')
    .then(response => response.json())
    .then(data => {
      expenses.value = data
    })
    .catch(error => {
      console.error('Error fetching expenses:', error)
    })
})
</script>

<template>
  <v-app>
    <v-app-bar app color="primary">
      <v-toolbar-title class="white--text">Expense Tracker</v-toolbar-title>
    </v-app-bar>

    <v-main>
      <v-container>
        <v-container>
          <v-form ref="form" v-model="valid" @submit.prevent="addExpense">
            <v-text-field v-model="expense.name" :rules="[v => !!v || 'Name is required']" label="Name"></v-text-field>
            <v-text-field v-model="expense.amount" :rules="[v => !!v || 'Amount is required']" label="Amount"></v-text-field>
            <v-btn :disabled="!valid" type="submit" color="primary">Add Expense</v-btn>
          </v-form>
        </v-container>

        <v-data-table :headers="headers" :items="expenses" class="elevation-1">
          <template v-slot:item.action="{ item }">
            <v-btn @click="deleteExpense(item)" color="error">Delete</v-btn>
          </template>
        </v-data-table>
      </v-container>
    </v-main>

    <v-footer app color="primary" dark>
      <span class="white--text">&copy; 2023</span>
    </v-footer>
  </v-app>
</template>