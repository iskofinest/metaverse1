<script setup lang="ts">

import {TresCanvas, useRenderLoop} from "@tresjs/core";
import {shallowRef} from "vue";
import {Levioso, OrbitControls, TorusKnot} from "@tresjs/cientos";

const gl = { clearColor: '#82DBC5' };

const { onLoop } = useRenderLoop();

const cubeRef = shallowRef(null);
const groupRef = shallowRef(null);
onLoop(({ elapsed }) => {
  if(cubeRef.value) {
    cubeRef.value.rotation.y = elapsed;
    cubeRef.value.rotation.z = elapsed;

    cubeRef.value.position.y = Math.sin(elapsed * 2);
    cubeRef.value.position.x = Math.sin(elapsed * 2);

    cubeRef.value.scale.set(
        Math.sin(elapsed * 2),
        Math.sin(elapsed * 2),
        Math.sin(elapsed * 2),
    );
  }
});
</script>

<template>

  <main style="height: 679px;">
    <tres-canvas v-bind="gl">
      <TresPerspectiveCamera />
      <OrbitControls />

      <Levioso ref="groupRef" :rotation-factor="10" :speed="5" :float-factor="5" :range="[-.1, -.1]">
        <TorusKnot :position="[-3, 0, 0]">
          <TresMeshNormalMaterial />
        </TorusKnot>
      </Levioso>

      <TresMesh ref="cubeRef">
        <TresBoxGeometry :args="[1, 1, 1]"/>
        <TresMeshNormalMaterial />
      </TresMesh>
    </tres-canvas>
  </main>
</template>
