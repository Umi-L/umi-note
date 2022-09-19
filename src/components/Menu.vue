<script lang="ts">
  import { defineComponent } from "vue"

  export default defineComponent({
    props: {
      options: Array<String>
    }
  })
</script>

<template>
  <div class="menu-container">
    <button v-for="option in options" class="menu-button">{{option}}</button>
  </div>
</template>

<style>
.hide {
  display: none;
}

.menu-container {
  position: absolute;
  width: 245px;
  max-height: 80%;
  background-color: var(--dark);

  outline: 2px solid var(--light1);
  border-radius: 2px;

  padding: 0;
  margin: 0;

  overflow-y: auto;
}

.menu-button {

  margin: 2px 3px 2px 3px;

  display: block;

  width: calc(100% - 6px);
  height: 2.5em;

  font-size: 1em;

  text-align: left;
  border: 0;
  background-color: rgba(255, 255, 255, 0.0);
  color: var(--text);

  border-radius: 2px;
}
.menu-button:hover{
  background-color: rgba(0, 0, 0, 0.2);
}
</style>