<template>
  <div id="app">
    <div class="page-top">
      <div class="page-content">
        <h2>任务计划列表</h2>
      </div>
    </div>
    <div class="main">
      <h3 class="big-title">添加任务</h3>
      <input v-model="newItem" placeholder="" class="task-input" type="text" v-on:keyup.enter="addTodo" />
      <!--渲染数据-->
      <h3 class="big-title">任务列表</h3>
      <div class="tasks">
        <ul class="todo-list">
          <li class="todo " :class="{completed:item.isChecked,editing:item===edtorTodos}" v-for="item in items">
            <div class="view">
              <input class="toggle" type="checkbox" v-model="item.isChecked" />
              <label @dblclick="edtorTodo(item)">{{item.title}}</label>
              <button class="destroy" @click="deleteTodo(item)"></button>
            </div>
            <input  class="edit" type="text" v-model="item.title" @blur="edtorTodoed(item)" @keyup.enter="edtorTodoed(item)" @keyup.esc="cancelTodo(item)" />
          </li>
        </ul>
      </div>
    </div>
  </div>
</template>

<script>
  import Store from "./store";
  export default {
    data: function() {
      return {
        items: Store.fetch(),
        newItem: "",
        edtorTodos: '', //记录正在编辑的数据
        beforeTitle: '', //记录一下正在编辑的数据的title
      };
    },
    watch: {
      items: {
        handler: function(items) {
          Store.save(items);
        },
        deep: true
      }
    },
    methods: {
      toggleFinish: function(item) {
        console.log((item.isChecked = !item.isChecked));
      },
      addTodo: function() {
        this.items.push({
          title: this.newItem,
          isChecked: false
        });
        this.newItem = "";
      },
      deleteTodo: function(todo) {
        var index = this.items.indexOf(todo);
        this.items.splice(index, 1);
      },
      edtorTodo: function(todo) { //编辑任务
        this.edtorTodos = todo; //用数据记录一下现在正在编辑的信息,
        // 编辑任务的时候，记录一项编辑这条任务的title，方便取消编辑的是好重新给之前的title
        this.beforeTitle = todo.title;
      },
      edtorTodoed: function(todo) { //编辑任务成功
        this.edtorTodos = "";
      },
      cancelTodo: function(todo) { //取消编辑任务
        todo.title = this.beforeTitle;
        this.beforeTitle = '';
        //让div显示出来，input框隐藏，可以选择将正在编辑的信息置为空
        this.edtorTodos = '';
      }
    }
  };
</script>

<style>
  body {
    margin: 0;
    background-color: #fafafa;
    font: 14px 'Helvetica Neue', Helvetica, Arial, sans-serif;
  }
  h2 {
    margin: 0;
    font-size: 12px;
  }
  a {
    color: #000;
    text-decoration: none;
  }
  input {
    outline: 0;
  }
  button {
    margin: 0;
    padding: 0;
    border: 0;
    background: none;
    font-size: 100%;
    vertical-align: baseline;
    font-family: inherit;
    font-weight: inherit;
    color: inherit;
    outline: 0;
  }
  ul {
    padding: 0;
    margin: 0;
    list-style: none;
  }
  .page-top {
    width: 100%;
    height: 40px;
    background-color: #db4c3f;
  }
  .page-content {
    width: 50%;
    margin: 0px auto;
  }
  .page-content h2 {
    line-height: 40px;
    font-size: 18px;
    color: #fff;
  }
  .main {
    width: 50%;
    margin: 0px auto;
    box-sizing: border-box;
  }
  .task-input {
    width: 99%;
    height: 30px;
    outline: 0;
    border: 1px solid #ccc;
  }
  .task-count {
    display: flex;
    margin: 10px 0;
  }
  .task-count li {
    padding-left: 10px;
    flex: 1;
    color: #dd4b39;
  }
  .task-count li:nth-child(1) {
    padding: 5px 0 0 10px;
  }
  .action {
    text-align: center;
    display: flex;
  }
  .action a {
    margin: 0px 10px;
    flex: 1;
    padding: 5px 0;
    color: #777;
  }
  .action a:nth-child(3) {
    margin-right: 0;
  }
  .active {
    border: 1px solid rgba(175, 47, 47, 0.2);
  }
  .tasks {
    background-color: #fff;
  }
  .no-task-tip {
    padding: 10px 0 10px 10px;
    display: block;
    border-bottom: 1px solid #ededed;
    color: #777;
  }
  .big-title {
    color: #222;
  }
  .todo-list {
    margin: 0;
    padding: 0;
    list-style: none;
  }
  .todo-list li {
    position: relative;
    font-size: 16px;
    border-bottom: 1px solid #ededed;
  }
  .todo-list li:hover {
    background-color: #fafafa;
  }
  .todo-list li.editing {
    border-bottom: none;
    padding: 0;
  }
  .todo-list li.editing .edit {
    display: block;
    width: 506px;
    padding: 13px 17px 12px 17px;
    margin: 0 0 0 43px;
  }
  .todo-list li.editing .view {
    display: none;
  }
  .todo-list li .toggle {
    text-align: center;
    width: 40px;
    /* auto, since non-WebKit browsers doesn't support input styling */
    height: auto;
    position: absolute;
    top: 5px;
    bottom: 0;
    margin: auto 0;
    border: none;
    /* Mobile Safari */
    -webkit-appearance: none;
  }
  .toggle {
    text-align: center;
    width: 40px;
    /* auto, since non-WebKit browsers doesn't support input styling */
    height: auto;
    position: absolute;
    top: 5px;
    bottom: 0;
    margin: auto 0;
    border: none;
    /* Mobile Safari */
    -webkit-appearance: none;
  }
  .toggle:after {
    content: url('data:image/svg+xml;utf8,<svg xmlns="http://www.w3.org/2000/svg" width="40" height="40" viewBox="-10 -18 100 135"><circle cx="50" cy="50" r="40" fill="none" stroke="#ededed" stroke-width="3"/></svg>');
  }
  .toggle:checked:after {
    content: url('data:image/svg+xml;utf8,<svg xmlns="http://www.w3.org/2000/svg" width="40" height="40" viewBox="-10 -18 100 135"><circle cx="50" cy="50" r="40" fill="none" stroke="#bddad5" stroke-width="3"/><path fill="#5dc2af" d="M72 25L42 71 27 56l-4 4 20 20 34-52z"/></svg>');
  }
  .todo-list li label {
    white-space: pre-line;
    word-break: break-all;
    padding: 15px 60px 15px 15px;
    margin-left: 45px;
    display: block;
    line-height: 1.2;
    transition: color 0.4s;
  }
  .todo-list li.completed label {
    color: #d9d9d9;
    text-decoration: line-through;
  }
  /*.tip-toggle {
                      padding-left: 0;
                      position: relative;
                  }
                  .tip-toggle .toggle {
                      top: -2px;
                  }
                  .tip-toggle span {
                      margin-left: 45px;
                  }*/
  .todo-list li .destroy {
    display: none;
    position: absolute;
    top: 0;
    right: 10px;
    bottom: 0;
    width: 40px;
    height: 40px;
    margin: auto 0;
    font-size: 30px;
    color: #cc9a9a;
    margin-bottom: 11px;
    transition: color 0.2s ease-out;
  }
  .todo-list li .destroy:hover {
    color: #af5b5e;
  }
  .todo-list li .destroy:after {
    content: '×';
  }
  .todo-list li:hover .destroy {
    display: block;
  }
  .todo-list li .edit {
    display: none;
  }
  .todo-list li.editing:last-child {
    margin-bottom: -1px;
  }
  #app {
    font-family: "Avenir", Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: left;
    color: #2c3e50;
    margin-top: 60px;
  }
</style>