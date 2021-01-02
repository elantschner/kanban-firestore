<template>
  <div class="bg">
    <div class="container p-8 mx-auto">
      <h1 class="my-4 text-3xl font-bold text-yellow-200 text-center">Kanban Board</h1>
      <input
        v-model="newTask"
        class="w-full px-3 py-2 leading-tight text-gray-700 border rounded shadow appearance-none focus:outline-none focus:shadow-outline"
        type="text"
        placeholder="New Task"
        @keypress.enter="createNewTask"
      >
      <div class="grid grid-cols-3 mt-4">
        <div class="px-3 py-3 mx-2 bg-purple-100 rounded-lg">
          <p class="text-sm font-semibold tracking-wide text-gray-700">
            To Do
          </p>
          <KanbanColumn :list="todo" @click-close="removeTask(...arguments, todo)" />
        </div>

        <div class="px-3 py-3 mx-2 bg-purple-100 rounded-lg">
          <p class="text-sm font-semibold tracking-wide text-gray-700">
            Doing
          </p>
          <KanbanColumn :list="doing" @click-close="removeTask(...arguments, doing)" />
        </div>

        <div class="px-3 py-3 mx-2 bg-purple-100 rounded-lg">
          <p class="text-sm font-semibold tracking-wide text-gray-700">
            Done
          </p>
          <KanbanColumn :list="done" @click-close="removeTask(...arguments, done)" />
        </div>
      </div>  
    </div>
    <Alerts :show="alert" @close="alert=false" />
  </div>
</template>

<script>
import KanbanColumn from '@/components/KanbanColumn.vue';
import Alerts from '@/components/Alerts.vue';

export default {
  components: {
    KanbanColumn,
    Alerts,
  },
  data() {
    return {
      newTask: '',
      todo: [
        { name: 'Test 1' },
        { name: 'Test 2' },
      ],
      doing: [],
      done: [],
      alert: false,
    };
  },
  methods: {
    createNewTask() {
      this.todo.forEach(el => {
        if (el.name == this.newTask){
          // alert("Name existiert bereits!");
          this.alert = true;
          this.newTask = '';
        }
      });
      if (this.newTask.length !== 0) {
        this.todo.push({ name: this.newTask });
        this.newTask = '';
      }
    },
    removeTask(task, list) {
      const index = list.indexOf(task);
      if (index > -1) {
        list.splice(index, 1);
      }
    }
  },
};
</script>

<style>
  .bg {
    top: 0;
    left: 0;
    height: 100vh;
    background-image: url(assets/yancy-min-842ofHC6MaI-unsplash.jpg);
    background-size: cover;
  }
</style>