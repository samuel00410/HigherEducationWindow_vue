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
    <IssuesSection />

    <!-- D: 你其實比想像中更需要訂閱 — 深紫 #3C1375，4張 answer-card -->
    <section
      class="bg-brand-deep rounded-[24px] xl:rounded-[80px] py-(--section-pad-y) px-(--gutter)"
    >
      <div class="w-full max-w-(--container-max) mx-auto flex flex-col gap-[16px] md:gap-[24px]">
        <!-- section heading -->
        <header class="flex flex-col gap-[8px]">
          <h2
            class="font-serif font-bold text-[24px] md:text-[44px] text-peach text-left sm:text-center"
          >
            你其實比想像中更需要訂閱！
          </h2>
          <p
            class="font-sans font-light text-[16px] md:text-[26px] text-peach text-left sm:text-center"
          >
            這些疑問將在《聯合報數位版》找到解答
          </p>
        </header>

        <!-- answer grid：4張卡，桌面 2×2，行動 1×4 -->
        <ul class="list-none m-0 p-0 grid grid-cols-1 gap-[32px] sm:grid-cols-2 sm:gap-[56px]">
          <!-- Card 1：台大 / 頂大學者、教授 -->
          <li
            class="flex flex-col overflow-hidden shadow-[inset_0_0_0_1px_#F2EEFF]"
            style="border-radius: var(--radius-brand)"
          >
            <div class="flex flex-row items-start gap-[8px] min-h-[54px] px-[8px] py-[12px]">
              <img
                :src="questionIcon"
                class="w-[24px] h-[24px] shrink-0 mt-[4px]"
                alt=""
                aria-hidden="true"
              />
              <span class="font-light text-[20px] text-white leading-[1.4]"
                >學術升等標準愈來愈高，我該怎麼準備？</span
              >
            </div>
            <div
              class="bg-white flex-1 flex flex-row items-center gap-2 p-3 md:p-4 rounded-br-3xl rounded-bl-lg md:rounded-br-[40px]"
            >
              <div class="flex flex-col gap-[8px] flex-1 justify-center">
                <p class="font-bold text-[18px] text-brand">台大 / 頂大學者、教授</p>
                <p class="hidden sm:block font-light text-[20px] text-(--c-text)">
                  掌握升等標準、學界趨勢、政策變動。
                </p>
              </div>
              <img
                :src="avatarProfessor"
                class="w-[55px] h-[64px] md:w-[80px] md:h-[93px] object-contain shrink-0"
                alt=""
                aria-hidden="true"
              />
            </div>
          </li>

          <!-- Card 2：中學 / 高中老師 -->
          <li
            class="flex flex-col overflow-hidden shadow-[inset_0_0_0_1px_#F2EEFF]"
            style="border-radius: var(--radius-brand)"
          >
            <div class="flex flex-row items-start gap-[8px] min-h-[54px] px-[8px] py-[12px]">
              <img
                :src="questionIcon"
                class="w-[24px] h-[24px] shrink-0 mt-[4px]"
                alt=""
                aria-hidden="true"
              />
              <span class="font-light text-[20px] text-white leading-[1.4]"
                >學校政策不斷改變，我怎麼跟上？</span
              >
            </div>
            <div
              class="bg-white flex-1 flex flex-row items-center gap-2 p-3 md:p-4 rounded-br-3xl rounded-bl-lg md:rounded-br-[40px]"
            >
              <div class="flex flex-col gap-[8px] flex-1 justify-center">
                <p class="font-bold text-[18px] text-brand">中學 / 高中老師</p>
                <p class="hidden sm:block font-light text-[20px] text-(--c-text)">
                  了解教育政策走向與課綱變革。
                </p>
              </div>
              <img
                :src="avatarTeacher"
                class="w-[55px] h-[64px] md:w-[80px] md:h-[93px] object-contain shrink-0"
                alt=""
                aria-hidden="true"
              />
            </div>
          </li>

          <!-- Card 3：博士生 / 博士後研究員 -->
          <li
            class="flex flex-col overflow-hidden shadow-[inset_0_0_0_1px_#F2EEFF]"
            style="border-radius: var(--radius-brand)"
          >
            <div class="flex flex-row items-start gap-[8px] min-h-[54px] px-[8px] py-[12px]">
              <img
                :src="questionIcon"
                class="w-[24px] h-[24px] shrink-0 mt-[4px]"
                alt=""
                aria-hidden="true"
              />
              <span class="font-light text-[20px] text-white leading-[1.4]"
                >博士畢業後，學術職涯怎麼走？</span
              >
            </div>
            <div
              class="bg-white flex-1 flex flex-row items-center gap-2 p-3 md:p-4 rounded-br-3xl rounded-bl-lg md:rounded-br-[40px]"
            >
              <div class="flex flex-col gap-[8px] flex-1 justify-center">
                <p class="font-bold text-[18px] text-brand">博士生 / 博士後研究員</p>
                <p class="hidden sm:block font-light text-[20px] text-(--c-text)">
                  掌握學術就業市場與研究趨勢。
                </p>
              </div>
              <img
                :src="avatarPhd"
                class="w-[55px] h-[64px] md:w-[80px] md:h-[93px] object-contain shrink-0"
                alt=""
                aria-hidden="true"
              />
            </div>
          </li>

          <!-- Card 4：學生家長 -->
          <li
            class="flex flex-col overflow-hidden shadow-[inset_0_0_0_1px_#F2EEFF]"
            style="border-radius: var(--radius-brand)"
          >
            <div class="flex flex-row items-start gap-[8px] min-h-[54px] px-[8px] py-[12px]">
              <img
                :src="questionIcon"
                class="w-[24px] h-[24px] shrink-0 mt-[4px]"
                alt=""
                aria-hidden="true"
              />
              <span class="font-light text-[20px] text-white leading-[1.4]"
                >孩子升學壓力這麼大，我能幫什麼？</span
              >
            </div>
            <div
              class="bg-white flex-1 flex flex-row items-center gap-2 p-3 md:p-4 rounded-br-3xl rounded-bl-lg md:rounded-br-[40px]"
            >
              <div class="flex flex-col gap-[8px] flex-1 justify-center">
                <p class="font-bold text-[18px] text-brand">學生家長</p>
                <p class="hidden sm:block font-light text-[20px] text-(--c-text)">
                  了解招生趨勢、掌握升學資訊。
                </p>
              </div>
              <img
                :src="avatarParents"
                class="w-[55px] h-[64px] md:w-[80px] md:h-[93px] object-contain shrink-0"
                alt=""
                aria-hidden="true"
              />
            </div>
          </li>
        </ul>
      </div>
    </section>

    <!-- E: 口碑陣容 — 白底，2張記者 author-card -->
    <section
      class="authors bg-white px-(--space-3) pt-(--space-5) pb-(--space-6) md:px-(--space-7) md:pt-(--space-6) md:pb-(--space-9)"
    >
      <div class="container">
        <!-- title -->
        <header class="text-left sm:text-center">
          <h2 class="text-brand font-serif font-bold text-[24px] md:text-[44px] text-(--c-text)">
            口碑陣容
          </h2>
          <p
            class="text-violet-soft font-sans font-light text-[16px] md:text-[26px] text-(--c-text) mt-[6px]"
          >
            深耕高教領域記者
          </p>
        </header>

        <!-- 記者卡 -->
        <div class="flex flex-col md:flex-row gap-(--space-5) md:gap-(--space-8) mt-(--space-6)">
          <article class="flex gap-[20px] md:flex-1 items-start md:items-center">
            <img
              :src="authorFeng"
              class="w-[90px] h-[90px] rounded-[24px] object-cover object-[50%_22.6%] md:w-[180px] md:h-auto md:self-stretch"
              alt="記者馮靖惠"
            />
            <div class="flex-1 min-w-0 flex flex-col gap-[8px]">
              <div class="flex flex-wrap md:flex-nowrap gap-2 md:gap-3">
                <span class="font-sans font-bold text-[30px] whitespace-nowrap text-(--c-text)"
                  >馮靖惠</span
                >
                <span
                  class="inline-flex items-center justify-center rounded-sm px-3 whitespace-nowrap font-sans font-normal text-[18px] text-brand bg-brand-tint md:text-[24px]"
                  >深入教育議題</span
                >
              </div>
              <p
                class="font-sans font-light text-[18px] leading-normal text-(--c-text) md:leading-none"
              >
                主跑教育，長期關心高教、國教、族群等議題。跑新聞致力於挖掘真相。
              </p>
            </div>
          </article>
          <article class="flex gap-[20px] md:flex-1 items-start md:items-center">
            <img
              :src="authorHsu"
              class="w-[90px] h-[90px] rounded-[24px] object-cover object-[50%_22.6%] md:w-[180px] md:h-auto md:self-stretch"
              alt="記者許維寧"
            />
            <div class="flex-1 min-w-0 flex flex-col gap-[8px]">
              <div class="flex flex-wrap md:flex-nowrap gap-2 md:gap-3">
                <span class="font-sans font-bold text-[30px] whitespace-nowrap text-(--c-text)"
                  >許維寧</span
                >
                <span
                  class="inline-flex items-center justify-center rounded-sm px-3 whitespace-nowrap font-sans font-normal text-[18px] text-brand bg-brand-tint md:text-[24px]"
                  >深入教育議題</span
                >
              </div>
              <p
                class="font-sans font-light text-[18px] leading-normal text-(--c-text) md:leading-none"
              >
                主跑教育，長期關心高教、國教、族群等議題。跑新聞致力於挖掘真相。
              </p>
            </div>
          </article>
        </div>
      </div>
    </section>

    <!-- F: 限時優惠 — 深紫 #3C1375，2張 discount-card + CTA -->
    <section
      class="discount bg-brand-deep rounded-t-[24px] xl:rounded-t-[80px] py-(--section-pad-y) px-(--gutter)"
    >
      <div class="container">
        <!-- title -->
        <h2 class="text-left sm:text-center font-serif font-bold text-h2 text-peach">
          【限時】5/25前享雙重優惠
        </h2>

        <!-- 優惠卡片 -->
        <div class="my-[24px] flex flex-col sm:flex-row sm:items-start gap-(--space-6)">
          <article class="relative flex-1 flex flex-col items-center gap-2">
            <!-- 優惠標籤 ribbon：取代原本的 <img :src="triangleIcon" .../> -->
            <div class="absolute -left-[15px] -top-[10px] z-10 flex items-center">
              <!-- 1. 標籤文字方塊 -->
              <span
                class="bg-peach text-(--c-brand) w-[103px] h-[60px] flex items-center justify-center rounded-l-[8px] font-serif font-bold text-[24px] whitespace-nowrap"
                >優惠1</span
              >
              <!-- 2. 三角形箭頭 -->
              <span
                class="bg-peach w-[17px] h-[60px] shrink-0"
                style="clip-path: polygon(100% 0, 0% 0, 0% 50%, 0% 100%, 100% 100%, 50% 50%)"
                aria-hidden="true"
              ></span>
            </div>

            <div class="rounded-[24px] overflow-hidden">
              <img :src="discount1" alt="" class="w-full aspect-square object-cover" />
            </div>

            <p class="text-white font-bold text-[24px]">【全年方案】首年85折</p>
          </article>
          <article class="relative flex-1 flex flex-col items-center gap-2">
            <!-- 優惠標籤 ribbon：取代原本的 <img :src="triangleIcon" .../> -->
            <div class="absolute -left-[15px] -top-[10px] z-10 flex items-center">
              <!-- 1. 標籤文字方塊 -->
              <span
                class="bg-peach text-(--c-brand) w-[103px] h-[60px] flex items-center justify-center rounded-l-[8px] font-serif font-bold text-[24px] whitespace-nowrap"
                >優惠2</span
              >
              <!-- 2. 三角形箭頭 -->
              <span
                class="bg-peach w-[17px] h-[60px] shrink-0"
                style="clip-path: polygon(100% 0, 0% 0, 0% 50%, 0% 100%, 100% 100%, 50% 50%)"
                aria-hidden="true"
              ></span>
            </div>

            <div class="rounded-[24px] overflow-hidden">
              <img :src="discount2" alt="" class="w-full aspect-square object-cover" />
            </div>
            <div class="text-center">
              <p class="text-white font-bold text-[24px]">Brother旗艦版標籤機</p>
              <p class="text-white text-[18px]">（售價 NT$ 14,710）</p>
            </div>
          </article>
        </div>

        <!--  CTA 區 -->
        <div class="flex flex-col items-center gap-[24px]">
          <a
            href="#"
            class="flex items-center justify-center gap-[8px] rounded-[8px] text-[30px] sm:text-[24px] px-(--space-5) py-(--space-3) w-full sm:w-[216px] h-[56px] md:h-[65px] bg-accent-red text-white"
            >立即訂閱
            <img :src="arrowIcon" class="w-[24px] h-[24px] brightness-0 invert" />
          </a>

          <div class="text-white text-left">
            <p>專人訂閱服務電話： 02-2649-1681 按 1（服務時間09:00至18:00）</p>
          </div>
        </div>
      </div>
    </section>

    <!-- G: 他網 vs 聯合報數位版 — 品牌紫 #472785 + 上圓角，6張 compare-card（橫向滑動） -->
    <CompareSection />

    <!-- H: 國內外大獎肯定 — 白底，3張 award-card -->
    <section class="awards"></section>

    <!-- 新聞不只是閱讀，而是你的專屬資料庫！ -->
    <section>
      <NmdService title="新聞不只是閱讀，而是你的專屬資料庫！" title-color="var(--c-brand)">
        <NmdServiceList />
      </NmdService>
    </section>
  </main>

  <footer class="site-footer">
    <NmdDigitalFooter />
  </footer>
</template>

<script setup lang="ts">
import {
  NmdDigitalHeader,
  NmdSubTextHide,
  NmdService,
  NmdServiceList,
  NmdDigitalFooter,
} from '@udn-digital-center/common-components'
import HeroSection from '@/components/HeroSection.vue'
import CompareSection from '@/components/CompareSection.vue'
import IssuesSection from '@/components/IssuesSection.vue'
import questionIcon from '@/assets/question-icon.svg'
import avatarProfessor from '@/assets/avatar-professor.svg'
import avatarTeacher from '@/assets/avatar-teacher.svg'
import avatarPhd from '@/assets/avatar-PhD.svg'
import avatarParents from '@/assets/avatar-parents.svg'
import authorFeng from '@/assets/author-feng.jpg'
import authorHsu from '@/assets/author-hsu.jpg'
import discount1 from '@/assets/discount-1.png'
import discount2 from '@/assets/discount-2.png'
import arrowIcon from '@/assets/arrow-icon.svg'
</script>

<style scoped></style>
