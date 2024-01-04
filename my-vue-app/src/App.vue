<script setup>
import { ref } from 'vue'

const expense = ref({ name: '', amount: '' })
const expenses = ref([])
const valid = ref(false)
const headers = [
  { text: 'Name', value: 'name' },
  { text: 'Amount', value: 'amount' },
  { text: 'Actions', value: 'action', sortable: false },
]

function addExpense() {
  expenses.value.push({ ...expense.value })
  expense.value = { name: '', amount: '' }
}

function deleteExpense(item) {
  const index = expenses.value.indexOf(item)
  if (index !== -1) expenses.value.splice(index, 1)
}

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

