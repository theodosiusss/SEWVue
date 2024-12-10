<script setup lang="ts">
import {defineProps} from 'vue';
import ToDoItem from "@/ToDoItem.vue";

const props = defineProps({
  toDoList: Array<{ name: string; date: Date; isCompleted: boolean }>
});

const emit = defineEmits(["update:is-completed",'delete']);

function updateItemCompletion(index: number, newValue: boolean) {
  emit("update:is-completed", { index, isCompleted: newValue });
}
function deleteToDo(index:number){
  emit('delete',index);
}


</script>

<template>
  <div v-if="toDoList.length > 0">
    <ToDoItem v-for="(item,index) in toDoList" :key="index" :name="item.name" :date="item.date" @delete="deleteToDo(index)"
              :isCompleted="item.isCompleted" @update:is-completed="(newValue : boolean) => updateItemCompletion(index, newValue)"></ToDoItem>
  </div>
  <div id="noToDO" v-else>
    <h1>KEINE TODOS DU NETTER MENSCH </h1>
  </div>
</template>

<style scoped>

#noToDO{
  display: flex;
  justify-content: center;
  color: #3498db;

}

</style>