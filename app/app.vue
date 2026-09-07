<template>
  <NuxtLayout>
    <NuxtPage />
  </NuxtLayout>
</template>

<script setup lang="ts">
useHead({
  htmlAttrs: {
    lang: 'pt-BR',
  },

  meta: [
    {
      name: 'viewport',
      content: 'width=device-width, initial-scale=1',
    },

    {
      name: 'theme-color',
      content: '#070B14',
    },

    {
      name: 'author',
      content: 'Hudson Kennedy',
    },

    {
      name: 'robots',
      content: 'index, follow',
    },

    {
      name: 'googlebot',
      content: 'index, follow',
    },
  ],

  link: [
    {
      rel: 'icon',
      type: 'image/png',
      href: '/favicon.png',
    },
  ],
})
</script>