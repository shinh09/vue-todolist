<script setup>
import { ref, watch } from 'vue';

const props = defineProps({
  item: Object,
});

const emit = defineEmits(['close', 'save']);

const editMsg = ref('');

watch(
  () => props.item,
  (newVal) => {
    if (newVal) editMsg.value = newVal.msg;
  },
  { immediate: true }
);

const save = () => {
  if (!editMsg.value.trim()) return;

  emit('save', {
    ...props.item,
    msg: editMsg.value.trim(),
  });
};
</script>

<template>
  <teleport to="body">
    <div class="modal-overlay">
      <div class="modal">
        <h3>수정하기</h3>

        <input v-model="editMsg" />

        <div class="modal-actions">
          <button class="modal-cancel-btn" @click="$emit('close')">취소</button>

          <button class="modal-save-btn" @click="save">저장</button>
        </div>
      </div>
    </div>
  </teleport>
</template>
