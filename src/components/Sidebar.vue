<script lang="ts">
import NoteButton from "./NoteButton.vue";

export default {
  components: {
    NoteButton
  }
}
</script>


<template>
  <aside id="sidebar">
    <h3>Notes</h3>
    <note-button></note-button>
    <note-button></note-button>
    <note-button></note-button>
    <note-button></note-button>
    <note-button></note-button>
    <note-button></note-button>
    <note-button></note-button>
    <note-button></note-button>
    <note-button></note-button>
    <note-button></note-button>
    <note-button></note-button>
    <note-button></note-button>
    <note-button></note-button>
    <note-button></note-button>
    <note-button></note-button>
    <note-button></note-button>
    <note-button></note-button>
    <note-button></note-button>
    <note-button></note-button>
    <note-button></note-button>
    <note-button></note-button>
    <note-button></note-button>
    <note-button></note-button>

  </aside>
</template>


<style>
#sidebar {
  display: flex;
  flex-direction: column;
  align-items: center;

  top: 0;
  bottom: 0;

  width: 200px;

  position: fixed;

  overflow-y: scroll;
  overflow-x: hidden;

  border-right: 2px solid var(--light1);
  background-color: var(--dark);
}

h3 {
  margin-top: 0.3em;
}
</style>
