<template>


  <section id="cour">
    <header>
  <p>{{ $store.state.year }} {{$store.state.month}} {{ $store.state.day }} </p>
  <p>
    nombre de tâche
  </p>
</header>


    <h1>VueJs Tutorial ToDo List</h1>
    <form @submit.prevent="addTodo()">
      <label>New ToDo </label>
      <input v-model="newTodo" name="newTodo" autocomplete="off" />
      <button>Ajouter à la liste</button>
    </form>

    <ul>
      <li v-for="(todo, index) in todos" :key="index">
        <span :class="{ done: todo.done }" @click="doneTodo(todo)">{{
          todo.content
        }}</span>
        <button @click="removeTodo(index)">Supprimer</button>
      </li>
    </ul>
    <h4 v-if="todos.length === 0">Liste Vide.</h4>
  </section>

  
</template>

<script>
import { ref } from "vue";




export default {
  name: "App",
  setup() {
    const newTodo = ref("");
    const defaultData = [
      {
        done: false,
        content: "Write a blog post",
      },
    ];
    const todosData = JSON.parse(localStorage.getItem("todos")) || defaultData;
    const todos = ref(todosData);
    function addTodo() {
      if (newTodo.value) {
        todos.value.push({
          done: false,
          content: newTodo.value,
        });
        newTodo.value = "";
      }
      saveData();
    }
  // stockage de la donnée
    function doneTodo(todo) {
      todo.done = !todo.done;
      saveData();
    }

    function removeTodo(index) {
      todos.value.splice(index, 1);
      saveData();
    }

    function saveData() {
      const storageData = JSON.stringify(todos.value);
      localStorage.setItem("todos", storageData);
    }

    return {
      todos,
      newTodo,
      addTodo,
      doneTodo,
      removeTodo,
      saveData,
    };
  }

};
</script>

<style>
#cour {
  background-color: #fff;
  max-width: 600px;
  margin-left: auto;
  margin-right: auto;
  padding: 20px;
}

header {
  width: 100%;
  display: flex;
  justify-content: space-around;
}

h1 {
  font-weight: bold;
  font-size: 28px;
  text-align: center;
}

form {
  display: flex;
  flex-direction: column;
  width: 100%;
}

label {
  font-size: 14px;
  font-weight: bold;
}

input,
button {
  height: 48px;
  box-shadow: none;
  outline: none;
  padding-left: 12px;
  padding-right: 12px;
  border-radius: 6px;
  font-size: 18px;
  margin-top: 6px;
  margin-bottom: 12px;
}

input {
  background-color: transparent;
  border:  2px solid rgb(white);
  color: inherit;
}

button {
  cursor: pointer;
  background-color: #02ff89;
  border: 1px solid #02ff89;
  color: #1f2023; 
  font-weight: bold;
  outline: none;
  border-radius: 6px;
}

h2 {
  font-size: 22px;
  border-bottom: 2px solid rgb(white);
  padding-bottom: 6px;
}

ul li {
  display: flex;
  justify-content: space-between;
  align-items: center;
  border:  2px solid rgb(white, alpha 0.35);
  padding: 12px 24px;
  border-radius: 6px;
  margin-bottom: 12px;
}

span {
  cursor: pointer;
}

.done {
  text-decoration: line-through;
}

button {
  font-size: 12px;
  padding: 6px;
}

h4 {
  text-align: center;
  opacity: 0.5;
  margin: 0;
}
</style>
