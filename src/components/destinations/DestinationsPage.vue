<template>
  <LankaNavBar :over-hero="true" />

  <main class="ds">
    <!-- Hero -->
    <section class="ds-hero" aria-labelledby="ds-title">
      <span class="ds-hero-bg" :style="{ backgroundImage: `url(${heroImg})` }" aria-hidden="true"></span>
      <span class="ds-hero-shade" aria-hidden="true"></span>
      <div class="ds-wrap ds-hero-inner">
        <nav class="ds-crumbs" aria-label="Breadcrumb">
          <a href="/">Home</a>
          <span aria-hidden="true">/</span>
          <span class="ds-crumb-now">Destinations</span>
        </nav>
        <p class="ds-eyebrow">Where we go</p>
        <h1 id="ds-title" class="ds-h1">Places people come back for</h1>
        <p class="ds-sub">
          Seven favourites across the island. Open any of them for highlights, the best time to go
          and a sample day.
        </p>
      </div>
      <svg class="ds-wave" viewBox="0 0 1440 80" preserveAspectRatio="none" aria-hidden="true">
        <path d="M0,40 C240,90 480,0 720,36 C960,72 1200,10 1440,44 L1440,80 L0,80 Z" fill="#ffffff" />
      </svg>
    </section>

    <!-- Grid -->
    <section class="ds-section" aria-label="All destinations">
      <div class="ds-wrap">
        <div class="ds-grid">
          <a
            v-for="(d, i) in destinations"
            :key="d.name"
            :href="d.link"
            class="ds-card"
            :class="d.size"
          >
            <span class="ds-photo" :style="{ backgroundImage: `url(${d.src})` }"></span>
            <span class="ds-shade"></span>
            <span class="ds-num">{{ String(i + 1).padStart(2, "0") }}</span>
            <span class="ds-text">
              <span class="ds-region">{{ d.region }}</span>
              <span class="ds-name">{{ d.name }}</span>
              <span class="ds-tag">{{ d.tagline }}</span>
              <span class="ds-more">Explore <i aria-hidden="true">&rarr;</i></span>
            </span>
          </a>
        </div>
      </div>
    </section>

    <!-- CTA -->
    <section class="ds-cta" aria-labelledby="ds-cta-title">
      <div class="ds-wrap ds-cta-inner">
        <h2 id="ds-cta-title" class="ds-cta-title">Not sure where to start? Tell us your dates.</h2>
        <a href="/plan-your-trip" class="ds-btn">Plan my trip</a>
      </div>
    </section>
  </main>

  <ArrowButton></ArrowButton>
  <FooterPage></FooterPage>
</template>

<script>
import LankaNavBar from "../Lankanavbar.vue";
import FooterPage from "../FooterPage.vue";
import ArrowButton from "../ArrowButton.vue";

import imgAmbuluwawa from "@/assets/carousel/Ambuluwawa.jpg";
import imgBeach from "@/assets/carousel/Beach.jpg";
import imgGalle from "@/assets/carousel/Gallefort.jpg";
import imgMirissa from "@/assets/carousel/Mirissa.jpg";
import imgNineArch from "@/assets/carousel/Ninearch.jpg";
import imgSigiriya from "@/assets/carousel/Seegiriya.png";

export default {
  name: "DestinationsPage",
  components: { LankaNavBar, FooterPage, ArrowButton },
  data() {
    return {
      heroImg: imgBeach,
      destinations: [
        { name: "Sigiriya", region: "Cultural Triangle", tagline: "Climb the ancient rock fortress at sunrise.", src: imgSigiriya, link: "/destinations/sigiriya", size: "is-big" },
        { name: "Ella", region: "Hill country", tagline: "Train rides, tea hills and Nine Arch Bridge.", src: imgNineArch, link: "/destinations/ella", size: "" },
        { name: "Kandy", region: "Central highlands", tagline: "Temple of the Tooth and misty hill views.", src: imgAmbuluwawa, link: "/destinations/kandy", size: "" },
        { name: "Galle", region: "South coast", tagline: "Walk the old Dutch fort ramparts at sunset.", src: imgGalle, link: "/destinations/galle", size: "" },
        { name: "Yala", region: "Southeast", tagline: "Early-morning safaris in leopard country.", src: "https://picsum.photos/seed/lk-yala/800/600", link: "/destinations/yala", size: "" },
        { name: "Mirissa", region: "South coast", tagline: "Whale watching, surf and palm-lined beaches.", src: imgMirissa, link: "/destinations/mirissa", size: "is-wide" },
        { name: "Nuwara Eliya", region: "Tea country", tagline: "Cool air, colonial charm and rolling estates.", src: "https://picsum.photos/seed/lk-nuwara/1200/600", link: "/destinations/nuwara-eliya", size: "is-wide" },
      ],
    };
  },
  mounted() {
    document.title = "Destinations | Ceylon Escapes";
  },
};
</script>

<style scoped>
.ds {
  --navy: #0b2a4a;
  --blue: #1565c0;
  --blue-bright: #1e88e5;
  --sky: #8ec9ff;
  --sun: #f5b94a;
  --text: #1f2933;
  --shadow-sm: 0 4px 14px rgba(11, 42, 74, 0.08);
  --shadow-md: 0 14px 34px rgba(11, 42, 74, 0.16);
  --font-display: "Young Serif", Georgia, "Times New Roman", serif;
  --font-body: "Figtree", system-ui, -apple-system, "Segoe UI", Roboto, sans-serif;
  font-family: var(--font-body);
  color: var(--text);
  overflow-x: hidden;
}
.ds-wrap {
  position: relative;
  width: 100%;
  max-width: 1200px;
  margin: 0 auto;
  padding: 0 max(24px, 4vw);
  box-sizing: border-box;
}

/* Hero */
.ds-hero {
  position: relative;
  display: flex;
  align-items: flex-end;
  min-height: 62vh;
  padding: 190px 0 110px;
  overflow: hidden;
  background: var(--navy);
}
.ds-hero-bg {
  position: absolute;
  inset: 0;
  background-size: cover;
  background-position: center;
}
.ds-hero-shade {
  position: absolute;
  inset: 0;
  background: linear-gradient(to top, rgba(11, 42, 74, 0.9) 0%, rgba(11, 42, 74, 0.4) 60%, rgba(11, 42, 74, 0.55) 100%);
}
.ds-hero-inner {
  z-index: 1;
}
.ds-wave {
  position: absolute;
  left: 0;
  bottom: -1px;
  width: 100%;
  height: 70px;
  pointer-events: none;
}
.ds-crumbs {
  display: flex;
  gap: 8px;
  margin-bottom: 24px;
  font-size: 0.88rem;
  color: rgba(255, 255, 255, 0.75);
}
.ds-crumbs a {
  color: inherit;
  text-decoration: none;
}
.ds-crumbs a:hover {
  color: var(--sun);
}
.ds-crumb-now {
  color: #fff;
  font-weight: 600;
}
.ds-eyebrow {
  display: inline-flex;
  align-items: center;
  gap: 10px;
  margin: 0 0 14px;
  font-size: 0.8rem;
  font-weight: 700;
  letter-spacing: 0.16em;
  text-transform: uppercase;
  color: var(--sun);
}
.ds-eyebrow::before {
  content: "";
  width: 28px;
  height: 2px;
  background: var(--sun);
  border-radius: 2px;
}
.ds-h1 {
  margin: 0;
  max-width: 16ch;
  font-family: var(--font-display);
  font-weight: 400;
  font-size: clamp(2.4rem, 6vw, 4.4rem);
  line-height: 1.06;
  color: #fff;
  text-wrap: balance;
}
.ds-sub {
  max-width: 46ch;
  margin: 18px 0 0;
  font-size: 1.15rem;
  line-height: 1.6;
  color: rgba(255, 255, 255, 0.9);
}

/* Grid */
.ds-section {
  padding: 40px 0 96px;
  background: #fff;
}
.ds-grid {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  grid-auto-rows: 250px;
  gap: 18px;
}
.ds-card {
  position: relative;
  display: block;
  overflow: hidden;
  border-radius: 18px;
  color: #fff;
  text-decoration: none;
  background: var(--navy);
  box-shadow: var(--shadow-sm);
  transition: transform 0.35s ease, box-shadow 0.35s ease;
}
.ds-card:hover,
.ds-card:focus-visible {
  transform: translateY(-6px);
  box-shadow: var(--shadow-md);
}
.ds-card:focus-visible {
  outline: 3px solid var(--sky);
  outline-offset: 3px;
}
.ds-card.is-big {
  grid-column: span 2;
  grid-row: span 2;
}
.ds-card.is-wide {
  grid-column: span 2;
}
.ds-photo {
  position: absolute;
  inset: 0;
  background-size: cover;
  background-position: center;
  transition: transform 0.7s ease;
}
.ds-card:hover .ds-photo {
  transform: scale(1.08);
}
.ds-shade {
  position: absolute;
  inset: 0;
  background: linear-gradient(to top, rgba(11, 42, 74, 0.92) 0%, rgba(11, 42, 74, 0.25) 45%, rgba(11, 42, 74, 0) 70%);
}
.ds-num {
  position: absolute;
  top: 14px;
  right: 16px;
  padding: 4px 12px;
  font-family: var(--font-display);
  font-size: 1.1rem;
  color: rgba(255, 255, 255, 0.85);
  background: rgba(11, 42, 74, 0.45);
  border-radius: 999px;
  backdrop-filter: blur(6px);
}
.ds-text {
  position: absolute;
  left: 22px;
  right: 22px;
  bottom: 20px;
  display: flex;
  flex-direction: column;
  gap: 4px;
}
.ds-region {
  font-size: 0.8rem;
  font-weight: 600;
  letter-spacing: 0.08em;
  text-transform: uppercase;
  color: var(--sun);
}
.ds-name {
  font-family: var(--font-display);
  font-size: 1.5rem;
  line-height: 1.15;
}
.ds-card.is-big .ds-name {
  font-size: 2.4rem;
}
.ds-tag {
  font-size: 0.95rem;
  line-height: 1.4;
  color: rgba(255, 255, 255, 0.92);
}
.ds-more {
  max-height: 0;
  opacity: 0;
  overflow: hidden;
  font-size: 0.9rem;
  font-weight: 600;
  color: var(--sky);
  transition: max-height 0.35s ease, opacity 0.35s ease, margin 0.35s ease;
}
.ds-more i {
  font-style: normal;
  display: inline-block;
  transition: transform 0.3s ease;
}
.ds-card:hover .ds-more,
.ds-card:focus-visible .ds-more {
  max-height: 28px;
  opacity: 1;
  margin-top: 6px;
}
.ds-card:hover .ds-more i {
  transform: translateX(5px);
}

/* CTA */
.ds-cta {
  padding: 80px 0;
  background: linear-gradient(135deg, var(--navy) 0%, #12457a 100%);
}
.ds-cta-inner {
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: 28px;
}
.ds-cta-title {
  margin: 0;
  max-width: 22ch;
  font-family: var(--font-display);
  font-weight: 400;
  font-size: clamp(1.7rem, 3vw, 2.4rem);
  line-height: 1.15;
  color: #fff;
}
.ds-btn {
  display: inline-block;
  padding: 13px 30px;
  font-weight: 600;
  text-decoration: none;
  color: var(--navy);
  background: var(--sun);
  border-radius: 999px;
  box-shadow: 0 8px 20px rgba(245, 185, 74, 0.4);
  transition: transform 0.25s ease, background-color 0.25s ease;
}
.ds-btn:hover {
  transform: translateY(-2px);
  background: #fff;
}

/* Responsive */
@media (max-width: 1024px) {
  .ds-grid {
    grid-template-columns: repeat(2, 1fr);
    grid-auto-rows: 230px;
  }
}
@media (max-width: 768px) {
  .ds-hero {
    padding: 150px 0 90px;
  }
  .ds-cta-inner {
    flex-direction: column;
    align-items: flex-start;
  }
}
@media (max-width: 560px) {
  .ds-grid {
    grid-template-columns: 1fr;
    grid-auto-rows: 220px;
  }
  .ds-card.is-big,
  .ds-card.is-wide {
    grid-column: span 1;
  }
  .ds-card.is-big {
    grid-row: span 1;
  }
  .ds-card.is-big .ds-name {
    font-size: 1.8rem;
  }
}
@media (prefers-reduced-motion: reduce) {
  .ds-card,
  .ds-photo,
  .ds-btn {
    transition: none;
  }
  .ds-card:hover,
  .ds-card:hover .ds-photo,
  .ds-btn:hover {
    transform: none;
  }
}
</style>
