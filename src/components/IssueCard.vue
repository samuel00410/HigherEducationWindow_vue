<template>
  <!--
    mobile  (< 744px)  : flex-row  → 左：標題＋bullets，右：icon
    desktop (≥ 1024px) : flex-col  → 上：icon，中：標題，下：bullets
  -->
  <article
    class="issue-card flex flex-row sm:flex-col gap-(--space-4) p-(--space-4) sm:p-(--space-6)"
  >
    <!-- 文字群組：mobile → 左（order-1）；744px+ → 下（order-2） -->
    <div class="flex flex-col gap-(--space-2) order-1 sm:order-2 flex-1 min-w-0">
      <h3 class="font-serif font-bold text-card-title sm:text-center">{{ title }}</h3>
      <ul class="flex flex-col gap-(--space-2) text-body text-(--c-text)">
        <li
          v-for="(bullet, index) in bullets"
          :key="index"
          class="flex items-start gap-2 before:content-['◆'] before:text-(--c-accent-red) before:shrink-0 before:text-[10px] before:mt-[5px]"
          :class="[index > 0 ? 'hidden sm:flex' : 'flex']"
        >
          <span>{{ bullet }}</span>
        </li>
      </ul>
    </div>

    <!-- icon：mobile → 右（order-2）；744px+ → 上（order-1） -->
    <img
      class="w-[48px] sm:w-[64px] shrink-0 self-end sm:self-center order-2 sm:order-1"
      :src="icon"
      alt=""
      aria-hidden="true"
    />
  </article>
</template>

<script setup lang="ts">
defineProps<{
  icon: string // SVG 圖示路徑
  title: string // 卡片標題
  bullets: string[] // 條列內容（陣列）
}>()
</script>

<style scoped>
.issue-card {
  position: relative;
}

/* 每張卡下方橫向漸層線（最後一張不加） */
.issue-card:not(:last-child)::after {
  content: '';
  position: absolute;
  bottom: 0;
  left: 0;
  right: 0;
  height: 1px;
  background: linear-gradient(to right, #f2eeff, #472785, #f2eeff);
}

@media (min-width: 744px) {
  .issue-card:not(:last-child)::after {
    /* 改成右側垂直線 */
    width: 1px;
    bottom: auto;
    top: 0;
    left: auto;
    right: 0;
    height: 100%;
    background: linear-gradient(to bottom, #f2eeff, #472785, #f2eeff);
  }
}
</style>
