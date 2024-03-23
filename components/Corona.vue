<template>
  <div relative>
    corona
    {{ { x, y, isScrolling, arrivedState, directions } }}
    <div relative left="-30%" :style="{}">
      <div
        v-for="(item, index) in items"
        class="item"
        inline-flex
        absolute
        top="0"
        left="0"
        whitespace-nowrap
        transform-origin-left
        text-50px
        :style="{
          transform: `translateY(${
            Math.sin(angleStep * getOffset(index)) * radius + 300
          }px) translateX(${
            Math.cos(angleStep * getOffset(index)) * radius - 75
          }px) rotate(${(angleStep * getOffset(index) * 180) / Math.PI}deg)`,
        }"
      >
        {{ item }}
      </div>
    </div>
  </div>
</template>

<script lang="ts" setup>
import { useScroll } from "@vueuse/core";

const items = ref(new Array(12).fill("Make a song for my friends"));
const angleStep = ((120 / items.value.length) * Math.PI) / 180;
const radius = ref(500);
const scrollY = ref(0);

const getOffset = (index: number) => {
  return -2 + index - Math.min(y.value, 250) * 0.04;
};

const { x, y, isScrolling, arrivedState, directions } = useScroll(window);
</script>

<style lang="scss" scoped>
.item {
}
</style>
