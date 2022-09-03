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
    add_element(index: number, type: string, data:any = undefined) {

      //@ts-ignore -- odd bug that makes ts assume "this.sub_components" doesn't exist
      this.sub_components.splice(index, 0, {[type]: data});
    },

    convert_element_to_element(type: string, index: number) {

      //@ts-ignore -- see above
      console.log(`converting ${this.sub_components[index]} to ${type} at index ${index}`);

      //@ts-ignore -- see above
      this.sub_components[index] = {[type]:Object.values(this.sub_components[index])[0]};
    },

    remove_element(index:number){
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
              @remove_element="remove_element"></Text>
      </template>

      <template v-if="Object.keys(component)[0] === 'paragraph'">
        <Text @convert_element_to_element="convert_element_to_element" @add_element="add_element"
              @open_context_menu="open_context_menu" :index="indx" :size="1"
              :value="component[Object.keys(component)[0]]"
              :type="Object.keys(component)[0]"
              @remove_element="remove_element"></Text>
      </template>

      <template v-else-if="Object.keys(component)[0] === 'divider'">
        <Divider :index="indx"></Divider>
      </template>
    </template>
  </div>

  <div id="new-element-area" @click="add_element('paragraph')"></div>


</template>

<style>
.note-body {
  margin-left: 250px;
  display: flex;
  flex-direction: column;
  padding-right: 10px;
}

#new-element-area {
  width: 100%;
  height: 100px;
}
</style>
    