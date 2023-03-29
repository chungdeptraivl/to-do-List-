<!-- eslint-disable no-console -->
<!-- eslint-disable vue/no-template-shadow -->
<!-- eslint-disable no-console -->
<template>
  <div class="container">
    <h1>My Vue Todo App</h1>

    <div class="add_task">
      <input
        id="add-task"
        v-model="newTask"
        type="text"
        placeholder="Enter task"
        @keypress.enter="addTask"
      />
      <button class="submit-btn" @click="addTask">SUBMIT</button>
    </div>

    <table style="width: 100%">
      <tr>
        <th class="task_content" style="width: 60%">Task</th>
        <th class="status" style="width: 20%">Status</th>
        <th class="delete" style="width: 10%">#</th>
        <th class="repair" style="width: 10%">#</th>
      </tr>
      <tr
        v-for="(task, i) in tasks"
        :key="i"
        :class="`task ${task.done ? 'finished' : ''}`"
      >
        <td class="content">{{ task.content }}</td>
        <td class="behavior" @click="toggleTask(i)">
          {{ task.done ? 'Finished' : 'To-do' }}
        </td>
        <td class="delete-btn" @click="removeTask(i)">
          <font-awesome-icon class="delete-icon" :icon="['fas', 'trash']" />
        </td>
        <td class="repair-btn" @click="repairTask(i)">
          <font-awesome-icon class="repair-icon" :icon="['fas', 'pen']" />
        </td>
      </tr>
    </table>
  </div>
</template>

<script>
export default {
  // eslint-disable-next-line vue/component-definition-name-casing
  name: 'Todo_List',

  // layout: 'header',

  data() {
    return {
      newTask: '',
      editTaskDone: null,

      tasks: [],
    }
  },

  created() {
    this.getData()
  },

  methods: {
    getData(tasks) {
      if (process.client) {
        this.tasks = localStorage.getItem('tasks')
          ? JSON.parse(localStorage.getItem('tasks'))
          : []
      }
    },

    addTask() {
      // eslint-disable-next-line no-useless-return
      try {
        if (this.newTask.length === 0) return
        if (this.editTaskDone === null) {
          this.tasks = [{ content: this.newTask, done: false }, ...this.tasks]
        } else {
          this.tasks[this.editTaskDone].content = this.newTask
          this.editTaskDone = null
        }
        this.newTask = ''

        // eslint-disable-next-line no-console
        console.log(this.tasks)
        localStorage.setItem('tasks', JSON.stringify(this.tasks))
      } catch (error) {
        // eslint-disable-next-line no-console
        console.log(error)
      }
    },

    toggleTask(i) {
      this.tasks[i].done = !this.tasks[i].done
      localStorage.setItem('tasks', JSON.stringify(this.tasks))
    },

    removeTask(i) {
      this.tasks.splice(i, 1)
      localStorage.setItem('tasks', JSON.stringify(this.tasks))
    },

    repairTask(i) {
      this.newTask = this.tasks[i].content
      this.editTaskDone = i
      localStorage.setItem('tasks', JSON.stringify(this.tasks))
    },
  },
}
</script>
