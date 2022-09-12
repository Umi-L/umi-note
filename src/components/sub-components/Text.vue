<script lang="ts">
import { defineComponent } from "vue";

export default defineComponent({
  props: {
    index: Number,
    size: Number,
    placeholder: String,
    value: String,
    type: String
  },

  methods: {
    handle_text_change(event: any) {

      this.$emit('value_change');

      if (this.index == undefined) {
        console.log("index is undefined");
        return;
      }

      let element = event.target as HTMLTextAreaElement;

      //command to open context menu
      if (element.value.slice(-2) == "//") {
        
        this.$emit('open_context_menu', element);
      }

      //macro to convert self to divider


      let lines = element.value.split("\n");

      let dividerMacroFound = false;

      lines.forEach((line: string) => {
        if (line == "---") {
          dividerMacroFound = true;
        }
      });

      if (dividerMacroFound){
        let splitOnDivider = element.value.split("---");

        //for each segment seperated by div macro
        for (let i = splitOnDivider.length-1; i > -1; i--) {

          console.log(i)

          let block = splitOnDivider[i]


          if (block.length > 0) {
            
            this.$emit('add_element', this.index + 1, this.type, block);
          }


          //if on last element don't put divider.
          if (i != 0) {
            
            this.$emit('add_element', this.index + 1, "divider")
          }
        }

        
        this.$emit('remove_element', this.index);
      }

      this.auto_grow(element)
    },

    auto_grow(element: any) {
      element.style.height = "0px";
      element.style.height = (element.scrollHeight) + "px";
    },

    get_component_value(): string {
      
      return this.$refs.textarea.value;
    }
  },
  mounted() {
    
    console.log(this.index, "mounted");

    
    this.auto_grow(this.$refs.textarea);
  },
  updated(){
    
    console.log(this.index, "updated");

    
    this.auto_grow(this.$refs.textarea);
  }
});
</script>


<template>
  <textarea ref="textarea"
            :placeholder="(!this.placeholder) ? 'Start typing here, or use /\/\ to pull up the elements window.' : this.placeholder"
            class="textarea-defaults" @input="handle_text_change($event)"
            :style="{'font-size': this.size + 'rem'}"
            :value="this.value"></textarea>
</template>

<style>
.textarea-defaults {
  background-color: transparent;
  border: none;
  outline: none;

  overflow-y: hidden;
  resize: none;

  color: var(--text);

  margin-top: 15px;

  font-size: 1rem;

  width: 100%;
}
</style>