<script setup lang="ts">
import { useHead } from '@vueuse/head';

useHead({ title: '一六工具箱 - 开发者的效率利器' });
</script>

<template>
  <div class="landing">
    <section class="hero">
      <div class="hero-bg" />
      <div class="hero-overlay" />
      <div class="hero-content">
        <h1 class="hero-title">一六工具箱</h1>
        <div class="hero-line" />
        <p class="hero-slogan">让每一行代码，都事半功倍</p>
      </div>
    </section>

    <section class="tools-section">
      <div class="tools-grid">
        <div class="tool-banner">
          <div class="banner-number">01</div>
          <div class="banner-icon">&#9702;</div>
          <h3 class="banner-title">常用工具</h3>
          <p class="banner-desc">常用开发工具集合</p>
        </div>
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
  transition: border-color 0.3s ease;
  cursor: default;

  &:hover {
    border-color: rgba(255, 255, 255, 0.2);
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
</style>
