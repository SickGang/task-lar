<template>
  <tr :class="{yellow: task.checked}">
    <th scope="row">{{index + 1}}</th>
    <td>{{task.title}}</td>
    <td>{{task.date}}</td>
    <td>
        <label>
        <input v-model="tasks.checked" @click="checkedTask(index)" type="checkbox" />
        <span></span>
      </label>
    </td>
    <td>
      <button @click="editTask(task,index)" class="btn-small blue mr-2"><i class="large material-icons">edit</i></button>
      <button @click="deleteTask(index)" class="btn-small red"><i class="large material-icons">delete</i></button>
    </td>
  </tr>
</template>

<script>
export default {
  props: ['tasks', 'index'],
  name: 'list',
  data () {
    return {
      task: {
        title: this.tasks.title,
        date: this.tasks.date,
        checked: this.tasks.checked
      }
    }
  },
  methods: {
    deleteTask (index) {
      this.$emit('deleteTask', index)
    },
    checkedTask (id) {
      const data = JSON.parse(localStorage.getItem('todos'))
      data.map(function (item, index, array) {
        if (index === id) {
          item.checked = !item.checked
        }
      })
      localStorage.setItem('todos', JSON.stringify(data))
    },
    editTask (task, index) {
      this.$emit('editTask', task, index)
    }
  }
}

</script>

<style scoped>

</style>
