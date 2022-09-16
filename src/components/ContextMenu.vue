<script lang="ts">
  import { defineComponent } from "vue"
  import Menu from "./Menu.vue"

  export default defineComponent({
    components: {
      Menu,
    },
    data(){
      return {
        options: ["delete", "convert to"]
      }
    }
  })
</script>

<template>
  <Menu id="context-menu" :options="options"></Menu>
</template>

<style>
.show {
  display: block;
}

.hide {
  display: none;
}

/*#context-menu {
  position: absolute;
  width: 100px;
  height: 250px;
  background-color: var(--dark);

  outline: 2px solid var(--light1);
  border-radius: 5px;
}*/
</style>