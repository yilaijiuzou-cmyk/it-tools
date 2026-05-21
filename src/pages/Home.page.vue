<script setup lang="ts">
import { useHead } from '@vueuse/head';

useHead({ title: '一六工具箱 - 开发者的效率利器' });
</script>

<template>
  <div class="hero-landing">
    <div class="hero-bg" />
    <div class="hero-overlay" />
    <div class="hero-content">
      <h1 class="hero-title">一六工具箱</h1>
      <div class="hero-line" />
      <p class="hero-slogan">{{ $t('home.subtitle') }}</p>
    </div>
  </div>
</template>

<style scoped lang="less">
.hero-landing {
  position: relative;
  width: 100%;
  height: 100vh;
  overflow: hidden;
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
  filter: brightness(0.85);
}

.hero-overlay {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: linear-gradient(
    180deg,
    rgba(0, 0, 0, 0.2) 0%,
    rgba(0, 0, 0, 0.5) 100%
  );
}

.hero-content {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  text-align: center;
  color: #fff;
  width: 90%;
}

.hero-title {
  font-size: 52px;
  font-weight: 300;
  letter-spacing: 12px;
  margin-bottom: 24px;
  text-shadow: 0 4px 20px rgba(0, 0, 0, 0.4);
}

.hero-line {
  width: 50px;
  height: 1px;
  background: rgba(255, 255, 255, 0.5);
  margin: 0 auto 24px;
}

.hero-slogan {
  font-size: 18px;
  font-weight: 300;
  letter-spacing: 4px;
  color: rgba(255, 255, 255, 0.85);
  text-shadow: 0 2px 10px rgba(0, 0, 0, 0.3);
}
</style>
