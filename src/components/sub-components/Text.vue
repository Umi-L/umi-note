<script lang="ts">

    export default {
        props:{
            index: Number,
            size: Number,
            placeholder: String,
        },

        methods: {
            handle_text_change(event:any) {                
                let element = event.target as HTMLTextAreaElement;

                //command to open context menu
                if (element.value.slice(-2) == "//"){
                    //@ts-ignore
                    this.$emit('open_context_menu', element);
                }

                //macro to convert self to devider
                

                let lines = element.value.split("\n");

                lines.forEach((line:string) => {
                    if (line == "---"){

                        if (lines.length == 1){
                            //@ts-ignore
                            this.$emit('convert_element_to_element', "devider", this.index);
                        }
                        else {
                            //@ts-ignore
                            this.$emit('add_element', "devider");
                        }
                    }
                });

                this.auto_grow(element)
            },

            auto_grow (element:any) {
                element.style.height = "0px";
                element.style.height = (element.scrollHeight)+"px";
            },

            get_content() : string {
                //@ts-ignore
                return this.$refs.textarea.value;
            }
        }
    }
</script>


<template>
    <textarea ref="textarea" :placeholder="(this.placeholder) ? 'Start typing here, or use /\/\ to pull up the elements window.' : this.placeholder" class="textarea-defaults" @input="handle_text_change($event)" :style="{'font-size': this.size + 'rem'}"></textarea>
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