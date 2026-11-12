<template>
  <LankaNavBar :over-hero="true" />

  <main class="dp">
    <!-- ================= HERO ================= -->
    <section class="dp-hero" :aria-label="`${name} introduction`">
      <span class="dp-hero-bg" :style="{ backgroundImage: `url(${hero})` }" aria-hidden="true"></span>
      <span class="dp-hero-shade" aria-hidden="true"></span>

      <div class="dp-wrap dp-hero-inner">
        <nav class="dp-crumbs" aria-label="Breadcrumb">
          <a href="/">Home</a>
          <span aria-hidden="true">/</span>
          <a href="/destinations">Destinations</a>
          <span aria-hidden="true">/</span>
          <span class="dp-crumb-now">{{ name }}</span>
        </nav>

        <p class="dp-eyebrow dp-eyebrow-light">{{ region }}</p>
        <h1 class="dp-h1">{{ name }}</h1>
        <p class="dp-tagline">{{ tagline }}</p>

        <div class="dp-hero-actions">
          <a href="/plan-your-trip" class="dp-btn dp-btn-sun">Plan my trip</a>
          <a href="#highlights" class="dp-btn dp-btn-line">See highlights</a>
        </div>
      </div>

      <svg class="dp-hero-wave" viewBox="0 0 1440 80" preserveAspectRatio="none" aria-hidden="true">
        <path d="M0,40 C240,90 480,0 720,36 C960,72 1200,10 1440,44 L1440,80 L0,80 Z" fill="#ffffff" />
      </svg>
    </section>

    <!-- ================= QUICK FACTS ================= -->
    <section class="dp-facts-band" aria-label="Quick facts">
      <div class="dp-wrap">
        <div class="dp-facts">
          <div class="dp-fact" v-for="f in facts" :key="f.label">
            <span class="dp-fact-icon"><i class="material-icons">{{ f.icon }}</i></span>
            <span>
              <span class="dp-fact-label">{{ f.label }}</span>
              <span class="dp-fact-value">{{ f.value }}</span>
            </span>
          </div>
        </div>
      </div>
    </section>

    <!-- ================= OVERVIEW ================= -->
    <section class="dp-section" aria-labelledby="overview-heading">
      <div class="dp-wrap dp-overview">
        <div>
          <p class="dp-eyebrow">Overview</p>
          <h2 id="overview-heading" class="dp-h2">Why visit {{ name }}</h2>
          <span class="dp-rule" aria-hidden="true"></span>
          <p class="dp-lead" v-for="(p, i) in intro" :key="i">{{ p }}</p>
        </div>

        <aside class="dp-side">
          <div class="dp-card dp-season">
            <p class="dp-card-kicker"><i class="material-icons">wb_sunny</i> Best time to go</p>
            <p class="dp-season-months">{{ bestTime.months }}</p>
            <p class="dp-card-text">{{ bestTime.note }}</p>
          </div>
          <div class="dp-card dp-help">
            <p class="dp-card-kicker"><i class="material-icons">support_agent</i> Need a hand?</p>
            <p class="dp-card-text">
              Tell us your dates and we will build {{ name }} into a trip that suits your pace.
            </p>
            <a :href="`tel:${phoneRaw}`" class="dp-help-phone">
              <i class="material-icons">phone</i> {{ phoneDisplay }}
            </a>
          </div>
        </aside>
      </div>
    </section>

    <!-- ================= HIGHLIGHTS ================= -->
    <section id="highlights" class="dp-section dp-sand" aria-labelledby="hl-heading">
      <div class="dp-wrap">
        <div class="dp-head">
          <p class="dp-eyebrow">Highlights</p>
          <h2 id="hl-heading" class="dp-h2">Things to see and do in {{ name }}</h2>
        </div>

        <div class="dp-hl-grid">
          <article class="dp-hl" v-for="(h, i) in highlights" :key="h.title">
            <span class="dp-hl-num">{{ String(i + 1).padStart(2, "0") }}</span>
            <span class="dp-hl-icon"><i class="material-icons">{{ h.icon }}</i></span>
            <h3 class="dp-hl-title">{{ h.title }}</h3>
            <p class="dp-hl-text">{{ h.text }}</p>
          </article>
        </div>
      </div>
    </section>

    <!-- ================= ITINERARY ================= -->
    <section class="dp-section" aria-labelledby="plan-heading">
      <div class="dp-wrap dp-plan">
        <div class="dp-plan-intro">
          <p class="dp-eyebrow">A sample day</p>
          <h2 id="plan-heading" class="dp-h2">How a day in {{ name }} can look</h2>
          <span class="dp-rule" aria-hidden="true"></span>
          <p class="dp-lead">
            This is only a starting point. We adjust timings, pace and stops to match your group.
          </p>
          <a href="/plan-your-trip" class="dp-btn dp-btn-solid">Customise this day</a>
        </div>

        <ol class="dp-timeline">
          <li class="dp-step" v-for="s in itinerary" :key="s.time">
            <span class="dp-step-dot" aria-hidden="true"></span>
            <p class="dp-step-time">{{ s.time }}</p>
            <h3 class="dp-step-title">{{ s.title }}</h3>
            <p class="dp-step-text">{{ s.text }}</p>
          </li>
        </ol>
      </div>
    </section>

    <!-- ================= TIPS ================= -->
    <section class="dp-section dp-tips-wrap" aria-labelledby="tips-heading">
      <div class="dp-wrap">
        <div class="dp-head">
          <p class="dp-eyebrow dp-eyebrow-light">Good to know</p>
          <h2 id="tips-heading" class="dp-h2 dp-h2-light">Travel tips for {{ name }}</h2>
        </div>
        <ul class="dp-tips">
          <li v-for="t in tips" :key="t">
            <i class="material-icons">check_circle</i>
            <span>{{ t }}</span>
          </li>
        </ul>
      </div>
    </section>

    <!-- ================= MORE DESTINATIONS ================= -->
    <section class="dp-section dp-sand" aria-labelledby="more-heading">
      <div class="dp-wrap">
        <div class="dp-head">
          <p class="dp-eyebrow">Keep exploring</p>
          <h2 id="more-heading" class="dp-h2">More places to combine with {{ name }}</h2>
        </div>

        <div class="dp-more-grid">
          <a v-for="d in otherDestinations" :key="d.name" :href="d.link" class="dp-more">
            <span class="dp-more-photo" :style="{ backgroundImage: `url(${d.src})` }"></span>
            <span class="dp-more-shade"></span>
            <span class="dp-more-text">
              <span class="dp-more-region">{{ d.region }}</span>
              <span class="dp-more-name">{{ d.name }}</span>
            </span>
          </a>
        </div>
      </div>
    </section>

    <!-- ================= CTA ================= -->
    <section class="dp-cta" aria-labelledby="cta-heading">
      <span class="dp-cta-bg" :style="{ backgroundImage: `url(${hero})` }" aria-hidden="true"></span>
      <div class="dp-wrap dp-cta-inner">
        <h2 id="cta-heading" class="dp-cta-title">Ready to see {{ name }}? We will plan the rest.</h2>
        <div class="dp-cta-actions">
          <a href="/plan-your-trip" class="dp-btn dp-btn-sun">Plan my trip</a>
          <a :href="`tel:${phoneRaw}`" class="dp-btn dp-btn-line">Call {{ phoneDisplay }}</a>
        </div>
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
import imgGalle from "@/assets/carousel/Gallefort.jpg";
import imgMirissa from "@/assets/carousel/Mirissa.jpg";
import imgNineArch from "@/assets/carousel/Ninearch.jpg";
import imgSigiriya from "@/assets/carousel/Seegiriya.png";

export default {
  name: "DestinationLayout",
  components: { LankaNavBar, FooterPage, ArrowButton },

  props: {
    name: { type: String, required: true },
    region: { type: String, default: "" },
    tagline: { type: String, default: "" },
    hero: { type: String, required: true },
    intro: { type: Array, default: () => [] },
    facts: { type: Array, default: () => [] },
    highlights: { type: Array, default: () => [] },
    bestTime: { type: Object, default: () => ({ months: "", note: "" }) },
    itinerary: { type: Array, default: () => [] },
    tips: { type: Array, default: () => [] },
  },

  data() {
    return {
      // ---- keep in sync with the navbar / footer ----
      phoneRaw: "+94776809002",
      phoneDisplay: "+94 77 680 9002",
      // -----------------------------------------------

      // Used by the "More places" strip (the current page is filtered out)
      allDestinations: [
        { name: "Sigiriya", region: "Cultural Triangle", link: "/destinations/sigiriya", src: imgSigiriya },
        { name: "Ella", region: "Hill country", link: "/destinations/ella", src: imgNineArch },
        { name: "Kandy", region: "Central highlands", link: "/destinations/kandy", src: imgAmbuluwawa },
        { name: "Galle", region: "South coast", link: "/destinations/galle", src: imgGalle },
        { name: "Yala", region: "Southeast", link: "/destinations/yala", src: "https://picsum.photos/seed/lk-yala/800/600" },
        { name: "Mirissa", region: "South coast", link: "/destinations/mirissa", src: imgMirissa },
        { name: "Nuwara Eliya", region: "Tea country", link: "/destinations/nuwara-eliya", src: "https://picsum.photos/seed/lk-nuwara/800/600" },
      ],
    };
  },

  computed: {
    otherDestinations() {
      return this.allDestinations.filter((d) => d.name !== this.name);
    },
  },

  mounted() {
    document.title = `${this.name} | Ceylon Escapes`;
  },
};
</script>

<style scoped>
/* ===== TOKENS (same blue theme as the home page) ===== */
.dp {
  --navy: #0b2a4a;
  --blue: #1565c0;
  --blue-bright: #1e88e5;
  --sky: #8ec9ff;
  --sand: #f6f1e7;
  --sun: #f5b94a;
  --text: #1f2933;
  --muted: #52606d;
  --shadow-sm: 0 4px 14px rgba(11, 42, 74, 0.08);
  --shadow-md: 0 14px 34px rgba(11, 42, 74, 0.16);
  --font-display: "Young Serif", Georgia, "Times New Roman", serif;
  --font-body: "Figtree", system-ui, -apple-system, "Segoe UI", Roboto, sans-serif;

  font-family: var(--font-body);
  color: var(--text);
  overflow-x: hidden;
}
.dp .material-icons {
  font-size: 22px;
  line-height: 1;
}
.dp-wrap {
  position: relative;
  width: 100%;
  max-width: 1200px;
  margin: 0 auto;
  padding: 0 max(24px, 4vw);
  box-sizing: border-box;
}
.dp-section {
  position: relative;
  padding: 88px 0;
  background: #fff;
}
.dp-sand {
  background: var(--sand);
}

/* ===== TYPE ===== */
.dp-h1,
.dp-h2,
.dp-cta-title {
  font-family: var(--font-display);
  font-weight: 400;
  text-wrap: balance;
}
.dp-h1 {
  margin: 0;
  font-size: clamp(2.8rem, 7vw, 5.2rem);
  line-height: 1.03;
  color: #fff;
}
.dp-h2 {
  margin: 0 0 10px;
  font-size: clamp(1.7rem, 3vw, 2.4rem);
  line-height: 1.15;
  color: var(--navy);
}
.dp-h2-light {
  color: #fff;
}
.dp-eyebrow {
  display: inline-flex;
  align-items: center;
  gap: 10px;
  margin: 0 0 14px;
  font-size: 0.8rem;
  font-weight: 700;
  letter-spacing: 0.16em;
  text-transform: uppercase;
  color: var(--blue);
}
.dp-eyebrow::before {
  content: "";
  width: 28px;
  height: 2px;
  background: var(--sun);
  border-radius: 2px;
}
.dp-eyebrow-light {
  color: var(--sun);
}
.dp-rule {
  display: block;
  width: 72px;
  height: 4px;
  margin: 18px 0 22px;
  border-radius: 4px;
  background: linear-gradient(90deg, var(--sun), var(--blue-bright));
}
.dp-lead {
  margin: 0 0 16px;
  max-width: 62ch;
  font-size: 1.08rem;
  line-height: 1.75;
  color: var(--muted);
}
.dp-head {
  margin-bottom: 44px;
}

/* ===== HERO ===== */
.dp-hero {
  position: relative;
  display: flex;
  align-items: flex-end;
  min-height: 78vh;
  padding: 190px 0 120px;
  overflow: hidden;
  background: var(--navy);
}
.dp-hero-bg {
  position: absolute;
  inset: 0;
  background-size: cover;
  background-position: center;
  transform: scale(1.04);
}
.dp-hero-shade {
  position: absolute;
  inset: 0;
  background:
    linear-gradient(to top, rgba(11, 42, 74, 0.92) 0%, rgba(11, 42, 74, 0.35) 55%, rgba(11, 42, 74, 0.55) 100%);
}
.dp-hero-inner {
  z-index: 1;
}
.dp-hero-wave {
  position: absolute;
  left: 0;
  bottom: -1px;
  width: 100%;
  height: 70px;
  pointer-events: none;
}
.dp-crumbs {
  display: flex;
  flex-wrap: wrap;
  align-items: center;
  gap: 8px;
  margin-bottom: 26px;
  font-size: 0.88rem;
  color: rgba(255, 255, 255, 0.75);
}
.dp-crumbs a {
  color: inherit;
  text-decoration: none;
  transition: color 0.2s ease;
}
.dp-crumbs a:hover {
  color: var(--sun);
}
.dp-crumb-now {
  color: #fff;
  font-weight: 600;
}
.dp-tagline {
  max-width: 34ch;
  margin: 16px 0 30px;
  font-size: clamp(1.1rem, 2vw, 1.4rem);
  line-height: 1.5;
  color: rgba(255, 255, 255, 0.92);
}
.dp-hero-actions {
  display: flex;
  flex-wrap: wrap;
  gap: 12px;
}

/* ===== QUICK FACTS ===== */
.dp-facts-band {
  position: relative;
  z-index: 2;
  margin-top: -56px;
  padding-bottom: 8px;
}
.dp-facts {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  gap: 0;
  padding: 10px;
  background: #fff;
  border: 1px solid #e3dccb;
  border-top: 5px solid var(--sun);
  border-radius: 20px;
  box-shadow: var(--shadow-md);
}
.dp-fact {
  display: flex;
  align-items: center;
  gap: 14px;
  padding: 18px 20px;
}
.dp-fact + .dp-fact {
  border-left: 1px solid #ece6d8;
}
.dp-fact-icon {
  flex: 0 0 auto;
  display: grid;
  place-items: center;
  width: 46px;
  height: 46px;
  color: #fff;
  background: linear-gradient(135deg, var(--blue) 0%, var(--blue-bright) 100%);
  border-radius: 14px;
  box-shadow: 0 6px 14px rgba(21, 101, 192, 0.3);
}
.dp-fact-label {
  display: block;
  font-size: 0.72rem;
  font-weight: 700;
  letter-spacing: 0.12em;
  text-transform: uppercase;
  color: var(--blue);
}
.dp-fact-value {
  display: block;
  margin-top: 2px;
  font-family: var(--font-display);
  font-size: 1.02rem;
  line-height: 1.25;
  color: var(--navy);
}

/* ===== OVERVIEW ===== */
.dp-overview {
  display: grid;
  grid-template-columns: 1.4fr 1fr;
  gap: 64px;
  align-items: start;
}
.dp-side {
  display: flex;
  flex-direction: column;
  gap: 20px;
}
.dp-card {
  padding: 28px;
  border-radius: 20px;
  box-shadow: var(--shadow-sm);
}
.dp-card-kicker {
  display: flex;
  align-items: center;
  gap: 8px;
  margin: 0 0 10px;
  font-size: 0.78rem;
  font-weight: 700;
  letter-spacing: 0.12em;
  text-transform: uppercase;
}
.dp-card-text {
  margin: 0;
  line-height: 1.65;
}
.dp-season {
  background: linear-gradient(135deg, #f2f8ff 0%, #e6f1fd 100%);
  border-left: 5px solid var(--blue-bright);
}
.dp-season .dp-card-kicker {
  color: var(--blue);
}
.dp-season-months {
  margin: 0 0 8px;
  font-family: var(--font-display);
  font-size: 2.1rem;
  line-height: 1.1;
  color: var(--navy);
}
.dp-season .dp-card-text {
  color: var(--muted);
}
.dp-help {
  color: #fff;
  background: linear-gradient(135deg, var(--navy) 0%, #12457a 100%);
}
.dp-help .dp-card-kicker {
  color: var(--sun);
}
.dp-help .dp-card-text {
  color: rgba(255, 255, 255, 0.85);
}
.dp-help-phone {
  display: inline-flex;
  align-items: center;
  gap: 8px;
  margin-top: 16px;
  font-weight: 700;
  color: var(--sun);
  text-decoration: none;
}
.dp-help-phone:hover {
  text-decoration: underline;
}

/* ===== HIGHLIGHTS ===== */
.dp-hl-grid {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 24px;
}
.dp-hl {
  position: relative;
  padding: 32px 28px 28px;
  background: #fff;
  border: 1px solid #e3dccb;
  border-radius: 20px;
  box-shadow: var(--shadow-sm);
  transition: transform 0.35s ease, box-shadow 0.35s ease;
}
.dp-hl:hover {
  transform: translateY(-8px);
  box-shadow: var(--shadow-md);
}
.dp-hl-num {
  position: absolute;
  top: 20px;
  right: 24px;
  font-family: var(--font-display);
  font-size: 1.5rem;
  color: rgba(21, 101, 192, 0.18);
}
.dp-hl-icon {
  display: grid;
  place-items: center;
  width: 52px;
  height: 52px;
  margin-bottom: 18px;
  color: var(--navy);
  background: var(--sun);
  border-radius: 16px;
  box-shadow: 0 6px 14px rgba(245, 185, 74, 0.45);
}
.dp-hl-title {
  margin: 0 0 8px;
  font-family: var(--font-display);
  font-weight: 400;
  font-size: 1.3rem;
  line-height: 1.2;
  color: var(--navy);
}
.dp-hl-text {
  margin: 0;
  line-height: 1.65;
  color: var(--muted);
}

/* ===== ITINERARY ===== */
.dp-plan {
  display: grid;
  grid-template-columns: 1fr 1.3fr;
  gap: 64px;
  align-items: start;
}
.dp-plan-intro {
  position: sticky;
  top: 110px;
}
.dp-timeline {
  position: relative;
  margin: 0;
  padding: 0 0 0 34px;
  list-style: none;
}
.dp-timeline::before {
  content: "";
  position: absolute;
  top: 8px;
  bottom: 8px;
  left: 9px;
  width: 3px;
  border-radius: 3px;
  background: linear-gradient(to bottom, var(--sun), var(--blue-bright));
}
.dp-step {
  position: relative;
  padding: 0 0 32px;
}
.dp-step:last-child {
  padding-bottom: 0;
}
.dp-step-dot {
  position: absolute;
  top: 4px;
  left: -34px;
  width: 21px;
  height: 21px;
  background: #fff;
  border: 4px solid var(--blue-bright);
  border-radius: 50%;
  box-sizing: border-box;
}
.dp-step-time {
  margin: 0 0 4px;
  font-size: 0.8rem;
  font-weight: 700;
  letter-spacing: 0.12em;
  text-transform: uppercase;
  color: var(--blue);
}
.dp-step-title {
  margin: 0 0 6px;
  font-family: var(--font-display);
  font-weight: 400;
  font-size: 1.35rem;
  color: var(--navy);
}
.dp-step-text {
  margin: 0;
  line-height: 1.65;
  color: var(--muted);
}

/* ===== TIPS ===== */
.dp-tips-wrap {
  background: linear-gradient(135deg, var(--navy) 0%, #12457a 100%);
}
.dp-tips-wrap::before {
  content: "";
  position: absolute;
  inset: 0;
  background-image: radial-gradient(rgba(255, 255, 255, 0.07) 1.5px, transparent 1.5px);
  background-size: 22px 22px;
  pointer-events: none;
}
.dp-tips {
  position: relative;
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  gap: 16px 28px;
  margin: 0;
  padding: 0;
  list-style: none;
}
.dp-tips li {
  display: flex;
  align-items: flex-start;
  gap: 14px;
  padding: 18px 22px;
  color: rgba(255, 255, 255, 0.92);
  line-height: 1.6;
  background: rgba(255, 255, 255, 0.07);
  border: 1px solid rgba(255, 255, 255, 0.12);
  border-radius: 16px;
}
.dp-tips .material-icons {
  flex: 0 0 auto;
  margin-top: 1px;
  color: var(--sun);
}

/* ===== MORE DESTINATIONS ===== */
.dp-more-grid {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  grid-auto-rows: 200px;
  gap: 18px;
}
.dp-more {
  position: relative;
  display: block;
  overflow: hidden;
  border-radius: 18px;
  text-decoration: none;
  color: #fff;
  background: var(--navy);
  box-shadow: var(--shadow-sm);
  transition: transform 0.35s ease, box-shadow 0.35s ease;
}
.dp-more:hover,
.dp-more:focus-visible {
  transform: translateY(-6px);
  box-shadow: var(--shadow-md);
}
.dp-more-photo {
  position: absolute;
  inset: 0;
  background-size: cover;
  background-position: center;
  transition: transform 0.7s ease;
}
.dp-more:hover .dp-more-photo {
  transform: scale(1.08);
}
.dp-more-shade {
  position: absolute;
  inset: 0;
  background: linear-gradient(to top, rgba(11, 42, 74, 0.9) 0%, rgba(11, 42, 74, 0) 65%);
}
.dp-more-text {
  position: absolute;
  left: 20px;
  right: 20px;
  bottom: 16px;
  display: flex;
  flex-direction: column;
  gap: 2px;
}
.dp-more-region {
  font-size: 0.75rem;
  font-weight: 600;
  letter-spacing: 0.08em;
  text-transform: uppercase;
  color: var(--sun);
}
.dp-more-name {
  font-family: var(--font-display);
  font-size: 1.4rem;
}

/* ===== CTA ===== */
.dp-cta {
  position: relative;
  overflow: hidden;
  padding: 90px 0;
  background: var(--navy);
}
.dp-cta-bg {
  position: absolute;
  inset: 0;
  background-size: cover;
  background-position: center;
  opacity: 0.35;
}
.dp-cta::after {
  content: "";
  position: absolute;
  inset: 0;
  background: linear-gradient(135deg, rgba(11, 42, 74, 0.92) 0%, rgba(21, 101, 192, 0.78) 100%);
}
.dp-cta-inner {
  z-index: 1;
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: 32px;
}
.dp-cta-title {
  margin: 0;
  max-width: 24ch;
  font-size: clamp(1.7rem, 3.2vw, 2.6rem);
  line-height: 1.15;
  color: #fff;
}
.dp-cta-actions {
  display: flex;
  flex-wrap: wrap;
  gap: 12px;
}

/* ===== BUTTONS ===== */
.dp-btn {
  display: inline-block;
  padding: 13px 28px;
  font-size: 0.95rem;
  font-weight: 600;
  text-decoration: none;
  border-radius: 999px;
  transition: background-color 0.25s ease, color 0.25s ease, border-color 0.25s ease,
    transform 0.25s ease, box-shadow 0.25s ease;
}
.dp-btn:hover {
  transform: translateY(-2px);
}
.dp-btn-solid {
  color: #fff;
  background: var(--blue);
  box-shadow: 0 6px 16px rgba(21, 101, 192, 0.3);
}
.dp-btn-solid:hover {
  background: var(--navy);
}
.dp-btn-sun {
  color: var(--navy);
  background: var(--sun);
  box-shadow: 0 8px 20px rgba(245, 185, 74, 0.4);
}
.dp-btn-sun:hover {
  background: #fff;
}
.dp-btn-line {
  color: #fff;
  border: 1.5px solid rgba(255, 255, 255, 0.6);
}
.dp-btn-line:hover {
  border-color: #fff;
  background: rgba(255, 255, 255, 0.12);
}
.dp-btn:focus-visible,
.dp-more:focus-visible {
  outline: 3px solid var(--sky);
  outline-offset: 3px;
}

/* ===== RESPONSIVE ===== */
@media (max-width: 1024px) {
  .dp-facts {
    grid-template-columns: repeat(2, 1fr);
  }
  .dp-fact:nth-child(3) {
    border-left: 0;
  }
  .dp-fact:nth-child(n + 3) {
    border-top: 1px solid #ece6d8;
  }
  .dp-overview,
  .dp-plan {
    grid-template-columns: 1fr;
    gap: 40px;
  }
  .dp-plan-intro {
    position: static;
  }
  .dp-hl-grid,
  .dp-more-grid {
    grid-template-columns: repeat(2, 1fr);
  }
}
@media (max-width: 768px) {
  .dp-section {
    padding: 60px 0;
  }
  .dp-hero {
    min-height: 70vh;
    padding: 150px 0 100px;
  }
  .dp-tips {
    grid-template-columns: 1fr;
  }
  .dp-cta {
    padding: 60px 0;
  }
  .dp-cta-inner {
    flex-direction: column;
    align-items: flex-start;
  }
}
@media (max-width: 560px) {
  .dp-facts {
    grid-template-columns: 1fr;
  }
  .dp-fact + .dp-fact {
    border-left: 0;
    border-top: 1px solid #ece6d8;
  }
  .dp-hl-grid,
  .dp-more-grid {
    grid-template-columns: 1fr;
  }
  .dp-more-grid {
    grid-auto-rows: 190px;
  }
}
@media (prefers-reduced-motion: reduce) {
  .dp-hl,
  .dp-more,
  .dp-more-photo,
  .dp-btn {
    transition: none;
  }
  .dp-hl:hover,
  .dp-more:hover,
  .dp-more:hover .dp-more-photo,
  .dp-btn:hover {
    transform: none;
  }
}
</style>
