<script lang="ts">
    import ContextMenu from './ContextMenu.vue';
    import Text from "./sub-components/Text.vue"
    import Devider from "./sub-components/Devider.vue"

    export default {

        components: {
            ContextMenu,
            Text,
            Devider
        },

        data(){
            return {
                sub_components: ["title", "paragraph"]
            }
        },

        methods: {
            say(message:string) {
                alert(message)
            },

            new_element(type:string) {

                //@ts-ignore -- odd bug that makes ts assume "this.sub_components" doesn't exist 
                this.sub_components.push(type);

                console.log(`added ${type}`);
            },

            convert_element_to_element(type:string, index:number) {

                //@ts-ignore -- see above
                console.log(`converting ${this.sub_components[index]} to ${type} at index ${index}`);

                //@ts-ignore -- see above
                this.sub_components[index] = type;
            },

            open_context_menu(elemenent:any) {
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

        <template v-for="component, indx in sub_components">

            <template v-if="component == 'title'">
                <Text placeholder="Untitled" @convert_element_to_element="convert_element_to_element" @add_element="add_element" @open_context_menu="open_context_menu" :index="indx" :size="2"></Text>
            </template>

            <template v-if="component == 'paragraph'">
                <Text @convert_element_to_element="convert_element_to_element" @add_element="add_element" @open_context_menu="open_context_menu" :index="indx" :size="1"></Text>
            </template>

            <template v-else-if="component == 'devider'">
                <Devider :index="indx"></Devider>
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

    #new-element-area {
        width: 100%;
        height: 100px;
    }
</style>
    