
<template>
  <li>
    <label>
      <input
        type="checkbox"
        :checked="todo.done"
        @change="handleCheck(todo.id)"
      />
      <span v-show="!todo.isEdit">{{ todo.title }}</span>
      <input
        v-show="todo.isEdit"
        type="text"
        :value="todo.title"
        @blur="handleBlur(todo, $event)"
        ref="Obtain"
      />
    </label>
    <button
      class="btn btn-danger"
      style="display: none"
      @click="handleDelete(todo.id)"
    >
      删除
    </button>

    <button
      v-if="!todo.isEdit"
      class="btn btn-danger"
      @click="handleEdit(todo)"
    >
      编辑
    </button>
  </li>
</template>


<script>
export default {
  name: "MyItem",
  props: ["todo"],
  methods: {
    // 勾选
    handleCheck(id) {
      this.$bus.$emit("checkTodo", id);
    },
    // 删除
    handleDelete(id) {
      if (confirm("确定删除吗")) {
        this.$bus.$emit("deleteTodo", id);
      }
    },
    // 编辑
    handleEdit(todo) {
      // 如果todo身上有isEdit就给他调成true，没有则创建

      if (todo.isEdit in todo) {
        todo.isEdit = true;
      } else {
        this.$set(todo, "isEdit", true);
      }
      //此处也可以使用updated
      this.$nextTick(function () {
        this.$refs.Obtain.focus();
        this.$refs.Obtain.select();
      });
      // 效果一样但不建议使用
      setTimeout(() => {
        this.$refs.Obtain.focus();
        this.$refs.Obtain.select();
      });
    },
    // 离开表单
    handleBlur(todo, e) {
      todo.isEdit = false;
      if (!e.target.value.trim()) {
        return alert("输入不能为空");
      }
      this.$bus.$emit("updataTodo", todo.id, e.target.value);
    },
  },
};
</script>

<style scoped>
/*item*/
li {
  list-style: none;
  height: 36px;
  line-height: 36px;
  padding: 0 5px;
  border-bottom: 1px solid #ddd;
}

li label {
  float: left;
  cursor: pointer;
}

li label li input {
  vertical-align: middle;
  margin-right: 6px;
  position: relative;
  top: -1px;
}

li button {
  float: right;
  display: none;
  margin-top: 3px;
}

li:before {
  content: initial;
}

li:last-child {
  border-bottom: none;
}
li:hover {
  background-color: #ddd;
}
li:hover .btn {
  display: inline-block !important;
}
.btn-danger:last-child {
  background-color: #a1564a;
  margin-right: 5px;
}
</style>