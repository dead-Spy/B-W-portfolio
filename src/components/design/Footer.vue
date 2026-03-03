<template>
  <footer
    class="relative flex flex-col items-center justify-center gap-20 p-[2%]"
  >
    <div class="grid w-full grid-cols-2 text-base sm:gap-x-6 md:grid-cols-12">
      <div
        v-for="section in footerSections"
        :key="section.title"
        class="flex flex-col mb-6 md:mb-0"
        :class="{
          // Menu takes up 9 columns
          'md:col-span-9': section.title === 'Menu',
          // Socials takes up 3 columns
          'md:col-span-3': section.title === 'Socials'
        }"
      >
        <p class="heading-5 font-medium mb-3 tracking-wide text-flax-smoke-900">
          {{ section.title }}
        </p>
        <div 
          :class="{ 
            'space-y-1': section.title === 'Menu', 
            'space-y-2': section.title === 'Socials' 
          }"
        >
          <p v-for="link in section.links" :key="link.label" class="heading-6">
            <Link
              v-if="section.title === 'Menu'"
              class="font-normal tracking-wide lowercase text-flax-smoke-800 hover:text-flax-smoke-900 transition-colors duration-200"
              :label="link.label"
              :url="link.url"
            />
            <Link
              v-else-if="section.title === 'Socials'"
              class="flex items-center gap-2 text-flax-smoke-800 hover:text-flax-smoke-900 transition-colors duration-200"
              :url="link.url"
            
              :label="link.label" 
              :show-label="true" 
              icon-size="size-5"
            />
          </p>
        </div>
      </div>
    </div>
    
    <div class="w-full border-t border-dashed border-flax-smoke-400"></div>

    <div class="grid w-full grid-cols-12 items-end">
      <div class="col-span-7 place-content-center md:col-span-6">
        <h6 class="heading-4 sm:heading-2 leading-none font-bold">
          © {{ new Date().getFullYear() }} Tanvir <br />
          All rights reserved.
        </h6>
      </div>

      <div
        class="col-span-5 place-content-center max-sm:place-content-end md:col-span-3"
      >
        <p class="heading-6 uppercase">Your Local time</p>
        <p class="heading-4 font-black text-flax-smoke-800 leading-none">{{ userLocalTime }}</p>
      </div>

      <div
        class="hidden md:col-span-2 md:col-start-11 md:block lg:col-span-1 lg:col-start-12"
      >
        <MagneticEffect divId="scroll-to-top" textId="scroll-to-top-icon">
          <div
            @click="lenis.scrollTo('#app', { duration: 2 })"
            id="scroll-to-top"
            class="flex-center bg-flax-smoke-400 size-20 cursor-pointer flex-col rounded-full"
          >
            <svg
              class="size-1/2"
              id="scroll-to-top-icon"
              xmlns="http://www.w3.org/2000/svg"
              viewBox="0 0 24 24"
              width="24"
              height="24"
              color="#000000"
              fill="none"
            >
              <path
                d="M12 4L12 20"
                stroke="currentColor"
                stroke-width="1.5"
                stroke-linecap="round"
                stroke-linejoin="round"
              />
              <path
                d="M16.9998 8.99996C16.9998 8.99996 13.3174 4.00001 11.9998 4C10.6822 3.99999 6.99982 9 6.99982 9"
                stroke="currentColor"
                stroke-width="1.5"
                stroke-linecap="round"
                stroke-linejoin="round"
              />
            </svg>
          </div>
        </MagneticEffect>
      </div>
    </div>
  </footer>
</template>

<script setup lang="ts">
  import { Link } from '..';
  import { onMounted, ref } from 'vue';
  import { lenis } from '@/main';
  import MagneticEffect from '../MagneticEffect.vue';
  import moment from 'moment-timezone';

  // --- DATA STRUCTURES ---
  const navbarLinks = [
    { label: 'home', url: '/' },
    { label: 'services', url: '/services' },
    { label: 'projects', url: '/projects' },
    { label: 'about', url: '/about' },
    { label: 'testimonials', url: '/testimonials' },
    { label: 'contact', url: '/contact' },
  ];

  const socialLinks = [
    // Note: I renamed 'x' to 'Facebook' here based on the icon path, assuming you want a clear label. If you intended 'x' (Twitter), please change the label back.
    { label: 'Facebook', url: '#', icon: 'M18 2h-3.5a4 4 0 00-4 4v2H8a.999.999 0 00-.999 1V13a1 1 0 001 1h2.5v7h4v-7h3.5l1-4H15V6a1 1 0 011-1h3.5V2z' },
    { label: 'X', url: '#', icon: 'M20 3c-1.05 0-2.05.2-3 .6-1.57.65-2.8 1.9-3.5 3.5-.7 1.6-.7 3.3 0 5s1.7 3 3.5 3.5c1.9.5 3.8.3 5.5-.3 1.7-.6 3.1-1.6 4.3-3 .1.1.2.2.3.4.1.2.1.3.1.5 0 .2 0 .4-.1.6-.1.2-.2.4-.4.5-.3.2-.6.3-1 .3-.3 0-.6 0-.9-.1-.3-.1-.5-.2-.8-.4-.2-.2-.4-.3-.5-.6-.1-.3-.1-.6 0-.9.1-.3.2-.5.4-.6.1-.1.2-.1.3-.1.1 0 .2.1.2.2.1.1.1.2.1.3.1.2.1.4.1.6 0 .2-.1.4-.2.5-.2.2-.4.4-.6.5-.4.2-.7.3-1.1.4-.4.1-.8.1-1.2.1-1.7 0-3.3-.5-4.6-1.5-1.3-1-2.3-2.3-2.9-3.9-.6-1.6-.7-3.3-.2-5 .5-1.7 1.5-3.1 3-4.1 1.4-1 3-1.6 4.8-1.7h.2V3h-.2zM12 21a9 9 0 100-18 9 9 0 000 18z' },
    { label: 'Github', url: '#', icon: 'M12 2C6.477 2 2 6.477 2 12c0 4.419 2.864 8.165 6.83 9.488.5.092.682-.217.682-.483 0-.237-.008-.868-.013-1.712-2.782.604-3.37-1.34-3.37-1.34-.454-1.156-1.11-1.463-1.11-1.463-.908-.62.069-.608.069-.608 1.004.07 1.532 1.03 1.532 1.03.89 1.528 2.336 1.086 2.903.829.091-.643.349-1.086.635-1.336-2.22-.252-4.555-1.11-4.555-4.943 0-1.09.39-1.984 1.03-2.682-.104-.252-.447-1.267.098-2.646 0 0 .84-.268 2.75 1.026A9.564 9.564 0 0112 6.844c.85.003 1.705.115 2.504.332 1.909-1.294 2.748-1.026 2.748-1.026.545 1.379.202 2.394.098 2.646.64.698 1.028 1.592 1.028 2.682 0 3.842-2.338 4.686-4.566 4.935.359.309.678.92.678 1.855 0 1.34-.012 2.427-.012 2.75 0 .268.18.579.688.482C21.139 20.163 24 16.418 24 12c0-5.523-4.477-10-10-10z' },
    { label: 'LinkedIn', url: '#', icon: 'M16 8a6 6 0 016 6v7h-4v-7a2 2 0 00-2-2 2 2 0 00-2 2v7h-4v-7a6 6 0 016-6zM4 21h4V8H4v13zM6 6a2 2 11 0 01-2-2h-2A2 2 0 006 2h-.1A2 2 0 014 4h2z' },
  ];
  
  const footerSections = [
    { title: 'Menu', links: navbarLinks },
    { title: 'Socials', links: socialLinks },
  ];

  const userLocalTime = ref('');

  onMounted(() => {
    const userTimeZone = Intl.DateTimeFormat().resolvedOptions().timeZone;
    userLocalTime.value = moment.tz(userTimeZone).format('h:mm:ss a');
    setInterval(() => {
      userLocalTime.value = moment.tz(userTimeZone).format('h:mm:ss a');
    }, 1000);
  });
</script>