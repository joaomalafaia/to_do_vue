<script setup>
import {reactive} from 'vue'
import Cabecalho from './components/Cabecalho.vue'
import Formulario from './components/Formulario.vue'
import ListaDeTarefas from './components/ListaDeTarefas.vue'

  const estado = reactive({
    filtro: 'todas',
    tarefaTemp: '',
    tarefas: [
      {
        título: 'Estudar ES6',
        finalizada: false,
      },
      {
        título: 'Estudar SASS',
        finalizada: false,
      },
      {
        título: 'Estudar JS',
        finalizada: true,
      }
    ]
  })

  const getTarefasPendentes = () => {
    return estado.tarefas.filter(tarefa => !tarefa.finalizada)
  }

  const getTarefasFinalizadas = () => {
    return estado.tarefas.filter(tarefa => tarefa.finalizada)
  }

  const getTarefasFiltradas = () => {
    const { filtro } = estado

    switch (filtro) {
      case 'pendentes':
        return getTarefasPendentes()
      case 'finalizadas':
        return getTarefasFinalizadas()
      default:
        return estado.tarefas
    }
  }

  const cadastraTarefa = () => {
    const novaTarefa = {
      título: estado.tarefaTemp,
      finalizada: false,
    }
    estado.tarefas.push(novaTarefa)
    estado.tarefaTemp = ''
  }
</script>

<template>
  <div class="container">
    <Cabecalho :tarefasPendentes="getTarefasPendentes().length" />
    <Formulario :trocarFiltro="evento => estado.filtro = evento.target.value" :tarefaTemp="estado.tarefaTemp" :editaTarefaTemp="evento => estado.tarefaTemp = evento.target.value" :cadastraTarefa="cadastraTarefa"/>
    <ListaDeTarefas :tarefas="getTarefasFiltradas()"/>
  </div>
</template>