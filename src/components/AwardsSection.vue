<template>
  <section
    class="awards bg-white py-(--section-pad-y) px-(--gutter) flex flex-col items-center gap-(--space-4)"
  >
    <div class="w-full max-w-(--container-max) mx-auto flex flex-col items-center gap-(--space-4)">

      <!-- section heading -->
      <header class="awards__heading w-full text-left lg:text-center flex flex-col gap-[6px]">
        <h2 class="font-serif font-bold text-[24px] lg:text-[44px] text-brand">
          國內外大獎肯定
        </h2>
        <!-- subtitle: mobile only (< 1024px) -->
        <p class="font-sans font-light text-[16px] lg:text-[26px] text-violet-soft lg:hidden">
          《聯合報》報導實力獲獎無數
        </p>
        <!-- subtitle: desktop only (≥ 1024px) -->
        <p class="font-sans font-light text-[16px] lg:text-[26px] text-violet-soft hidden lg:block">
          《聯合報數位版》獲獎無數
        </p>
      </header>

      <!-- awards grid: mobile = flex-col, sm = flex-row (3 cards side by side), card flip at lg -->
      <div class="awards__grid flex flex-col sm:flex-row items-start gap-(--space-4) w-full">

        <!-- Card 1 -->
        <article class="award-card flex flex-row lg:flex-col items-start gap-[8px] w-full sm:flex-1">
          <div
            class="award-card__pic w-[100px] shrink-0 self-stretch lg:w-auto lg:h-[340px] lg:self-stretch overflow-hidden"
          >
            <img
              :src="awardIcon1"
              alt="2024世界新聞出版協會 最佳訂閱增長計畫 全球冠軍"
              class="w-full h-full object-cover object-center block bg-(--c-line)"
            />
          </div>
          <p
            class="font-sans font-light text-[20px] leading-[1.5] text-(--c-text) lg:text-center flex-1 self-stretch"
          >
            2024世界新聞出版協會<br />
            最佳訂閱增長計畫 全球冠軍
          </p>
        </article>

        <!-- Card 2 -->
        <article class="award-card flex flex-row lg:flex-col items-start gap-[8px] w-full sm:flex-1">
          <div
            class="award-card__pic w-[100px] shrink-0 self-stretch lg:w-auto lg:h-[340px] lg:self-stretch overflow-hidden"
          >
            <img
              :src="awardIcon2"
              alt="2024國際新聞媒體協會 最佳訂閱增長計畫 全球冠軍"
              class="w-full h-full object-cover object-center block bg-(--c-line)"
            />
          </div>
          <p
            class="font-sans font-light text-[20px] leading-[1.5] text-(--c-text) lg:text-center flex-1 self-stretch"
          >
            2024國際新聞媒體協會<br />
            最佳訂閱增長計畫 全球冠軍
          </p>
        </article>

        <!-- Card 3 -->
        <article class="award-card flex flex-row lg:flex-col items-start gap-[8px] w-full sm:flex-1">
          <div
            class="award-card__pic w-[100px] shrink-0 self-stretch lg:w-auto lg:h-[340px] lg:self-stretch overflow-hidden"
          >
            <img
              :src="awardIcon3"
              alt="2024世界新聞出版協會 最佳數位訂閱/讀者收入專案 亞洲冠軍"
              class="w-full h-full object-cover object-center block bg-(--c-line)"
            />
          </div>
          <p
            class="font-sans font-light text-[20px] leading-[1.5] text-(--c-text) lg:text-center flex-1 self-stretch"
          >
            2024世界新聞出版協會<br />
            最佳數位訂閱/讀者收入專案 亞洲冠軍
          </p>
        </article>

      </div>
      <!-- /.awards__grid -->

    </div>
  </section>
</template>

<script setup lang="ts">
import awardIcon1 from '@/assets/award-1.jpg'
import awardIcon2 from '@/assets/award-2.jpg'
import awardIcon3 from '@/assets/award-3.jpg'
</script>

<style scoped></style>
