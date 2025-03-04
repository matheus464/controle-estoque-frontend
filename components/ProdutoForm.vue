<script setup>
import { ref } from 'vue'

const produto = ref({
  nome: '',
  descricao: '',
  quantidade: 1,
  preco: 0,
  categoriaId: null
})

const enviarProduto = async () => {
  await fetch('http://localhost:5049/api/Produtos', {
    method: 'POST',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify(produto.value)
  })
  alert('Produto cadastrado com sucesso!')
}
</script>

<template>
  <v-card class="pa-4">
    <v-card-title>Cadastro de Produto</v-card-title>
    <v-card-text>
      <v-text-field label="Nome" v-model="produto.nome" required></v-text-field>
      <v-text-field label="Descrição" v-model="produto.descricao"></v-text-field>
      <v-text-field label="Quantidade" v-model="produto.quantidade" type="number"></v-text-field>
      <v-text-field label="Preço (R$)" v-model="produto.preco" type="number"></v-text-field>
      <v-btn color="primary" @click="enviarProduto">Salvar</v-btn>
    </v-card-text>
  </v-card>
</template>
