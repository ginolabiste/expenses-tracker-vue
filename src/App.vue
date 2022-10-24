<script setup>

import { ref, reactive, computed } from 'vue';

const incomes = ref([]);
const input_desc = ref('')
const input_value = ref('')
const input_date = ref('')

const FormHandler = () => {
  if (input_desc.value === '' || input_value.value === '' || input_date.value === '') {
    return
  }

  incomes.value.push({
    desc: input_desc.value,
    value: input_value.value,
    date: input_date.value
  })

  input_desc.value = ''
  input_value.value = ''
  input_date.value = ''
}

const totalIncome = computed(() => incomes.value.reduce((n, { value }) => n + value, 0))

const incomeTotal_asc = computed(() => incomes.value.sort((a, b) => {
    return b.date - a.date
}))

const removeIncome = income => {
    incomes.value = incomes.value.filter(t => t !== income)
}

</script>

<template>
  <main class="app">
    <div class="header">
      <h1>Expenses Tracker</h1>
      <div class="total-income">₱ {{ totalIncome }}</div>
    </div>

    <div class="form">
        <form @submit.prevent="FormHandler">
            <input type="text" placeholder="Income Description..." v-model="input_desc" />
            <input type="number" placeholder="Income Value..." v-model="input_value" />
            <input type="date" placeholder="Income Date..." v-model="input_date" />
            <input type="submit" value="SUBMIT" />
        </form>
    </div>

    <div class="income">
      <div class="income-list">
        <div >
          <div v-for="income in incomeTotal_asc" class="income-item">
            <div class="removeItem" @click="removeIncome(income)">x</div>
            <div class="desc">
              {{ income.desc }}
            </div>
            <div class="price">
              ₱ {{ income.value }}
            </div>
            <div class="date">
              {{ income.date }}
            </div>
          </div>
        </div>
      </div>
    </div>
  </main>
  
</template>

<style scoped>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  font-family: 'Fira Sans', sans-serif;
}
body {
  background-color: rgb(141, 13, 13);
}
div.header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 15px 30px;
  background-color: #313131;
  border-bottom: 5px solid #FFCE00;
}
div.header h1 {
  color: #EEE;
  font-size: 28px;
}
div.header .total-income {
  font-family: 'Fira Code', 'Fira Sans', sans-serif;
  background-color: #FFCE00;
  color: #FFF;
  font-size: 20px;
  font-weight: 900;
  padding: 5px 10px;
  min-width: 100px;
  text-align: center;
  border-radius: 8px;
  box-shadow: inset 0px 0px 6px rgba(0, 0, 0, 0.25);
  text-shadow: 0px 3px 3px rgba(0, 0, 0, 0.25);
}
form {
  display: flex;
  justify-content: center;
  margin-top: 30px;
}
form input {
  color: #888;
  border: none;
  outline: none;
  font-size: 20px;
}
form input::placeholder {
  color: #AAA;
}
form input:not([type="submit"]) {
  display: block;
  background: #eee;
  border: none;
  outline: none;
  padding: 5px 15px;
}
form input[type="submit"] {
  display: block;
  background: none;
  border: none;
  outline: none;
  color: #eee;
  font-weight: 500;
  text-shadow: 0px 1px 3px rgba(0, 0, 0, 0.2);
  padding: 5px 15px;
  background-color: #FFCE00;
  cursor: pointer;
}
form input:first-of-type {
  border-radius: 8px 0px 0px 8px;
}
form input:last-of-type {
  border-radius: 0px 8px 8px 0px;
}
.income-list {
  margin-top: 20px;
  padding: 15px;
}
.income-item {
  position: relative;
  display: flex;
  padding: 15px 15px 15px 0px;
  background-color: #eee;
  border-radius: 8px;
  max-width: 600px;
  margin: 0 auto 30px;
}
.removeItem {
  color: #EF2D2D;
  font-weight: 600;
  font-size: 20px;
  line-height: 1;
  text-align: center;
  margin: 0 15px;
}
.desc {
  color: #666;
  flex: 1 1 50%;
  font-size: 20px;
}
.price {
  color: #666;
  min-width: 100px;
  font-size: 20px;
}
.date {
  color: #666;
  text-align: right;
  font-size: 20px;
}
</style>
