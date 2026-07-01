<template>
  <section
    class="hero relative overflow-hidden min-h-[560px] lg:min-h-[800px] flex items-center justify-center"
  >
    <!-- 背景圖(鋪滿層) -->
    <img
      class="absolute inset-0 w-full h-full object-cover"
      :src="sectionOneBg"
      alt=""
      aria-hidden="true"
    />
    <!-- 深色遮罩 -->
    <div class="hero__overlay absolute inset-0 bg-black/45" aria-hidden="true"></div>

    <!-- 前景內容 -->
    <div
      class="hero__inner relative z-10 flex flex-col items-center text-center text-white w-full max-w-(--container-max) mx-auto px-4 py-16 lg:py-24 gap-6 lg:gap-8"
    >
      <!-- 小標題 -->
      <img
        class="w-full max-w-[290px] md:max-w-[580px] object-contain"
        :src="educateLogoTitle"
        alt="高教視窗"
      />

      <!-- 副標 -->
      <p class="text-hero-sub text-peach font-bold leading-snug max-w-[90%] lg:max-w-none">
        升等、教育變革、學術趨勢⸺《聯合報數位版》助你領先業界！
      </p>

      <!-- Feature Pills -->
      <ul
        class="flex flex-col sm:flex-row gap-3 list-none border border-white/50 rounded-(--radius-pill) px-(--space-3) py-(--space-2) lg:px-(--space-6) xl:px-(--space-10) bg-white/10"
        aria-label="訂閱亮點"
      >
        <li class="flex items-center gap-2 text-sm lg:text-base whitespace-nowrap">
          <img :src="noAdsLogo" class="w-[64px] h-[64px] shrink-0" alt="" aria-hidden="true" />

          <span>全站無跳出廣告</span>
        </li>
        <li class="flex items-center gap-2 text-sm lg:text-base whitespace-nowrap">
          <img :src="exclusiveLogo" class="w-[64px] h-[64px] shrink-0" alt="" aria-hidden="true" />
          <span>3000+篇高教獨家新聞</span>
        </li>
        <li class="flex items-center gap-2 text-sm lg:text-base whitespace-nowrap">
          <img :src="educatorLogo" class="w-[64px] h-[64px] shrink-0" alt="" aria-hidden="true" />
          <span>1萬+教育專業人士選擇</span>
        </li>
      </ul>
    </div>
  </section>
</template>

<script setup lang="ts">
import sectionOneBg from '@/assets/section-one-bg.jpg'
import educateLogoTitle from '@/assets/educate-logo-title.svg'
import educatorLogo from '@/assets/educator.svg'
import exclusiveLogo from '@/assets/exclusive.svg'
import noAdsLogo from '@/assets/no-ads.svg'
</script>

<style scoped></style>
