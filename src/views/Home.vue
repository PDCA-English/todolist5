<template>
  <div id="app">
    <h1>Todo List</h1>
      <div class="todo">
        <label for="todo"></label>
        <input type="text" name="todo" id="todo" v-model="newTodo" />
      </div>
      <button @click="insertTodo">追加</button>
    <div class="table">
      <table>
        <tr v-for="item in Todos" :key="item.id">
          <td><input type="text" v-model="item.todo" /></td>
          <td>
            <button @click="updateTodo(item.id, item.todo)">
              更新
            </button>
          </td>
          <td>
            <button @click="deleteTodo(item.id)">削除</button>
          </td>
        </tr>
      </table>
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
      const resData = await axios.get("http://127.0.0.1:8000/api/todo");
      this.Todos = resData.data.data;
    },
    async insertTodo() {
      const sendData = {
        todo: this.newTodo,
      };
      await axios.post("http://127.0.0.1:8000/api/todo", sendData);
      await this.getTodo();
      this.newTodo = "";
    },
    async updateTodo(id, todo) {
      const sendData = {
        todo: todo,
      };
      await axios.put("http://127.0.0.1:8000/api/todo/" + id, sendData);
      await this.getTodo();
    },
    async deleteTodo(id) {
      await axios.delete("http://127.0.0.1:8000/api/todo/" + id);
      await this.getTodo();
    },
  },
  created() {
    this.getTodo();
  },
};
</script>

<style>
</style>