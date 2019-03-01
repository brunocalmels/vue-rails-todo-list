<template>
  <div id="app">
    <h2>Incompletes</h2>
    <ul id="incompletes">
      <li
        v-for="todo in incompletes"
        :class="{ incomplete: !todo.completo, editing: todo == editedTodo }"
        :key="todo.id"
        @dblclick="toggle(todo)"
        @click="rename(todo)"
      >
        <div class="view">
          <p>{{todo.nombre}}</p>
        </div>
        <div class="edit">
          <input
            type="text"
            v-model="todo.nombre"
            v-todo-focus="todo == editedTodo"
            @blur="doneEdit(todo)"
            @keyup.enter="doneEdit(todo)"
            @keyup.esc="escapeEdit(todo)"
          >
        </div>
      </li>
    </ul>
    <hr>

    <h2>Completes</h2>
    <ul id="completes">
      <li
        v-for="todo in completes"
        :class="{ complete: todo.completo, editing: todo == editedTodo  }"
        :key="todo.id"
        @dblclick="toggle(todo)"
        @click="rename(todo)"
      >
        <div class="view">
          <p>{{todo.nombre}}</p>
        </div>
        <div class="edit">
          <input
            type="text"
            v-model="todo.nombre"
            v-todo-focus="todo == editedTodo"
            @blur="doneEdit(todo)"
            @keyup.enter="doneEdit(todo)"
            @keyup.esc="escapeEdit(todo)"
          >
        </div>
      </li>
    </ul>

    <div id="add-todo" @click="addTodo">
      <div class="boton">
        <span>+</span>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data: () => {
    return {
      counter: 3,
      editedTodo: null,
      todos: [
        {
          nombre: "Barrer",
          completo: false,
          id: 0
        },
        {
          nombre: "Cocinar",
          completo: false,
          id: 1
        },
        {
          nombre: "Saber VueJS",
          completo: true,
          id: 2
        }
      ]
    };
  },
  computed: {
    completes() {
      return this.todos.filter(todo => {
        return todo.completo;
      });
    },
    incompletes() {
      return this.todos.filter(todo => {
        return !todo.completo;
      });
    }
  },
  methods: {
    toggle: todo => {
      todo.completo = !todo.completo;
    },
    addTodo() {
      this.todos.push({
        nombre: "Renombrame",
        id: this.counter++,
        completo: false
      });
    },
    rename(todo) {
      this.cacheNombre = todo.nombre;
      this.editedTodo = todo;
    },
    doneEdit(todo) {
      console.log("doneEdit", todo);

      todo.nombre = todo.nombre.trim();
      if (!todo.nombre) {
        this.removeTodo(todo);
      }
      this.editedTodo = null;
    },
    escapeEdit(todo) {
      todo.nombre = this.cacheNombre;
      this.editedTodo = null;
    },
    removeTodo(todo) {
      this.todos.splice(this.todos.indexOf(todo), 1);
    }
  },
  directives: {
    "todo-focus": (el, binding) => {
      if (binding.value) {
        el.focus();
      }
    }
  }
};
</script>

<style scoped lang='scss'>
#app {
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
}
hr {
  width: 80vw;
}
p {
  font-size: 2em;
  text-align: center;
}
#add-todo {
  position: fixed;
  bottom: 60px;
  cursor: pointer;
  .boton {
    padding: 10px;
    border: 1px solid white;
    border-radius: 50%;
    background-color: green;
    color: white;
    width: 50px;
    height: 50px;
    display: flex;
    justify-content: center;
    align-items: center;
    span {
      font-size: 30pt;
      font-display: bold;
    }
  }
}
ul {
  list-style: none;
  padding-left: 0;
  li {
    cursor: pointer;
    // &:hover {
    //   border: 1px dashed grey;
    // }
    &.complete {
      p {
        color: green;
      }
    }
    &.incomplete {
      p {
        color: red;
      }
    }
    &:not(.editing) {
      .view {
        display: initial;
      }
      .edit {
        display: none;
      }
    }
    &.editing {
      .view {
        display: none;
      }
      .edit {
        display: initial;
      }
    }
  }
  &#completes {
    li {
      text-transform: line-through;
    }
  }
}
</style>
