<template>
  <div :class="{ 'bordered sticky': open }" class="is-hidden-mobile">
    <NeoSidebar :reduce="false" :open="open" fullheight>
      <EventTypeFilter v-if="isCollectionActivityTab" expanded fluid-padding />
      <StatusFilter v-else expanded fluid-padding />
      <PriceFilter v-if="!isCollectionActivityTab" fluid-padding />
    </NeoSidebar>
  </div>
</template>

<script lang="ts" setup>
import { NeoSidebar } from '@kodadot1/brick'
import StatusFilter from '@/components/shared/filters/modules/StatusFilter.vue'
import EventTypeFilter from '@/components/shared/filters/modules/EventTypeFilter.vue'
import PriceFilter from '@/components/shared/filters/modules/PriceFilter.vue'
import { usePreferencesStore } from '@/stores/preferences'
const route = useRoute()

const preferencesStore = usePreferencesStore()
const open = computed(() => preferencesStore.getsidebarFilterCollapse)
const isCollectionActivityTab = computed(
  () => route.name === 'prefix-collection-id-activity'
)
</script>

<style lang="scss" scoped>
@import '@/styles/abstracts/variables';
.bordered {
  border-right: 1px solid;
}

.sticky {
  position: -webkit-sticky;
  position: sticky;
  top: 84px;
  height: calc(100vh - 84px);
  margin-right: $fluid-container-padding;
}

.o-side {
  position: relative;
}
</style>
