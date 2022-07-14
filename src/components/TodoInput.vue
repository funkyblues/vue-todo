<template>
  <div>
    <!-- v-model은 two-way binding -->
    <input type="text" v-model="newTodoItem" />
    <button v-on:click="addTodo">add</button>
  </div>
</template>

<script>
export default {
  data: function () {
    return {
      newTodoItem: '',
    };
  },
  methods: {
    addTodo: function () {
      // console.log(this.newTodoItem);
      // 저장하는 로직 후, input 내용을 지우면 된다.
      localStorage.setItem(this.newTodoItem, this.newTodoItem);
      this.newTodoItem = '';
    },
  },
};
</script>

<style></style>
