<template>
  <section
    class="compare bg-(--c-brand) py-(--section-pad-y) px-(--gutter) rounded-b-[24px] xl:rounded-b-[80px]"
  >
    <!-- title -->
    <div class="compare__title mb-[24px] text-left sm:text-center">
      <h2 class="text-peach font-serif font-bold text-h2 mb-[24px] lg:mb-[40px]">
        他網教育新聞 vs. 聯合報數位版
      </h2>
      <p class="text-white text-h2-sub">6大優勢讓你知道有什麼不同！</p>
    </div>

    <!-- 捲動軌道 -->
    <HorizontalScroller
      track-class="2xl:overflow-x-visible 2xl:justify-center 2xl:gap-6 2xl:max-w-[1800px] 2xl:mx-auto"
      arrow-bg-class="bg-(--c-brand)/80"
    >
      <!-- 卡片 -->
      <CompareCard v-for="card in compareCards" :key="card.id" :card="card"
    /></HorizontalScroller>
  </section>
</template>

<script setup lang="ts">
// component
import HorizontalScroller from '@/components/HorizontalScroller.vue'
import CompareCard from '@/components/CompareCard.vue'

// Compare section icons
import compareNewspaper from '@/assets/compare-newspaper.svg'
import compareExclusive from '@/assets/compare-exclusive.svg'
import compareAd from '@/assets/compare-ad.svg'
import compareNews from '@/assets/compare-news.svg'
import compareReport from '@/assets/compare-report.svg'
import compareTopics from '@/assets/compare-topics.svg'

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
