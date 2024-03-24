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
        color="#979797"
        :style="{
          ...getTransformStyle(index),
          color: active === index ? '#000' : '#979797',
        }"
      >
        <span
          v-show="active === index"
          bg="#ff5f2c"
          w-20px
          h-20px
          b-rd="50%"
          overflow-hidden
          absolute
          top="50%"
          translate-y--50%
          left--50px
        ></span>
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

const offsetMap = ref(new Map());

const getTransformStyle = (index: number) => {
  const offset = getOffset(index);
  offsetMap.value.set(index, offset);

  return {
    transform: `translateY(${
      Math.sin(angleStep * offset) * radius.value + 300
    }px) translateX(${
      Math.cos(angleStep * offset) * radius.value - 75
    }px) rotate(${(angleStep * offset * 180) / Math.PI}deg)`,
  };
};

const active = computed(() => {
  const min = Array.from(offsetMap.value.entries()).sort((a, b) => {
    return Math.abs(a[1]) - Math.abs(b[1]);
  })[0];

  if (Math.abs(angleStep * min[1] * 180) / Math.PI > 5) return -1;

  return min[0];
});

const getOffset = (index: number) => {
  return -2 + index - Math.min(y.value, 250) * 0.04;
};

const { x, y, isScrolling, arrivedState, directions } = useScroll(window);
</script>

<style lang="scss" scoped>
.item {
}
</style>
