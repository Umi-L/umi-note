<script lang="ts">
  import { defineComponent } from "vue"
  import Menu from "./Menu.vue"

  export default defineComponent({
    components: {
      Menu,
    },
    data(){
      return {
        options: [
          {
            name:"Delete", 
            icon: "fa-solid fa-trash", 
            callback: ()=>{console.log("delete")}
          },
          {
            name:"Convert To", 
            icon: "fa-solid fa-repeat",
            callback: ()=>{console.log("convert")}
          },
        ]
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