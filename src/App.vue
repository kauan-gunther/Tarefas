<script setup>
import { ref, computed } from 'vue'

const tarefa = ref([
  { id: 1, titulo: 'Tarefa 1', concluida: false },
  { id: 2, titulo: 'Tarefa 2', concluida: false },
  { id: 3, titulo: 'Tarefa 3', concluida: true }
])

const a = ref('')
const b = ref('')
const paia = ref(null)
const lixo = ref(null)
const f = ref('todas')

function add() {
  if (a.value.trim().length < 5) return
  const id = tarefa.value.length > 0 ? Math.max(...tarefa.value.map(i => i.id)) + 1 : 1
  tarefa.value.push({ id, titulo: a.value, concluida: false })
  a.value = ''
}

function remover(id) {
  const pos = tarefa.value.findIndex(i => i.id == id)
  if (pos !== -1) {
    lixo.value = tarefa.value[pos]
    tarefa.value.splice(pos, 1)
  }
}

function editar(item) {
  paia.value = paia.value === item.id ? null : item.id
}

function recuperar() {
  if (lixo.value) {
    tarefa.value.push(lixo.value)
    lixo.value = null
  }
}

const filtrados = computed(() => {
  let lista = tarefa.value.filter(t => t.titulo.toLowerCase().includes(b.value.toLowerCase()))
  if (f.value === 'pendentes') return lista.filter(t => !t.concluida)
  if (f.value === 'concluidas') return lista.filter(t => t.concluida)
  return lista
})

const p = computed(() => tarefa.value.filter(t => !t.concluida).length)
const c = computed(() => tarefa.value.filter(t => t.concluida).length)
</script>

<template>
  <div class="a-fundo">
    <div class="b">

      <div class="c">
        <input v-model="a" @keyup.enter="add">
        <button class="lixo" @click="add">Add</button>
      </div>

      <div class="chato">
        <span @click="f = 'todas'" :class="{ 'ativo': f === 'todas' }">Todas</span>
        <span @click="f = 'pendentes'" :class="{ 'ativo': f === 'pendentes' }">Pendentes</span>
        <span @click="f = 'concluidas'" :class="{ 'ativo': f === 'concluidas' }">Concluidas</span>
      </div>

      <ul class="paia">
        <li v-for="i in filtrados" :key="i.id">
          <span v-if="paia !== i.id" class="texto" :class="{ 'risco': i.concluida }"
            @click="i.concluida = !i.concluida">
            {{ i.titulo }}
          </span>
          <input v-else v-model="i.titulo">

          <div class="botao">
            <a @click.prevent="editar(i)">{{ paia === i.id ? 'Save' : 'Edit' }}</a>
            <a @click.prevent="remover(i.id)">Del</a>
          </div>
        </li>
      </ul>

      <input v-model="b" class="filtro" placeholder="Filtrar...">

      <div class="c">
        <button class="lixo" @click="tarefa.sort((x, y) => x.titulo.localeCompare(y.titulo))">Ordenar</button>
        <button class="lixo" @click="recuperar" :disabled="!lixo">Recuperar</button>
      </div>

      <div class="a">
        <span>Pendentes: {{ p }}</span>
        <span>Concluidas: {{ c }}</span>
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
.chato {
  display: flex;
  justify-content: space-between;
  margin: 10px 0;
}
.ativo {
  color: #fff;
  text-decoration: underline;
}
.paia {
  list-style: none;
  padding: 0;
}
.paia li {
  display: flex;
  justify-content: space-between;
  margin-top: 10px;
}
.texto {
  flex: 1;
}
.risco {
  text-decoration: line-through;
}
.botao a {
  color: #00c853;
  margin-left: 10px;
}
.filtro {
  width: 100%;
  margin: 10px 0;
}
.a {
  display: flex;
  justify-content: space-between;
  margin-top: 20px;
}
</style>
