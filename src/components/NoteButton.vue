<script lang="ts">

  import { defineComponent } from "vue";

  export default defineComponent({
    emits:["open_file"],
    props: {
      file: String,
      name: Promise<string>    
    },
    data(){
      return {
        name_resolved: "loading..."
      }
    },
    methods: {
      open_file() {
        this.$emit('open_file', this.file);
      },
    },
    mounted() {
      this.name.then((name)=>{this.name_resolved = name})
    },
    updated(){
      this.name.then((name)=>{
        this.name_resolved = name;
      })
    }
  })

</script>

<template>
  <button class="note-button" @click="open_file()">
    {{name_resolved}}
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
