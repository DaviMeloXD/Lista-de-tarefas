<template class="general">
  <header class="meu-header">
    <h1 class="titulo">Lista de Tarefas</h1>
    <img class="titulo__img" src="./assets/clip01cort.png" alt="clip de papel">

  </header>
    <div>
      <ul>
        <li v-for="tarefa in tarefas" :key="tarefa.id" class="lilink">
          <div class="checkbox-letrasaida-container">
            <input type="checkbox" v-model="tarefa.concluida" />
            <span :class="{ 'concluida': tarefa.concluida }" class="letrasaida">{{ tarefa.id === editandoId ? '' : tarefa.texto }}</span>
          </div>
          <div class="editar__remover-container">
            <input v-if="tarefa.id === editandoId" type="text" v-model="tarefaEditando" @keyup.enter="salvarEdicao(tarefa.id)" />
            <button v-if="tarefa.id !== editandoId" @click="editarTarefa(tarefa.id)" class="editar__remover"> Editar </button>
            <button @click="removerTarefa(tarefa.id)" class="editar__remover">Remover</button>
          </div>
        </li>
      </ul>

      <form @submit.prevent="adicionarTarefa">
        <input type="text" v-model="novaTarefa" placeholder="Nova Tarefa" />
        <button type="submit" class="novatarefa"> Adicionar Tarefa </button>
      </form>
      <button @click="marcarTodasConcluidas"> Marcar Todas como Concluídas </button>
    </div>
  </template>
  
  <script>
  export default {
    data() {
      return {
        tarefas: [],
        novaTarefa: '',
        editandoId: null,
        tarefaEditando: ''
      };
    },
    created() {
      const tarefasSalvas = localStorage.getItem('tarefas');
      if (tarefasSalvas) {
        this.tarefas = JSON.parse(tarefasSalvas);
      }
    },
    methods: {
      adicionarTarefa() {
        if (this.novaTarefa.trim() !== '') {
          const novaTarefa = { id: this.tarefas.length + 1, texto: this.novaTarefa, concluida: false };
          this.tarefas.push(novaTarefa);
          this.salvarTarefas();
          this.novaTarefa = '';
        }
      },
      removerTarefa(id) {
        this.tarefas = this.tarefas.filter(tarefa => tarefa.id !== id);
        this.salvarTarefas();
      },
      editarTarefa(id) {
        this.editandoId = id;
        this.tarefaEditando = this.tarefas.find(tarefa => tarefa.id === id).texto;
      },
      salvarEdicao(id) {
        const index = this.tarefas.findIndex(tarefa => tarefa.id === id);
        if (index !== -1) {
          this.tarefas[index].texto = this.tarefaEditando;
          this.editandoId = null;
          this.tarefaEditando = '';
          this.salvarTarefas();
        }
      },
      marcarTodasConcluidas() {
        this.tarefas.forEach(tarefa => tarefa.concluida = true);
        this.salvarTarefas();
      },
      salvarTarefas() {
        localStorage.setItem('tarefas', JSON.stringify(this.tarefas));
      }
    }
  };
  </script>
  
  <style>
  
  </style>
  <style src="./ListaDeTarefas.css"></style>

  