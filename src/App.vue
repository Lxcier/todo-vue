<script setup>
  import { reactive } from 'vue';
  import { Cabecalho } from './components/Cabecalho.vue';
  import { Formulario } from './components/Formulario.vue';
  import { ToDoList } from './components/ToDoList.vue';


  const estado = reactive ({
    filtro: 'todas',
    tarefaTemp: '',
    tarefas: [
      {
        titulo: 'Estudar ES6',
        finalizada: false
      },
      {
        titulo: 'Estudar SASS',
        finalizada: false,
      },
      {
        titulo: 'Dormir até domingo',
        finalizada: true,
      }
    ]
  })

  const getTarefasFinalizadas = () => {
    return estado.tarefas.filter(tarefa => tarefa.finalizada)
  }

  const getTarefasPendentes = () => {
    return estado.tarefas.filter(tarefa => !tarefa.finalizada)
  }
  
  const getTarefasFiltradas = () => {
    const { filtro } = estado;

    switch (filtro) {
      case 'pendentes':
        return getTarefasPendentes();
      case 'finalizadas': 
        return getTarefasFinalizadas();
      default:
        return estado.tarefas;
    }
  }

  const cadastraTarefa = () => {
    const tarefaNova = {
      titulo: estado.tarefaTemp,
      finalizada: false,
    }
    estado.tarefas.push(tarefaNova);
    estado.tarefaTemp = '';
  }
</script>

<template>
  <div class="container">
    <Cabecalho :tarefas-pendentes="getTarefasPendentes().length" />
    <Formulario  :trocar-filtro="evento => estado.filto = evento.target.value":tarefa-temp="estado.tarefaTemp" :edita-tarefa-temp="evento => estado.tarefaTempo.targe.value" :cadastra-tarefa="cadastraTarefa()" />
    <ToDoList :tarefas="getTarefasFiltradas()" />
  </div>
</template>
