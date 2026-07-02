<template>
  <div class="horizontal-scroller relative">
    <div
      ref="trackRef"
      class="flex flex-row gap-[16px] overflow-x-auto snap-x snap-mandatory"
      :class="props.trackClass"
      @scroll="onTrackScroll"
    >
      <slot />
    </div>

    <!-- 最左側時隱藏，2xl(1920以上) 以上恆隱藏 -->
    <button
      v-show="!isAtStart"
      class="absolute left-0 top-1/2 -translate-y-1/2 -translate-x-1/2 w-11 h-11 rounded-full flex items-center justify-center cursor-pointer 2xl:hidden"
      :class="props.arrowBgClass"
      style="box-shadow: inset 0 0 0 1px #000"
      aria-label="向左滑動"
      @click="scrollPrev"
    >
      <img :src="arrowIcon" class="w-6 h-6 rotate-180 brightness-0 invert" alt="" />
    </button>

    <!-- 最右側時隱藏，2xl(1920以上) 以上恆隱藏 -->
    <button
      v-show="!isAtEnd"
      class="absolute right-5 top-1/2 -translate-y-1/2 translate-x-1/2 w-11 h-11 rounded-full flex items-center justify-center cursor-pointer 2xl:hidden"
      :class="props.arrowBgClass"
      style="box-shadow: inset 0 0 0 1px #000"
      aria-label="向右滑動"
      @click="scrollNext"
    >
      <img :src="arrowIcon" class="w-6 h-6 brightness-0 invert" alt="" />
    </button>
  </div>
</template>

<script setup lang="ts">
import { ref, onMounted } from 'vue'
import arrowIcon from '@/assets/arrow-icon.svg'

defineOptions({ inheritAttrs: false })

// 允許外部傳入完整 Tailwind class 字串來自訂按鈕背景色
const props = withDefaults(
  defineProps<{
    arrowBgClass?: string
    trackClass?: string
  }>(),
  {
    arrowBgClass: 'bg-black/50',
    trackClass: '',
  },
)

const trackRef = ref<HTMLElement | null>(null)
const isAtStart = ref(true)
const isAtEnd = ref(false)

// 手動滑動時即時更新箭頭顯隱狀態
function onTrackScroll() {
  const el = trackRef.value
  if (!el) return
  isAtStart.value = el.scrollLeft <= 1
  isAtEnd.value = el.scrollLeft + el.clientWidth >= el.scrollWidth - 1
}

// 量測第一張卡片寬度動態決定捲動距離，避免寫死數字
function getScrollStep() {
  const firstCard = trackRef.value?.firstElementChild as HTMLElement | null
  if (!firstCard) return 0
  return firstCard.offsetWidth + 16 // 16 對應 gap-[16px]
}

function scrollPrev() {
  trackRef.value?.scrollBy({ left: -getScrollStep(), behavior: 'smooth' })
}

function scrollNext() {
  trackRef.value?.scrollBy({ left: getScrollStep(), behavior: 'smooth' })
}

// 修正初始狀態：若卡片全部塞得下，next 箭頭應直接隱藏
onMounted(() => {
  const el = trackRef.value
  isAtStart.value = true
  isAtEnd.value = el ? el.clientWidth >= el.scrollWidth : false
})
</script>

<style scoped>
/* Tailwind 無隱藏捲軸 utility，需手寫 */
.horizontal-scroller :first-child {
  scrollbar-width: none;
}
.horizontal-scroller :first-child::-webkit-scrollbar {
  display: none;
}
</style>
