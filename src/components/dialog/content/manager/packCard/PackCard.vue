<template>
  <Card
    class="absolute inset-0 flex flex-col overflow-hidden rounded-2xl shadow-elevation-4 dark-theme:bg-dark-elevation-1 transition-all duration-200"
    :class="{
      'outline outline-[6px] outline-[var(--p-primary-color)]': isSelected
    }"
    :pt="{
      body: { class: 'p-0 flex flex-col h-full rounded-2xl' },
      content: { class: 'flex-1 flex flex-col rounded-2xl' },
      title: { class: 'p-0 m-0' },
      footer: { class: 'p-0 m-0' }
    }"
  >
    <template #title>
      <div class="flex justify-between p-5 pb-1 align-middle text-sm">
        <span class="flex items-start mt-2">
          <i
            class="pi pi-box text-muted text-2xl ml-1 mr-5"
            style="opacity: 0.5"
          ></i>
          <span class="text-lg relative top-[.25rem]">{{
            $t('manager.nodePack')
          }}</span>
        </span>
        <div class="flex items-center gap-2.5">
          <div
            v-if="nodePack.downloads"
            class="flex items-center text-sm text-muted tracking-tighter"
          >
            <i class="pi pi-download mr-2"></i>
            {{ formatNumber(nodePack.downloads) }}
          </div>
          <PackInstallButton />
        </div>
      </div>
    </template>
    <template #content>
      <ContentDivider />
      <div class="flex flex-1 p-5 mt-3 cursor-pointer">
        <div class="flex-shrink-0 mr-4">
          <PackIcon :node-pack="nodePack" />
        </div>
        <div class="flex flex-col flex-1 min-w-0">
          <span
            class="text-lg font-bold pb-4 truncate overflow-hidden text-ellipsis"
            :title="nodePack.name"
          >
            {{ nodePack.name }}
          </span>
          <div class="flex-1">
            <p
              v-if="nodePack.description"
              class="text-sm text-color-secondary m-0 line-clamp-3 overflow-hidden"
              :title="nodePack.description"
            >
              {{ nodePack.description }}
            </p>
          </div>
        </div>
      </div>
    </template>
    <template #footer>
      <ContentDivider :width="0.1" />
      <div class="flex justify-between p-5 text-xs text-muted">
        <div class="flex items-center gap-2 cursor-pointer">
          <span v-if="nodePack.publisher?.name">
            {{ nodePack.publisher.name }}
          </span>
          <PackVersionBadge
            v-if="isPackInstalled"
            :node-pack="nodePack"
            v-model:version="selectedVersion"
          />
          <span v-else-if="nodePack.latest_version">
            {{ nodePack.latest_version.version }}
          </span>
        </div>
        <div
          v-if="nodePack.latest_version"
          class="flex items-center gap-2 truncate"
        >
          {{ $t('g.updated') }}
          {{
            $d(new Date(nodePack.latest_version.createdAt), {
              dateStyle: 'medium'
            })
          }}
        </div>
      </div>
    </template>
  </Card>
</template>

<script setup lang="ts">
import Card from 'primevue/card'
import { computed, ref } from 'vue'

import ContentDivider from '@/components/common/ContentDivider.vue'
import PackInstallButton from '@/components/dialog/content/manager/PackInstallButton.vue'
import PackVersionBadge from '@/components/dialog/content/manager/PackVersionBadge.vue'
import PackIcon from '@/components/dialog/content/manager/packIcon/PackIcon.vue'
import type { components } from '@/types/comfyRegistryTypes'
import { formatNumber } from '@/utils/formatUtil'

const { nodePack, isSelected = false } = defineProps<{
  nodePack: components['schemas']['Node']
  isSelected?: boolean
}>()
const selectedVersion = ref<string | undefined>(undefined)
const isPackInstalled = computed(
  () =>
    // TODO: after manager store added
    // managerStore.isPackInstalled(nodePack?.id)
    true
)
</script>
