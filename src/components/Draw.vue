<template>
  <h1>What to eat today?</h1>
  <h1>{{ lunch }}</h1>
  <button @click="getFood">Eat lahh</button>

  <select v-model="type" @change="typeHandlerChange">
    <option value="all">All</option>
    <option value="food">Food</option>
    <option value="beverage">Baverage</option>
  </select>

  <div v-for="item in filteredDataCollection" :key="item">
    <p>{{ item.name }}</p>
    <p>{{ item.type }}</p>
    <p>{{ item.floor }}</p>
    <p>{{ item.vege }}</p>
    <p>{{ item.food }}</p>
    <p>{{ item.note }}</p>
    <p>{{ item.frequency }}</p>
    <hr/>
  </div>

</template>

<script setup>
import { onMounted, ref } from 'vue'

const lunch = ref('KFC')
const excel_id = "1swPh8Y90SAw1ToYEdNQv-Kg0cquMYyw8pe7iTFZ4cy4"
const sheet = "Sheet1"
const API_key = "AIzaSyANGhSZfpWd5EyFueJpWbOFaJcQ5T_YhTA"
const dataCollection = ref()
const filteredDataCollection = ref([])
const type = ref('food')

async function getData() {
  const res = await fetch(`https://sheets.googleapis.com/v4/spreadsheets/${excel_id}/values/${sheet}?alt=json&key=${API_key}`)

  const data = await res.json()
  let collection = data.values.slice(1, data.values.length)
  dataCollection.value = collection.map((item) => {
    return {
      name: item[0],
      cuisine : item[1],
      floor : item[2],	
      vege : item[3] === "1" ? "Vege" : "",	
      type : item[4] === "1" ? "Food" : "Beverage",
      note : item[5],	
      frequency : item[6] === "0" ? "favourite" : "",
    }
  })

  filteredDataCollection.value = dataCollection.value.filter((item) => item.type === "Food")
}

function getFood() {
  console.log('getFood')

  const total = dataCollection.value.length
  const randomNum = Math.floor(Math.random() * total)
  lunch.value = dataCollection.value[randomNum].name
}

function typeHandlerChange() {
  switch(type.value) {
    case "all":
      filteredDataCollection.value = dataCollection.value
      break
      
    case "food":
      filteredDataCollection.value = dataCollection.value.filter((item) => item.type === "Food")
      console.log(filteredDataCollection.value)
      break

    case "beverage":
      filteredDataCollection.value = dataCollection.value.filter((item) => item.type === "Beverage")
      console.log(filteredDataCollection.value)
      break
  }
}

onMounted(()=> {
  getData()
})

</script>