<template>
    <v-container>
      <h1 class="text-h4 mb-4">Lista de Produtos</h1>
      <ProdutoForm @produtoAdicionado="fetchProdutos" />
  
      <v-data-table
        :headers="headers"
        :items="produtos"
        class="elevation-1 mt-4"
        item-value="id"
        >
        <template v-slot:item.acao="{ item }">
            <v-btn icon color="blue" @click="editarProduto(item)">
                <mdi-pencil>✏️</mdi-pencil>
            </v-btn>
            <v-btn icon color="red" @click="deletarProduto(item.id)">
                <mdi-delete>🗑️</mdi-delete>
            </v-btn>
        </template>

      </v-data-table>
  
      
      <ProdutoEdit 
        v-if="produtoEditando" 
        :produto="produtoEditando" 
        :dialog="dialog" 
        @produtoAtualizado="fetchProdutos" 
        @close="dialog = false" 
    />
    </v-container>
</template>
  
<script setup>

  import { ref, onMounted } from 'vue'
  import ProdutoForm from '@/components/ProdutoForm.vue'
  import ProdutoEdit from '@/components/ProdutoEdit.vue'
  
  const produtos = ref([])
  const produtoEditando = ref(null)
  const dialog = ref(false)
  
  // Buscar produtos da API
  const fetchProdutos = async () => {
    const response = await fetch('http://localhost:5049/api/Produtos')
    produtos.value = await response.json()
  }
  
  // Abrir modal de edição
  const editarProduto = (produto) => {
    produtoEditando.value = { ...produto }
    dialog.value = true
  }
  
  // Excluir produto
  const deletarProduto = async (id) => {
    if (!confirm("Tem certeza que deseja excluir este produto?")) return;
  
    await fetch(`http://localhost:5049/api/Produtos/${id}`, {
      method: 'DELETE'
    });
  
    fetchProdutos();
  }
  
  // Cabeçalhos da tabela
  const headers = ref([
    { title: "Nome", key: "nome" },
    { title: "Quantidade", key: "quantidade" },
    { title: "Preço (R$)", key: "preco" },
    { title: "Ações", key: "acao", sortable: false }
  ])
  
  onMounted(fetchProdutos)
</script>
  