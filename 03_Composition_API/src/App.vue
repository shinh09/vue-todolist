<!--
  [App.vue 처리 단계 설명]

  1단계. TodoHeader, TodoList, TodoInput 컴포넌트를 import 한다.
  2단계. 할 일 목록(todo)과 현재 필터 탭(current)을 ref로 반응형 상태로 만든다.
  3단계. 앱이 처음 열릴 때 localStorage에 저장된 todo 데이터를 읽어와 복원한다.
  - setup에 정의하기
  4단계. computed를 사용해 현재 탭에 맞는 화면 출력용 목록(filteredTodo)을 계산한다.
  5단계. addTodo(), deleteTodo(), toggleTodo(), clearCompleted(), updateTab() 함수를 만든다.
  6단계. watch를 사용해 todo 목록이 바뀔 때마다 localStorage에 자동 저장한다.
  7단계. props와 emit으로 자식 컴포넌트와 연결해 전체 Todo 앱이 동작하도록 구성한다.
-->

<script setup>
// Vue의 반응형 상태(ref), 계산된 상태(computed), 상태 감시(watch), 최초 실행 시점(onMounted)을 가져온다.
import { ref, computed, watch, onMounted } from 'vue';
// 상단 제목/탭 컴포넌트를 가져온다.
import TodoHeader from './components/TodoHeader.vue';
// 목록 출력 컴포넌트를 가져온다.
import TodoList from './components/TodoList.vue';
// 입력창 컴포넌트를 가져온다.
import TodoInput from './components/TodoInput.vue';
// localStorage에 사용할 키 이름을 상수로 정의한다.
const STORAGE_KEY = 'vue-todo';

// todo는 전체 할 일 목록을 저장하는 반응형 배열이다.
const todo = ref([]);

// current는 현재 선택된 탭 상태를 저장한다.
// all = 전체 보기, completed = 완료만 보기
const current = ref('all');

// filteredTodo는 current 값에 따라 실제 화면에 보여줄 목록을 계산한다.
const computedTodo = computed(() => {
  if (current.value === 'all') {
    return todo.value;
  } else {
    return todo.value.filter((v) => v.completed);
  }
});

// completedCount는 완료된 할 일 개수를 계산한다.
const completedCount = computed(() => {
  return todo.value.filter((v) => v.completed).length;
});

// remainingCount는 아직 완료되지 않은 할 일 개수를 계산한다.
const remainingCount = computed(() => {
  return todo.value.filter((v) => !v.completed).length;
});

// addTodo는 입력받은 문자열을 새 할 일 객체로 만들어 목록에 추가한다.
const addTodo = (inputMsg) => {
  const item = {
    id: Math.random(),
    msg: inputMsg.trim(), //공백처리
    completed: false,
  };
  todo.value.push(item);
};

// 새 할 일 객체를 생성한다.

// updateTab은 상단 탭에서 전달받은 값을 current에 저장한다.
const updateTab = (tab) => {
  current.value = tab;
};
// deleteTodo는 전달받은 id와 일치하지 않는 항목만 남겨 삭제를 처리한다.

const deleteTodo = (id) => {
  todo.value = todo.value.filter((v) => v.id !== id);
};
// toggleTodo는 전달받은 id와 일치하는 항목의 completed 값을 true/false로 뒤집는다.
const toggleTodo = (id) => {
  const target = todo.value.find((v) => v.id === id);
  if (target) target.completed = !target.completed;
};
// clearCompleted는 완료된 항목을 한 번에 제거한다.
const clearCompleted = () => {
  todo.value = todo.value.filter((v) => !v.completed);
};

// loadTodos는 localStorage에서 저장된 목록을 읽어 todo 상태에 복원한다.
const loadTodos = () => {
  const data = localStorage.getItem(STORAGE_KEY);

  if (data) {
    todo.value = JSON.parse(data);
  }
};
// 컴포넌트가 화면에 마운트되면 저장된 할 일 목록을 먼저 불러온다.
onMounted(() => {
  loadTodos();
});
//todo의 변화를 감시하고 최신 todo를 브라우저에 저장하기 위해서 watch 사용
//

watch(
  todo,
  (newValue) => {
    localStorage.setItem(STORAGE_KEY, JSON.stringify(newValue));
  },
  { deep: true }
);
// todo 값이 바뀔 때마다 localStorage에 최신 상태를 문자열로 저장한다.
</script>

<template>
  <div class="todo-app">
    <!-- 상단 제목과 탭 UI를 출력한다. -->
    <TodoHeader :current="current" @update-tab="updateTab" />

    <!-- 입력창과 등록 버튼 UI를 출력한다. -->
    <TodoInput @add-todo="addTodo" />

    <!-- 할 일 통계 정보를 간단히 보여준다. -->
    <div class="todo-summary">
      <p>전체 {{ todo.length }}개</p>
      <p>완료 {{ completedCount }}개</p>
      <p>남은 일 {{ remainingCount }}개</p>
    </div>

    <!-- 현재 탭에 맞는 목록을 출력한다. -->
    <TodoList
      :items="computedTodo"
      @delete-todo="deleteTodo"
      @toggle-todo="toggleTodo"
    />

    <!-- 완료된 항목이 1개 이상 있을 때만 일괄 삭제 버튼을 보여준다. -->
    <div class="todo-actions">
      <button class="todo-clear-btn" @click="clearCompleted">
        완료 항목 전체 삭제
      </button>
    </div>
  </div>
</template>
