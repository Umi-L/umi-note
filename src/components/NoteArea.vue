<script lang="ts">
import ContextMenu from './ContextMenu.vue';
import Text from "./sub-components/Text.vue"
import Divider from "./sub-components/Divider.vue"
import ContextButton from "./ContextButton.vue"
import { defineComponent } from "vue";
import { readTextFile, writeTextFile, BaseDirectory } from '@tauri-apps/api/fs';

export default defineComponent({

  components: {
    ContextButton,
    ContextMenu,
    Text,
    Divider
  },

  emits: ["note_change"],

  data() {
    return {
      // array of component types and data can be used to reconstruct any note.
      default_sub_components: [{component:"title", value:""}, {component:"paragraph", value:""}],
      sub_components: [],
      current_file: "",
    }
  },

  methods: {
    add_element(index: number | string, type: string, data:any = undefined) {

      if (index == "end") {
        this.sub_components.push({component: type, value: data});
      }
      else if (typeof index == "number") {
        this.sub_components.splice(index, 0, {component: type, value: data});
      }

      console.log(`added element at index ${index} of type '${type}' with data '${data}'`);
    },

    convert_element_to_element(type: string, index: number) {

      console.log(`converting ${this.sub_components[index]} to ${type} at index ${index}`);

      this.sub_components[index] = {component: type, value:Object.values(this.sub_components[index])[0]};
    },

    remove_element(index:number){

      console.log([...this.sub_components]);

      this.sub_components.splice(index, 1);

      console.log(`removed element at ${index}`)
    },

    open_context_menu(elemenent: any) {
      let rect = elemenent.getBoundingClientRect();

      let context_menu = document.getElementById("context-menu") as HTMLDivElement;

      context_menu.classList.remove("hide");

      context_menu.style.top = (rect.bottom + window.scrollY) + "px";
      context_menu.style.left = (rect.right - context_menu.clientWidth) + "px";
    },

    async pack_note():Promise<string> {

      await this.$nextTick(() => {
        this.fetch_component_data();
      });

      let title = "Untitled";

      if (this.$refs.sub_components[0]){
        if (this.$refs.sub_components[0].get_component_value){
          let title_value = this.$refs.sub_components[0].get_component_value();
          if (typeof title_value === "string" && title_value.length > 0)
          {
            title = title_value;
          }
        }
      }

      return JSON.stringify(
          {data: this.sub_components, title: title},
          function(k, v) { return v === undefined ? null : v; }
      )
    },

    async save(){
      let note = await this.pack_note();

      if (this.current_file == ""){
        let now = new Date();
        let dd = String(now.getDate()).padStart(2, '0');
        let mm = String(now.getMonth() + 1).padStart(2, '0'); //January is 0!
        let yyyy = now.getFullYear();
        let hh = String(now.getHours()).padStart(2, '0');
        let min = String(now.getMinutes()).padStart(2, '0');
        let sec = String(now.getSeconds()).padStart(2, '0');

        this.current_file = `notes/${sec}-${min}-${hh}-${dd}-${mm}-${yyyy}.unote`;
      }

      writeTextFile(this.current_file, note, {dir: BaseDirectory.App});

      this.$emit("note_change")
    },

    fetch_component_data(){
      for (let i = 0; i < this.sub_components.length; i++){

        if (!this.$refs.sub_components[i])
          continue

        if (this.$refs.sub_components[i].get_component_value) {
          this.sub_components[i]["value"] = this.$refs.sub_components[i].get_component_value();
        }
        else{
          this.sub_components[i]["value"] = null;
        }
      }
    },

    open_file(file: string) {
      readTextFile(file).then((text_data) => {
        console.log(text_data);

        this.sub_components = [];

        this.$nextTick(() => {
          this.current_file = file;

          try{
            let components = JSON.parse(text_data).data;

            if (components){
              this.sub_components = components;
            }
            else{
              this.sub_components = this.default_sub_components;
            }
          }
          catch (error){
            console.log("parsing error!")
            console.error(error)
          }

          if (this.sub_components.length == 0){
            this.sub_components = this.default_sub_components;
          }
          console.log(this.default_sub_components);
        });
      });
    },

    sub_component_updated(){
      console.log("sub update")
      this.save();
    }
  }
});
</script>

<template>
  <ContextMenu></ContextMenu>

  <div class="note-body">
    <!-- render component list -->
    <template v-for="(component, indx) in sub_components" :key="indx">

      <div class="component-container">
        <ContextButton :index="indx"></ContextButton>

        <template v-if="component.component === 'title'">
          <Text placeholder="Untitled" @convert_element_to_element="convert_element_to_element" @add_element="add_element"
                @open_context_menu="open_context_menu" :index="indx" :size="2"
                :value="component.value"
                :type="component.component"
                @remove_element="remove_element"
                @value_change="sub_component_updated"
                ref="sub_components"></Text>
        </template>

        <template v-if="component.component === 'paragraph'">
          <Text @convert_element_to_element="convert_element_to_element" @add_element="add_element"
                @open_context_menu="open_context_menu" :index="indx" :size="1"
                :value="component.value"
                :type="component.component"
                @remove_element="remove_element"
                @value_change="sub_component_updated"
                ref="sub_components"></Text>
        </template>

        <template v-else-if="component.component === 'divider'">
          <Divider :index="indx" ref="sub_components"
          ></Divider>
        </template>
      </div>
    </template>
  </div>

  <div v-if="current_file != ''" id="new-element-area" @click="add_element('end', 'paragraph')">
    <p class="add-text">+</p>
  </div>


</template>

<style>
.component-container{
  display: flex;

  flex-direction: row;  
  align-items: center;

  transition-duration: 0.2s;

  border-left: dashed transparent 1px;
}

.component-container:hover{
  border-left: solid rgba(255, 255, 255, 0.2) 1px;
}

.component-container:hover .context-button{
  opacity: 50%;
}

.component-container:hover .context-button .context-button-image{
  transform: rotate(0deg) scale(1);
}

.note-body {
  margin-left: calc(var(--sidebar-width) + var(--left-margin));
  display: flex;
  flex-direction: column;
  padding-right: 10px;
}

#new-element-area {
  display: flex;

  margin-left: calc(var(--sidebar-width) + var(--left-margin));

  width: calc(100% - var(--sidebar-width) - calc(var(--left-margin) * 2));
  height: 100px;

  transition-duration: 0.3s;

  flex-direction: column;
  align-items: center;
  justify-content: center;

  cursor: pointer;

  margin-top: 20px;

}

#new-element-area:hover{
  background-color: rgba(0, 0, 0, 0.2);

  border-radius: 10px ;
}

.add-text{
  font-size: 50px;

  color: var(--dark);

  filter: brightness(150%);
}
</style>
