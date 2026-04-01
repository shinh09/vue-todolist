<!-- 
options API ->> Composition API (setup()) 활용해보기
1. setup()추가하기 
2. components: , props: 는 아직 그대로
2. data를 setup안에 넣고 ref로 바꾸기
3. methods ->> 각각 변수화? 
  - const addTodo = (inputMsg) => {} 이런식으로 바꾼다
  - computed같은 경우도 const a = computed(()=>{}) 이런식ㅇ로..!!
4.
-->

<script>
import { ref, computed } from 'vue';
import TodoHeader from '@/components/TodoHeader.vue';
import TodoList from '@/components/TodoList.vue';
import TodoInput from '@/components/TodoInput.vue';

export default {
  components: {
    TodoHeader,
    TodoList,
    TodoInput,
  },
  setup() {
    //데이터
    const todo = ref([]);
    const current = ref('all');
    //함수
    const addTodo = (inputMsg) => {
      const item = {
        id: Math.random(),
        msg: inputMsg,
        completed: false,
      };
      todo.value.push(item);
    };
    const updateTab = (tab) => {
      current.value = tab;
    };
    const deleteTodo = (id) => {
      todo.value = todo.value.filter((v) => v.id !== id);
    };
    const updateTodo = (id) => {
      todo.value = todo.value.map((v) =>
        v.id === id ? { ...v, completed: !v.completed } : v
      );
    };
    //computed 함수
    const computedTodo = computed(() => {
      if (current.value === 'all') {
        return todo.value;
      } else {
        return todo.value.filter((v) => v.completed);
      }
    });

    return {
      todo,
      current,
      addTodo,
      updateTab,
      deleteTodo,
      updateTodo,
      computedTodo,
    };
  },
};
</script>
<template>
  <div class="todo">
    <TodoHeader :current @update-tab="updateTab" />
    <TodoList
      :computed-todo="computedTodo"
      @delete-todo="deleteTodo"
      @update-todo="updateTodo"
    />
    <TodoInput @add-todo="addTodo" />
  </div>
</template>
