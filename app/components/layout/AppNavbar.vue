<script setup lang="ts">
import { computed, nextTick, onBeforeUnmount, onMounted, ref } from 'vue'

import logo from '~/assets/images/logo.png'

const { isDark, isReady: isThemeReady, toggleTheme } = useTheme()

const activeSection = ref<string | null>(null)
const isMobileMenuOpen = ref(false)

const themeIcon = computed(() => {
  if (!isThemeReady.value || isDark.value) {
    return 'mdi-weather-sunny'
  }

  return 'mdi-weather-night'
})

const themeLabel = computed(() => {
  return isDark.value ? 'Ativar tema claro' : 'Ativar tema escuro'
})

const sections = [
  {
    id: 'sobre',
    label: 'Sobre',
    icon: 'mdi-information-outline',
  },
  {
    id: 'formacoes',
    label: 'Formações',
    icon: 'mdi-folder-outline',
  },
  {
    id: 'experiencia',
    label: 'Experiências',
    icon: 'mdi-briefcase-outline',
  },
  {
    id: 'depoimentos',
    label: 'Depoimentos',
    icon: 'mdi-comment-outline',
  },
  {
    id: 'tecnologias',
    label: 'Tecnologias',
    icon: 'mdi-laptop',
  },
  {
    id: 'contato',
    label: 'Contato',
    icon: 'mdi-phone-outline',
  },
]

function getSectionElement(id: string) {
  return document.getElementById(id)
}

function getHeaderHeight() {
  const header = document.querySelector('.q-header') as HTMLElement | null

  return header?.offsetHeight ?? 72
}

function updateActiveSection() {
  const headerHeight = getHeaderHeight()

  const activationLine = window.scrollY + headerHeight + Math.min(180, window.innerHeight * 0.25)

  let currentSection: string | null = null

  for (const section of sections) {
    const element = getSectionElement(section.id)

    if (!element) {
      continue
    }

    const sectionTop = element.getBoundingClientRect().top + window.scrollY

    if (sectionTop <= activationLine) {
      currentSection = section.id
    }
  }

  const documentHeight = document.documentElement.scrollHeight

  const viewportBottom = window.scrollY + window.innerHeight

  const reachedPageBottom = viewportBottom >= documentHeight - 8

  if (reachedPageBottom) {
    const contactElement = getSectionElement('contato')

    if (contactElement) {
      currentSection = 'contato'
    }
  }

  activeSection.value = currentSection
}

let ticking = false

function handleScroll() {
  if (ticking) {
    return
  }

  ticking = true

  window.requestAnimationFrame(() => {
    updateActiveSection()
    ticking = false
  })
}

function scrollToSection(id: string) {
  const element = getSectionElement(id)

  if (!element) {
    return
  }

  activeSection.value = id

  const headerHeight = getHeaderHeight()

  const targetPosition = element.getBoundingClientRect().top + window.scrollY - headerHeight - 12

  window.scrollTo({
    top: Math.max(targetPosition, 0),
    behavior: 'smooth',
  })

  window.history.replaceState(null, '', `#${id}`)

  isMobileMenuOpen.value = false
}

function handleLogoClick() {
  window.scrollTo({
    top: 0,
    behavior: 'smooth',
  })

  activeSection.value = null

  window.history.replaceState(null, '', window.location.pathname)
}

onMounted(async () => {
  await nextTick()

  window.addEventListener('scroll', handleScroll, {
    passive: true,
  })

  window.addEventListener('resize', handleScroll, {
    passive: true,
  })

  requestAnimationFrame(() => {
    updateActiveSection()
  })
})

onBeforeUnmount(() => {
  window.removeEventListener('scroll', handleScroll)

  window.removeEventListener('resize', handleScroll)
})
</script>

<template>
  <!-- =====================================================
       HEADER
       ===================================================== -->

  <q-header
    bordered
    height-hint="50"
    :class="isDark ? 'bg-transparent backdrop-blur' : 'bg-primary'"
  >
    <q-toolbar class="wrapper q-px-md">
      <!-- Logo -->
      <q-toolbar-title class="col-auto">
        <a
          href="/"
          class="row items-center no-decoration"
          aria-label="Voltar ao início"
          @click.prevent="handleLogoClick"
        >
          <img :src="logo" alt="HK Dev" height="60" width="60" class="q-pa-xs" />
        </a>
      </q-toolbar-title>

      <q-space />

      <!-- Menu principal -->
      <q-tabs
        v-model="activeSection"
        class="gt-sm"
        :active-color="isDark ? 'primary' : 'dark'"
        :indicator-color="isDark ? 'primary' : 'dark'"
        narrow-indicator
        shrink
        align="center"
      >
        <q-tab
          v-for="section in sections"
          :key="section.id"
          :name="section.id"
          :label="section.label"
          no-caps
          :color="isDark ? 'primary' : 'dark'"
          @click="scrollToSection(section.id)"
        />
      </q-tabs>

      <q-space />

      <!-- Tema desktop -->
      <div class="row items-center q-gutter-xs gt-sm">
        <q-btn
          flat
          round
          :icon="themeIcon"
          :aria-label="themeLabel"
          :title="themeLabel"
          @click="toggleTheme"
        />
      </div>

      <!-- Tema mobile -->
      <q-btn
        flat
        round
        :icon="themeIcon"
        class="lt-md"
        :aria-label="themeLabel"
        :title="themeLabel"
        @click="toggleTheme"
      />

      <!-- Botão menu mobile -->
      <q-btn
        flat
        round
        icon="mdi-menu"
        class="lt-md"
        aria-label="Abrir menu"
        @click="isMobileMenuOpen = true"
      />
    </q-toolbar>
  </q-header>

  <q-drawer
    v-model="isMobileMenuOpen"
    side="right"
    overlay
    bordered
    behavior="mobile"
    :dark="isDark"
    :width="280"
    
  >
    <!-- Cabeçalho do drawer -->
    <div
      class="row items-center justify-between q-pa-md"
      :class="isDark ? 'text-white text-bold' : 'text-bold'"
    > 
     <span> Menu</span>
      <q-btn
        flat
        round
        icon="mdi-close"
        aria-label="Fechar menu"
        @click="isMobileMenuOpen = false"
      />
    </div>

    <q-separator :dark="isDark" />

    <!-- Links -->
    <q-list padding :class="isDark ? 'text-white text-bold' : 'text-dark text-bold'">
      <q-item
        v-for="section in sections"
        :key="section.id"
        clickable
        :active="activeSection === section.id"
        active-class="text-primary"
        @click="scrollToSection(section.id)"
      >
        <q-item-section avatar>
          <q-icon :name="section.icon" size="18px" aria-hidden="true" />
        </q-item-section>

        <q-item-section>
          {{ section.label }}
        </q-item-section>
      </q-item>
    </q-list>
  </q-drawer>
</template>
