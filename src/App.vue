<script setup lang="ts">
import {onBeforeMount, onMounted, onUnmounted, type Ref, ref} from "vue";
import ToDoList from "@/ToDoList.vue";
import ToDoAdder from "@/ToDoAdder.vue";

const text = ref("")


let todoliste: Ref<{ name: string, date: Date, isCompleted: boolean }[]> = ref([]);

function updateToDoItem({index, isCompleted}: { index: number; isCompleted: boolean }) {
  todoliste.value[index].isCompleted = isCompleted;
}

function createToDo(name: string) {
  todoliste.value.push({name: name, date: new Date(), isCompleted: false});
  localStorage.setItem("TODOS",JSON.stringify([]));
  localStorage.setItem("TODOS", JSON.stringify(todoliste.value));
}


const showPopup = ref(false);

onBeforeMount(() => {
  const storage = localStorage.getItem("TODOS");
  if (storage != null) {
    todoliste.value = JSON.parse(storage, (key, value) => {
      if (key === "date") {
        return new Date(value);
      }

      return value;
    });

  }

})
function deleteToDo(index:number) {
  console.log(index)
  todoliste.value.splice(index, 1)
  localStorage.setItem("TODOS",JSON.stringify([]));
  localStorage.setItem("TODOS", JSON.stringify(todoliste.value));
}
function handleKeydown(event: KeyboardEvent) {
  if (event.key === "Enter") {
    setTimeout(()=> showPopup.value = true,1)

  }
}

onMounted(() => {
  window.addEventListener("keydown", handleKeydown);
});

onUnmounted(() => {
  window.removeEventListener("keydown", handleKeydown);
});

</script>

<template>
  <div>
    <h1>ToDo App</h1>
    <button  @click="showPopup = true">neues To Do</button>

    <ToDoAdder
        v-if="showPopup"
        title="Willkommen!"
        message="Das ist ein einfaches Popup-Beispiel."
        @close="showPopup = false"
        @name="createToDo"
    />
  </div>

  <ToDoList :to-do-list=todoliste @update:is-completed="updateToDoItem" @delete="deleteToDo"></ToDoList>
</template>
  <style scoped>
  html {
    background-color: #1e1e1e;
  }
  body {

    font-family: Arial, sans-serif;
    background-color: #1e1e1e;
    margin: 0;
    padding: 0;
    box-sizing: border-box;
  }

  h1 {

    text-align: center;
    color: #3498db;
    margin: 20px 0;
  }

  /* Container für die ToDo-App */
  div {
    max-width: 1920px;
    margin: 0 auto;
    background: #1e1e1e;
    padding: 20px;
    }

    /* Buttons */
    button {
      background-color: #3498db;
      color: white;
      border: none;
      border-radius: 4px;
      padding: 8px 12px;
      font-size: 14px;
      cursor: pointer;
      transition: background-color 0.3s ease;
    }

    button:hover {
      background-color: darkcyan;
    }

    button:focus {
      outline: none;
    }

  </style>
