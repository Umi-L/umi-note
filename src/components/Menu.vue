<script lang="ts">
  import { defineComponent } from "vue"

  type IButton = {
    name: String,
    icon: String,
    callback: Function,
  }

  export default defineComponent({
    props: {
      options: Array<IButton>
    }
  })
</script>

<template>
  <div class="menu-container hide">
    <button v-for="option in options" class="menu-button" @click="option.callback">
      <i :class="option.icon"></i>
      {{option.name}}
    </button>
  </div>
</template>

<style scoped>

.hide {
  visibility: hidden !important;
}

.menu-container {
  display: flex;

  position: absolute;
  width: 245px;
  max-height: 80%;
  background-color: var(--dark);

  border: 1px solid var(--light1);
  border-radius: 3px;

  padding: 0;
  margin: 0;

  overflow-y: auto;

  flex-direction: column;
  justify-content: center;

  box-shadow: 0px 0px 4px black;
}

.menu-button {

  --side-margins: 3px;
  --top-margins: 3px;

  margin: var(--top-margins) var(--side-margins) var(--top-margins) var(--side-margins);

  display: block;

  width: calc(100% - calc(var(--side-margins)*2));
  height: 2.5em;

  font-size: 1em;

  text-align: left;
  border: 0;
  background-color: rgba(255, 255, 255, 0.0);
  color: var(--text);

  border-radius: 4px;
}
.menu-button:hover{
  background-color: rgba(0, 0, 0, 0.2);
}

i {
  margin-right: 5px;
}
</style>