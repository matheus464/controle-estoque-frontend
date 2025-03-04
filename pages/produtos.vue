<template>
    <v-container>
      <h1 class="text-h4 mb-4">Lista de Produtos</h1>
      <ProdutoForm />
  
      <v-data-table
        :headers="headers"
        :items="produtos"
        class="elevation-1 mt-4"
        item-value="id"
        header-class="font-weight-bold"
      >
        <template v-slot:top>
          <v-toolbar flat>
            <v-toolbar-title>Produtos</v-toolbar-title>
          </v-toolbar>
        </template>
      </v-data-table>
    </v-container>
</template>
  
<script setup>
  import { ref, onMounted } from 'vue'
  import ProdutoForm from '@/components/ProdutoForm.vue'
  
  const produtos = ref([])
  
  const fetchProdutos = async () => {
    const response = await fetch('http://localhost:5049/api/Produtos')
    produtos.value = await response.json()
  }
  
  onMounted(fetchProdutos)
  
  const headers = ref([
    { title: "Nome", key: "nome" },
    { title: "Quantidade", key: "quantidade" },
    { title: "Preço (R$)", key: "preco" }
  ])
</script>

<style>

</style>
  