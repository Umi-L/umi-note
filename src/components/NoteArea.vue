<script lang="ts">
import ContextMenu from './ContextMenu.vue';
import Text from "./sub-components/Text.vue"
import Divider from "./sub-components/Divider.vue"

export default {

  components: {
    ContextMenu,
    Text,
    Divider
  },

  data() {
    return {
      // array of component types and data can be used to reconstruct any note.
      sub_components: [{"title":""}, {"paragraph":""}]
    }
  },

  methods: {
    add_element(index: number | string, type: string, data:any = undefined) {

      if (index == "end") {
        //@ts-ignore -- odd bug that makes ts assume "this.sub_components" doesn't exist
        this.sub_components.push({[type]: data});
      }
      else if (typeof index == "number") {
        //@ts-ignore
        this.sub_components.splice(index, 0, {[type]: data});
      }

      console.log(`added element at index ${index} of type '${type}' with data '${data}'`);
    },

    convert_element_to_element(type: string, index: number) {

      //@ts-ignore -- see above
      console.log(`converting ${this.sub_components[index]} to ${type} at index ${index}`);

      //@ts-ignore -- see above
      this.sub_components[index] = {[type]:Object.values(this.sub_components[index])[0]};
    },

    remove_element(index:number){

      //@ts-ignore -- see above
      console.log([...this.sub_components]);

      //@ts-ignore -- see above
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

    pack_note(){
      console.log(JSON.stringify({data: this.sub_components}));
    },

    fetch_component_data(){
      for (let i = 0; i < this.sub_components.length; i++){
        this.sub_components.
        this.$refs[i].get_component_value();
      }
    }
  }
};
</script>

<template>
  <ContextMenu></ContextMenu>

  <div class="note-body">
    <!-- render component list -->

    <template v-for="(component, indx) in sub_components">

      <template v-if="Object.keys(component)[0] === 'title'">
        <Text placeholder="Untitled" @convert_element_to_element="convert_element_to_element" @add_element="add_element"
              @open_context_menu="open_context_menu" :index="indx" :size="2"
              :value="component[Object.keys(component)[0]]"
              :type="Object.keys(component)[0]"
              @remove_element="remove_element"
              :ref="indx"></Text>
      </template>

      <template v-if="Object.keys(component)[0] === 'paragraph'">
        <Text @convert_element_to_element="convert_element_to_element" @add_element="add_element"
              @open_context_menu="open_context_menu" :index="indx" :size="1"
              :value="component[Object.keys(component)[0]]"
              :type="Object.keys(component)[0]"
              @remove_element="remove_element"
              :ref="indx"></Text>
      </template>

      <template v-else-if="Object.keys(component)[0] === 'divider'">
        <Divider :index="indx"
                 :ref="indx"></Divider>
      </template>

      <template v-if="indx === sub_components.length-1">
        {{this.pack_note()}}
      </template>
    </template>

  </div>

  <div id="new-element-area" @click="add_element('end', 'paragraph')">
    <p class="add-text">+</p>
  </div>


</template>

<style>
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
    