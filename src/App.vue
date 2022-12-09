<template>
  <main class="flex flex-col justify-center h-full mx-auto max-w-600px">
    <section class="flex flex-col items-center leading-loose text-center">
      <div class="text-3xl">
        <span class="i-twemoji-christmas-tree"></span>
        {{ t('happyHolidays') }}
        <span class="i-twemoji-world-map"></span>
      </div>
      <!-- Dates - Check out locales/en.json for the key -->
      <i18n-t keypath="christmasIsComing" tag="div">
        <template #date>
          <span>{{ d(christmasDate, 'long') }}</span>
        </template>
        <template #time>
          <span class="text-green-600">{{ t('day', { count: daysDiff }) }}</span>
        </template>
      </i18n-t>
      <!-- Controls - I give you an .icon-button class if you want to use it -->
      <!-- Flags - the current locale -->
      <div class="flex justify-between w-50">
        <button class="icon-button" @click="changeLocale">
          <i class="i-carbon-language"></i>
        </button>
        <div>
          <i :class="mapIcon"></i>
          {{ t('language') }}
        </div>
      </div>
    </section>
  </main>
</template>

<script setup>
import { computed } from 'vue'
import { useI18n } from 'vue-i18n'
const { t, d, locale, messages } = useI18n()

// See the README about tricky timezone issues!
// I figured since this is i18n-friendly, we'd wanna
// make sure the timezones were right :-)
const christmasDate = new Date('2022/12/25')
const today = new Date()

const daysDiff = computed(() => {
  const diff = christmasDate.getTime() - today.getTime()
  return Math.ceil(diff / (1000 * 3600 * 24))
})

const localeFlagMap = {
  'en': 'i-twemoji-flag-united-states',
  'de': 'i-twemoji-flag-germany',
  'ja-JP': 'i-twemoji-flag-japan',
  'zh-TW': 'i-twemoji-flag-taiwan'
}

const mapIcon = computed(() => {
  return localeFlagMap[locale.value]
})

const locales = Object.keys(messages.value)

const changeLocale = () => {
  const index = locales.indexOf(locale.value)
  let nextIndex = index + 1
  if (nextIndex > locales.length) {
    nextIndex = 0
  }
  locale.value = locales[nextIndex]
}
</script>

<style scoped>
.icon-button {
  @apply text-xl
    w-32px
    h-32px
    rounded-full
    border-1
    border-transparent
    bg-transparent
    cursor-pointer
    duration-300
    hover:ring-2
    hover:border-green-500
    hover:ring-green-500
    hover:ring-opacity-40
    hover:text-green-600;
}
</style>
