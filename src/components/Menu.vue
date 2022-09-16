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
  width: 150px;
  max-height: 250px;
  background-color: var(--dark);

  outline: 2px solid var(--light1);
  border-radius: 5px;
}

.menu-button {
  width: 100%;
  text-align: center;
  border: 0;
  background-color: rgba(255, 255, 255, 0.0);
  color: var(--text);
}
.menu-button:hover{
  background-color: rgba(255, 255, 255, 1.0);
}
</style>