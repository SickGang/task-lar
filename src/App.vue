<template>
  <div id="app">
    <div class="container">
      <h5 class="text-left">Список дел: {{tasks.length}}</h5>
      <table class="table table-hover table-borded table-striped">
        <thead class="thead-dark">
        <tr>
          <th scope="col">#</th>
          <th scope="col">Наименование</th>
          <th scope="col">Дата</th>
          <th scope="col">Статус</th>
          <th scope="col">Действия</th>
        </tr>
        </thead>
        <tbody class="table-striped">
          <list
          v-for="(task,index) in tasksFiltered" :key="task.title"
          :tasks="task"
          :index="index"
          :class="{yellow: task.checked}"
          @deleteTask="deleteTask"
          @editTask="editTask"
          />
        </tbody>
        <tr>
          <th scope="row"></th>
          <td><input class="white" type="text" placeholder="Введите название задачи" v-model="newTask.title"></td>
          <td><input type="text" placeholder="Введите дату выполнения задачи" v-model="newTask.date"></td>
          <td></td>
          <td>
            <button @click="addTask()" class="btn-small green mr-2"><i class="large material-icons">add</i></button>
          </td>
        </tr>
      </table>
      <div class="row col-12">
        <div class="btn_filter col-6 text-left align-items-center">
          <button @click="filter = 'all'" class="btn mr-3">Все</button>
          <button @click="filter = 'active'" class="btn mr-3 green">Активные</button>
          <button @click="filter = 'completed'" class="btn red">Завершенные</button>
        </div>
        <div class="input_check-all col-6 text-right">
          <label>
            <input @click="checkAll" type="checkbox" />
            <span>Завершить все</span>
          </label>
        </div>
      </div>
      <div class="" v-show="isModal">
         <div class="modal-dialog">
          <div class="modal-content">
            <div class="modal-header">
              <h5 class="modal-title">Редактировать</h5>
              <button type="button" class="close" data-dismiss="modal" aria-label="Close">
                <span aria-hidden="true">&times;</span>
              </button>
            </div>
            <div class="modal-body">
              <input type="text" v-model="editidTask.title">
              <input type="text" v-model="editidTask.date">
            </div>
            <div class="modal-footer">
              <button type="button" class="btn btn-secondary" data-dismiss="modal" @click="isModal = false">Close</button>
              <button type="button" class="btn btn-primary" @click="saveEditTask">Save changes</button>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import list from './components/list.vue'

export default {
  name: 'App',
  data () {
    return {
      newTask: {
        title: '',
        date: '',
        checked: false
      },
      filter: 'all',
      isModal: false,
      editidTask: {
        title: '',
        date: '',
        id: ''
      },
      tasks: []
    }
  },
  mounted () {
    const data = localStorage.getItem('todos')
    if (data) {
      this.tasks = JSON.parse(data)
    }
  },
  computed: {
    tasksFiltered () {
      if (this.filter === 'all') {
        return this.tasks
      } else if (this.filter === 'active') {
        return this.tasks.filter(task => !task.checked)
      } else if (this.filter === 'completed') {
        return this.tasks.filter(task => task.checked)
      }
      return this.tasks
    }
  },
  methods: {
    addTask () {
      if (this.newTask.title !== '' && this.newTask.date !== '') {
        this.tasks.push({
          title: this.newTask.title,
          date: this.newTask.date,
          checked: false
        })
        this.newTask.title = ''
        this.newTask.date = ''
        localStorage.setItem('todos', JSON.stringify(this.tasks))
      }
    },
    deleteTask (index) {
      this.tasks.splice(index, 1)
      localStorage.setItem('todos', JSON.stringify(this.tasks))
    },
    checkAll () {
      this.tasks.forEach(function (task) {
        task.checked = event.target.checked
      })
      localStorage.setItem('todos', JSON.stringify(this.tasks))
    },
    editTask (task, index) {
      this.isModal = true
      this.editidTask.title = task.title
      this.editidTask.date = task.date
      this.editidTask.id = index
    },
    saveEditTask () {
      this.tasks.map((task, index) => {
        if (index === this.editidTask.id) {
          console.log(this.editTask.date)
          // task.date = this.editTask.date
          // task.title = this.editTask.title
        }
        // console.log(this.tasks)
      })
    }
  },
  components: {
    list
  }
}
</script>

<style lang="scss">
@import '~materialize-css/dist/css/materialize.min.css';

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

input[type=text]:not(.browser-default) {
  height: 2rem;
}

</style>
