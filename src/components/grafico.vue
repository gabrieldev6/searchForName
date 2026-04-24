<script setup lang="ts">
import {computed} from 'vue'

import {
  Chart as ChartJS,
  ArcElement,
  Tooltip,
  Legend
} from 'chart.js'

import { Doughnut} from 'vue-chartjs'

type User = {
  user: string,
  sector: string,
  receive: boolean
}

type Props = {
    list: Array<User>
}

ChartJS.register(
  ArcElement, Tooltip, Legend
)

const props = defineProps<Props>()

let notGet = computed(() => props.list.filter( value => value.receive == false))
let get = computed(() => props.list.filter( value => value.receive == true))
const chartKey = computed(() => props.list.length)



const chartData = computed(()=> (
  {
  labels: ['Recebeu', 'Não Recebeu'],
  datasets: [
    {
      label: 'Entrega de kits',
      data: [get.value.length, notGet.value.length],
      borderColor: '#212228',
      backgroundColor: [
        '#5FF58A',
        '#F5354E'
      ],
      hoverOffset: 2
    }
  ]
}
)) 



// opções
const chartOptions = {
  responsive: true,
  plugins: {
    legend: {
      display: true
    }
  }
}
</script>

<template>
    <div>
        <Doughnut :key="chartKey" :data="chartData" :options="chartOptions" />
        
    </div>
    
</template>