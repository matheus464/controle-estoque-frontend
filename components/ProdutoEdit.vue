<template>
    <v-dialog v-model="isDialogOpen" max-width="500px">
      <v-card>
        <v-card-title>Editar Produto</v-card-title>
        <v-card-text>
          <v-text-field label="Nome" v-model="produtoEditado.nome"></v-text-field>
          <v-text-field label="Descrição" v-model="produtoEditado.descricao"></v-text-field>
          <v-text-field label="Quantidade" v-model="produtoEditado.quantidade" type="number"></v-text-field>
          <v-text-field label="Preço (R$)" v-model="produtoEditado.preco" type="number"></v-text-field>
        </v-card-text>
        <v-card-actions>
          <v-btn color="blue" @click="salvarEdicao">Salvar</v-btn>
          <v-btn color="gray" @click="fecharModal">Cancelar</v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
</template>
  
<script setup>

  import { ref, watch } from 'vue'
  
  const props = defineProps(['produto', 'dialog'])
  const emit = defineEmits(['produtoAtualizado', 'close'])
  
  const isDialogOpen = ref(props.dialog)
  const produtoEditado = ref({ ...props.produto })
  
  watch(() => props.dialog, (newVal) => {
    isDialogOpen.value = newVal
  })
  
  const salvarEdicao = async () => {
    await fetch(`http://localhost:5049/api/Produtos/${produtoEditado.value.id}`, {
      method: 'PUT',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify(produtoEditado.value)
    })
  
    emit('produtoAtualizado')
    fecharModal()
  }
  
  const fecharModal = () => {
    isDialogOpen.value = false
    emit('close')
  }
</script>
  