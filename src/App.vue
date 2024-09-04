<template>
  <div class="container mt-5">
    <h1 class="text-center">ToDo App</h1>
    <div class="mt-4">
      <form @submit.prevent="addTask">
        <div class="mb-3">
          <input v-model="newTaskTitle" type="text" class="form-control" placeholder="Titre de la tâche" required>
        </div>
        <div class="mb-3">
          <textarea v-model="newTaskDescription" class="form-control" placeholder="Description de la tâche" required></textarea>
        </div>
        <button type="submit" class="btn btn-primary">Ajouter une tâche</button>
      </form>
    </div>

    <h2 class="mt-5">Tâches à faire</h2>
    <ul class="list-group">
      <li v-for="(task, index) in pendingTasks" :key="index" class="list-group-item d-flex justify-content-between align-items-center">
        <div>
          <h5>{{ task.title }}</h5>
          <p>{{ task.description }}</p>
        </div>
        <button @click="completeTask(index)" class="btn btn-success">Terminer</button>
      </li>
    </ul>

    <h2 class="mt-5">Tâches terminées</h2>
    <ul class="list-group">
      <li v-for="(task, index) in completedTasks" :key="index" class="list-group-item d-flex justify-content-between align-items-center">
        <div>
          <h5><del>{{ task.title }}</del></h5>
          <p><del>{{ task.description }}</del></p>
        </div>
      </li>
    </ul>
  </div>
</template>

<script>
import { ref, computed } from 'vue';

export default {
  setup() {
    const tasks = ref([]);
    const newTaskTitle = ref('');
    const newTaskDescription = ref('');

    const addTask = () => {
      tasks.value.push({
        title: newTaskTitle.value,
        description: newTaskDescription.value,
        completed: false,
      });
      newTaskTitle.value = '';
      newTaskDescription.value = '';
    };

    const completeTask = (index) => {
      tasks.value[index].completed = true;
    };

    const pendingTasks = computed(() => tasks.value.filter(task => !task.completed));
    const completedTasks = computed(() => tasks.value.filter(task => task.completed));

    return {
      newTaskTitle,
      newTaskDescription,
      addTask,
      completeTask,
      pendingTasks,
      completedTasks,
    };
  },
};
</script>

<style scoped>
.completed {
  text-decoration: line-through;
}
</style>
