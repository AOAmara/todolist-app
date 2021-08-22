<template>
  <div class="container">
    <h2 class="text-center mt-5">Todo list App</h2>
    <!-- Input -->
    <div class="d-flex">
      <input type="text" placeholder="Add new task" class="form-control rounded-0" v-model="task">
      <button class="btn btn-primary rounded-0" @click="submitTask">SUBMIT</button>
    </div>
    <!-- Tasks table -->
    <table class="table table-bordered mt-5">
      <thead>
      <tr>
        <th scope="col">Task</th>
        <th scope="col">Status</th>
        <th scope="col" class="text-center">#</th>
        <th scope="col" class="text-center">#</th>
      </tr>
      </thead>
      <tbody>
      <tr v-for="(task, index) in tasks" :key="index">
        <th>
          <span :class="{'finished': task.status === 'finished'}">{{ task.name }}</span>
        </th>
        <td style="width: 120px">
          <span class="pointer"
                @click="changeStatus(index)"
                :class="{
                'text-primary': task.status === 'to-do',
                'text-warning': task.status === 'in-progress',
                'text-success': task.status === 'finished'
                }"
          >
            {{ firstCharUpper(task.status) }}
          </span>
        </td>
        <td>
          <div class="text-center text-info pointer" @click="updateTask(index)">
            <span class="fa fa-pen"></span>
          </div>
        </td>
        <td>
          <div class="text-center text-danger pointer" @click="removeTask(index)">
            <span class="fa fa-trash"></span>
          </div>
        </td>
      </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
export default {
  name: 'TodoApp',
  props: {
    msg: String
  },
  data() {
    return {
      tasks: [],
      task: '',
      updatedTask: null,
      availableStatuses: ['to-do', 'in-progress', 'finished']
    }
  },
  mounted() {
    if(localStorage.getItem('tasks')) {
      try {
        this.tasks = JSON.parse(localStorage.getItem('tasks'));
      } catch (e) {
        localStorage.removeItem('tasks');
      }
    }
  },
  methods: {
    submitTask() {
      if(this.task.length === 0) return;
      if(this.updatedTask === null) {
        this.tasks.push({
          name: this.task,
          status: 'to-do'
        });
      } else {
        this.tasks[this.updatedTask].name = this.task;
        this.updatedTask = null;
      }
      this.task = '';
      this.saveToLocalStorage();
    },
    saveToLocalStorage() {
      const parsed = JSON.stringify(this.tasks);
      localStorage.setItem('tasks', parsed);
    },
    firstCharUpper(str) {
      return str.charAt(0).toUpperCase() + str.slice(1);
    },
    updateTask(index) {
      this.task = this.tasks[index].name;
      this.updatedTask = index;
    },
    changeStatus(index) {
      let newIndex = this.availableStatuses.indexOf(this.tasks[index].status);
      if(++newIndex > 2) newIndex = 0;
      this.tasks[index].status = this.availableStatuses[newIndex];
    },
    removeTask(index) {
      this.tasks.splice(index, 1);
      this.saveToLocalStorage();
    }
  }
}
</script>

<style scoped>
  .pointer {
    cursor: pointer;
  }
  .finished {
    color: gray;
    font-weight: normal;
    text-decoration: line-through;
  }
</style>
