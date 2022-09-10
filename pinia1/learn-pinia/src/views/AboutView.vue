<template>
  <div class="about">
    <h1>This is an about page</h1>
    <button class="counter-button" @click="storeCounter.increaseCount">
      {{storeCounter.count}}
    </button>
  </div>
</template>
<script setup>
import {useCounterStore} from '@/stores/counter.js'

const storeCounter = useCounterStore()

</script>
<style>
.counter-button {
  font-size: 50px;
}
</style>
