<script setup lang="ts">
import { computed } from 'vue'
import { usePlayground } from '../store/playground'

const play = usePlayground()
const currentThemeType = computed(() => play.allThemes.find(i => i.id === play.theme)?.type || 'inherit')
</script>

<template>
  <div
    class="language-ts vp-adaptive-theme mini-playground transition-none!" shadow
    :style="[play.preStyle, { colorScheme: currentThemeType }]"
  >
    <div absolute z-10 p2 px3 pl5 flex="~ gap-1 items-center" left-0 top-0 right-0 border="b-solid gray/5">
      <div i-carbon:chevron-down op50 />
      <select v-model="play.lang" font-mono :style="play.preStyle">
        <option v-for="lang in play.allLanguages" :key="lang.id" :value="lang.id">
          {{ lang.name }}
        </option>
      </select>
      <div i-carbon:chevron-down op50 />
      <select v-model="play.theme" font-mono :style="play.preStyle">
        <option v-for="theme in play.allThemes.filter(i => i.type === 'light')" :key="theme.id" :value="theme.id">
          {{ theme.displayName }}
        </option>
        <option disabled>
          ──────────
        </option>
        <option v-for="theme in play.allThemes.filter(i => i.type === 'dark')" :key="theme.id" :value="theme.id">
          {{ theme.displayName }}
        </option>
      </select>
      <div flex-auto />
      <div
        i-svg-spinners-3-dots-fade
        :class="play.isLoading ? 'op100' : 'op0'"
        flex-none transition-opacity
      />
      <div op50 text-xs mx-2>
        Playground
      </div>
      <button title="Randomize" hover="bg-gray/10" p1 rounded @click="play.randomize">
        <div i-carbon:shuffle op50 />
      </button>
    </div>
    <div relative mt-10 min-h-100>
      <span v-html="play.output" />
      <textarea
        v-model="play.input"
        font-mono bg-transparent absolute inset-0 py-20px px-24px
        text-transparent caret-gray tab-4 resize-none z-10
        class="line-height-$vp-code-line-height font-$vp-font-family-mono text-size-$vp-code-font-size"
        autocomplete="off" autocorrect="off" autocapitalize="off" spellcheck="false"
      />
    </div>
  </div>
</template>

<style>
.mini-playground select {
  background: transparent;
  color: inherit;
  min-width: 8em;
  padding: 0 !important;
  position: relative;
}
.mini-playground select:focus {
  outline: none;
}

.mini-playground select:before {
  content: '';
  position: absolute;
  width: 1em;
  height: 1em;
  background: red;
}
</style>
