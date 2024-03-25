<template>
  <div relative>
    <div relative left="-300px">
      <div
        v-for="(item, index) in items"
        class="item"
        inline-flex
        absolute
        top="0"
        left="0"
        whitespace-nowrap
        transform-origin-left
        text-75px
        color="#979797"
        :class="{ active: active === index }"
        hover-c="#666"
        cursor-pointer
        :style="{
          ...getTransformStyle(index),
        }"
        @click="active = index"
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
const { x, y, isScrolling, arrivedState, directions } = useScroll(window);

const items = ref(new Array(12).fill("Make a song for my friends"));
const angleStep = ((150 / items.value.length) * Math.PI) / 180;
const radius = ref(500);

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

const active = ref(-1);
const getActive = () => {
  const min = Array.from(offsetMap.value.entries()).sort((a, b) => {
    return Math.abs(a[1]) - Math.abs(b[1]);
  })[0];

  if (Math.abs(angleStep * min[1] * 180) / Math.PI > 5) return -1;

  return min[0];
};

const getOffset = (index: number) => {
  return -2 + index - Math.min(y.value, 250) * 0.04;
};

const updateActive = () => {
  active.value = getActive();
};

watch(y, updateActive);
onMounted(() => {
  updateActive();
});
</script>

<style lang="scss" scoped>
.item.active {
  color: #000;
}
</style>
