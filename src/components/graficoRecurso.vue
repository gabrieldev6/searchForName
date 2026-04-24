<script setup lang="ts">
import {
  Chart as ChartJS,
  Title,
  Tooltip,
  Legend,
  BarElement,
  CategoryScale,
  LinearScale
} from 'chart.js'
import { ref, computed, watch } from 'vue'

import { Bar } from 'vue-chartjs'

// registrar o necessário pro gráfico de barra
ChartJS.register(
  Title,
  Tooltip,
  Legend,
  BarElement,
  CategoryScale,
  LinearScale
)

type Props = {
  decreaseTrigger: number
}

const props = defineProps<Props>()


let rec = ref<string>('')
let qtd = ref<number>()

let listLabels = ref<Array<string>>([])
let listData = ref<Array<number>>([])



watch(() => props.decreaseTrigger, () => {
  listData.value = listData.value.map(v => v - 1)
})
// dados
const chartData = computed(() => ({
  labels: [...listLabels.value],
  datasets: [
    {
      label: 'Produto',
      data: [...listData.value],
      backgroundColor: '#3bfff6'
    }
  ]
}))

const chartKey = computed(() => listLabels.value.length)

const event = () => {
  if (!rec.value || !qtd.value) return

  listLabels.value.push(rec.value)
  listData.value.push(qtd.value)

  rec.value = ''
  qtd.value = 0
}

const chartOptions = {
  responsive: true,
  plugins: {
    legend: {
      display: true
    },
    title: {
      display: true,
      text: 'Insumo de Kit'
    }
  }
}


</script>

<template>
  <div>

    <!-- <p>Adicione partes do kit para analisar quando estiver perto de acabar</p> -->
    <ul class="flex">
      <li><input v-model="rec" class="w-50 p-1 mr-0.5 border border-gray-700 rounded-md"
          placeholder="Ex.: ovo, pão, peixe... " /></li>
      <li><input v-model.number="qtd" class="w-15 p-1 mr-0.5 border border-gray-700 rounded-md" type="number" min="1"
          placeholder="Qtd." /></li>
      <li><button @click="event"
          class="bg-gray-950 hover:bg-gray-800 hover:cursor-pointer p-1 px-2 border border-gray-700 rounded-md">Adicionar</button>
      </li>
    </ul>
    <Bar :key="chartKey" :data="chartData" :options="chartOptions" />
  </div>
</template>