<script lang="ts">
    import ContextMenu from './ContextMenu.vue';
    import Paragraph from "./sub-components/Paragraph.vue"

    export default {

        components: {
            ContextMenu,
            Paragraph
        },

        data(){
            return {
                sub_components: ["paragraph"]
            }
        },

        methods: {
            say(message:string) {
                alert(message)
            },

            auto_grow (element:any) {            
                element.style.height = "0px";
                element.style.height = (element.scrollHeight)+"px";
            },
            
            handle_text_change(event:any) {

                let element = event.target as HTMLTextAreaElement;

                if (element.value.slice(-2) == "//"){
                    this.open_context_menu(element);
                }

                this.auto_grow(element)
            },

            open_context_menu(elemenent:any) {
                let rect = elemenent.getBoundingClientRect();

                let context_menu = document.getElementById("context-menu") as HTMLDivElement;

                context_menu.classList.remove("hide");

                context_menu.style.top = (rect.bottom + window.scrollY) + "px";
                context_menu.style.left = (rect.right - context_menu.clientWidth) + "px";
            },

            new_element(type:string) {

                //@ts-ignore -- odd bug that makes ts assume "this.sub_components" doesn't exist
                this.sub_components.push(type);
            }
        }
    };
</script>

<template>
    <ContextMenu></ContextMenu>

    <div class="note-body">
        <textarea placeholder="Title" class="title-textarea" @input="handle_text_change($event)"></textarea>
        <!-- <textarea placeholder="Start typing here, or use // to pull up the elements window." class="body-textarea" @input="handle_text_change($event)"></textarea> -->

        <template v-for="component in sub_components">
            <template v-if="component == 'paragraph'">
                <Paragraph></Paragraph>
            </template>
        </template>
    </div>

    <div id="new-element-area" @click="new_element('paragraph')"></div>
    

    
</template>

<style>
    .note-body{
        margin-left: 250px;
        display: flex;
        flex-direction: column;
        padding-right: 10px;
    }

    .title-textarea {
        background-color: transparent;
        border: none;
        outline: none;

        resize: none;

        color: var(--text);
        font-size: 2rem;

        margin-top: 20px;

        overflow: hidden;
    }

    .body-textarea {
        background-color: transparent;
        border: none;
        outline: none;

        overflow-y: hidden;
        resize: none;

        color: var(--text);
        font-size: 1rem;

        margin-top: 15px;

        width: 100%;
    }

    #new-element-area {
        width: 100%;
        height: 100px;
    }
</style>
    