<script setup lang="ts">
import { IconDragDrop } from '@tabler/icons-vue';
import { useHead } from '@vueuse/head';
import { computed } from 'vue';
import Draggable from 'vuedraggable';
import ToolCard from '../components/ToolCard.vue';
import { useToolStore } from '@/tools/tools.store';
import { config } from '@/config';

const toolStore = useToolStore();
const isLandingSite = computed(() => config.app.siteMode === 'home');

useHead({ title: isLandingSite.value ? '一六工具箱 - 高效便捷，触手可及' : '一六工具箱 - 开发者的效率利器' });
const { t } = useI18n();

const favoriteTools = computed(() => toolStore.favoriteTools);

function onUpdateFavoriteTools() {
  toolStore.updateFavoriteTools(favoriteTools.value);
}
</script>

<template>
  <div v-if="isLandingSite" class="landing">
    <section class="hero">
      <div class="hero-bg" />
      <div class="hero-overlay" />
      <div class="hero-content">
        <h1 class="hero-title">一六工具箱</h1>
        <div class="hero-line" />
        <p class="hero-slogan">{{ $t('home.subtitle') }}</p>
      </div>
    </section>

    <section class="tools-section">
      <div class="tools-grid">
        <a href="https://ihgpoints.yilaijiuzou.com" target="_blank" class="tool-banner banner-link">
          <div class="banner-number">01</div>
          <div class="banner-icon">&#9702;</div>
          <h3 class="banner-title">常用工具</h3>
          <p class="banner-desc">日常开发工具集合</p>
        </a>
        <div class="tool-banner">
          <div class="banner-number">02</div>
          <div class="banner-icon">&#9702;</div>
          <h3 class="banner-title">更多工具</h3>
          <p class="banner-desc">更多实用开发工具</p>
        </div>
        <div class="tool-banner">
          <div class="banner-number">03</div>
          <div class="banner-icon">&#9702;</div>
          <h3 class="banner-title">即将上线</h3>
          <p class="banner-desc">更多工具陆续上线</p>
        </div>
      </div>
    </section>
  </div>

  <div v-else class="pt-50px">
    <div class="grid-wrapper">
      <transition name="height">
        <div v-if="toolStore.favoriteTools.length > 0">
          <h3 class="mb-5px mt-25px text-neutral-400 font-500">
            {{ $t('home.categories.favoriteTools') }}
            <c-tooltip :tooltip="$t('home.categories.favoritesDndToolTip')">
              <n-icon :component="IconDragDrop" size="18" />
            </c-tooltip>
          </h3>
          <Draggable
            :list="favoriteTools"
            class="grid grid-cols-1 gap-12px lg:grid-cols-3 md:grid-cols-3 sm:grid-cols-2 xl:grid-cols-4"
            ghost-class="ghost-favorites-draggable"
            item-key="name"
            @end="onUpdateFavoriteTools"
          >
            <template #item="{ element: tool }">
              <ToolCard :tool="tool" />
            </template>
          </Draggable>
        </div>
      </transition>

      <div v-if="toolStore.newTools.length > 0">
        <h3 class="mb-5px mt-25px text-neutral-400 font-500">
          {{ t('home.categories.newestTools') }}
        </h3>
        <div class="grid grid-cols-1 gap-12px lg:grid-cols-3 md:grid-cols-3 sm:grid-cols-2 xl:grid-cols-4">
          <ToolCard v-for="tool in toolStore.newTools" :key="tool.name" :tool="tool" />
        </div>
      </div>

      <h3 class="mb-5px mt-25px text-neutral-400 font-500">
        {{ $t('home.categories.allTools') }}
      </h3>
      <div class="grid grid-cols-1 gap-12px lg:grid-cols-3 md:grid-cols-3 sm:grid-cols-2 xl:grid-cols-4">
        <ToolCard v-for="tool in toolStore.tools" :key="tool.name" :tool="tool" />
      </div>
    </div>
  </div>
</template>

<style scoped lang="less">
.landing {
  width: 100%;
  min-height: 100vh;
}

.hero {
  position: relative;
  width: 100%;
  height: 100vh;
  display: flex;
  align-items: center;
  justify-content: center;
}

.hero-bg {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-image: url('/hero-bg.jpg');
  background-size: cover;
  background-position: center;
  filter: brightness(0.75);
}

.hero-overlay {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: linear-gradient(
    180deg,
    rgba(0, 0, 0, 0.3) 0%,
    rgba(0, 0, 0, 0.5) 60%,
    rgba(0, 0, 0, 0.7) 100%
  );
}

.hero-content {
  position: relative;
  z-index: 2;
  text-align: center;
  color: #fff;
}

.hero-title {
  font-size: 52px;
  font-weight: 300;
  letter-spacing: 12px;
  margin: 0;
  text-shadow: 0 4px 20px rgba(0, 0, 0, 0.4);
}

.hero-line {
  width: 50px;
  height: 1px;
  background: rgba(255, 255, 255, 0.5);
  margin: 24px auto;
}

.hero-slogan {
  font-size: 18px;
  font-weight: 300;
  letter-spacing: 4px;
  color: rgba(255, 255, 255, 0.85);
  text-shadow: 0 2px 10px rgba(0, 0, 0, 0.3);
}

.tools-section {
  padding: 80px 26px 100px;
  background: #0f1419;
}

.tools-grid {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 24px;
  max-width: 900px;
  margin: 0 auto;
}

.tool-banner {
  background: rgba(255, 255, 255, 0.04);
  border: 1px solid rgba(255, 255, 255, 0.08);
  border-radius: 12px;
  padding: 40px 20px;
  text-align: center;
  transition: border-color 0.3s ease, background 0.3s ease;
  cursor: default;
  text-decoration: none;
  display: block;
  color: inherit;

  &:hover {
    border-color: rgba(255, 255, 255, 0.2);
  }
}

.banner-link {
  cursor: pointer;

  &:hover {
    border-color: rgba(255, 255, 255, 0.3);
    background: rgba(255, 255, 255, 0.07);
  }
}

.banner-number {
  font-size: 14px;
  color: rgba(255, 255, 255, 0.3);
  letter-spacing: 4px;
  margin-bottom: 16px;
}

.banner-icon {
  font-size: 32px;
  color: rgba(255, 255, 255, 0.2);
  margin-bottom: 16px;
}

.banner-title {
  font-size: 18px;
  font-weight: 500;
  color: rgba(255, 255, 255, 0.7);
  margin: 0 0 8px;
}

.banner-desc {
  font-size: 14px;
  color: rgba(255, 255, 255, 0.35);
  margin: 0;
}

.height-enter-active,
.height-leave-active {
  transition: all 0.5s ease-in-out;
  overflow: hidden;
  max-height: 500px;
}

.height-enter-from,
.height-leave-to {
  max-height: 42px;
  overflow: hidden;
  opacity: 0;
  margin-bottom: 0;
}

.ghost-favorites-draggable {
  opacity: 0.4;
  background-color: #ccc;
  border: 2px dashed #666;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.2);
  transform: scale(1.1);
  animation: ghost-favorites-draggable-animation 0.2s ease-out;
}

@keyframes ghost-favorites-draggable-animation {
  0% {
    opacity: 0;
    transform: scale(0.9);
  }
  100% {
    opacity: 0.4;
    transform: scale(1);
  }
}
</style>
