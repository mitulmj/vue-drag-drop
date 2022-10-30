<template>
    <div class="drop-zone" @drop="onDrop($event,1)" @dragenter.prevent @dragover.prevent
        >
        <div v-for="item in getList(1)" 
        :key="item.id" 
        class="drag-el"
        draggable="true"
        @dragstart="startDrag($event,item)"
        >
            {{item.title}}
        </div>
    </div>

    <div class="drop-zone" @drop="onDrop($event,2)" @dragenter.prevent @dragover.prevent>
        <div v-for="item in getList(2)" 
        :key="item.id" 
        class="drag-el"
        draggable="true"
        @dragstart="startDrag($event,item)"
        >
            {{item.title}}
        </div>
    </div>
</template>

<script setup lang="ts">
import { Ref, ref } from 'vue';

interface item {
    id:number;
    title:string;
    list:number;
}

interface items extends Array<item>{}

const items: Ref<items> = ref([
    {id:0, title:"Component A", list : 1},
    {id:1, title:"Component B", list : 1},
    {id:2, title:"Component C", list : 2}
])


const getList = (list:number) => {
  const item =  items.value.filter(item => item.list === list)
  return item
}

const startDrag = (event,item)=>{
    console.log(item);
    event.dataTransfer.dropEffect= "move",
    event.dataTransfer.effectAllowed="move",
    event.dataTransfer.setData('itemId',item.id)
}

const onDrop = (event,list) =>{
    const itemId = event.dataTransfer.getData("itemId");
    console.log(itemId);
    
    const item = items.value.find((item)=> item.id == itemId)
    item!.list = list
}

</script>

<style scoped>
.drop-zone{
    width: 50%;
    margin: 50px auto;
    background: #ecf0f1;
    padding: 10px;
    min-height: 10px;
}
.drag-el{
    background: lightblue;
    color: white;
    padding: 5px;
    margin-bottom: 10px;
}
</style>