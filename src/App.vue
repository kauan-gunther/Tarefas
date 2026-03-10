<script setup>

import { ref } from 'vue'
const tarefas = ref(['Avaliação de Física', 'Seminário de Educação Física'])
const novatarefa = ref('')
const aviso = ref(false)
const alterar = ref(-1)

function addtarefa() {
  if (novatarefa.value.trim().length < 5) {
    aviso.value = true;
  } else {
    if (alterar.value == -1) {
      tarefas.value.push(novatarefa.value)
      novatarefa.value = ''
    }
    else {
      tarefas.value.splice(alterar.value, 1, novatarefa.value);
      novatarefa.value =
        alterar.value = -1
    }
  }
}
function deleteTarefa(talvez) {
  const nada = tarefas.value.indexOf(talvez);
  tarefas.value.splice(nada, 1)
}
function editarTarefa(caso) {
  alterar.value = tarefas.value.indexOf(caso)
  novatarefa.value = caso;
}
</script>

<template>
  <div class="container">
    <h1>Lista de Tarefas</h1>
    <input type="text" v-model="novatarefa" placeholder="Adicione uma tarefa" @keyup.enter="addtarefa"
      @input="novatarefa.length < 5 ? aviso = true : aviso = false">
    <button @click="addtarefa">Add</button>
    <div v-show="aviso" class="aviso">Digite ao menos 5 caracteres</div>
    <ul>
      <li v-for="tarefa in tarefas" :key="tarefa">
        {{ tarefa }}
        <span>
          <a href="#" @click.prevent="deleteTarefa(tarefa)">Delete</a>
          <a href="#" @click.prevent="editarTarefa(tarefa)">Editar</a>
        </span>
      </li>

    </ul>
    <button @click="tarefas.sort((a, b) => a.localeCompare(b, 'pt-BR'))">Ordenar</button>
  </div>
</template>

<style scoped>
.aviso {
  background-color: red;
  color: white;
}
</style>