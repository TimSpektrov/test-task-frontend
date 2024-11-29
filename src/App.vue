<template>
    <div class="container">
      <TheBlock v-model="topLeftList" />
      <TheBlock v-model="topRightList" />
      <TheBlock :model-value="leftList" @update:model-value="updateLeftList"/>
      <TheBlock :model-value="rightList" @update:model-value="updateRightList" />
    </div>
</template>

<script setup lang="ts">
import TheBlock from './components/TheBlock.vue'
import {computed, ref} from "vue";
import LEFT_LIST from './db/left.json'
import RIGHT_LIST from './db/right.json'

interface IListItem {
  id: number,
  isActive: boolean,
  name: string,
}

const leftList = ref<IListItem[]>(LEFT_LIST.map(item => {
  return {
    ...item,
    isActive: false
  }
}))
const rightList = ref(RIGHT_LIST.map(item => {
  return {
    ...item,
    isActive: false
  }
}))

const topLeftList = computed(() => {
  return leftList.value.filter(item => item.isActive)
})

const topRightList = computed(() => {
  return rightList.value.filter(item => item.isActive)
})

const updateLeftList = (arr: IListItem[], id: number) => {
  if(arr.filter(item => item.isActive).length > 6) {
    const index = arr.findIndex(item => item.id === id)
    leftList.value[index].isActive = !leftList.value[index].isActive
  }
}

const updateRightList = (arr: IListItem[], id: number) => {
  if(arr.filter(item => item.isActive).length > 1) {
    rightList.value = rightList.value.map(item => ({...item, isActive: item.id === id}))
  }
}
</script>

<style scoped lang="scss">
.container {
  padding: 8px;
  height: 100vh;
  display: grid;
  grid-template-columns: repeat(2, 50%);
  grid-template-rows: repeat(2, 50%);
  gap: 8px;
  background-color: #181818;

}
</style>
