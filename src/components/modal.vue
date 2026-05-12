<script setup lang="ts">
import { X } from '@lucide/vue';
import img1 from '../assets/image1.png'
import img2 from '../assets/image2.png'
import img3 from '../assets/image3.png'
import img4 from '../assets/image4.png'
import img5 from '../assets/image5.png'
</script>
<template>
    <div
        class="w-full h-full backdrop-blur text-left bg-white/10 absolute z-10 inset-0 flex justify-center items-center">
        <div class="w-150 h-100 bg-white text-black rounded p-7 overflow-y-auto">

            <div class="w-full justify-between flex pb-5">
                <h2 class="text-black">Modo de usar</h2>
                <X class="w-7 h-7"/>
            </div>
            <hr class="text-gray-500 h-2">
            <div class="">
                <p>Olá, seja bem vindo ao sistema de entrega de Kits. Este é guia que permite
                    você entender como utilizar essa ferramenta bastante util para agilizar,
                    controlar e analisar a entrega de kit's em datas comemorativas como, Natal,
                    Pascoa e São João... entre outras datas.</p>

                <p>Para que possamos iniciar o guia, baixe o arquivo .csv de exemplo neste
                    <a href="">link</a>, nele contém uma lista de Colaboradores para que o sistema
                    armazene em cache os dados da lista. Quer detalhes de como criar sua propria lista?
                    acesse este <a href="">link</a> para ler o PDF explicando melhor como você pode está
                    fazendo sua propria lista.
                </p>

                <p>Ao baixar o arquivo .csv, click em: </p>

                <div class="w-full p-3 flex justify-center">
                    <img class="rounded w-50" :src="img1" alt="">
                </div>

                <p>Procure o arquivo que você baixou, ao carrega-lo, uma lista com Colaboradores aparecerá.</p>
                <div class="w-full p-3 flex justify-center">
                    <img class="rounded w-70" :src="img4" alt="">
                </div>
                <p> Utilize a barra de pesquisa para buscar o Colaborador desejado</p>
                <div class="w-full p-3 flex justify-center">
                    <img class="rounded w-70" :src="img2" alt="">
                </div>
                <p>Marque com um check o colaborador que recebeu o kit e pronto, simples assim!</p>
                <div class="w-full p-3 flex justify-center">
                    <img class="rounded w-70" :src="img3" alt="">
                </div>
                <p>
                    A medida que os kit's vão sendo entregues, o grafíco a direita apresenta a quantidade 
                    de pessoas q faltam receber e as pessoas que ja receberam, assim você poderá saber quantas pessoas
                    receber.
                </p>
                
                <div class="w-full p-3 flex justify-center">
                    <img class="rounded w-70" :src="img5" alt="">
                </div>
            </div>

        </div>
    </div>
</template>