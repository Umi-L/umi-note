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

      let name = parsed_file_data.title;

      if (!name)
        name = "Untitled";

      return name;

    },
    open_file(file: string) {
      this.$emit('open_file', file);
    },
    create_new_note(){
      console.log("newNote");

      let now = new Date();
      let dd = String(now.getDate()).padStart(2, '0');
      let mm = String(now.getMonth() + 1).padStart(2, '0'); //January is 0!
      let yyyy = now.getFullYear();
      let hh = String(now.getHours()).padStart(2, '0');
      let min = String(now.getMinutes()).padStart(2, '0');
      let sec = String(now.getSeconds()).padStart(2, '0');

      let filename = `notes/${sec}-${min}-${hh}-${dd}-${mm}-${yyyy}.unote`;

      writeTextFile(filename, "{}", {dir: BaseDirectory.App});

      this.fetchNotes();
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

    <button class="note-add" @click="create_new_note()">+</button>

  </aside>
</template>


<style>

.note-add{
  height: 3em;
  min-height: 3em;
  width: 100%;
  padding-top: 1em;
  padding-bottom: 1em;

  background-color: var(--dark);

  border: none;

  border-top: 1px solid var(--light1);
  border-bottom: 1px solid var(--light1); 

  color: var(--text);

  transition-duration: 0.2s;

  font-size: 1rem;

  padding: 0;
}
.note-add:hover{
  background-color: var(--light1);
}

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
