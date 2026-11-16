<script>
import task from "./components/task.vue";

export default {
  components: {
    task
  },

  data() {
    return {
      tasks: JSON.parse(localStorage.getItem('toDoItems') ?? '[]'),
    }
  },

  methods: {
    updateTasks(newTasks) {
      this.tasks = newTasks
      localStorage.setItem('toDoItems', JSON.stringify(newTasks))
    },

    createTask(e) {
      const newTask = {
        item: this.newTaskValue,
        status: 'unfinished',
      }
      if (this.newTaskValue.trim().length > 0) {
        this.tasks.push(newTask)
        localStorage.setItem('toDoItems', JSON.stringify(this.tasks))
        document.getElementById('newTask').value = ''
        this.newTaskValue = ''
      }
    },
    filterTasks(e) {
      const unfinished = [...document.getElementsByClassName('unfinished')]
      const finished = [...document.getElementsByClassName('finished')]
      switch (e.target.value) {
        case "all":
          unfinished.forEach((El) => {
            El.classList.remove('hidden');
          });
          finished.forEach((El) => {
            El.classList.remove('hidden');
          });
          break;
        case "finished":
          unfinished.forEach((El) => {
            El.classList.add('hidden');
          });
          finished.forEach((El) => {
            El.classList.remove('hidden');
          });
          break;
        case "unfinished":
          unfinished.forEach((El) => {
            El.classList.remove('hidden');
          });
          finished.forEach((El) => {
            El.classList.add('hidden');
          });
          break;
      }
    }
  }
}
</script>

<template>
  <nav>
    <h1>To-Do List</h1>
  </nav>

  <main>
    <div class="main-head">
      <h2 class="title">My list</h2>
      <form class="new-task">
        <input v-model="newTaskValue" placeholder="New task" type="text" id="newTask" />
        <button type="submit" id="newTaskBtn" @click.prevent="createTask($event)">
          <img src="/plus.svg" alt="add new task" />
        </button>
      </form>
    </div>
    <div class="select-div">
      <select @change="filterTasks($event)" id="filter">
        <option value="all" selected>All</option>
        <option value="finished">Finished</option>
        <option value="unfinished">Unfinished</option>
      </select>
    </div>
    <table>
      <task :tasks="tasks" @update-tasks="updateTasks" />
    </table>
  </main>
</template>
