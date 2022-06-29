
<template>
  <div class="todo-footer" v-show="total">
    <label>
      <!-- <input type="checkbox" :checked="isAll" @change="checkAll" /> -->
      <!--
         读取时：通过对数组长度和已完成的状态做出判断(只要单个任务复选框发生变化就会读取)，来决定是否勾选当前总的复选框
         修改时：v-model 当内容修改时调用计算属性，通过函数遍历所有数据让其checked(todos.done)的状态等于当前按钮的状态 
      -->
      <input type="checkbox" v-model="isAll" />
    </label>
    <span>
      <span>已完成{{ todoTotal }}</span> / 全部 {{ total }}
    </span>

    <button class="btn btn-danger" @click="clearAll">清除已完成任务</button>
  </div>
</template>

<script>
export default {
  name: "MyFooter",
  props: ["todos"],
  methods: {
    // 通过点击得到按钮当前的状态，调用函数返回给父组件，父组件遍历
    // checkAll(e) {
    //   this.checkAllTodo(e.target.checked);
    // },
    clearAll() {
      this.$emit("clearAllTodo");
    },
  },
  computed: {
    // 所有的任务
    total() {
      return this.todos.length;
    },
    // 对已完成的做判断，判断为done为true的个数
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
        this.$emit("checkAllTodo", value);
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

