<script lang="ts">
import { ref} from 'vue'
const showPopup = ref(false);

function openPopup() {
  showPopup.value = true;
}
function closePopup() {
  showPopup.value = false;
}


</script>

<template>
  <button >
    <slot name="trigger" :open-popup="openPopup()"></slot>
  </button>
</template>

<style scoped>

</style>