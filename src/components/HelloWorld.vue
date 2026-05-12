<script setup lang="ts">
import { ref, computed } from 'vue';
import Papa from 'papaparse'

import Grafico from './grafico.vue';
import graficoRecurso from './graficoRecurso.vue';
import { Check, User, X, FileClock, Search } from '@lucide/vue';
import Modal from './modal.vue';

type User = {
  index: number
  user: string
  sector: string
  receive: boolean
}

let inputName = ref<string>('')
let fileContent = ref([]);
let parseList = ref<Array<User>>([]);
let auxNegative = ref(1)
const showOverlay = ref(false)

const filteredUsers = computed(() => {
  const query = inputName.value.toUpperCase()

  if (!query) return parseList.value

  return parseList.value.filter(user =>
    user.user.toUpperCase().includes(query)
  )

})

const removeDuplicados = (data: any[]) => {
  const map = new Map()

  data.forEach(item => {
    map.set(item["Usuário"], item)
    // se repetir, sobrescreve → mantém o último
  })

  return Array.from(map.values())
}

const handleFileUpload = (event: any) => {
  const file = event.target.files[0]; // Pega o primeiro arquivo selecionado

  if (!file) return;



  // Define o que acontece quando o arquivo é lido
  Papa.parse(file, {
    delimiter: ";", // IMPORTANTE
    header: true,   // usa a primeira linha como chave
    skipEmptyLines: true,
    complete: (results: any) => {
      fileContent.value = results.data


      let notDouble = removeDuplicados(fileContent.value)
      parseList.value = notDouble.map((item, index): User => ({
        index: index,
        user: item["Usuário"],
        sector: item["Perfil"],
        receive: false
      }))

    },
    error: (err) => {
      console.error("Erro ao parsear CSV", err)
    }
  })

};
const markAsReceived = (index: number) => {
  parseList.value[index].receive = true
  auxNegative.value++
}

</script>

<template>
  <div class="flex content-between ">

    <ul class="mx-10 pt-15">
      <li class="pb-5">
        <h2>Sistema de entrega de kits</h2>
      </li>
      <li class="flex justify-center">
        <input v-model="inputName" class="w-90 p-3 border text-gray-700 dark:text-gray-300 border-gray-300 dark:border-gray-700 rounded-l-md"
          placeholder="Digite o nome do colaborador" />
          <button class="dark:bg-gray-950 bg-gray-400 text-white p-3 border border-gray-400 dark:border-gray-700 rounded-r-md"
          ><Search/>
        </button>
      </li>

      <li class="mx-13 mt-5 h-60 w-110 overflow-y-auto">
        <div v-if="inputName" v-for="value in filteredUsers" class="w-full flex hover:bg-gray-300 dark:hover:bg-gray-800 p-1.5">
          
          <p class="w-100 flex text-left">{{ value.user }}</p>
          <p><input class="w-4 h-4" type="checkbox" v-model="parseList[value.index].receive" @change="markAsReceived(value.index)" name=""></p>
        </div>

      </li>
      <!-- lista de colaboradores -->
      <li class="mt-15">
        <div v-if="parseList.length != 0">
          <h2>Lista completa de Colaboradores</h2>
          <ul class="flex border pt-2 border-gray-300 dark:border-gray-700 rounded-t-md">
            <li class="w-80 pl-3 py-2 text-xs text-left">
              <p class="font-bold text-gray-400 dark:text-white">Nome</p>
            </li>
            <li class="w-40 py-2 text-xs text-left">
              <p class="font-bold text-gray-400 dark:text-white">Setor</p>
            </li>
            <li class="w-20 py-2 text-xs">
              <p class="font-bold text-gray-400 dark:text-white">Recebido</p>
            </li>
          </ul>
          <div class="h-90 overflow-y-auto">
            <ul class="" v-for="value in parseList">

              <li class="flex border-t text-gray-400 dark:border-gray-700 dark:bg-gray-800 py-3 text-xs">
                <p class="w-80 pl-3 text-left">{{ value.user }}</p>
                <p class="w-40 text-left">{{ value.sector }}</p>
                <div class="w-20 flex justify-center">
                  <Check class="text-green-400" v-if="value.receive" />
                  <X class="text-red-400" v-if="!value.receive" />
                </div>


              </li>

            </ul>
          </div>

        </div>
        <div v-else class="flex justify-center text-center">
          <ul class="">
            <li class="hover:bg-gray-200 hover:text-gray-800 text-gray-600 dark:hover:text-gray-500 dark:hover:bg-gray-800  px-5 py-15  rounded-md border border-gray-300 dark:border-gray-700">
              <label for="file-upload">
                <div class="flex justify-center">
                  <FileClock class="w-30 h-30" />
                </div>
                <p>adicione um arquivo .CSV para começar</p>
              </label>

              <input id="file-upload"
                class="hidden" type="file"
                accept=".csv" @change="handleFileUpload">
              </input>

            </li>
            <li class="flex justify-center"></li>
            <li class="pt-5"></li>
          </ul>


        </div>
      </li>
    </ul>


    <ul class="">
      <li class="mt-15">
        <h2>Estatísticas</h2>
      </li>

      <li>
        <Grafico :list="parseList" />
      </li>

      <li>
        <h2 class="pt-5">Gestão de kit</h2>
      </li>

      <li>
        <graficoRecurso :decrease-trigger="auxNegative"/>
      </li>

      <li>
        <button @click="showOverlay = true" class="w-full bg-gray-400 dark:bg-gray-950 text-white hover:bg-gray-500 dark:hover:bg-gray-800 hover:cursor-pointer mt-5 p-1 px-2 border border-gray-300 dark:border-gray-700 rounded-md">Instruções de uso</button>
      </li>
      
    </ul>
    <div v-if="showOverlay" class="w-full h-full">
     <Modal/> 
    </div>

  </div>
</template>
