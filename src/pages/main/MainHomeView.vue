<script setup lang="ts">
import { useElementBounding, useScroll } from '@vueuse/core'
import { max } from 'lodash-es'
import { computed, onMounted, ref } from 'vue'
import type { VMain } from 'vuetify/components'

import ErrorAlert from '@/components/alert/ErrorAlert.vue'
import UnsafeBypassAlert from '@/components/alert/UnsafeBypassAlert.vue'
import CenterSpace from '@/components/CenterSpace.vue'
import { carousel } from '@/data/home'
import { useHomeStore } from '@/store/home'
import { useCachedArrayMap } from '@/utils/array'
import { isChrome } from '@/utils/browser'
import { useVMainScroller } from '@/utils/element'
import { renderAnnouncement } from '@/utils/markdown'

import HomeCarousel from './components/HomeCarousel.vue'

const homeStore = useHomeStore()
onMounted(() => {
  homeStore.ensureData()
})

const isLoading = computed(() => homeStore.isLoading)

const mainComponent = ref<InstanceType<typeof VMain>>()
const { y: scrollY } = useScroll(useVMainScroller(mainComponent))

const carouselComponent = ref<InstanceType<typeof HomeCarousel>>()
const { height: carouselHeight } = useElementBounding(carouselComponent, { windowScroll: false })

/**
 * 刷新主页数据
 */
function refresh() {
  homeStore.loadData()
}

defineExpose({ refresh })

/**
 * 轮播图高度 + 16
 */
const progressMarginTop = computed(() => {
  return max([-scrollY.value + carouselHeight.value + 16, 0])
})

const { cachedArray: renderedAnnouncements } = useCachedArrayMap(
  computed(() => homeStore.announcements),
  (item) => ({
    ...item,
    content: renderAnnouncement(item.content),
  }),
  (item) => `${item.timestamp} ${item.key}`,
)
</script>

<template>
  <v-main scrollable ref="mainComponent">
    <!-- 轮播图 -->
    <HomeCarousel ref="carouselComponent" un-margin="4" :items="carousel.items" :ratio="carousel.ratio" />

    <!-- Alerts -->
    <ErrorAlert v-if="homeStore.hasErrors" un-margin="4" :errors="homeStore.errorArray" />
    <UnsafeBypassAlert v-if="homeStore.hasErrors && isChrome" un-margin="4" />

    <!-- 公告 -->
    <app-list v-if="homeStore.hasAnnouncements" un-margin="4">
      <app-list-category
        v-for="(announcement, index) in renderedAnnouncements"
        :key="index"
        :subheader="$t('announcement_withName', { name: announcement.sourceName })"
      >
        <!-- eslint-disable-next-line vue/no-v-html vue/no-v-text-v-html-on-component -->
        <v-list-item class="typo-style announcement-content" v-html="announcement.content" />
      </app-list-category>
    </app-list>
    <CenterSpace v-if="isLoading" :top="progressMarginTop">
      <v-progress-circular indeterminate />
    </CenterSpace>
  </v-main>
</template>

<style scoped lang="scss">
.announcement-content {
  display: block;
  user-select: text;
  min-height: 0;
}
</style>
