<template>
  <main class="main-container">
    <!-- SVG filter untuk efek listrik -->
    <svg class="svg-container">
      <defs>
        <filter id="turbulent-displace" colorInterpolationFilters="sRGB" x="-20%" y="-20%" width="140%" height="140%">
          <feTurbulence type="turbulence" baseFrequency="0.02" numOctaves="10" result="noise1" seed="1" />
          <feOffset in="noise1" dx="0" dy="0" result="offsetNoise1">
            <animate attributeName="dy" values="700; 0" dur="6s" repeatCount="indefinite" calcMode="linear" />
          </feOffset>

          <feTurbulence type="turbulence" baseFrequency="0.02" numOctaves="10" result="noise2" seed="1" />
          <feOffset in="noise2" dx="0" dy="0" result="offsetNoise2">
            <animate attributeName="dy" values="0; -700" dur="6s" repeatCount="indefinite" calcMode="linear" />
          </feOffset>

          <feTurbulence type="turbulence" baseFrequency="0.02" numOctaves="10" result="noise1" seed="2" />
          <feOffset in="noise1" dx="0" dy="0" result="offsetNoise3">
            <animate attributeName="dx" values="490; 0" dur="6s" repeatCount="indefinite" calcMode="linear" />
          </feOffset>

          <feTurbulence type="turbulence" baseFrequency="0.02" numOctaves="10" result="noise2" seed="2" />
          <feOffset in="noise2" dx="0" dy="0" result="offsetNoise4">
            <animate attributeName="dx" values="0; -490" dur="6s" repeatCount="indefinite" calcMode="linear" />
          </feOffset>

          <feComposite in="offsetNoise1" in2="offsetNoise2" result="part1" />
          <feComposite in="offsetNoise3" in2="offsetNoise4" result="part2" />
          <feBlend in="part1" in2="part2" mode="color-dodge" result="combinedNoise" />
          <feDisplacementMap in="SourceGraphic" in2="combinedNoise" scale="30" xChannelSelector="R" yChannelSelector="B" />
        </filter>
      </defs>
    </svg>

    <!-- CARD -->
    <div class="card-container electric-animated">
      <div class="inner-container">
        <div class="border-outer">
          <div class="main-card"></div>
        </div>
        <div class="glow-layer-1 pulse"></div>
        <div class="glow-layer-2 pulse delay"></div>
      </div>

      <div class="overlay-1"></div>
      <div class="overlay-2"></div>
      <div class="background-glow moving-glow"></div>

      <div class="content-container">
        <div class="content-top">
          <div class="scrollbar-glass">Electric</div>
          <p class="title">Lightning Pulse</p>
        </div>
        <hr class="divider" />
        <div class="content-bottom">
          <p class="description">Electric border card with pulsing energy and sparks.</p>
        </div>
      </div>
    </div>
  </main>
</template>

<script setup>
// Tidak ada script khusus
</script>

<style scoped>
:root {
  --electric-border-color: #6a48ff;
  --electric-light-color: oklch(from var(--electric-border-color) l c h);
  --gradient-color: oklch(from var(--electric-border-color) 0.3 calc(c / 2) h / 0.4);
  --color-neutral-900: oklch(0.185 0 0);
}
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
body {
  font-family: system-ui, -apple-system, sans-serif;
  background-color: oklch(0.145 0 0);
  color: oklch(0.985 0 0);
  height: 100vh;
  overflow: hidden;
}
.main-container {
  display: flex;
  align-items: center;
  justify-content: center;
  height: 100vh;
}
.svg-container {
  position: absolute;
}

/* === CARD CORE === */
.card-container {
  padding: 2px;
  border-radius: 24px;
  position: relative;
  background: linear-gradient(-30deg, var(--gradient-color), transparent, var(--gradient-color)),
    linear-gradient(to bottom, var(--color-neutral-900), var(--color-neutral-900));
  animation: subtleVibrate 4s infinite ease-in-out;
}

/* Vibrating motion (soft electrical vibration) */
@keyframes subtleVibrate {
  0%, 100% { transform: translate(0, 0) rotate(0deg); }
  25% { transform: translate(0.5px, -0.5px) rotate(0.3deg); }
  50% { transform: translate(-0.5px, 0.5px) rotate(-0.3deg); }
  75% { transform: translate(0.3px, -0.3px) rotate(0.2deg); }
}

.inner-container {
  position: relative;
}
.border-outer {
  border: 2px solid rgba(72, 87, 221, 0.5);
  border-radius: 24px;
  padding-right: 4px;
  padding-bottom: 4px;
}
.main-card {
  width: 350px;
  height: 500px;
  border-radius: 24px;
  border: 2px solid var(--electric-border-color);
  margin-top: -4px;
  margin-left: -4px;
  filter: url(#turbulent-displace);
}

/* === GLOW ANIMATION === */
.glow-layer-1,
.glow-layer-2 {
  border-radius: 24px;
  width: 100%;
  height: 100%;
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
}
.glow-layer-1 {
  border: 2px solid rgba(221, 132, 72, 0.6);
  filter: blur(2px);
}
.glow-layer-2 {
  border: 2px solid var(--electric-light-color);
  filter: blur(5px);
}

/* Pulsing border glow */
@keyframes electricPulse {
  0%, 100% { opacity: 0.6; filter: blur(3px) brightness(1); }
  50% { opacity: 1; filter: blur(5px) brightness(1.5); }
}
.pulse {
  animation: electricPulse 2.5s ease-in-out infinite;
}
.pulse.delay {
  animation-delay: 1.2s;
}

/* === SPARK EFFECT === */
.electric-animated::after {
  content: "";
  position: absolute;
  width: 140%;
  height: 3px;
  left: -20%;
  top: 50%;
  background: linear-gradient(90deg, transparent, white, transparent);
  opacity: 0;
  transform: rotate(5deg);
  animation: electricSpark 5s infinite ease-in-out;
}
@keyframes electricSpark {
  0%, 95%, 100% { opacity: 0; transform: translateY(-30px) scaleX(0.2); }
  45%, 50% { opacity: 1; transform: translateY(0) scaleX(1); filter: blur(1px); }
}

/* === MOVING GLOW === */
.background-glow {
  position: absolute;
  width: 100%;
  height: 100%;
  top: 0; left: 0; right: 0; bottom: 0;
  border-radius: 24px;
  filter: blur(32px);
  transform: scale(1.1);
  opacity: 0.3;
  z-index: -1;
  background: linear-gradient(-30deg, var(--electric-light-color), transparent, var(--electric-border-color));
}
.moving-glow {
  animation: glowShift 6s ease-in-out infinite alternate;
}
@keyframes glowShift {
  0% { background-position: 0% 50%; opacity: 0.25; }
  50% { background-position: 100% 50%; opacity: 0.45; }
  100% { background-position: 0% 50%; opacity: 0.3; }
}

/* === CONTENT === */
.content-container {
  position: absolute;
  top: 0; left: 0; right: 0; bottom: 0;
  display: flex;
  flex-direction: column;
}
.content-top {
  padding: 48px;
  padding-bottom: 16px;
  height: 100%;
}
.content-bottom {
  padding: 48px;
  padding-top: 16px;
}
.scrollbar-glass {
  background: rgba(255,255,255,0.05);
  border-radius: 14px;
  width: fit-content;
  height: fit-content;
  padding: 8px 16px;
  text-transform: uppercase;
  font-weight: bold;
  font-size: 14px;
  color: rgba(255, 255, 255, 0.8);
}
.title { font-size: 36px; font-weight: 500; margin-top: auto; }
.description { opacity: 0.5; }

/* Divider line */
.divider {
  margin-top: auto;
  border: none;
  height: 1px;
  background-color: currentColor;
  opacity: 0.1;
  mask-image: linear-gradient(to right, transparent, black, transparent);
}
</style>
