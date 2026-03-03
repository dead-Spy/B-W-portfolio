<template>
  <div
    v-if="!isLoading"
    id="loading-screen"
    class="flex-center fixed bottom-0 z-99999 size-full cursor-wait"
  >
    <div class="size-full flex-col">
      <svg
        :width="width"
        :height="height * 2"
        class="fill-flax-smoke-800 absolute top-0 z-0 h-[calc(100%_+_300px)] brightness-50"
      >
        <path ref="path" class="w-full" :d="pathData"></path>
      </svg>
      <div
        id="text"
        style="transform: translateZ(0px)"
        class="text-flax-smoke-50/75 z-1 flex size-full flex-col items-center justify-center text-center text-4xl font-bold opacity-0 md:text-6xl"
        :class="{ 'text-4xl font-bold md:text-6xl': true }"
      >
        <div class="relative flex h-32 w-32 items-center justify-center">
          <div
            class="loading-diamond absolute h-28 w-28 border-4 border-flax-smoke-50/20"
            style="animation-delay: 0s"
          ></div>
          <div
            class="loading-diamond absolute h-24 w-24 border-4 border-flax-smoke-50/40"
            style="animation-delay: 0.2s"
          ></div>
          <div
            class="loading-diamond absolute h-20 w-20 border-4 border-flax-smoke-50/60"
            style="animation-delay: 0.4s"
          ></div>
          <div
            class="loading-diamond absolute h-16 w-16 border-4 border-flax-smoke-50/80"
            style="animation-delay: 0.6s"
          ></div>
        </div>

        <p class="heading-6 overflow-clip font-normal mt-10">
          <span
            class="loading-text absolute bottom-10 left-5 inline-block font-mono sm:left-14"
          >
            Please wait...!
          </span>

          <span
            class="loading-text absolute right-5 bottom-10 inline-block animate-pulse font-mono sm:right-14"
          >
            Loading...
          </span>
        </p>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
  import { useWindowSize } from '@vueuse/core';
  import { computed, onMounted, Ref, ref, watch } from 'vue';
  import {
    animateLoadingPath,
    animateLoadingText,
    animateLoadingTextContainer,
  } from '@/animations';

  const emit = defineEmits(['isLoading']);

  const isLoading = ref(false);
  const index = ref(-1);
  const pathData = ref('');
  const path = ref<SVGPathElement>();

  const { width, height } = useWindowSize();
  let curveHeight = computed(() => {
    // Default for larger screens
    let multiplier = 0.3;

    if (width.value < 600) {
      // Smaller multiplier for phones
      multiplier = 0.15;
    } else if (width.value < 900) {
      // Adjust for tablets or medium screens
      multiplier = 0.2;
    }

    // multiplier *= -1;
    return height.value + height.value * multiplier;
  });
  const initialPath = ref(
    `M0 0 L${width.value} 0 L${width.value} ${height.value} Q${width.value / 2} ${curveHeight.value} 0 ${height.value}  L0 0`,
  );
  const targetPath = ref(
    `M0 0 L${width.value} 0 L${width.value} ${height.value} Q${width.value / 2} ${height.value} 0 ${height.value}  L0 0`,
  );

  const isSamsungBrowser = /samsung/i.test(navigator.userAgent);

  onMounted(() => {
    index.value++;
    pathData.value = initialPath.value;
    animateLoadingTextContainer();
    animateLoadingText('span.loading-text');

    animateLoadingPath(
      path as Ref<SVGPathElement>,
      targetPath.value,
      isSamsungBrowser,
    );
  });

  // TODO: remove it
  watch(
    [width, height],
    () => {
      initialPath.value = `M0 0 L${width.value} 0 L${width.value} ${height.value} Q${width.value / 2} ${curveHeight.value} 0 ${height.value} L0 0`;
      targetPath.value = `M0 0 L${width.value} 0 L${width.value} ${height.value} Q${width.value / 2} ${height.value} 0 ${height.value}  L0 0`;

      pathData.value = initialPath.value;
    },
    {
      immediate: true,
    },
  );

  watch(isLoading, (newVal) => {
    emit('isLoading', newVal);
  });
</script>

<style scoped>
.loading-diamond {
  transform: rotate(45deg) scale(0); /* Start as a diamond, initially invisible */
  opacity: 0;
  animation: diamond-load 2s ease-in-out infinite; /* Apply the custom animation */
}

@keyframes diamond-load {
  0% {
    transform: rotate(45deg) scale(0);
    opacity: 0;
  }
  25% {
    transform: rotate(45deg) scale(1.1);
    opacity: 1;
  }
  50% {
    transform: rotate(135deg) scale(1); /* Rotate and slightly shrink */
    opacity: 1;
  }
  75% {
    transform: rotate(225deg) scale(0.9); /* Further rotate and shrink */
    opacity: 0.7;
  }
  100% {
    transform: rotate(315deg) scale(0); /* Final rotation and fade out */
    opacity: 0;
  }
}
</style>