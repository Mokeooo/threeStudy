<template>
  <v-card>
    <v-tabs v-model="tab" bg-color="primary">
      <v-tab :value="index" v-for="(item, index) of configs" :key="index">{{ item.name }}</v-tab>
    </v-tabs>

    <v-card-text>
      <v-window v-model="tab">
        <v-window-item :value="index" v-for="(item, index) of configs" :key="index">
          <Suspense>
            <div class="base card-container"><component :is="item.component"></component></div
          ></Suspense>
        </v-window-item>
      </v-window>
    </v-card-text>
  </v-card>
</template>
<script lang="ts" setup>
import Sandbox from './Sandbox.vue'
import Box from './Box.vue'
import { ref } from 'vue'
import MyFirstScene from './myFirstScene.vue'
const tab = ref()
const configs = [
  {
    name: 'base-demo',
    component: MyFirstScene
  },
  {
    name: 'demo2',
    component: Sandbox
  },
  {
    name: 'demo3',
    component: Box
  }
]
</script>
<style scoped>
.base {
  height: calc(100vh - 5rem);
}
</style>
