<script lang="ts">
import {defineComponent, watch, ref} from 'vue'

export default defineComponent({
  name: "ToDoAdder",
  emits: ["close", "name"],

  data() {
    return {
      text: ""
    }

  },
  watch: {
    text(oldText,newText){
      if(newText.length > 50 ){
        alert("too many characters (max 50)")
      }
    }
  },
  mounted() {
      if (this.$refs.focusMe) {
        (this.$refs.focusMe as HTMLInputElement).focus();
      }
    document.addEventListener('keydown', this.handleEscapeKey);


  },
  methods: {
    handleEscapeKey(event: KeyboardEvent) {
      if (event.key === 'Escape') {
        this.closePopup();
      }
    },

    closePopupAndSafe() {
      setTimeout(()=>{   this.$emit("close");
        this.$emit("name", this.text.substring(0,51));
      },10)


    },


    closePopup() {
      this.$emit("close");


    }
  },

})


</script>

<template>

  <div class="popup-overlay" >
    <div class="popup-content">
      <div style="display: flex;flex-direction: row ;justify-content: right; height: 20px">
        <button @click="closePopup" style="padding: 2px; margin: 0; justify-content: flex-end">x</button>
      </div>
      <h2>Neues To Do Erstellen</h2>
      <form @submit.prevent="closePopupAndSafe" >
        <label for="textInput">Name: </label>
        <input ref="focusMe"  type="text" id="textInput" placeholder="Max Musterman" v-model="text">
      </form>
      <button type="submit"  v-on:keyup.enter="(closePopupAndSafe)" @click="closePopupAndSafe">Abschicken&Schließen</button>

    </div>
  </div>
</template>
<style scoped>
/* Overlay für das Popup */
.popup-overlay {
  position: fixed;
  top: 0;
  left: 0;
  width: 100vw; /* Vollständige Breite */
  height: 100vh; /* Vollständige Höhe */
  background: rgba(0, 0, 0, 0.8); /* Dunklere Verdunkelung */
  display: flex;
  align-items: center;
  justify-content: center;
  z-index: 1000; /* Sicherstellen, dass das Popup über allem liegt */
  overflow: hidden; /* Verhindert Scrollen im Hintergrund */
}

/* Popup-Inhalt */
.popup-content {
  background: #1e1e1e; /* Dunkler Hintergrund */
  color: #ffffff; /* Weißer Text */
  padding: 40px;
  border-radius: 12px; /* Runde Ecken */
  box-shadow: 0 8px 16px rgba(0, 0, 0, 0.3); /* Tiefer Schatten */
  text-align: center;
  width: 50vw; /* Breite: 50% des Bildschirms */
  height: 50vh; /* Höhe: 50% des Bildschirms */
  max-width: 400px; /* Maximale Breite */
  max-height: 400px; /* Maximale Höhe */
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  position: relative;
}

/* Schließen-Button im Popup */
button.close-button {
  background: none;
  color: #ffffff;
  border: none;
  font-size: 20px;
  font-weight: bold;
  cursor: pointer;
  position: absolute;
  top: 10px;
  right: 15px;
  padding: 5px;
  transition: color 0.3s ease;
}

button.close-button:hover {
  color: #e74c3c; /* Rot beim Hover */
}

/* Titel des Popups */
.popup-content h2 {
  margin: 20px 0;
  font-size: 24px;
  color: #f1f1f1;
}

/* Formular-Inhalt */
form {
  margin-top: 20px;
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 20px;
}

label {
  font-size: 18px;
  color: #dddddd;
  align-self: flex-start;
}

input {
  width: 100%;
  padding: 10px;
  font-size: 16px;
  border: 1px solid #444444;
  border-radius: 6px;
  background: #2c2c2c;
  color: #ffffff;
}

input::placeholder {
  color: #aaaaaa;
}

/* Absenden-Button */
button {
  background-color: #3498db;
  color: white;
  border: none;
  border-radius: 6px;
  padding: 12px 20px;
  font-size: 16px;
  cursor: pointer;
  transition: background-color 0.3s ease;
}

button:hover {
  background-color: #2980b9;
}

/* Spezifische Anpassungen für Popup-Schließen */
button:focus {
  outline: none;
}
</style>

