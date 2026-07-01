<template>
  <section
    class="compare bg-(--c-brand) py-(--section-pad-y) px-(--gutter) rounded-b-[24px] xl:rounded-b-[80px]"
  >
    <!-- title -->
    <div class="compare__title text-left sm:text-center">
      <h2 class="text-peach font-serif font-bold text-h2 mb-[24px] lg:mb-[40px]">
        他網教育新聞 vs. 聯合報數位版
      </h2>
      <p class="text-white text-h2-sub">6大優勢讓你知道有什麼不同！</p>
    </div>

    <!-- track wrapper：relative 讓 next 按鈕可以 absolute 定位在右側 -->
    <div class="compare__track-wrapper relative mt-6">
      <!-- 捲動軌道 -->
      <div
        ref="trackRef"
        class="compare__track flex flex-row gap-4 overflow-x-auto 2xl:overflow-x-visible 2xl:justify-center 2xl:gap-6 2xl:max-w-[1800px] 2xl:mx-auto"
        @scroll="onTrackScroll"
      >
        <!-- 卡片 -->
        <div
          v-for="card in compareCards"
          :key="card.id"
          class="compare__card w-[258px] lg:w-[280px] h-[458px] bg-white flex flex-col gap-2 p-4 shrink-0 border-2 border-[#9E9E9E] overflow-hidden"
          style="border-radius: var(--radius-brand)"
        >
          <!-- 卡片上半 -->
          <div class="card-top h-[198px] flex flex-col items-center gap-2">
            <div class="card-badge text-accent-red border-b-[3px] border-accent-red">
              {{ card.badge }}
            </div>
            <h3 class="card-top__title text-[30px] font-bold text-center whitespace-nowrap">
              {{ card.title }}
            </h3>
            <img :src="card.icon" class="card-top__icon w-24 h-24 object-contain" />
          </div>

          <!-- 卡片下半：白框比較表 -->
          <div
            class="card-bottom h-[220px] flex flex-col overflow-hidden bg-[#472785]"
            style="border-radius: 12px 12px 32px 12px"
          >
            <!-- 勝列（聯合報） -->
            <div class="card-row flex flex-row items-center flex-1 bg-white">
              <!-- 紅標籤 -->
              <div
                class="card-row__tag w-7 self-stretch flex items-center justify-center bg-[#CB2529]"
              >
                <span class="text-[20px] font-bold text-white">勝</span>
              </div>
              <!-- 文字欄 -->
              <div class="card-row__body flex-1 flex flex-col gap-1 p-3 justify-center">
                <p class="card-row__name text-[20px] text-accent-red">聯合報數位版</p>
                <p class="card-row__desc text-black">{{ card.win }}</p>
              </div>
            </div>
            <!-- 負列（他網） -->
            <div class="card-row flex flex-row items-center flex-1 bg-[rgb(232,232,232)]">
              <!-- 灰標籤（空白） -->
              <div
                class="card-row__tag bg-[#E8E8E8] w-7 self-stretch flex items-center justify-center"
              ></div>
              <!-- 文字欄 -->
              <div class="card-row__body flex-1 flex flex-col gap-1 p-3 justify-center">
                <p class="card-row__name text-[18px] text-(--c-brand)">其他新聞網站</p>
                <p class="card-row__desc text-[18px] text-black">{{ card.lose }}</p>
              </div>
            </div>
          </div>
        </div>
      </div>

      <!-- prev 按鈕：float 在 wrapper 左側垂直置中，最左側時隱藏，桌面隱藏 -->
      <button
        v-show="!isAtStart"
        class="compare__arrow compare__arrow--prev absolute left-0 top-1/2 -translate-y-1/2 -translate-x-1/2 w-11 h-11 rounded-full bg-(--c-brand)/80 flex items-center justify-center cursor-pointer lg:hidden"
        style="box-shadow: inset 0 0 0 1px #000"
        aria-label="向左滑動"
        @click="scrollPrev"
      >
        <img :src="arrowIcon" class="w-6 h-6 rotate-180 brightness-0 invert" alt="" />
      </button>

      <!-- next 按鈕：float 在 wrapper 右側垂直置中，最右側時隱藏，桌面隱藏 -->
      <button
        v-show="!isAtEnd"
        class="compare__arrow compare__arrow--next absolute right-[20px] top-1/2 -translate-y-1/2 translate-x-1/2 w-11 h-11 rounded-full bg-(--c-brand)/80 flex items-center justify-center cursor-pointer lg:hidden"
        style="box-shadow: inset 0 0 0 1px #000"
        aria-label="向右滑動"
        @click="scrollNext"
      >
        <img :src="arrowIcon" class="w-6 h-6 brightness-0 invert" alt="" />
      </button>
    </div>
  </section>
</template>

<script setup lang="ts">
import { ref, onMounted } from 'vue'

// Compare section icons
import compareNewspaper from '@/assets/compare-newspaper.svg'
import compareExclusive from '@/assets/compare-exclusive.svg'
import compareAd from '@/assets/compare-ad.svg'
import compareNews from '@/assets/compare-news.svg'
import compareReport from '@/assets/compare-report.svg'
import compareTopics from '@/assets/compare-topics.svg'
import arrowIcon from '@/assets/arrow-icon.svg'

// scroll track ref for arrow buttons
const trackRef = ref<HTMLElement | null>(null)

// true when scroll container is at the leftmost position
const isAtStart = ref(true)

// true when scroll container is at the rightmost position
const isAtEnd = ref(false)

// card width (258px) + gap (16px) = 274px per scroll step
const SCROLL_STEP = 274

function onTrackScroll() {
  const el = trackRef.value
  if (!el) return
  isAtStart.value = el.scrollLeft <= 1
  isAtEnd.value = el.scrollLeft + el.clientWidth >= el.scrollWidth - 1
}

function scrollPrev() {
  trackRef.value?.scrollBy({ left: -SCROLL_STEP, behavior: 'smooth' })
}

function scrollNext() {
  trackRef.value?.scrollBy({ left: SCROLL_STEP, behavior: 'smooth' })
}

// calculate initial arrow state after DOM is ready
onMounted(() => {
  const el = trackRef.value
  isAtStart.value = true
  // if all cards fit within the container, right button should be hidden immediately
  isAtEnd.value = el ? el.clientWidth >= el.scrollWidth : false
})

const compareCards = [
  {
    id: 1,
    badge: '優勢1',
    title: '完整報紙內容',
    icon: compareNewspaper,
    win: '與紙本同步，完整新聞與評論。',
    lose: '片段內容，無法讀完整新聞。',
  },
  {
    id: 2,
    badge: '優勢2',
    title: '獨家深度報導',
    icon: compareExclusive,
    win: '深度報導，全面了解教育議題。',
    lose: '僅表面報導，缺乏深度分析。',
  },
  {
    id: 3,
    badge: '優勢3',
    title: '無廣告干擾',
    icon: compareAd,
    win: '純淨閱讀，沒有廣告干擾。',
    lose: '頁面充斥廣告，閱讀體驗差。',
  },
  {
    id: 4,
    badge: '優勢4',
    title: '即時教育新聞',
    icon: compareNews,
    win: '第一手教育資訊，即時更新。',
    lose: '轉載為主，資訊往往延遲。',
  },
  {
    id: 5,
    badge: '優勢5',
    title: '深度分析報告',
    icon: compareReport,
    win: '專業記者深度分析，洞察趨勢。',
    lose: '缺乏深度分析，流於表面。',
  },
  {
    id: 6,
    badge: '優勢6',
    title: '專題策展',
    icon: compareTopics,
    win: '系統性策展，完整掌握專題。',
    lose: '零散報導，缺乏系統整理。',
  },
]
</script>

<style scoped>
.compare__track {
  scroll-snap-type: x mandatory;
  scrollbar-width: none; /* Firefox */
}
.compare__track::-webkit-scrollbar {
  display: none; /* Chrome / Safari */
}
.compare__card {
  scroll-snap-align: start;
}
</style>
