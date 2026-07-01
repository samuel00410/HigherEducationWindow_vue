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

    <!-- 捲動軌道 -->
    <div
      class="compare__track mt-[24px] flex flex-row gap-[16px] overflow-x-auto 2xl:overflow-x-visible 2xl:justify-center 2xl:gap-[24px] 2xl:max-w-[1800px] 2xl:mx-auto"
    >
      <!-- 卡片 -->
      <div
        v-for="card in compareCards"
        :key="card.id"
        class="compare__card w-[258px] lg:w-[280px] h-[458px] bg-white flex flex-col gap-[8px] p-[16px] shrink-0 border-[2px] border-[#9E9E9E] overflow-hidden"
        style="border-radius: var(--radius-brand)"
      >
        <!-- 卡片上半 -->
        <div class="card-top h-[198px] flex flex-col items-center gap-[8px]">
          <div class="card-badge text-accent-red border-b-[3px] border-accent-red">
            {{ card.badge }}
          </div>
          <h3 class="card-top__title text-[30px] font-bold text-center whitespace-nowrap">
            {{ card.title }}
          </h3>
          <img :src="card.icon" class="card-top__icon w-[96px] h-[96px] object-contain" />
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
              class="card-row__tag w-[28px] self-stretch flex items-center justify-center bg-[#CB2529]"
            >
              <span class="text-[20px] font-bold text-white">勝</span>
            </div>
            <!-- 文字欄 -->
            <div class="card-row__body flex-1 flex flex-col gap-[4px] p-[12px] justify-center">
              <p class="card-row__name text-[20px] text-accent-red">聯合報數位版</p>
              <p class="card-row__desc text-black">{{ card.win }}</p>
            </div>
          </div>
          <!-- 負列（他網） -->
          <div class="card-row flex flex-row items-center flex-1 bg-[rgb(232,232,232)]">
            <!-- 灰標籤（空白） -->
            <div
              class="card-row__tag bg-[#E8E8E8] w-[28px] self-stretch flex items-center justify-center"
            ></div>
            <!-- 文字欄 -->
            <div class="card-row__body flex-1 flex flex-col gap-[4px] p-[12px] justify-center">
              <p class="card-row__name text-[18px] text-(--c-brand)">其他新聞網站</p>
              <p class="card-row__desc text-[18px] text-black">{{ card.lose }}</p>
            </div>
          </div>
        </div>
      </div>
    </div>
  </section>
</template>

<script setup lang="ts">
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
