<script setup>
import { ref, computed } from 'vue'

const tarefa = ref([
  { id: 1, titulo: 'Tarefa 1' },
  { id: 2, titulo: 'Tarefa 2' }
])

const a = ref('')
const b = ref('')
const paia = ref(null)
const lixo = ref([])

function add() {
  if (a.value == '') return
  const id = tarefa.value.length > 0 ? Math.max(...tarefa.value.map(i => i.id)) + 1 : 1
  tarefa.value.push({ id, titulo: a.value })
  a.value = ''
}

function remover(id) {
  const pos = tarefa.value.findIndex(i => i.id == id)
  if (pos !== -1) {
    lixo.value.push(tarefa.value[pos])
    tarefa.value.splice(pos, 1)
  }
}

function editar(item) {
  paia.value = paia.value === item.id ? null : item.id
}

function recuperar() {
  if (lixo.value.length > 0) {
    const ultimo = lixo.value.pop()
    tarefa.value.push(ultimo)
  }
}

const filtrados = computed(() => {
  return tarefa.value.filter(t => 
    t.titulo.toLowerCase().indexOf(b.value.toLowerCase()) === 0
  )
})
</script>

<template>
  <div class="a-fundo">
    <div class="b">
      
      <div class="c">
        <input v-model="a" @keyup.enter="add" placeholder="Adicionar Tarefas:">
        <button class="lixo" @click="add">Add</button>
      </div>

      <ul class="paia">
        <li v-for="i in filtrados" :key="i.id">
          <span v-if="paia !== i.id" class="texto">
            {{ i.titulo }}
          </span>
          <input v-else v-model="i.titulo">
          
          <div class="botao">
            <a @click.prevent="editar(i)">{{ paia === i.id ? 'Save' : 'Edit' }}</a>
            <a @click.prevent="remover(i.id)">Del</a>
          </div>
        </li>
      </ul>

      <input v-model="b" class="filtro" placeholder="Filtrar por Nome:">

      <div class="c">
        <button class="lixo" @click="tarefa.sort((x,y) => x.titulo.localeCompare(y.titulo))">Ordenar</button>
        <button class="lixo" @click="recuperar" :disabled="lixo.length === 0">Recuperar</button>
      </div>

    </div>
  </div>
</template>

<style scoped>
.b {
  width: 320px;
}
.c {
  display: flex;
}
input {
  background-color: #fff;
  color: #000;
  flex: 1;
}
.lixo {
  background-color: #fff;
}
.paia {
  list-style: none;
  padding: 0;
  margin: 10px 0;
}
.paia li {
  display: flex;
  justify-content: space-between;
  margin-top: 5px;
}
.texto {
  flex: 1;
}
.botao a {
  color: #00c853;
  margin-left: 10px;
}
.filtro {
  width: 100%;
  margin: 10px 0;
}
</style>
