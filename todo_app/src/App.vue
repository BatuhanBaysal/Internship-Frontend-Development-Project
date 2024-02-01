<template>
  <div class="content">
    <h1>To-Do List App</h1>
    <form @submit.prevent="addToDo">
      <div class="todo">
          <input 
            type="text" 
            class="input" 
            v-model="todo" 
            placeholder="Please, Enter a To-Do! (max 100 characters)"
            :maxlength="maxCharCount"
            @input="checkLowerCase"
          />
      </div>
      <button type="submit" class="button">Add ToDo</button>
      <p v-if="showWarning" class="warning">
        Please enter a To-Do before adding! (Only Letters)
      </p>
    </form>
    <div class="todo-content"
        v-for="todo in todos" 
        :key="todo.id" 
        :class="{ 'done-item': todo.done }"
        @mouseover="hoverBackground(todo)"
        @mouseleave="resetBackground(todo)">
        <p @click="done(todo)" 
          :class="{done: todo.done}">
          {{ todo.content }}
        </p>
        <p>
          Done: {{ todo.done }}
        </p>
    </div>
  </div>
</template>

<script>
import { ref } from "vue";
export default {
  setup() {
    const todo = ref("");
    const todos = ref([]);
    const showWarning = ref(false);
    const maxCharCount = 100; 

    function addToDo() {
      if (todo.value.trim() !== "" && todo.value === todo.value.toLowerCase()) {
        todos.value.push({
          done: false,
          content: todo.value,
          id: Date.now(),
        });
        todo.value = "";
        showWarning.value = false;
      } else {
        showWarning.value = true;
      }
    }

    function done(todo) {
      todo.done = !todo.done;
    }

    function hoverBackground(todo) {
      if (!todo.done) {
        todo.hoverBackground = "#ffcccc"; 
      } else {
        todo.hoverBackground = "#99ff99"; 
      }
    }

    function resetBackground(todo) {
      todo.hoverBackground = ""; 
    }

    function checkLowerCase() {
      const lowerCaseValue = todo.value.toLowerCase();
      const onlyLettersAndSpaces = /^[a-zA-Z\s]+$/.test(lowerCaseValue);

      if (onlyLettersAndSpaces || lowerCaseValue.trim() === "") {
        todo.value = lowerCaseValue;
        showWarning.value = false;
      } else {
        todo.value = "";  
        showWarning.value = true;
      }
    }

    return { todo, todos, addToDo, done, showWarning, hoverBackground, resetBackground, maxCharCount, checkLowerCase };
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

.content {
  padding-top: 20px;
  padding-bottom: 20px;
  padding-left: 100px;
  padding-right: 100px;
}

.input {
  height: 30px;
  width: 75%; 
  font-size: 20px;
  margin: 0 auto; 
  box-sizing: border-box; 
}

.button {
  display: inline-block;
  padding: 20px 40px;
  margin-top: 20px;
  font-size: 16px;
  text-align: center;
  text-decoration: none;
  cursor: pointer;
  border: 2px solid #3498db;
  border-radius: 5px;
  color: #ffffff;
  background-color: #3498db;
  transition: background-color 0.3s, color 0.3s;
}

.button:active {
  background-color: #008000;
  border-color: #008000;
}

.todo-content {
  margin-top: 20px;
  margin-bottom: 5px;
  padding: 10px;
  padding-left: 100px;
  padding-right: 100px;
  background-color: #f5f5f5;
  border-radius: 15px; 
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.2); 
}

.todo-content.done-item {
  background-color: #99ff99; 
}

.todo-content:hover {
  background-color: #ffcccc; 
}

.done {
  text-decoration: line-through;
}

.cursor {
  cursor: pointer;
}

.warning {
  color: red;
}
</style>