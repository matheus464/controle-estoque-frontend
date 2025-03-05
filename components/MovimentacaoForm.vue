<template>
    <v-card class="pa-4">
      <v-card-title>Registrar Movimentação</v-card-title>
      <v-card-text>
        <v-select
          label="Produto"
          :items="produtos"
          item-title="nome"
          item-value="id"
          v-model="novaMovimentacao.produtoId"
        ></v-select>
  
        <v-select
          label="Tipo de Movimentação"
          :items="['entrada', 'saida']"
          v-model="novaMovimentacao.tipo"
        ></v-select>
  
        <v-text-field label="Quantidade" type="number" v-model="novaMovimentacao.quantidade"></v-text-field>
  
        <v-alert v-if="erro" type="error" class="mt-3 mb-2">{{ erro }}</v-alert>
  
        <v-btn color="primary" @click="registrarMovimentacao">Salvar</v-btn>
      </v-card-text>
    </v-card>
</template>

<script setup>
import { ref, onMounted } from 'vue'

const novaMovimentacao = ref({
  produtoId: null,
  tipo: 'entrada',
  quantidade: 1
})

const produtos = ref([])
const produtoSelecionado = ref(null)
const erro = ref(null)

const fetchProdutos = async () => {
  const response = await fetch('http://localhost:5049/api/Produtos')
  produtos.value = await response.json()
}

onMounted(fetchProdutos)

const registrarMovimentacao = async () => {
  erro.value = null

  // Buscar o produto selecionado para verificar estoque
  const produto = produtos.value.find(p => p.id === novaMovimentacao.value.produtoId)

  if (!produto) {
    erro.value = "Produto inválido."
    return
  }

  // Verificar se a saída é maior que o estoque disponível
  if (novaMovimentacao.value.tipo === "saida" && novaMovimentacao.value.quantidade > produto.quantidade) {
    erro.value = `Estoque insuficiente! Apenas ${produto.quantidade} disponíveis.`
    return
  }

  await fetch('http://localhost:5049/api/Movimentacoes', {
    method: 'POST',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify(novaMovimentacao.value)
  })

  alert('Movimentação registrada!')
}
</script>

