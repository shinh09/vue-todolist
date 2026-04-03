<script setup>
// Vue의 ref를 가져온다.
import { ref } from 'vue';

// 부모로 보낼 이벤트를 선언한다.
const emit = defineEmits(['add-todo']);

// inputMsg는 사용자가 입력하는 텍스트를 저장하는 반응형 변수이다.
const inputMsg = ref('');

// addTodo는 현재 입력값을 부모에게 전달하고 입력창을 초기화한다.
const addTodo = () => {
  const value = inputMsg.value;

  if (!value || !value.trim()) return; //중복방지

  emit('add-todo', value.trim());

  inputMsg.value = '';
};
</script>

<template>
  <div class="todo-input-wrap">
    <!-- keydown -> keyup : input 두번 렌더링 문제 해결 -->
    <input
      v-model="inputMsg"
      type="text"
      class="todo-input"
      placeholder="할 일을 입력하세요."
      @keyup.enter="addTodo"
    />

    <button class="todo-add-btn" type="button" @click="addTodo">등록</button>
  </div>
</template>
