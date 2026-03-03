<template>
  <component
    :is="tag"
    class="w-fit select-none"
  >
    <a @click="gotoSection(url)" :href="url" class="group flex items-center">
      
      <template v-if="iconPath">
        <svg
          xmlns="http://www.w3.org/2000/svg"
          viewBox="0 0 24 24"
          role="img"
          class="fill-current hover:scale-110 transition-transform duration-300" 
          :class="iconSize"
        >
          <path :d="iconPath"></path>
        </svg>
        
        <div v-if="showLabel" class="h-[1.2em] overflow-y-hidden ml-2">
            <p
              class="font-fancy leading-tight -translate-y-0 transition-all duration-300 ease-in-out will-change-auto group-hover:translate-y-[-100%] uppercase tracking-wider text-base font-normal"
            >
              {{ label }}
            </p>
            <p
              class="font-fancy leading-tight transition-all duration-300 ease-in-out will-change-auto group-hover:translate-y-[-100%] uppercase tracking-wider text-base font-normal"
            >
              {{ label }}
            </p>
        </div>
      </template>

      <div v-else class="h-[1.2em] overflow-y-hidden">
        <p
          class="font-fancy leading-tight -translate-y-0 transition-all duration-300 ease-in-out will-change-auto group-hover:translate-y-[-100%]"
        >
          {{ label }}
        </p>
        <p
          class="font-fancy leading-tight transition-all duration-300 ease-in-out will-change-auto group-hover:translate-y-[-100%]"
        >
          {{ label }}
        </p>
      </div>

    </a>
  </component>
</template>

<script setup lang="ts">
  import { gotoSection } from '@/functions';

  defineProps({
    tag: {
      type: String,
      default: 'p',
    },
    label: {
      type: String,
      required: true,
    },
    url: {
      type: String,
      required: true,
    },
    iconPath: {
      type: String,
      required: false,
      default: undefined,
    },
    showLabel: {
      type: Boolean,
      default: false,
    },
    iconSize: {
      type: String,
      default: 'size-8',
    },
  });
</script>