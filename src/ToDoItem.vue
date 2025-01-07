<script setup lang="ts">
const props = defineProps({
  name : String,
  date : Date,
  isCompleted: Boolean
})


function timePassed(date : Date | undefined ) {
  if(date) {
    const currentDate = new Date();
    const timeDifference = currentDate.getTime() - date.getTime();

    const msInMinute = 60 * 1000;
    const msInHour = 60 * msInMinute;
    const msInDay = 24 * msInHour;

    const days = Math.floor(timeDifference / msInDay);
    const hours = Math.floor((timeDifference % msInDay) / msInHour);
    const minutes = Math.floor((timeDifference % msInHour) / msInMinute);

    return "Age: " + days + " days, " + hours + " hours and " + minutes + " minutes";
  }
}

const emit = defineEmits(['update:isCompleted','delete']);
function toggleMarkAsDone() {

  emit('update:isCompleted',!props.isCompleted);
}
function deleteToDo(){
  emit('delete')
}

</script>

<template>

  <div id="Task">
    <div id="TaskInput">
      <div style="display: flex; justify-content: space-between">
        <h1>{{name}}</h1>
        <button id="delete" @click="deleteToDo">delete</button>
      </div>
      <hr>
      <p>{{timePassed(date)}}</p>
      <p>Completed: {{isCompleted}}</p>
      <button @click=" toggleMarkAsDone " v-if="!isCompleted">  mark as done</button>
      <button @click=" toggleMarkAsDone " v-else> mark as undone</button>
    </div>
  </div>
</template>

<style scoped>
/* Gesamtlayout für die Aufgabe */
#Task {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 20px;
  margin: 10px 0;
  background-color: #2c2c2c;
  border: 1px solid #ddd;
  border-radius: 8px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

/* Bereich für den Inhalt der Aufgabe */
#TaskInput {
  width: 100%;
}

/* Titel der Aufgabe */
#TaskInput h1 {
  font-size: 20px;
  margin: 0;
  padding: 0;
  color: white;

}

/* Trennlinie */
#TaskInput hr {
  color: white;

  border: none;
  border-top: 1px solid #ddd;
  margin: 10px 0;
}

/* Zeitangabe */
#TaskInput p {

  margin: 5px 0;
  font-size: 14px;
  color: white;
}

/* Statusanzeige */
#TaskInput p:last-of-type {
  font-weight: bold;
  color: white;

}

/* Buttons */
button {
  background-color: #007bff;
  color: white;
  border: none;
  border-radius: 4px;
  padding: 8px 12px;
  font-size: 14px;
  cursor: pointer;
  transition: background-color 0.3s ease;
}

button:hover {
  background-color: #0056b3;
}

button:focus {
  outline: none;
}

/* Abgeschlossene Aufgaben (optional, falls visuelle Unterscheidung gewünscht) */
#Task.completed {
  background-color: #e6ffe6;
  border-color: #66cc66;
}

#Task.completed h1 {
  text-decoration: line-through;
  color: #666;
}


#delete {
  background-color: #bf3222;
  color: white;
  border: none;
  border-radius: 4px;
  padding: 8px 12px;
  font-size: 14px;
  cursor: pointer;
  transition: background-color 0.3s ease;
}

#delete:hover {
  background-color: red;
}

#delete:focus {
  outline: none;
}
</style>
