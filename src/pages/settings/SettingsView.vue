<script setup lang="ts">
import { computed } from 'vue'
import { useI18n } from 'vue-i18n'

import BackButton from '@/components/appbar/BackButton.vue'
import AppListCategory from '@/components/list/AppListCategory.vue'
import AppListSingleSelectItem from '@/components/list/AppListSingleSelectItem.vue'
import { refTransformer } from '@/composables/converts'
// import { usePageTransition, usePreferredDesignLanguage, usePreferredLocale } from '@/composables/settings'
import { useTitle } from '@/composables/title'
import { designLanguageCodes, designLanguages, languages } from '@/data/settings'
import { usePageTransition, usePreferredDesignLanguage, usePreferredLocale } from '@/preferences/ui'
import { currentDesign } from '@/themes'

const { t, locale } = useI18n()
const title = useTitle(computed(() => t('settings')))

const appVersion = __VERSION__

const language = refTransformer(locale, usePreferredLocale())
const preferredDesignLanguage = usePreferredDesignLanguage()

const isDesignChanged = computed(
  // preferredDesignLanguage可能会包含不支持的值，但currentDesign会在出现不支持的值的时候跳回默认。
  // 所以需要判断preferredDesignLanguage是否属于支持的值，否则一律视作没有更改设计语言。
  () => currentDesign !== preferredDesignLanguage.value && designLanguageCodes.includes(preferredDesignLanguage.value),
)

const pageTransitionEnabled = usePageTransition()
</script>

<template>
  <app-page>
    <v-app-bar>
      <back-button />
      <v-app-bar-title :text="title" />
    </v-app-bar>
    <v-main scrollable>
      <app-list class="v-list--with-prepend-icon" un-margin="4" divider-inset>
        <app-list-category :subheader="$t('userInterface', 2)">
          <app-list-single-select-item
            v-model="preferredDesignLanguage"
            prepend-icon="$setting_design"
            :title="$t('designLanguage')"
            :items="designLanguages"
            :value-getter="(item) => item.code"
            :disabled-getter="(item) => item.disabled ?? false"
            :name-getter="(item) => item.name"
          >
            <v-list-item-subtitle v-if="isDesignChanged" un-color="vuetify-warning">
              {{ $t('designLanguageTakeEffectMessage') }}
            </v-list-item-subtitle>
          </app-list-single-select-item>
          <app-list-single-select-item
            v-model="language"
            prepend-icon="$setting_language"
            :title="$t('language')"
            :items="languages"
            :value-getter="(item) => item.code"
            :name-getter="(item) => item.name"
          />
          <app-list-item
            prepend-icon="$settings_animation"
            :title="$t('pageHierarchyTransition')"
            @click="pageTransitionEnabled = !pageTransitionEnabled"
            lastInVertical
          >
            <template #append>
              <v-switch v-model="pageTransitionEnabled" tabindex="-1" />
            </template>
          </app-list-item>
        </app-list-category>
        <app-list-category :subheader="$t('app.title')">
          <app-list-item
            prepend-icon="$foh"
            :title="$t('metadataSourcesManager')"
            :to="{ name: 'MetadataSources' }"
            append-icon="$next"
          />
          <app-list-item
            prepend-icon="$info"
            :title="$t('about')"
            :subtitle="`v${appVersion}`"
            :to="{ name: 'About' }"
            append-icon="$next"
            lastInVertical
          />
        </app-list-category>
      </app-list>
    </v-main>
  </app-page>
</template>
