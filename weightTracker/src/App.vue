<script setup>
import { ref, shallowRef, computed, watch, nextTick} from 'vue'
import chart from 'chart.js/auto'
const weights = ref([])

const weightCharEl = ref(null)

const weightChart = shallowRef(null)

const weightInput = ref (60.0)

const currentWeight = computed( () => {
  return weights.value.sort((a,b) => b.date -a.date)[0] || {weight: 0}
})

const addWeight = () => {
  weights.value.push({
    weight: weightInput.value,
    date: new Date().getTime()
  })
}
watch(weights, newWeights => {
  const ws = [...newWeights]
  if (weightChart.value) {
    weightChart.value.data.labels =  ws
        .sort((a,b)=> a.date -b.date)
        .map(w => new Date(w.date).toLocaleTimeString())
        .slice(-7)
    weightChart.value.data.datasets[0].data =  ws
        .sort((a,b)=> a.date -b.date)
        .map(w => w.weight)
        .slice(-7)
    weightChart.value.update()
    return
  }
  nextTick(()=> {
    weightChart.value = new chart(weightCharEl.value.getContext('2d'),{
      type: 'line',
      data: {
        labels: ws
            .sort((a,b)=> a.date -b.date)
            .map(w => new Date(w.date).toLocaleTimeString()),
        datasets: [
          {
            label: 'Weight',
            data: ws
                .sort((a,b)=>a.date - b.date)
                .map(w => w.weight),
            backgroundColor: 'rgba(255,105,180,0.2)',
            borderColor: 'rgb(255,105,180)',
            borderWidht: 1,
            fill: true
          }
        ]
      },
      options: {
        responsive: true,
        maintainAspectRatio:false
      }
    })
  })
},{deep : true} )
</script>

<template>
<main>
  <h1>Weight Tracker</h1>
  <div class="current">
    <span>{{currentWeight.weight}}</span>
    <small>current weight (kg</small>
  </div>
  <form @submit.prevent="addWeight">
    <input type="number" v-model="weightInput" step="0.1">
    <input type="submit" value="Add weight">
  </form>
  <div v-if="weights && weights.length > 0">
    <h2>Last 7 days </h2>
    <div class="canvas-box">
      <canvas ref="weightCharEl"></canvas>
    </div>
    <div class="weight-history">
      <h2>weight history</h2>
      <ul>
        <li v-for="weight in weights">
          <span>{{ weight.weight}} kg</span>
          <small>{{new Date(weight.date).toLocaleTimeString()}}</small>
        </li>
      </ul>
    </div>
  </div>
</main>
</template>

<style scoped>
*{
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  font-family: 'montserrat',sans-serif;
}
body{
  background-color: #eee;
}
main {
  padding: 1.5rem;
}
h1 {
  font-size: 2rem;
  text-align: center;
  margin-bottom: 2rem;
}
h2 {
  margin-bottom: 1rem;
  color: #888;
  font-weight: 400;
}
.current {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  width: 200px;
  height: 200px;
  text-align: center;
  background: white;
  border-radius: 999px;
  box-shadow: 0 4px 12px rgba(0,0,0,0.1);
  border: 5px solid hwb(330 42% 0%);
  margin: 0 auto 2rem ;
}
.current span {
  display: block;
  font-size: 2rem;
  font-weight: bold;
  overflow: hidden;
  transition: 200ms;
}
form:focus-within,
form:hover{
  border-color: hotpink;
  border-width: 2px;
}
form input[type='number']{
  appearance: none;
  outline: none;
  border: none;
  background-color: white;
  flex: 1 1 0%;
  padding: 1rem 1.5rem;
  font-size: 1.25rem;
}
form input[type='submit'] {
  appearance: none;
  outline: none;
  border: none;
  cursor: pointer;
  background-color: hotpink;

  padding: 0.5rem 1rem;
  color: white;
  font-size: 1.25rem;
  font-weight: 700;
  transition: 200ms linear;
  border-left: 3px solid transparent;
}
form input[type='submit']:hover{
  background-color: white;
  color: pink;
  border-left-color: hotpink;
}
.canvas-box{
  width: 100%;
  max-width: 720px;
  background-color: white;
  padding: 1rem;
  border-radius: 0.5rem;
  box-shadow: -4px 12px rgba(0,0,0,0.1);
  margin-bottom: 2rem;
}
.weight-history ul{
  list-style: none;
  padding: 0;
  margin: 0;
}
.weight-history ul li {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding:0.5rem;
  cursor: pointer;
}
.weight-history ul li:nth-child(even) {
  background-color: #dfdfdf;
}
.weight-history ul li:hover {
  background-color: #f8f8f8;
}
.weight-history ul li:last-of-type {
  border-bottom: none;
}
.weight-history ul li small {
  color: #888;
  font-style: italic;
}
</style>
