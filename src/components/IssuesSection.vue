<template>
  <section class="issues">
    <div class="container">
      <!-- heading -->
      <h2 class="text-brand text-h2 font-serif font-bold text-center">最受關注的3大高教議題</h2>
      <p class="text-h2-sub text-violet-soft text-center">
        不只是新聞，更是學術與教育決策的關鍵情報！
      </p>

      <!-- issues__grid：3個 IssueCard，用淺紫卡容器包起來 -->
      <div
        style="border-radius: var(--radius-brand)"
        class="bg-brand-tint grid grid-cols-1 sm:grid-cols-3 mt-(--space-6) overflow-hidden"
      >
        <IssueCard
          :icon="eduIconPromotion"
          title="學術升等指南"
          :bullets="['台大教授延退爭議，影響年輕學者職涯。', '破解「內卷」困境，提升升等競爭力。']"
        />
        <IssueCard
          :icon="eduIconTrend"
          title="學界勞資與退休趨勢"
          :bullets="[
            '台大、清大、成大等頂大教授升等關鍵資訊。',
            '破解「內卷」困境，提升升等競爭力。',
          ]"
        />

        <IssueCard
          :icon="eduIconAdmissions"
          title="招生與學術趨勢"
          :bullets="['台積電熱潮如何影響頂尖科系報考率？', '少子化衝擊，私校與國立大學競爭態勢。']"
        />
      </div>

      <!-- issues__cta：桃色 CTA 條 -->
      <aside
        style="border-radius: var(--radius-brand)"
        class="issue-cta mt-(--space-3) flex flex-col md:flex-row sm:items-center gap-(--space-3) md:gap-(--space-4) px-(--space-3) md:px-(--space-6) py-(--space-4) bg-(--c-peach)"
      >
        <div class="flex-1">
          <p class="font-bold text-(--c-text)">訂閱後，你將能搶先閱讀這些不公開的深入分析！</p>

          <ul class="mt-(--space-3) flex flex-row flex-wrap gap-(--space-3)">
            <li class="flex items-center gap-[4px]">
              <img :src="noAdsIcon" class="w-[24px] h-[24px] shrink-0" alt="無廣告干擾" />
              <span>無廣告干擾</span>
            </li>
            <li class="flex items-center gap-[4px]">
              <img :src="exclusiveIcon" class="w-[24px] h-[24px] shrink-0" alt="獨家內容" />
              <span>獨家內容</span>
            </li>
            <li class="flex items-center gap-[4px]">
              <img :src="reportIcon" class="w-[24px] h-[24px] shrink-0" alt="深度報導" />
              <span>深度報導</span>
            </li>
          </ul>
        </div>

        <a
          href="#"
          class="px-(--space-5) py-(--space-3) w-full sm:w-[282px] h-[56px] md:h-[65px] md:self-center md:shrink-0 flex items-center justify-center bg-accent-red rounded-[6px] text-white"
          >訂閱首年85折
          <img :src="arrowIcon" class="w-[24px] h-[24px] brightness-0 invert" />
        </a>
      </aside>
    </div>
  </section>
</template>

<script setup lang="ts">
import IssueCard from '@/components/IssueCard.vue'
import eduIconPromotion from '@/assets/edu-icon-promotion.svg'
import eduIconTrend from '@/assets/edu-icon-trend.svg'
import eduIconAdmissions from '@/assets/edu-icon-admissions.svg'
import noAdsIcon from '@/assets/no-ads-s.svg'
import exclusiveIcon from '@/assets/exclusive-s.svg'
import reportIcon from '@/assets/report-s.svg'
import arrowIcon from '@/assets/arrow-icon.svg'
</script>

<style lang="scss" scoped></style>
