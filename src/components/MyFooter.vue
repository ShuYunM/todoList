
<template>
  <div class="todo-footer" v-show="total">
    <label>
      <!-- <input type="checkbox" :checked="isAll" @change="checkAll" /> -->
      <input type="checkbox" v-model="isAll" />
    </label>
    <span>
      <span>已完成{{ todoTotal }}</span> / 全部 {{ total }}
    </span>
    <button class="btn btn-danger" @click="clearAllTodo">清除已完成任务</button>
  </div>
</template>

<script>
export default {
  name: "MyFooter",
  props: ["todos", "checkAllTodo", "clearAllTodo"],
  methods: {
    // checkAll(e) {
    //   this.checkAllTodo(e.target.checked);
    // },
    clearAll() {
      this.clearAllTodo();
    },
  },
  computed: {
    // 所有的任务
    total() {
      return this.todos.length;
    },
    // 读取已完成的任务
    todoTotal() {
      // 方法一：
      // return this.todos.filter((item) => item.done === true).length;
      return this.todos.reduce((pre, current) => {
        return pre + (current.done ? 1 : 0);
        // if (current.done) {
        //   return ++pre;
        // } else {
        //   // 如果为false不执行任何操作 pre + 0，不写这个的话返回undefined
        //   return pre;
        // }
      }, 0);
    },
    // isAll() {
    //   return this.todoTotal === this.total && this.total > 0;
    // },
    isAll: {
      get() {
        return this.todoTotal === this.total && this.total > 0;
      },
      set(value) {
        this.checkAllTodo(value);
      },
    },
  },
};
</script>

<style scoped>
/*footer*/
.todo-footer {
  height: 40px;
  line-height: 40px;
  padding-left: 6px;
  margin-top: 5px;
}

.todo-footer label {
  display: inline-block;
  margin-right: 20px;
  cursor: pointer;
}

.todo-footer label input {
  position: relative;
  top: -1px;
  vertical-align: middle;
  margin-right: 5px;
}

.todo-footer button {
  float: right;
  margin-top: 5px;
}
</style>>

