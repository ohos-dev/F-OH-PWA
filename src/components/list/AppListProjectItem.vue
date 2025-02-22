<script setup lang="ts">
import { computed } from 'vue'

import type { AppInfo } from '@/data/apps'
import { completeApiUrl } from '@/utils/url'

const props = withDefaults(
  defineProps<{
    item: Pick<AppInfo, 'name' | 'packageName' | 'icon' | 'version' | 'desc'>
    to?: boolean | string | object
  }>(),
  {
    to: true,
  },
)
const to = computed((): string | object | undefined => {
  if (props.to === true) {
    return `/app/${props.item.packageName}`
  } else if (props.to) {
    return to
  } else {
    return undefined
  }
})

const iconCompletePath = computed(() => completeApiUrl(props.item.icon))
</script>

<template>
  <v-list-item class="project-list-item" lines="two" :to="to">
    <template #prepend>
      <!-- v-img 大量使用会导致卡顿，所以使用原生的 img 标签 -->
      <img class="ohos-app-icon border" width="48" height="48" :src="iconCompletePath" alt="" draggable="false" />
    </template>
    <v-list-item-title class="title">
      {{ item.name }} <span class="version text-body-2">v{{ item.version }}</span>
    </v-list-item-title>
    <v-list-item-subtitle>{{ item.desc }}</v-list-item-subtitle>
    <slot />
  </v-list-item>
</template>

<style scoped lang="scss">
.project-list-item {
  --border-margin-left: 80px;

  .title {
    .version {
      opacity: var(--v-medium-emphasis-opacity);
    }
  }
}
:deep(.v-list-item__spacer) {
  width: 16px;
}
</style>
