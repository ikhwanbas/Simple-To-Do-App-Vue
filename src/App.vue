<template>
  <div class="container" style="margin-top: 20px; width: 100%;">
    <div class="card">
      <div class="card-body">
        <h5 class="card-title"> Simple To Do App</h5>
        <p>this app created using Vue JS and save current data to local storage</p>
        <div class="row">
          <div class="col-10">
            <input v-model="todo" @keyup.enter="add" type="text" class="form-control">
          </div>
          <div class="col-2">
            <button class="btn btn-success" @click="add">ADD</button>
          </div>
        </div>
        <List :todos="todos" @deleteTODO="deleteTODO" @doneTodo="doneTodo" />
        <br>
        <div class="row">
          <div class="col-8">
            <small>Total To Do : {{ totalTODO }}</small>
          </div>
          <div class="col-4">
            <button class="btn btn-danger" @click="clearStorage">Clear All Data</button>
          </div>
        </div>

      </div>
    </div>
  </div>
</template>

<script>
import List from "./components/List.vue";
export default {
  components: { List },
  data() {
    return {
      todo: "",
      todos: []
    }
  },
  beforeCreate() {
    if (localStorage.getItem('todos') == undefined) {
      localStorage.setItem('todos', JSON.stringify([]));
    }
  },
  mounted() {
    this.todos = JSON.parse(localStorage.getItem('todos'))
  },
  computed: {
    totalTODO() {
      if (this.todos) {
        return this.todos.length
      } else {
        return 0
      }
    },
  },
  methods: {
    add() {
      this.todos.unshift(
        {
          activity: this.todo,
          isDone: false
        }
      );
      this.todo = "";
      this.saveToLocalStorage();
    },
    deleteTODO(todoIndex) {
      this.todos = this.todos.filter((item, index) => {
        if (index != todoIndex) {
          return item
        }
      });
      this.saveToLocalStorage();

    },
    doneTodo(todoIndex) {
      this.todos = this.todos.filter((item, index) => {
        if (index == todoIndex) {
          if (item.isDone) {
            item.isDone = false
          } else {
            item.isDone = true
          }
        }
        return item
      });
      this.saveToLocalStorage();

    },
    saveToLocalStorage() {
      localStorage.setItem('todos', JSON.stringify(this.todos));
    },
    clearStorage() {
      localStorage.clear();
      this.todos = [];
    }
  }

}
</script>