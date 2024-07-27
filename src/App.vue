<script setup>
import { reactive } from 'vue';


const estado = reactive({ 
  tarefaTemp: '',
  filtro: 'todas',
  tarefas: [
    {
      titulo: 'Estudar ES6',
      finalizado: false
    },
    {
      titulo: 'Estudar SASS',
      finalizado: false
    },
    {
      titulo: 'Ir para a academia',
      finalizado: true,
    }
  ]
})

  const getTarefasPendentes = () => {
    return estado.tarefas.filter(tarefa => !tarefa.finalizado)
  }

  const getTarefasFinalizadas = () => {
    return estado.tarefas.filter(tarefa => tarefa.finalizado)
  }

  const getTarefasFiltradas = () =>{
    const { filtro } = estado;

    switch(filtro){
      case 'pendentes': 
        return getTarefasPendentes()
      case 'finalizadas':
        return getTarefasFinalizadas();
      default: 
        return estado.tarefas;
    }
  }

  const cadastraTarefa = () => {
    const tarefaNova = {
      titulo: estado.tarefaTemp,
      finalizado: false,
    }

    estado.tarefas.push(tarefaNova)
    estado.tarefaTemp = '';
  }

</script>

<template>
  <div class="container">
    <header class="p-5 mb-4 mt-4 bg-light rounded-3">
      <h1 >Minhas Tarefas</h1>
      <p>
        Você possui {{ getTarefasPendentes().length }} tarefas pendentes
      </p>
    </header>
    <form @submit.prevent="cadastraTarefa">
      <div class="row">
        <div class="col">
          <input :value="estado.tarefaTemp" @change="evento => estado.tarefaTemp = evento.target.value" required type="text" placeholder="Digite Aqui a descrição da tarefa" class="form-control">
        </div>
        <div class="col-md-2">
          <button type="submit" class="btn btn-primary">Cadastrar</button>
        </div>
        <div class="col-md-2">
          <select class="form-control" @change="evento => estado.filtro = evento.target.value">
            
            <option value="todas">Todas as tarefas</option>
            <option value="pendentes">Pendentes</option>
            <option value="finalizadas">Finalizadas</option>
          </select>
        </div>
      </div>
    </form>

    
    <ul class="list-group mt-4">
      <li class="list-group-item mb-2"  v-for="tarefa in getTarefasFiltradas()">
        <input @change="evento => tarefa.finalizado =  evento.target.checked" :checked="tarefa.finalizado" :id="tarefa.titulo" type="checkbox">
        
        <label :class="{ done: tarefa.finalizado}" for="tarefa.titulo" class="ms-3">
          {{ tarefa.titulo }}
        </label>
      </li>
    </ul>
  </div>
</template>

<style scoped>
  .done{
    text-decoration: line-through;
  }
</style>
