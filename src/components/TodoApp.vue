<template>
  <div class="container">
    <h2 class="text-center">Todo list App</h2>
    <!-- Input -->
    <div class="d-flex">
      <input type="text" placeholder="Enter todo item" class="form-control" v-model="task">
      <button class="btn btn-warning rounded-0" @click="submitTask">SUBMIT</button>
    </div>
    <!-- Tasks table -->
    <table class="table table-bordered mt-5">
      <thead>
      <tr>
        <th scope="col">Task</th>
        <th scope="col">Status</th>
        <th scope="col" class="text-center">Update</th>
        <th scope="col" class="text-center">Delete</th>
      </tr>
      </thead>
      <tbody>
      <tr v-for="(task, index) in tasks" :key="index">
        <th>{{ task.name }}</th>
        <td>{{ task.status }}</td>
        <td>
          <div class="text-center" @click="updateTask(index)">
            <span class="fa fa-pen"></span>
          </div>
        </td>
        <td>
          <div class="text-center" @click="removeTask(index)">
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
      task: '',
      updatedTask: null,
      tasks: [
        {
          name: 'Take out the trash.',
          status: 'to-do'
        },
        {
          name: 'Visit the dentist.',
          status: 'in-progress'
        }
      ]
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
    },
    updateTask(index) {
      this.task = this.tasks[index].name;
      this.updatedTask = index;
    },
    removeTask(index) {
      this.tasks.splice(index, 1);
    }
  }
}
</script>

<style scoped>

</style>
