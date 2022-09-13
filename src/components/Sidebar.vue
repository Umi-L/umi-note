<script lang="ts">
import NoteButton from "./NoteButton.vue";
import { createDir, BaseDirectory, readDir, writeTextFile, readTextFile } from '@tauri-apps/api/fs';
import { defineComponent } from "vue";

export default defineComponent({
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
    async fetchNotes() {
      // Create the `$APPDIR/notes` directory
      await createDir('notes', {dir: BaseDirectory.App, recursive: true});
      // Reads the `$APPDIR/users` directory recursively

      let entries = await readDir('notes', {dir: BaseDirectory.App, recursive: true});

      this.notes = entries;
    },
    async get_file_name(file: string) {
      let file_data = await readTextFile("notes/" + file, {dir: BaseDirectory.App});

      let parsed_file_data = JSON.parse(file_data);

      return parsed_file_data.title;

    },
    open_file(file: string) {
      this.$emit('open_file', file);
    }
  },
  beforeMount(){
    this.fetchNotes();
  },

});
</script>


<template>
  <aside id="sidebar">
    <h3>Notes</h3>
    <template v-for="(note, indx) in notes">
      <template v-if="note.name.match(/\.[0-9a-z]+$/i)[0] === '.unote'">
        <NoteButton :file="note.path" @open_file="open_file" :name="get_file_name(note.name)"></NoteButton>
      </template>
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
