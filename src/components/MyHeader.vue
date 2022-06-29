<template>
  <div class="todo-header">
    <input
      type="text"
      placeholder="请输入你的任务名称，按回车键确认"
      @keyup.enter="add"
      v-model.trim="title"
    />
  </div>
</template>

<script>
// 随机生成一个id
import { nanoid } from "nanoid";
export default {
  name: "MyHeader",
  data() {
    return {
      title: "",
    };
  },
  methods: {
    add(e) {
      // 什么也不输入时弹出对话框
      if (!this.title.trim()) {
        alert("不能为空！");
        return;
      }
      // 获取用户的输入，包装为todo对象
      console.log(e.target.value);
      const tood = { id: nanoid(), title: this.title, done: false };
      this.$emit("addTodo", tood);
      this.title = "";
    },
  },
};
</script>

<style>
/*header*/
.todo-header input {
  width: 560px;
  height: 28px;
  font-size: 14px;
  border: 1px solid #ccc;
  border-radius: 4px;
  padding: 4px 7px;
}

.todo-header input:focus {
  outline: none;
  border-color: rgba(82, 168, 236, 0.8);
  box-shadow: inset 0 1px 1px rgba(0, 0, 0, 0.075),
    0 0 8px rgba(82, 168, 236, 0.6);
}
</style>