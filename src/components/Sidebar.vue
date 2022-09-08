<script lang="ts">
import NoteButton from "./NoteButton.vue";
import { createDir, BaseDirectory, readDir, writeTextFile } from '@tauri-apps/api/fs';

export default {
  components: {
    NoteButton
  },
  data() {
    return {
      // array of notes
      notes: []
    }
  },
  methods: {
    async fetchNotes(){
      // Create the `$APPDIR/notes` directory
      await createDir('notes', { dir: BaseDirectory.App, recursive: true });
      // Reads the `$APPDIR/users` directory recursively

      await writeTextFile("notes/somenote.unote", "abcd", { dir: BaseDirectory.App})
      await writeTextFile("notes/note2.2.2.unote", "abcd", { dir: BaseDirectory.App})

      let entries = await readDir('notes', { dir: BaseDirectory.App, recursive: true });

      console.log(entries);

      this.notes = entries;
    }
  },
  beforeMount(){
    this.fetchNotes();
  }
}
</script>


<template>
  <aside id="sidebar">
    <h3>Notes</h3>
    <template v-for="(note, indx) in notes">
      <NoteButton>{{(note.name).replace(/\.[^/.]+$/, "")}}</NoteButton>
    </template>

  </aside>
</template>


<style>
#sidebar {
  display: flex;
  flex-direction: column;
  align-items: center;

  top: 0;
  bottom: 0;

  width: var(--sidebar-width);

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
