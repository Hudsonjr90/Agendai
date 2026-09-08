<script setup lang="ts">
import {
  nextTick,
  onBeforeUnmount,
  onMounted,
  ref,
  watch,
} from 'vue'

const { isDark, isReady: isThemeReady } = useTheme()

const cursorX = ref(0)
const cursorY = ref(0)

const glowX = ref(0)
const glowY = ref(0)

const isVisible = ref(false)
const isInteractive = ref(false)

let animationFrame = 0
let targetX = 0
let targetY = 0
let prefersReducedMotion = false

function isLightTheme() {
  return (
    isThemeReady.value &&
    !isDark.value &&
    document.body.classList.contains('body--light')
  )
}

function updateCursorPosition(event: MouseEvent) {
  if (!isLightTheme()) {
    isVisible.value = false
    return
  }

  targetX = event.clientX
  targetY = event.clientY

  cursorX.value = event.clientX
  cursorY.value = event.clientY

  if (prefersReducedMotion) {
    glowX.value = targetX
    glowY.value = targetY
  }

  isVisible.value = true
}

function animateGlow() {
  glowX.value += (targetX - glowX.value) * 0.14
  glowY.value += (targetY - glowY.value) * 0.14

  animationFrame = window.requestAnimationFrame(animateGlow)
}

function handleMouseLeave() {
  isVisible.value = false
}

function handleMouseEnter() {
  if (isLightTheme()) {
    isVisible.value = true
  }
}

function handlePointerOver(event: MouseEvent) {
  if (!isLightTheme()) {
    isInteractive.value = false
    return
  }

  const target = event.target

  if (!(target instanceof Element)) {
    isInteractive.value = false
    return
  }

  isInteractive.value = Boolean(
    target.closest(
      'a, button, [role="button"], .q-btn, .q-tab, .q-item, input, textarea, select',
    ),
  )
}

function updateVisibility() {
  if (!isLightTheme()) {
    isVisible.value = false
  }
}

watch(isVisible, (visible) => {
  document.body.classList.toggle('cursor-effect-active', visible)
})

watch(
  [isDark, isThemeReady],
  async () => {
    await nextTick()
    updateVisibility()
  },
)

onMounted(() => {
  const hasFinePointer = window.matchMedia(
    '(hover: hover) and (pointer: fine)',
  ).matches

  prefersReducedMotion = window.matchMedia(
    '(prefers-reduced-motion: reduce)',
  ).matches

  if (!hasFinePointer) {
    return
  }

  targetX = window.innerWidth / 2
  targetY = window.innerHeight / 2

  glowX.value = targetX
  glowY.value = targetY

  window.addEventListener(
    'mousemove',
    updateCursorPosition,
    { passive: true },
  )

  window.addEventListener(
    'mouseover',
    handlePointerOver,
    { passive: true },
  )

  document.documentElement.addEventListener(
    'mouseleave',
    handleMouseLeave,
  )

  document.documentElement.addEventListener(
    'mouseenter',
    handleMouseEnter,
  )

  if (!prefersReducedMotion) {
    animationFrame = window.requestAnimationFrame(animateGlow)
  }

  updateVisibility()
})

onBeforeUnmount(() => {
  window.removeEventListener(
    'mousemove',
    updateCursorPosition,
  )

  window.removeEventListener(
    'mouseover',
    handlePointerOver,
  )

  document.documentElement.removeEventListener(
    'mouseleave',
    handleMouseLeave,
  )

  document.documentElement.removeEventListener(
    'mouseenter',
    handleMouseEnter,
  )

  document.body.classList.remove('cursor-effect-active')

  if (animationFrame) {
    window.cancelAnimationFrame(animationFrame)
  }
})
</script>

<template>
  <div
    v-if="isThemeReady && !isDark"
    class="cursor-effect"
    :class="{
      'cursor-effect--visible': isVisible,
      'cursor-effect--interactive': isInteractive,
    }"
    aria-hidden="true"
  >
    <div
      class="cursor-effect__glow"
      :style="{
        transform: `translate3d(${glowX}px, ${glowY}px, 0) translate(-50%, -50%)`,
      }"
    />

    <div
      class="cursor-effect__ring"
      :style="{
        transform: `translate3d(${cursorX}px, ${cursorY}px, 0) translate(-50%, -50%)`,
      }"
    />

    <div
      class="cursor-effect__dot"
      :style="{
        transform: `translate3d(${cursorX}px, ${cursorY}px, 0) translate(-50%, -50%)`,
      }"
    />
  </div>
</template>