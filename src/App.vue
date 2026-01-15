<script setup>

import { ref } from 'vue';
import ShoppingItem from './components/ShoppingItem.vue';
import { computed } from 'vue';

const novoItem = ref('');
const lista = ref([]);

const item = {
  nome: '',
  comprado: false,
  criadoEm: new Date()
}


function toggleComprado(item) {
  item.comprado = !item.comprado;
}

function adicionarItem(){
  if(novoItem.value === '')
  return alert('Digite um item');
  else{
    const item = {
      nome: novoItem.value,
      comprado: false,
      criadoEm: new Date(),
      id: Date.now()
    }
    lista.value.push(item);
    novoItem.value = '';
  }
}

function removerItem(item) {
  lista.value.splice(lista.value.indexOf(item),1);
}

const itensComprados = computed(() =>{
  return lista.value.filter(item => item.comprado).length
})

const itensPendentes = computed(() =>{
  return lista.value.filter(item => !item.comprado).length
})

const listaOrdenada = computed(() => {
  const copiaLista = [...lista.value];
  copiaLista.sort((a,b) => a.comprado - b.comprado);
  return copiaLista;
});

</script>

<template>

<div class="app">
<h1 class="title">🛒 Lista de Compras</h1>

<div class="add-item">
<input placeholder="Digite um item da compra" v-model="novoItem" />
<button @click="adicionarItem()">Adicionar</button>
</div>

<p class="empty" v-if="lista.length === 0">A lista está vazia</p>
<div v-else v-for="(item) in listaOrdenada" :key="item.id">
  <ShoppingItem :item="item" @toggleComprado="toggleComprado(item)" @removerItem="removerItem(item)" />
</div>

<div class="status">
  <div class="status-box comprado">
    🟢 Comprados
    <strong>{{ itensComprados }}</strong>
  </div>

  <div class="status-box pendente">
    🔴 Pendentes
    <strong>{{ itensPendentes }}</strong>
  </div>
</div>


</div>

</template>

<style scoped>

.app {
  max-width: 500px;
  margin: 40px auto;
  padding: 20px;

  background: #ffffff;
  border-radius: 8px;

  font-family: Arial, sans-serif;
}

.add-item {
  display: flex;
  gap: 10px;
  margin-bottom: 20px;
}

.add-item input {
  flex: 1;
  padding: 10px;

  border: 1px solid #ccc;
  border-radius: 6px;
  font-size: 14px;
}

.add-item input:focus {
  outline: none;
  border-color: #42b983;
}

.add-item button {
  padding: 10px 16px;

  background: #42b983;
  color: white;

  border: none;
  border-radius: 6px;
  cursor: pointer;
}

.add-item button:hover {
  background: #369a6e;
}

.title {
  display: flex;
  align-items: center;
  gap: 8px;

  font-size: 26px;
  margin-bottom: 20px;
  color: #2c3e50;
}

.status {
  display: flex;
  gap: 12px;
  margin-top: 20px;
}

.status-box {
  flex: 1;
  padding: 12px;
  border-radius: 8px;
  text-align: center;

  font-size: 14px;
}

.status-box strong {
  display: block;
  font-size: 22px;
  margin-top: 4px;
}

.status-box.comprado {
  background: #e6f7ee;
  color: #0f9d58;
}

.status-box.pendente {
  background: #fdecea;
  color: #c0392b;
}

.empty {
  margin: 20px 0;
  text-align: center;

  color: #777;
  font-size: 14px;
}

</style>