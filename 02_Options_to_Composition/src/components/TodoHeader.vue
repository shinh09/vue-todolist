<!-- 
 props와 emit이 존재하는경우에는 setup(props,{emit}) 의 인자값으로 넣어준다. -->

<script>
export default {
  props: {
    current: {
      type: String,
      default: 'all',
    },
  },
  setup(props, { emit }) {
    const updateTab = (tab) => {
      emit('update-tab', tab);
    };
    return { updateTab, current: props.current }; //props.current를 current로 정의
  },
};
</script>

<template>
  <div class="todo__title">
    <h1 class="todo__text">Todo List</h1>
    <ul class="todo__tab">
      <li
        :class="{ 'todo__tab--active': current === 'all' }"
        @click="updateTab('all')"
      >
        전체
      </li>
      <li
        :class="{ 'todo__tab--active': current === 'completed' }"
        @click="updateTab('completed')"
      >
        완료
      </li>
    </ul>
  </div>
</template>
