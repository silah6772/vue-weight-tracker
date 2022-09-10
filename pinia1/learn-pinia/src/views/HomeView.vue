<template>
  <div class="home">
    <h1>This is an home page</h1>
    <div class="count">
      {{storeCounter.count}}
    </div>
    <div class="buttons">
      <button @click="storeCounter.increaseCount">+</button>
      <button @click="storeCounter.decreaseCount">-</button>
    </div>
    <hr>
    <div>
      <p>Number is {{storeCounter.oddOrEven}}</p>
    </div>
    <hr>
    <div>
      <h3>Edit Counter:</h3>
      <input type="number" v-model="storeCounter.count">
    </div>
  </div>
</template>
<script setup>
import { useCounterStore } from '@/stores/counter'
const storeCounter = useCounterStore()

</script>
<style scoped>
.count{
  font-size: 60px;
  margin: 20px;
}
.buttons button{
  margin: 10px;
  font-size: 40px;
}
</style>
