<template>
  <v-container>
    <h1 class="text-h4 mb-4">Movimentações de Estoque</h1>

    <MovimentacaoForm @movimentacaoRegistrada="fetchMovimentacoes" />

    <v-data-table
      :headers="headers"
      :items="movimentacoes"
      class="elevation-1 mt-4"
      item-value="id"
      :items-per-page="10"
    >
      <template v-slot:item.tipo="{ item }">
        <v-chip :color="item.tipo === 'entrada' ? 'green' : 'red'" dark>
          {{ item.tipo.toUpperCase() }}
        </v-chip>
      </template>
    </v-data-table>
  </v-container>
</template>

<script setup>

import { ref, onMounted } from 'vue'
import MovimentacaoForm from '@/components/MovimentacaoForm.vue'

const movimentacoes = ref([])

const fetchMovimentacoes = async () => {
  const response = await fetch('http://localhost:5049/api/Movimentacoes')
  movimentacoes.value = await response.json()
}

onMounted(fetchMovimentacoes)

const headers = ref([
  { title: "Produto", key: "produto.nome" },
  { title: "Tipo", key: "tipo" },
  { title: "Quantidade", key: "quantidade" },
  { title: "Data", key: "dataMovimentacao" }
])

</script>
