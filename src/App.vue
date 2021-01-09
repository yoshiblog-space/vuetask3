<template>
  <div id="app">
    <h1>ToDoリスト</h1>
    <div class="status">
      <input type="radio" name="state" checked="checked">すべて
      <input type="radio" name="state">作業中
      <input type="radio" name="state">完了
    </div>

    <table>
      <thead>
        <tr>
          <th>ID</th>
          <th>コメント</th>
          <th>状態</th>
        </tr>
      </thead>
      <tbody>
        <tr  v-for="todo in todoList" v-bind:key="todo.todoId">
          <td>{{ todo.todoId }}</td>
          <td>{{ todo.todoComment }}</td>
          <td><button type="button"  @click="changeTodoState( todo.todoState, todo.todoId )">{{ todo.todoState }}</button></td>
          <td><button type="button" @click="delTodoList( todo.todoId )">削除</button></td>
        </tr>

      </tbody>
    </table>

    <h2>新規タスクの追加</h2>
    <input v-model="todoInput">
    <button @click="addTodoList" type="button">追加</button>
  </div>
</template>

<script>
export default {
  name: "App",
  data () {
    return {
      todoInput: '',
      todoList: [],
      todoIdCount: 0,
      todoStateDefault: '作業中',
    }
  },
  methods:{
    addTodoList: function(){
      if( !this.todoInput ){
        return;
      }
      this.todoList.push({
        todoId: this.todoIdCount++,
        todoComment: this.todoInput,
        todoState:this.todoStateDefault,
      });
      this.todoInput = '';
    },
    delTodoList: function( delTodoId ){
      this.$delete( this.todoList,delTodoId );
      this.todoList.forEach(( element,key ) => {
        element.todoId = key;
      });
      this.todoIdCount = this.todoList.length;
      },
    changeTodoState: function( todoStatus,todoId ){
      if( todoStatus === '作業中' ){
        this.todoList[ todoId ].todoState = '完了';
      }else{
        this.todoList[ todoId ].todoState = '作業中';
      }
    }
  }
}
</script>

<style>
  body{
    font-family:'Times New Roman', Times, serif
  }
</style>