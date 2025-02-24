<template>
  <div class="nft-card">
    <nuxt-link :to="`/${prefix}/gallery/${nft.id}`">
      <MediaItem
        :key="nft.image"
        class="nft-media"
        :src="nft.image"
        :animation-src="nft.animationUrl"
        :mime-type="nft.mimeType"
        :title="nft?.name" />
    </nuxt-link>
    <div
      class="nft-media-info is-flex is-flex-direction-column"
      :class="`nft-media-info__${variant}`">
      <div class="is-flex is-flex-direction-column">
        <span class="is-ellipsis has-text-weight-bold">{{
          nft.name || '--'
        }}</span>

        <CollectionDetailsPopover
          v-if="
            variant !== 'minimal' && (nft.collection.name || nft.collection.id)
          "
          :show-delay="collectionPopoverShowDelay"
          :nft="nft"
          class="is-ellipsis">
          <template #trigger>
            <nuxt-link
              :title="nft.collectionName || nft.collection.name"
              :to="`/${prefix}/collection/${nft.collection.id}`"
              class="is-size-7 nft-info-collection-name">
              {{ nft.collection.name || '--' }}
            </nuxt-link>
          </template>
        </CollectionDetailsPopover>
      </div>

      <div
        class="is-flex is-align-items-center mt-2 is-ellipsis nft-media-info-footer"
        :class="[
          showPrice
            ? 'is-justify-content-space-between'
            : 'is-justify-content-end',
        ]">
        <CommonTokenMoney
          v-if="showPrice"
          :value="nft.price"
          data-cy="card-money" />
        <span
          v-if="variant !== 'minimal'"
          class="chain-name is-capitalized is-size-7"
          >{{ getChainNameByPrefix(prefix) }}</span
        >
      </div>
    </div>
  </div>
</template>

<script lang="ts" setup>
import MediaItem from '../MediaItem/MediaItem.vue'
import CommonTokenMoney from '@/components/shared/CommonTokenMoney.vue'
import type { NFT } from '@/components/rmrk/service/scheme'
import { getChainNameByPrefix } from '@/utils/chain'
import { NftCardVariant } from '@kodadot1/brick'

withDefaults(
  defineProps<{
    nft: NFT
    prefix: string
    showPrice: boolean
    collectionPopoverShowDelay?: number
    variant?: NftCardVariant
  }>(),
  {
    collectionPopoverShowDelay: 500,
    variant: 'primary',
  }
)
</script>

<style lang="scss" scoped>
@import './NeoNftCard.scss';
</style>
