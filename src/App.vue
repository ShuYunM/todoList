
<template>
  <div id="root">
    <div class="todo-container">
      <div class="todo-wrap">
        <MyHeader @addTodo="addTodo"></MyHeader>
        <MyList :todos="todos"></MyList>
        <MyFooter
          :todos="todos"
          @checkedAll="checkAllTodo"
          @clearAllTodo="clearAllTodo"
        ></MyFooter>
      </div>
    </div>
  </div>
</template>

<script>
import MyHeader from "./components/MyHeader.vue";
import MyFooter from "./components/MyFooter.vue";
import MyList from "./components/MyList.vue";

export default {
  name: "App",
  data() {
    return {
      // 判断本地存储中是否有内容，如果为空本地会返回一个null，这是我们添内容是做不到的，因为无法将对象放入null
      todos: JSON.parse(localStorage.getItem("todos")) || [],
    };
  },
  methods: {
    addTodo(value) {
      console.log("我是App组件,我收到了数据", this.todos.unshift(value));
    },
    // 勾选or取消勾选一个todo
    checkTodo(id) {
      this.todos.forEach((todo) => {
        if (todo.id === id) todo.done = !todo.done;
      });
    },
    updataTodo(id, title) {
      this.todos.forEach((todo) => {
        if (todo.id === id) todo.title = title;
      });
    },
    // 删除一个对应id
    deleteTodo(id) {
      // 方法一
      // const index = this.todos.findIndex((itme) => itme.id === id);
      // this.todos.splice(index, 1);
      // 方法二
      console.log(this.todos.filter((item) => item.id !== id));
      this.todos = this.todos.filter((item) => item.id !== id);
    },
    // 全选or取消全选
    checkAllTodo(done) {
      this.todos.forEach((item) => {
        item.done = done;
      });
    },
    // 所有清除选中数据
    clearAllTodo() {
      // 返回所有为false(未选中)的数据
      this.todos = this.todos.filter((item) => !item.done);
    },
  },
  // 监视todos的变化，将最新的放入本地存储
  watch: {
    todos: {
      // 开启深度检测用于点击按钮以后，刷新页面，按钮也是同步状态的
      deep: true,
      handler(value) {
        localStorage.setItem("todos", JSON.stringify(value));
      },
    },
  },
  mounted() {
    this.$bus.$on("checkTodo", this.checkTodo);
    this.$bus.$on("deleteTodo", this.deleteTodo);
    this.$bus.$on("updataTodo", this.updataTodo);
  },
  beforeDestroy() {
    this.$bus.$off("checkTodo");
    this.$bus.$off("deleteTodo");
    this.$bus.$off("updataTodo");
  },
  components: {
    MyHeader,
    MyFooter,
    MyList,
  },
};
</script>

<style>
/*base*/
body {
  background: #fff;
}

.btn {
  display: inline-block;
  padding: 4px 12px;
  margin-bottom: 0;
  font-size: 14px;
  line-height: 20px;
  text-align: center;
  vertical-align: middle;
  cursor: pointer;
  box-shadow: inset 0 1px 0 rgba(255, 255, 255, 0.2),
    0 1px 2px rgba(0, 0, 0, 0.05);
  border-radius: 4px;
}

.btn-danger {
  color: #fff;
  background-color: #da4f49;
  border: 1px solid #bd362f;
}

.btn-danger:hover {
  color: #fff;
  background-color: #bd362f;
}

.btn:focus {
  outline: none;
}

.todo-container {
  width: 600px;
  margin: 0 auto;
}
.todo-container .todo-wrap {
  padding: 10px;
  border: 1px solid #ddd;
  border-radius: 5px;
}
</style>
