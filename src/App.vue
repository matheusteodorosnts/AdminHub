<script setup>
import { User, Shield, ArrowLeft, CircleX, CircleCheck, Trash2, UndoDot } from 'lucide-vue-next'
import anime from 'animejs/lib/anime.es.js';
import { ref } from 'vue'

// Variables
const nameUser = ref('')
const emailUser = ref('')
const passwordUser = ref('')
const idUser = ref('')

// Correção: inicializar como arrays vazios
const nameList = ref([])
const emailList = ref([])
const passwordList = ref([])
const idList = ref([])

// Screens
const showScreenUser = ref(false)
const showScreenAdmin = ref(false)
const showScreenSelectOption = ref(true)

const toggleScreenUser = () => {
  showScreenUser.value = !showScreenUser.value
  showScreenSelectOption.value = !showScreenSelectOption.value
}

const toggleScreenAdmin = () => {
  showScreenAdmin.value = !showScreenAdmin.value
  showScreenSelectOption.value = !showScreenSelectOption.value
}

const toggleScreenSelectOption = () => {
  showScreenSelectOption.value = !showScreenSelectOption.value
  showScreenUser.value = false
  showScreenAdmin.value = false
}

const pushUserList = () => {
  if (nameUser.value.trim() && passwordUser.value.trim() && emailUser.value.includes('@')) {
    nameList.value.push(nameUser.value)
    emailList.value.push(emailUser.value)
    passwordList.value.push(passwordUser.value)

    nameUser.value = ''
    emailUser.value = ''
    passwordUser.value = ''

    // Div of Valid Informations (AnimeJS)
    anime({
      targets: '#validDiv',
      translateX: -250
    });
    
    setTimeout(() => {
      anime({
        targets: '#validDiv',
        translateX: 250
      });
    }, 3000);
  } else {
    // Div of Invalid Informations (AnimeJS)
    anime({
      targets: '#invalidDiv',
      translateX: -250
    });
    
    setTimeout(() => {
      anime({
        targets: '#invalidDiv',
        translateX: 250
      });
    }, 3000);
  }
}

const removeUserList = (index) => {
  nameList.value.splice(index, 1)
  emailList.value.splice(index, 1)
  passwordList.value.splice(index, 1)
}
</script>

<template>
  <div class="flex flex-row justify-center items-center min-h-screen">
    <header class="absolute top-4 left-4">
      <div v-if="showScreenUser" id="invalidDiv" class="bg-[#FC515B] text-[#562731] absolute left-[-250px] bottom-150 flex flex-row rounded p-2 w-50 gap-2"><CircleX />Error!</div>
      <div v-if="showScreenUser" id="validDiv" class="bg-[#51FC5B] text-[#275631] absolute left-[-250px] bottom-150 flex flex-row rounded p-2 w-50 gap-2"><CircleCheck />Done!</div>
      <button @click="toggleScreenSelectOption" v-if="!showScreenSelectOption" class="text-[#FC515B] cursor-pointer transition-colors p-2 rounded-full">
        <ArrowLeft size="24" />
      </button>
    </header>
    <main class="animate-fade-up">
      <!-- Screen of Select Option -->
      <div class="gap-2 flex flex-row max-sm:flex-col" v-if="showScreenSelectOption">
        <button @click="toggleScreenUser" class="hover:bg-[#562731] hover:scale-106 rounded cursor-pointer p-2 text-[#562731] border-4 border-[#562731] duration-500 transition-all hover:text-white w-40 h-40 flex justify-center items-center"><User class="w-20 h-20"/></button>
        <button @click="toggleScreenAdmin" class="hover:bg-[#562731] hover:scale-106 rounded cursor-pointer p-2 text-[#562731] border-4 border-[#562731] duration-500 transition-all hover:text-white w-40 h-40 flex justify-center items-center"><Shield class="w-20 h-20"/></button>
      </div>
      <!-- Screen of User -->
      <div v-if="showScreenUser">
        <div class="bg-[#562731] w-80 h-80 rounded flex flex-col items-center justify-center">
          <form class="flex flex-col items-center text-[#FC515B]" @submit.prevent="pushUserList">
            <label>Name</label>
            <input v-model="nameUser" type="text" class="focus:outline-none border-2 border-[#FC515B] rounded">
            <label>E-mail</label>
            <input v-model="emailUser" type="email" class="focus:outline-none border-2 border-[#FC515B] rounded">
            <label>Password</label>
            <input v-model="passwordUser" type="password" class="focus:outline-none border-2 border-[#FC515B] rounded">
            <button @click="pushUserList" class="bg-[#FC515B] text-white rounded p-2 w-40 hover:scale-107 transition-all relative top-5 duration-500 shadow-2xl hover:shadow-[#FC515B] cursor-pointer">Send</button>
          </form>
        </div>
      </div>
      <!-- Screen of Admin -->
      <div v-if="showScreenAdmin" class="bg-[#562731] w-140 h-80 rounded text-white max-sm:w-100 overflow-auto p-4">
        <h2 class="text-center mb-4 text-[#FC515B]">Lista de Usuários</h2>
        <div v-if="nameList.length === 0" class="text-center text-gray-300">
          None User
        </div>
        <div v-else>
          <div v-for="(name, index) in nameList" :key="index" class="flex justify-between items-center mb-2 border-b border-[#FC515B] pb-2">
            <div class="flex-1">
              <div><span class="text-[#FC515B]">Nome:</span> {{ name }}</div>
              <div><span class="text-[#FC515B]">Email:</span> {{ emailList[index] }}</div>
              <div><span class="text-[#FC515B]">Senha:</span> {{ passwordList[index] }}</div>
            </div>
            <button @click="removeUserList(index)" class="text-[#FC515B] cursor-pointer hover:text-white">
              <Trash2 />
            </button>
          </div>
        </div>
      </div>
    </main>
  </div>
</template>

<style scoped></style>