<script setup>

const props = defineProps({
    item: Object
})

const emit = defineEmits (['toggleComprado', 'removerItem'])

function formatarData(data){
    return new Date(data).toLocaleDateString('pt-BR')

}

</script>

<template>

<div class="item" :class="{ 'comprado-item': item.comprado }">
<input type="checkbox" :checked="item.comprado" @change="emit('toggleComprado')" />

<div class="item-info">
<p :class="{comprado: item.comprado}">
{{ item.nome }}
</p>

<small>{{ formatarData(item.criadoEm) }}</small>

</div>

<button class="remove" @click="emit('removerItem')">Remover</button>
</div>

</template>

<style scoped>

.comprado {
  text-decoration: line-through;
  opacity: 0.6;
   color: #555;
}

.item.comprado-item {
    background: #f4f6f8;
}


.item {
  display: flex;
  align-items: center;
  gap: 12px;

  padding: 12px;
  margin-bottom: 10px;

  background: #f9f9f9;
  border-radius: 8px;

  box-shadow: 0 2px 6px rgba(0, 0, 0, 0.05);
  transition: background 0.2s;
}

.item:hover {
  background: #f1f1f1;
}

.item-info {
  flex: 1;
}

.item-info p {
  margin: 0;
  font-weight: 500;
}

.item-info small {
  color: #777;
  font-size: 12px;
}

.remove {
  background: transparent;
  border: 1px solid #ddd;

  padding: 6px 10px;
  border-radius: 6px;

  font-size: 13px;
  cursor: pointer;
  color: #555;

  transition: all 0.2s;
}

.remove:hover {
  background: #ffecec;
  border-color: #e74c3c;
  color: #e74c3c;
}
  
</style>