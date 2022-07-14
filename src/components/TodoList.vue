<template>
  <div>
    <ul>
      <!-- v-bind:key를 넣어서 리스트의 값이 중복되지 않게 됨(?) -->
      <li
        v-for="(todoItem, index) in todoItems"
        v-bind:key="todoItem.item"
        class="shadow"
      >
        <!-- v-bind:class로 동적인 값을 부여할 수 있다. false면 없어짐. true면 출력. -->
        <font-awesome-icon
          class="check"
          size="lg"
          icon="fa-solid fa-check"
          v-bind:class="{ checkBtnCompleted: todoItem.completed }"
          v-on:click="toggleComplete(todoItem, index)"
        />
        <!-- v-bind:class로 동적인 값을 부여할 수 있다. false면 없어짐. true면 출력. -->
        <span v-bind:class="{ textCompleted: todoItem.completed }">{{
          todoItem.item
        }}</span>
        <span class="removeBtn" v-on:click="removeTodo(todoItem, index)">
          <font-awesome-icon icon="fa-solid fa-trash-can" />
        </span>
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  data: function () {
    return {
      todoItems: [],
    };
  },
  methods: {
    removeTodo: function (todoItem, index) {
      console.log(todoItem, index);
      localStorage.removeItem(todoItem);
      // 배열의 특정 인덱스에서 하나 제거 후 새로운 배열 반환해줌
      this.todoItems.splice(index, 1);
    },
    toggleComplete: function (todoItem) {
      // console.log(todoItem, index);
      todoItem.completed = !todoItem.completed;
      // localStorage에는 update API가 없으므로... 제거하고 다시 저장(갱신).
      localStorage.removeItem(todoItem.item);
      localStorage.setItem(todoItem.item, JSON.stringify(todoItem));
    },
  },

  // Vue 인스턴스가 시작하자 마자 바로 실행되는 라이프사이클 훅
  created: function () {
    // console.log('created');
    if (localStorage.length > 0) {
      for (var i = 0; i < localStorage.length; i++) {
        if (localStorage.key(i) !== 'loglevel:webpack-dev-server') {
          this.todoItems.push(
            JSON.parse(localStorage.getItem(localStorage.key(i)))
          );
        }
        // console.log(localStorage.key(i));
      }
    }
  },
};
</script>

<style scoped>
ul {
  /*점 없애는거 list-style-type: none; */
  list-style-type: none;
  padding-left: 0px;
  margin-top: 0;
  text-align: left;
}

li {
  display: flex;
  align-items: center;
  min-height: 50px;
  height: 50px;
  line-height: 50px;
  margin: 0.5rem 0;
  padding: 0 0.9rem;
  background: white;
  border-radius: 5px;
}

.check {
  color: #5d9ed2;
  margin-right: 0.5rem;
}

.checkBtn {
  line-height: 45px;
  color: #62acde;
  margin-right: 5px;
}

.removeBtn {
  margin-left: auto;
  color: #de4343;
}
.checkBtnCompleted {
  color: #b3adad;
}

.textCompleted {
  text-decoration: line-through;
  color: #b3adad;
}
</style>
