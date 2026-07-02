<template>
  <div
    class="compare__card w-[258px] lg:w-[280px] h-[458px] bg-white flex flex-col gap-2 p-4 shrink-0 border-2 border-[#9E9E9E] overflow-hidden snap-start"
    style="border-radius: var(--radius-brand)"
  >
    <!-- 卡片上半 -->
    <div class="card-top h-[198px] flex flex-col items-center gap-2">
      <div class="card-badge text-accent-red border-b-[3px] border-accent-red">
        {{ props.card.badge }}
      </div>
      <h3 class="card-top__title text-[30px] font-bold text-center whitespace-nowrap">
        {{ props.card.title }}
      </h3>
      <img :src="props.card.icon" class="card-top__icon w-24 h-24 object-contain" />
    </div>

    <!-- 卡片下半：白框比較表 -->
    <div
      class="card-bottom h-[220px] flex flex-col overflow-hidden bg-[#472785]"
      style="border-radius: 12px 12px 32px 12px"
    >
      <!-- 勝列（聯合報） -->
      <div class="card-row flex flex-row items-center flex-1 bg-white">
        <!-- 紅標籤 -->
        <div class="card-row__tag w-7 self-stretch flex items-center justify-center bg-[#CB2529]">
          <span class="text-[20px] font-bold text-white">勝</span>
        </div>
        <!-- 文字欄 -->
        <div class="card-row__body flex-1 flex flex-col gap-1 p-3 justify-center">
          <p class="card-row__name text-[20px] text-accent-red">聯合報數位版</p>
          <p class="card-row__desc text-black">{{ props.card.win }}</p>
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
          <p class="card-row__desc text-[18px] text-black">{{ props.card.lose }}</p>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
const props = defineProps<{
  card: {
    id: number
    badge: string
    title: string
    icon: string
    win: string
    lose: string
  }
}>()
</script>

<style scoped></style>
