<template>
  <!-- A: Header Bar — 品牌紫 #472785，高 60px，logo + 訂閱 CTA -->
  <NmdDigitalHeader
    class="header-digital"
    custom-bg="var(--c-brand)"
    url-query="?utm_source=landing_page"
    text-theme="white"
  >
    訂閱<NmdSubTextHide>享</NmdSubTextHide>首年優惠
  </NmdDigitalHeader>

  <main>
    <!-- B: Hero / section01 — 全幅背景照，高教視窗標題 + feature pills ×3 -->
    <HeroSection />

    <!-- C: 3大高教議題 — 白底 + 淺紫卡，3欄 issue-card + CTA 條 -->
    <section class="issues"></section>

    <!-- D: 你其實比想像中更需要訂閱 — 深紫 #3C1375，4張 answer-card -->
    <section class="need"></section>

    <!-- E: 口碑陣容 — 白底，2張記者 author-card -->
    <section class="authors"></section>

    <!-- F: 限時優惠 — 深紫 #3C1375，2張 discount-card + CTA -->
    <section class="discount"></section>

    <!-- G: 他網 vs 聯合報數位版 — 品牌紫 #472785 + 上圓角，6張 compare-card（橫向滑動） -->
    <section class="compare"></section>

    <!-- H: 國內外大獎肯定 — 白底，3張 award-card -->
    <section class="awards"></section>
  </main>

  <footer class="site-footer">
    <!-- I: Footer 資訊區 — 連結 / 聯絡資訊（共用元件） -->
    <div class="footer-info"></div>

    <!-- J: Educate Footer — 版權列 -->
    <div class="footer-bar"></div>
  </footer>
</template>

<script setup lang="ts">
import { NmdDigitalHeader, NmdSubTextHide } from '@udn-digital-center/common-components'
import HeroSection from '@/components/HeroSection.vue'
</script>

<style scoped></style>
