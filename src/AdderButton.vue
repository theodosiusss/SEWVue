<template>
  <button name="hallo" @click="onClick">
    <slot>Default Button Text</slot>
  </button>
</template>

<script>
export default {
  name: "AdderButton",
  props: {
    onClickHandler: {
      type: Function,
      default: null,
    },
  },
  methods: {
    onClick() {
      if (this.onClickHandler) {
        this.onClickHandler();
      } else {
        this.$emit("click");
      }
    },
  },
};
</script>

<style scoped>
/* Hier können Sie die Stile für den Button anpassen */
button {
  background-color: #007bff;
  color: white;
  border: none;
  padding: 10px 20px;
  border-radius: 5px;
  cursor: pointer;
}

button:hover {
  background-color: #0056b3;
}
</style>
