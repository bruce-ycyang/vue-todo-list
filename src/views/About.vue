<template>
  <div class="about">
    <h1>This is an about page</h1>
    <div class="test">
      {{ msg }}
    </div>
    <div class="try try">
      {{ this.encrypt("test101abcdp108A") }}
    </div>
    <div class="try try1">
      {{ this.decrypt("|m{|989ijklx98@I") }}
    </div>
    <h2>what's your score</h2>
    <input type="text" v-model="score" />
    <div v-if="score >= 60">pass exam</div>
    <div v-if="score < 60">fail exam</div>
    <ul>
      <li v-for="item in colors" :key="item">
        {{ item }}
      </li>
    </ul>
    <input
      type="text"
      placeholder="what you need to do"
      @keyup.enter="addTodo(newTodo)"
      v-model="newTodo"
    />
    <h2>todo list</h2>
    <ul>
      <li v-for="todo in todos" :key="todo">
        {{ todo.content }}
        - <a href="#" @click.prevent="removeTodo(todo)">delete</a>
      </li>
    </ul>
    <h1>Joke App</h1>
    <button @click="showJoke">show Joke</button>
    <div v-if="joke">
      <p>{{ joke }}</p>
    </div>
  </div>
</template>

<script>
import axios from "axios";
export default {
  data() {
    return {
      msg: "A".charCodeAt(0),
      messsage: [],
      score: 0,
      loading: false,
      colors: ["red", "blue", "black"],
      todos: [],
      newTodo: "",
      joke: "",
    };
  },
  methods: {
    addTodo: function (todo) {
      this.todos.push({ content: todo, completed: false });
    },
    removeTodo: function (todo) {
      this.todos.splice(this.todos.indexOf(todo), 1);
    },
    async showJoke() {
      let config = {
        headers: {
          Accept: "application/json",
        },
      };
      const joke = await axios.get("https://icanhazdadjoke.com", config);
      this.joke = joke.data.joke;
    },
    encrypt(value) {
      const str = value;
      let output = "";
      let store = [];
      for (let i = 0; i < str.length; i++) {
        let ascii = str[i].charCodeAt(0) + 8;
        store.push(ascii);
      }
      store.forEach((item) => {
        output += String.fromCharCode(item);
      });
      return output;
    },
    decrypt(value) {
      const str = value;
      let output = "";
      let store = [];
      for (let i = 0; i < str.length; i++) {
        let ascii = str[i].charCodeAt(0) - 8;
        store.push(ascii);
      }
      store.forEach((item) => {
        output += String.fromCharCode(item);
      });
      return output;
    },
  },
};
</script>
