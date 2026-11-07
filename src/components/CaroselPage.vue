<template>
  <section
    class="tour-hero"
    aria-roledescription="carousel"
    aria-label="Featured destinations"
    @mouseenter="pauseAutoPlay"
    @mouseleave="startAutoPlay"
    @focusin="pauseAutoPlay"
    @focusout="startAutoPlay"
  >
    <!-- Slides -->
    <div class="slides">
      <article
        v-for="(item, i) in items"
        :key="i"
        class="slide"
        :class="{ active: currentSlide === i }"
        :aria-hidden="currentSlide !== i"
        role="group"
        aria-roledescription="slide"
        :aria-label="`${i + 1} of ${items.length}`"
      >
        <div
          class="photo"
          role="img"
          :aria-label="item.alt"
          :style="{ backgroundImage: `url(${item.src})`, backgroundPosition: item.pos || 'center' }"
        ></div>
        <div class="shade"></div>

        <div class="copy">
          <p class="region">{{ item.region }}</p>
          <h2 class="title">{{ item.title }}</h2>
          <p class="lede">{{ item.subtitle }}</p>

          <ul class="highlights" v-if="item.highlights">
            <li v-for="(h, hIndex) in item.highlights" :key="hIndex">{{ h }}</li>
          </ul>

          <div class="actions">
            <a :href="item.link" class="btn btn-primary">{{ item.cta || 'Plan this trip' }}</a>
            <a :href="item.guideLink || item.link" class="btn btn-ghost">Read the guide</a>
          </div>
        </div>
      </article>
    </div>

    <!-- Arrows -->
    <button class="arrow arrow-prev" @click="prev" aria-label="Previous destination">
      <svg width="22" height="22" viewBox="0 0 24 24" fill="none" aria-hidden="true">
        <path d="M15 18L9 12L15 6" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" />
      </svg>
    </button>
    <button class="arrow arrow-next" @click="next" aria-label="Next destination">
      <svg width="22" height="22" viewBox="0 0 24 24" fill="none" aria-hidden="true">
        <path d="M9 18L15 12L9 6" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" />
      </svg>
    </button>

    <!-- Destination strip: doubles as navigation + autoplay progress -->
    <nav class="strip" aria-label="Choose a destination">
      <button
        v-for="(item, index) in items"
        :key="index"
        class="tab"
        :class="{ active: currentSlide === index }"
        :aria-current="currentSlide === index ? 'true' : null"
        @click="goTo(index)"
      >
        <span class="bar">
          <span
            v-if="currentSlide === index"
            :key="`bar-${index}-${cycle}`"
            class="bar-fill"
            :style="{ animationDuration: `${INTERVAL}ms` }"
          ></span>
        </span>
        <span class="tab-name">{{ item.tab }}</span>
        <span class="tab-region">{{ item.region }}</span>
      </button>
    </nav>
  </section>
</template>

<script>
/*
  Fonts (add once, e.g. in index.html / app.blade.php):
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
  <link href="https://fonts.googleapis.com/css2?family=Figtree:wght@400;500;600&family=Young+Serif&display=swap" rel="stylesheet">

  Images live in:  src/assets/carousel/
    Seegiriya.png, Kandy.jpg, Ninearch.jpg, Yala.jpg, Gallefort.jpg, Beach1.jpg, Ambuluwawa.jpg
*/
export default {
  name: 'TourismCarousel',
  data() {
    return {
      currentSlide: 0,
      autoPlayTimer: null,
      INTERVAL: 8000,
      cycle: 0, // restarts the progress bar whenever autoplay restarts
      touchStartX: 0,
      items: [
        {
          src: require('@/assets/carousel/Seegiriya.png'),
          alt: 'Sigiriya Rock Fortress rising above green parkland',
          pos: 'center 30%',
          tab: 'Sigiriya',
          region: 'Ancient cities & temples',
          title: 'Climb the Lion Rock at sunrise',
          subtitle: 'A fifth-century rock fortress, royal gardens and frescoes, explored with guides who know every step.',
          highlights: ['Expert local guides', 'Sunrise climb', 'Cultural day tours'],
          cta: 'Plan a heritage tour',
          link: '#sigiriya'
        },
        {
          src: require('@/assets/carousel/Kandy.jpg'),
          alt: 'Temple of the Sacred Tooth Relic beside Kandy Lake',
          pos: 'center',
          tab: 'Kandy',
          region: 'Sacred city',
          title: 'Visit the Temple of the Sacred Tooth Relic',
          subtitle: 'The golden-roofed temple in Kandy that houses a relic of the Buddha, set beside the lake and wrapped in daily ritual and drumming.',
          highlights: ['Temple of the Tooth', 'Evening puja ceremony', 'Kandy Lake walks'],
          cta: 'Plan a Kandy visit',
          link: '#kandy'
        },
        {
          src: require('@/assets/carousel/Ninearch.jpg'),
          alt: 'Blue train crossing the Nine Arch Bridge in Ella',
          pos: 'center',
          tab: 'Ella',
          region: 'Hill country & tea trails',
          title: 'Ride the blue train through the hills',
          subtitle: 'Misty forests, tea estates and the Nine Arch Bridge, best seen slowly from a carriage window.',
          highlights: ['Scenic train rides', 'Guided waterfall hikes', 'Plantation stays'],
          cta: 'Plan a hill escape',
          link: '#ella'
        },
        {
          src: require('@/assets/carousel/Yala.jpg'),
          alt: 'Leopard resting in Yala National Park',
          pos: 'center',
          tab: 'Yala',
          region: 'Wildlife safari',
          title: 'Go on safari in Yala National Park',
          subtitle: 'Jeep safaris through scrub, lagoons and rock outcrops, home to one of the densest leopard populations in the world, plus elephants and sloth bears.',
          highlights: ['Leopard spotting', 'Half & full-day safaris', 'Elephants & birdlife'],
          cta: 'Plan a safari',
          link: '#yala'
        },
        {
          src: require('@/assets/carousel/Galle-Fort.png'),
          alt: 'Galle Fort lighthouse at sunset beside the sea',
          pos: 'center 40%',
          tab: 'Galle Fort',
          region: 'Colonial coast',
          title: 'Watch the sun set from the old ramparts',
          subtitle: 'Cobbled lanes, cafés and a white lighthouse inside the walls of a living fortress town.',
          highlights: ['Fort walking tours', 'Boutique stays', 'Sunset on the walls'],
          cta: 'Plan a Galle stay',
          link: '#galle'
        },
        {
          src: require('@/assets/carousel/Beach1.jpg'),
          alt: 'Mirissa beach with palm trees and clear shallow water',
          pos: 'center 55%',
          tab: 'Mirissa',
          region: 'South coast beaches',
          title: 'Wake up where the sea begins',
          subtitle: 'Pink-gold sand, palm-lined bays and the best whale watching season on the island.',
          highlights: ['Whale watching', 'Surf & snorkel', 'Beachfront stays'],
          cta: 'Plan a beach escape',
          link: '#mirissa'
        },
        {
          src: require('@/assets/carousel/Ambuluwawa.jpg'),
          alt: 'Ambuluwawa tower rising above green hills',
          pos: 'center 35%',
          tab: 'Ambuluwawa',
          region: 'Hilltop views',
          title: 'Stand above the hills at Ambuluwawa',
          subtitle: 'A spiral tower on a hilltop with views over valleys and ridges in every direction.',
          highlights: ['360° viewpoint', 'Short scenic hike', 'Easy day trip from Kandy'],
          cta: 'Plan a hilltop day',
          link: '#ambuluwawa'
        }
      ]
    };
  },
  methods: {
    goTo(index) {
      this.currentSlide = ((index % this.items.length) + this.items.length) % this.items.length;
      this.restartAutoPlay();
    },
    next() {
      this.goTo(this.currentSlide + 1);
    },
    prev() {
      this.goTo(this.currentSlide - 1);
    },
    startAutoPlay() {
      this.stopAutoPlay();
      this.cycle += 1;
      this.autoPlayTimer = setInterval(() => this.next(), this.INTERVAL);
    },
    stopAutoPlay() {
      if (this.autoPlayTimer) {
        clearInterval(this.autoPlayTimer);
        this.autoPlayTimer = null;
      }
    },
    pauseAutoPlay() {
      this.stopAutoPlay();
    },
    restartAutoPlay() {
      this.startAutoPlay();
    },
    handleKeydown(e) {
      if (e.key === 'ArrowRight') this.next();
      if (e.key === 'ArrowLeft') this.prev();
    },
    handleTouchStart(e) {
      this.touchStartX = e.touches[0].clientX;
    },
    handleTouchEnd(e) {
      const diff = this.touchStartX - e.changedTouches[0].clientX;
      if (Math.abs(diff) > 50) diff > 0 ? this.next() : this.prev();
    }
  },
  mounted() {
    this.startAutoPlay();
    window.addEventListener('keydown', this.handleKeydown);
    this.$el.addEventListener('touchstart', this.handleTouchStart, { passive: true });
    this.$el.addEventListener('touchend', this.handleTouchEnd);
  },
  beforeUnmount() {
    this.stopAutoPlay();
    window.removeEventListener('keydown', this.handleKeydown);
    this.$el.removeEventListener('touchstart', this.handleTouchStart);
    this.$el.removeEventListener('touchend', this.handleTouchEnd);
  }
};
</script>

<style scoped>
/* ===== TOKENS ===== */
.tour-hero {
  --ink: #0a1c22;        /* deep lagoon night */
  --sand: #f3ead8;       /* sun-bleached sand */
  --glass: #8fd6cb;      /* sea glass */
  --sun: #f5b94a;        /* low-sun amber */
  --font-display: 'Young Serif', Georgia, 'Times New Roman', serif;
  --font-body: 'Figtree', system-ui, -apple-system, 'Segoe UI', Roboto, sans-serif;

  position: relative;
  width: 100%;
  height: clamp(520px, 88vh, 720px);
  overflow: hidden;
  background: var(--ink);
  color: var(--sand);
  font-family: var(--font-body);
}

/* ===== SLIDES (crossfade) ===== */
.slides {
  position: absolute;
  inset: 0;
}

.slide {
  position: absolute;
  inset: 0;
  opacity: 0;
  visibility: hidden;
  transition: opacity 1s ease, visibility 0s linear 1s;
}

.slide.active {
  opacity: 1;
  visibility: visible;
  transition: opacity 1s ease, visibility 0s;
}

.photo {
  position: absolute;
  inset: 0;
  background-size: cover;
  background-position: center;
  background-repeat: no-repeat;
  transform: scale(1);
}

/* the one deliberate motion: a slow drift on the active photo */
.slide.active .photo {
  animation: drift 14s ease-out forwards;
}

@keyframes drift {
  from { transform: scale(1.08); }
  to   { transform: scale(1); }
}

/* darkens the left (text side) and the bottom (strip) only */
.shade {
  position: absolute;
  inset: 0;
  background:
    linear-gradient(to top, rgba(10, 28, 34, 0.92) 0%, rgba(10, 28, 34, 0) 38%),
    linear-gradient(to right, rgba(10, 28, 34, 0.78) 0%, rgba(10, 28, 34, 0.25) 55%, rgba(10, 28, 34, 0) 80%);
}

/* ===== COPY ===== */
.copy {
  position: absolute;
  left: 0;
  right: 0;
  top: 0;
  bottom: 120px;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: flex-start;
  padding: 0 max(6%, 24px);
  max-width: 720px;
}

.region {
  margin: 0 0 14px;
  font-size: 0.95rem;
  font-weight: 500;
  color: var(--glass);
  letter-spacing: 0.01em;
}

.title {
  margin: 0 0 16px;
  font-family: var(--font-display);
  font-weight: 400;
  font-size: clamp(2.2rem, 5.4vw, 4.2rem);
  line-height: 1.08;
  letter-spacing: -0.01em;
  color: #fff;
  text-wrap: balance;
}

.lede {
  margin: 0 0 22px;
  max-width: 52ch;
  font-size: clamp(1rem, 1.6vw, 1.2rem);
  line-height: 1.55;
  color: rgba(243, 234, 216, 0.92);
}

.highlights {
  list-style: none;
  margin: 0 0 30px;
  padding: 0;
  display: flex;
  flex-wrap: wrap;
  gap: 8px 10px;
}

.highlights li {
  padding: 6px 14px;
  font-size: 0.85rem;
  font-weight: 500;
  color: var(--sand);
  background: rgba(243, 234, 216, 0.12);
  border: 1px solid rgba(243, 234, 216, 0.28);
  border-radius: 999px;
  backdrop-filter: blur(4px);
}

/* entrance only when a slide becomes active */
.slide.active .copy > * {
  animation: rise 0.9s cubic-bezier(0.22, 0.61, 0.36, 1) both;
}
.slide.active .copy > :nth-child(2) { animation-delay: 0.08s; }
.slide.active .copy > :nth-child(3) { animation-delay: 0.16s; }
.slide.active .copy > :nth-child(4) { animation-delay: 0.24s; }
.slide.active .copy > :nth-child(5) { animation-delay: 0.32s; }

@keyframes rise {
  from { opacity: 0; transform: translateY(18px); }
  to   { opacity: 1; transform: translateY(0); }
}

/* ===== BUTTONS ===== */
.actions {
  display: flex;
  flex-wrap: wrap;
  gap: 12px;
}

.btn {
  display: inline-block;
  padding: 13px 26px;
  font-size: 0.95rem;
  font-weight: 600;
  text-decoration: none;
  border-radius: 999px;
  transition: background-color 0.25s ease, color 0.25s ease, border-color 0.25s ease;
}

.btn-primary {
  background: var(--sun);
  color: var(--ink);
}
.btn-primary:hover {
  background: #fff;
}

.btn-ghost {
  color: var(--sand);
  border: 1.5px solid rgba(243, 234, 216, 0.55);
}
.btn-ghost:hover {
  border-color: #fff;
  background: rgba(255, 255, 255, 0.12);
}

.btn:focus-visible,
.arrow:focus-visible,
.tab:focus-visible {
  outline: 3px solid var(--glass);
  outline-offset: 3px;
}

/* ===== ARROWS ===== */
.arrow {
  position: absolute;
  top: 42%;
  z-index: 20;
  width: 48px;
  height: 48px;
  display: flex;
  align-items: center;
  justify-content: center;
  color: var(--sand);
  background: rgba(10, 28, 34, 0.35);
  border: 1px solid rgba(243, 234, 216, 0.3);
  border-radius: 50%;
  backdrop-filter: blur(6px);
  cursor: pointer;
  opacity: 0;
  transition: opacity 0.3s ease, background-color 0.25s ease;
}
.tour-hero:hover .arrow,
.arrow:focus-visible {
  opacity: 1;
}
.arrow:hover {
  background: rgba(10, 28, 34, 0.7);
}
.arrow-prev { left: 20px; }
.arrow-next { right: 20px; }

/* ===== DESTINATION STRIP ===== */
.strip {
  position: absolute;
  left: 0;
  right: 0;
  bottom: 0;
  z-index: 20;
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(0, 1fr));
  gap: 18px;
  padding: 0 max(6%, 24px) 28px;
}

.tab {
  display: flex;
  flex-direction: column;
  align-items: flex-start;
  gap: 2px;
  min-width: 0;
  padding: 0;
  text-align: left;
  color: rgba(243, 234, 216, 0.62);
  background: none;
  border: none;
  cursor: pointer;
  font-family: inherit;
  transition: color 0.25s ease;
}
.tab:hover,
.tab.active {
  color: #fff;
}

.bar {
  position: relative;
  display: block;
  width: 100%;
  height: 3px;
  margin-bottom: 12px;
  background: rgba(243, 234, 216, 0.28);
  border-radius: 2px;
  overflow: hidden;
}

.bar-fill {
  position: absolute;
  inset: 0;
  background: var(--sun);
  transform-origin: left center;
  transform: scaleX(0);
  animation: fill linear forwards;
}

@keyframes fill {
  to { transform: scaleX(1); }
}

.tab-name {
  max-width: 100%;
  font-family: var(--font-display);
  font-size: 1.15rem;
  line-height: 1.2;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
}

.tab-region {
  max-width: 100%;
  font-size: 0.8rem;
  opacity: 0.85;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
}

/* ===== RESPONSIVE ===== */
@media (max-width: 768px) {
  .tour-hero {
    height: clamp(520px, 86vh, 640px);
  }
  .copy {
    bottom: 90px;
  }
  .shade {
    background:
      linear-gradient(to top, rgba(10, 28, 34, 0.92) 0%, rgba(10, 28, 34, 0.25) 60%, rgba(10, 28, 34, 0.55) 100%);
  }
  .arrow {
    opacity: 0.85;
    width: 40px;
    height: 40px;
  }
  .arrow-prev { left: 10px; }
  .arrow-next { right: 10px; }
  .strip {
    gap: 8px;
    padding-bottom: 20px;
  }
  .tab-name { font-size: 0.95rem; }
  .tab-region { display: none; }
  .highlights li:nth-child(n + 3) { display: none; }
}

@media (max-width: 480px) {
  .btn-ghost { display: none; }
  .strip { gap: 6px; }
  .tab-name { font-size: 0.7rem; }
}

@media (max-width: 768px) and (orientation: landscape) {
  .tour-hero { height: 420px; }
  .highlights { display: none; }
}

/* ===== REDUCED MOTION ===== */
@media (prefers-reduced-motion: reduce) {
  .slide,
  .slide.active { transition: none; }
  .slide.active .photo,
  .slide.active .copy > * { animation: none; }
  .bar-fill { animation-duration: 0.01ms !important; transform: scaleX(1); }
}
</style>