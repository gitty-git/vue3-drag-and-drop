<template>
    <div class="drop-zone"
         @drop="onDrop($event, 1)"
         @dragenter.prevent
         @dragover.prevent
    >
        <div v-for="item in getList(1)"
             @dragstart="startDragging($event, item)"
             draggable="true"
             :key="item.id"
             class="drag-element"
        >
            {{ item.name }}
        </div>
    </div>
    <div class="drop-zone"
         @drop="onDrop($event, 2)"
         @dragenter.prevent
         @dragover.prevent
    >
        <div v-for="item in getList(2)"
             @dragstart="startDragging($event, item)"
             draggable="true"
             :key="item.id"
             class="drag-element"
        >
            {{ item.name }}
        </div>
    </div>
</template>

<script>
import { ref } from "vue";

export default {
    name: 'App',
    setup() {
        const items = ref([
            {id: 0, name: '1', list: 1},
            {id: 1, name: '2', list: 1},
            {id: 2, name: '3', list: 1},
            {id: 3, name: '4', list: 1},
        ])

        const getList = list => {
            return items.value.filter(item => item.list === list)
        }

        const startDragging = (e, item) => {
            console.log(item.id)
            e.dataTransfer.dropEffect = 'move'
            e.dataTransfer.effectAllowed = 'move'
            e.dataTransfer.setData('id', item.id)
        }

        const onDrop = (e, list) => {
            const id = e.dataTransfer.getData('id')
            const item = items.value.find(item => item.id.toString() === id)
            item.list = list
        }

        return {items, getList, startDragging, onDrop}
    }
}
</script>

<style>
#app {
    font-family: Avenir, Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
    margin-top: 60px;
    display: flex;
    justify-content: center;
}
.drop-zone {
    border-radius: 20px;
    display: grid;
    grid-template-columns: 1fr 1fr;
    grid-template-rows: 1fr 1fr;
    grid-gap: 20px;
    width: 200px;
    height: 200px;
    margin: 20px;
    padding: 20px;
    background-color: gray;
}
.drag-element {
    border-radius: 10px;
    font-size: 30px;
    border: 2px dashed gray;
    display: flex;
    align-items: center;
    justify-content: center;
    background-color: #fff;
}
</style>
