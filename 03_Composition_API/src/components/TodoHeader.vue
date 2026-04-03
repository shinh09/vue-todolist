<script setup>
// 부모 컴포넌트에서 전달받는 props를 선언한다.
const props = defineProps({
  current: {
    type: String,
    default: 'all',
  },
});

// 부모로 전달할 수 있는 사용자 정의 이벤트를 선언한다.
const emit = defineEmits(['update-tab']);
// updateTab 함수는 클릭한 탭 값을 부모에게 전달한다.
const updateTab = (tab) => {
  emit('update-tab', tab);
};
</script>

<template>
  <header class="todo-header">
    <h1 class="todo-title">Todo List</h1>

    <p class="todo-description">
      Composition API + localStorage 저장 기능이 적용된 Todo 앱
    </p>

    <ul class="todo-tab">
      <li :class="{ active: current === 'all' }" @click="updateTab('all')">
        전체
      </li>

      <li
        :class="{ active: current === 'completed' }"
        @click="updateTab('completed')"
      >
        완료
      </li>
    </ul>
  </header>
</template>
