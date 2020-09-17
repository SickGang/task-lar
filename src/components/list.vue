<template>
  <tr>
    <th scope="row">{{task.id}}</th>
    <td>{{task.title}}</td>
    <td>{{task.date}}</td>
    <td>
        <label>
        <input v-model="tasks.checked" @click="checkedTask(index)" type="checkbox" />
        <span></span>
      </label>
    </td>
    <td>
      <button @click="isModal = true" class="btn-small blue mr-2"><i class="large material-icons">edit</i></button>
      <button @click="deleteTask(index)" class="btn-small red"><i class="large material-icons">delete</i></button>
    </td>
    <div class="modal-wrap" v-show="isModal">
        <div class="modal-dialog">
        <div class="modal-content">
          <div class="modal-header">
            <h5 class="modal-title">Окно редактирования</h5>
            <button type="button" class="close" data-dismiss="modal" aria-label="Close" @click="isModal = false">
              <span aria-hidden="true">&times;</span>
            </button>
          </div>
          <div class="modal-body">
            <input type="text" v-model="task.title">
            <input type="text" v-model="task.date">
          </div>
          <div class="modal-footer">
            <button type="button" class="btn red" data-dismiss="modal" @click="isModal = false">Закрыть</button>
            <button type="button" class="btn green" @click="saveEditTask(index)">Сохранить изменения</button>
          </div>
        </div>
      </div>
    </div>
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
        checked: this.tasks.checked,
        id: this.tasks.id
      },
      isModal: false
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
    saveEditTask (id) {
      this.isModal = false
      const data = JSON.parse(localStorage.getItem('todos'))
      data.forEach((element, index) => {
        if (id === index) {
          element.title = this.task.title
          element.date = this.task.date
        }
      })
      localStorage.setItem('todos', JSON.stringify(data))
    }
  }
}

</script>

<style scoped>

.modal-wrap {
  position: absolute;
  top: 30%;
  left: 40%;
}

</style>
