<script setup lang="ts">
import { defineProps } from "vue";
import ToDoItem from "@/ToDoItem.vue";

const props = defineProps({
  toDoList: Array<{ name: string; date: Date; isCompleted: boolean }>,
});

const emit = defineEmits(["update:is-completed", "delete"]);

function updateItemCompletion(index: number, newValue: boolean) {
  emit("update:is-completed", { index, isCompleted: newValue });
}
function deleteToDo(index: number) {
  emit("delete", index);
}
</script>

<template>
  <div>
    <TransitionGroup name="fade" tag="div">
      <ToDoItem
          v-for="(item, index) in toDoList"
          :key="item.name"
          :name="item.name"
          :date="item.date"
          @delete="deleteToDo(index)"
          :isCompleted="item.isCompleted"
          @update:is-completed="(newValue: boolean) => updateItemCompletion(index, newValue)"
      />
    </TransitionGroup>
    <div id="noToDO" v-if="toDoList.length === 0">
      <h1>KEINE TODOS DU NETTER MENSCH</h1>
    </div>
  </div>
</template>

<style scoped>
/* Fade-In und Fade-Out Animation */
.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.3s ease;
}

.fade-enter-from,
.fade-leave-to {
  opacity: 0;
}

.fade-enter-to,
.fade-leave-from {
  opacity: 1;
}

/* Styling für KEINE TODOS Nachricht */
#noToDO {
  display: flex;
  justify-content: center;
  color: #3498db;
}
</style>
