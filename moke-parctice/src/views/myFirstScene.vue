<template>
  <TresCanvas v-bind="gl">
    <TresPerspectiveCamera :position="cameraPosition" :look-at="[0, 0, 0]" />
    <OrbitControls />
    <TresMesh cast-shadow ref="boxRef">
      <TresTorusGeometry :args="[1, 0.5, 16, 32]" />
      <TresMeshBasicMaterial color="orange" />
      <TresAmbientLight :intensity="1" cast-shadow />
    </TresMesh>
    <primitive :object="model" :position="[5, 0, 0]" />
  </TresCanvas>
</template>

<script setup lang="ts">
import { OrbitControls } from '@tresjs/cientos'
import { TresCanvas, useRenderLoop } from '@tresjs/core'
import * as THREE from 'three'
import { ref, shallowRef } from 'vue'
import { useAnimations, useGLTF } from '@tresjs/cientos'

const { scene: model, animations } = await useGLTF(
  'https://raw.githubusercontent.com/Tresjs/assets/main/models/gltf/ugly-naked-bunny/ugly-naked-bunny-animated.gltf'
)

const { actions } = useAnimations(animations, model)

const currentAction = ref(actions.Greeting)

currentAction.value.play()
const gl = {
  clearColor: '#82DBC5',
  shadows: true,
  alpha: false,
  shadowMapType: THREE.BasicShadowMap,
  outputColorSpace: THREE.SRGBColorSpace,
  toneMapping: THREE.NoToneMapping
}
const boxRef = shallowRef()

const { onLoop } = useRenderLoop()
const cameraPosition = [3, 3, 3]
onLoop(({ delta, elapsed }) => {
  if (boxRef.value) {
    boxRef.value.rotation.y += delta
  }
})
</script>

<style scoped></style>
