<template>
  <div id="app">
    <h2>To do: ({{ todos.length }})</h2>
    <input
      type="text"
      v-model="todo"
      placeholder="Type something and press ENTER"
      @keyup.enter="addItem"
    />
    <ol>
      <draggable
        v-model="todos"
        group="todos"
        ghost-class="ghost"
        @start="drag = true"
        @end="drag = false"
        @change="dropOnTodos"
      >
        <li v-for="(item, index) in todos" :key="`todoItem_${index}`">
          <label
            :for="`todoItem_${index}`"
            @click.prevent="toggleCompleted(item)"
          >
            <input
              :id="`todoItem_${index}`"
              type="checkbox"
              :checked="item.completed === true"
            />
            <span>{{ item.text }}</span>
          </label>
          <a href="#" class="remove" @click.prevent="removeTodo(item)"
            >Remove</a
          >
        </li>
      </draggable>
    </ol>
    <hr />
    <h2>Completed: ({{ completed.length }})</h2>
    <ol>
      <draggable
        v-model="completed"
        group="todos"
        ghost-class="ghost"
        @start="drag = true"
        @end="drag = false"
      >
        <li v-for="(item, index) in completed" :key="`todoItem_${index}`">
          <label
            :for="`todoItem_${index}`"
            @click.prevent="toggleCompleted(item)"
          >
            <input
              :id="`todoItem_${index}`"
              type="checkbox"
              :checked="item.completed === true"
            />
            <span>{{ item.text }}</span>
          </label>
          <a href="#" class="remove" @click.prevent="removeCompleted(item)"
            >Remove</a
          >
        </li>
      </draggable>
    </ol>
  </div>
</template>

<script>
import draggable from "vuedraggable";

export default {
  name: "App",

  components: {
    draggable,
  },

  data() {
    return {
      todos: [
        { text: "Learn about Vue", completed: false },
        { text: "Learn about Fliplet", completed: false },
        { text: "Play around in JSFiddle", completed: false },
        { text: "Show us what you've got", completed: false },
      ],
      completed: [],
      todo: "",
    };
  },

  methods: {
    addItem() {
      this.todos.push({ text: this.todo, completed: false });
      this.todo = "";
    },

    removeTodo(item) {
      this.todos = this.todos.filter((todo) => todo.text !== item.text);
    },

    removeCompleted(item) {
      this.completed = this.completed.filter(
        (completed) => completed.text != item.text
      );
    },

    toggleCompleted(item) {
      item.completed = !item.completed;
      if (item.completed === true) {
        this.completed.push(item);
        this.removeTodo(item);
      } else {
        this.todos.push(item);
        this.removeCompleted(item);
      }
    },

    dropOnTodos(prop) {
      if (prop.added) {
        this.todos = this.todos.map((todo) => ({ ...todo, completed: false }));
      }
      if (prop.removed) {
        this.completed = this.completed.map((completed) => ({
          ...completed,
          completed: true,
        }));
      }
    },
  },
};
</script>

<style>
body {
  background: #20262e;
  padding: 20px;
  font-family: Helvetica;
}

#app {
  background: #fff;
  border-radius: 4px;
  padding: 20px;
  transition: all 0.2s;
}

li {
  margin: 8px 0;
}

h2 {
  font-weight: bold;
  margin-bottom: 15px;
}

del {
  color: rgba(0, 0, 0, 0.3);
}

input[type="text"] {
  padding: 10px;
  width: 200px;
}

label .completed {
  color: green;
  font-weight: bold;
}

label + .remove {
  display: none;
  color: red;
}

li:hover .remove {
  display: inline-block;
  margin-left: 10px;
  color: red;
  opacity: 0.5;
}

.ghost {
  background-color: #ddd;
}
</style>