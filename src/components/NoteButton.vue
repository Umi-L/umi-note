<script lang="ts">

  import { defineComponent } from "vue";

  export default defineComponent({
    props: {
      file: String,
    },
    methods: {
      open_file() {
        this.$emit('open_file', this.file);
      },
    },
  })

</script>

<template>
  <button class="note-button" @click="open_file()">
    <slot></slot>
  </button>
</template>

<style>
.note-button {
  height: 3em;
  width: 100%;
  padding-top: 1em;
  padding-bottom: 1em;

  background-color: var(--dark);

  border: none;

  border-top: 1px solid var(--light1);
  /* border-bottom: 1px solid var(--light1); */

  color: var(--text);

  transition-duration: 0.2s;

  font-size: 1rem;

  padding: 0;
}

.note-button:hover {
  background-color: var(--light1);
}
</style>
