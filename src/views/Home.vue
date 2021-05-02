<template>
  <div id="app">
    <p class="tittle">Todo List</p>
    <div class="todo">
      <input type="text" name="todo" id="todo" v-model="newTodo" />
      <button @click="insertTodo" id="add">追加</button>
    </div>
    <div class="list" v-for="item in Todos" :key="item.id">
      <input type="text" v-model="item.todo" />
      <div class="lbtn">
        <button @click="updateTodo(item.id, item.todo)" id="update">
          更新
        </button>
        <button @click="deleteTodo(item.id)" id="delete">
          削除
        </button>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
export default {
  data() {
    return {
      newTodo: "",
      Todos: [],
    };
  },
  methods: {
    async getTodo() {
      const resData = await axios.get("https://rocky-harbor-51355.herokuapp.com/api/todo");
      this.Todos = resData.data.data;
    },
    async insertTodo() {
      const sendData = {
        todo: this.newTodo,
      };
      await axios.post("https://rocky-harbor-51355.herokuapp.com/api/todo", sendData);
      await this.getTodo();
      this.newTodo = "";
    },
    async updateTodo(id, todo) {
      const sendData = {
        todo: todo,
      };
      await axios.put("https://rocky-harbor-51355.herokuapp.com/api/todo/" + id, sendData);
      await this.getTodo();
    },
    async deleteTodo(id) {
      await axios.delete("https://rocky-harbor-51355.herokuapp.com/api/todo/" + id);
      await this.getTodo();
    },
  },
  created() {
    this.getTodo();
  },
};
</script>

<style>
html {
    background-color: #2d197c;
    height: 100vh;
    width: 100vw;
    position: relative;
}

#app {
    background-color: #fff;
    width: 50vw;
    padding: 30px;
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    border-radius: 10px;
}

input {
    width: 80%;
    padding: 10px;
    border-radius: 5px;
    border: 1px solid #ccc;
    -webkit-appearance: none;
    -moz-appearance: none;
    appearance: none;
    font-size: 14px;
    outline: none;
    text-align: left;
}

button {
    text-align: center;
    border: 2px solid;
    font-size: 12px;
    background-color: #fff;
    font-weight: bold;
    padding: 8px 16px;
    border-radius: 5px;
    cursor: pointer;
    outline: none;
    
}

#add {
    border-color: #dc70fa;
    color: #dc70fa;
}

#add:hover {
    background-color: #dc70fa;
    color: #fff;
    transition: .4s;
}

#update {
    border-color: #fa9770;
    color: #fa9770;
    margin-left: 5px;
}

#update:hover {
    background-color: #fa9770;
    color: #fff;
    transition: .4s;
}

#delete {
    border-color: #71fadc;
    color: #71fadc;
    margin-left: 5px;
}

#delete:hover {
    background-color: #71fadc;
    color: #fff;
    transition: .4s;
}

table {
  position: relative;
  right: 3px;
}

.tittle {
  font-weight: bold;
  font-size: 24px;
  margin: 0 0 10px 0;
}

.todo {
  display: flex;
  justify-content: space-between;
  margin: 15px 0;
}

.list {
  display: flex;
  margin: 5px 0;
}

.list .lbtn {
  margin: 0 0 0 auto;
}

.list input {
  width: 30%;
}




</style>