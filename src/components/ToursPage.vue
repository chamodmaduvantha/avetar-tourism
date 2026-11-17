<template>
  <!-- The ONLY nav bar on this page. Do not add another one here or in a parent layout. -->
  <LankaNavBar />

  <main class="tr-page">
    <!-- ===== Hero ===== -->
    <section class="tr-hero" aria-labelledby="tr-title">
      <span class="tr-hero-photo" :style="{ backgroundImage: `url(${heroBg})` }" aria-hidden="true"></span>
      <span class="tr-hero-shade" aria-hidden="true"></span>

      <div class="tr-wrap tr-hero-inner">
        <nav class="tr-crumbs" aria-label="Breadcrumb">
          <a href="/">Home</a>
          <span aria-hidden="true">/</span>
          <span aria-current="page">Tours</span>
        </nav>

        <h1 id="tr-title" class="tr-hero-title">Journeys planned around you</h1>
        <p class="tr-hero-lead">
          Start with one of our routes, then change anything. Every trip can be shortened,
          stretched or mixed with another, and we price it once we know your dates.
        </p>

        <div class="tr-hero-actions">
          <a href="#tr-list" class="tr-btn tr-btn-sun">Browse the tours</a>
          <a href="/plan-your-trip" class="tr-btn tr-btn-line">Plan my own trip</a>
        </div>
      </div>

      <svg class="tr-hero-wave" viewBox="0 0 1440 80" preserveAspectRatio="none" aria-hidden="true">
        <path d="M0,40 C240,90 480,0 720,36 C960,72 1200,10 1440,44 L1440,80 L0,80 Z" fill="#ffffff" />
      </svg>
    </section>

    <!-- ===== Filters + list ===== -->
    <section id="tr-list" class="tr-section" aria-labelledby="tr-list-heading">
      <div class="tr-wrap">
        <div class="tr-head">
          <p class="tr-eyebrow">Our tours</p>
          <h2 id="tr-list-heading" class="tr-h2">Start somewhere, then make it yours</h2>
          <p class="tr-sub">
            Filter by the kind of trip you have in mind, or search by place. Every route is a
            first draft that we rewrite around your pace and budget.
          </p>
        </div>

        <!-- Toolbar -->
        <div class="tr-toolbar">
          <div class="tr-search">
            <label for="tr-search-input" class="tr-visually-hidden">Search tours</label>
            <input
              id="tr-search-input"
              v-model.trim="query"
              type="search"
              placeholder="Search by place or trip, e.g. Ella"
              autocomplete="off"
            />
          </div>

          <div class="tr-sort">
            <label for="tr-sort-select">Sort by</label>
            <select id="tr-sort-select" v-model="sortBy">
              <option value="featured">Featured</option>
              <option value="shortest">Shortest first</option>
              <option value="longest">Longest first</option>
            </select>
          </div>
        </div>

        <div class="tr-filters" role="group" aria-label="Filter tours by type">
          <button
            v-for="c in categories"
            :key="c"
            type="button"
            class="tr-filter"
            :class="{ 'is-active': activeCategory === c }"
            :aria-pressed="activeCategory === c ? 'true' : 'false'"
            @click="activeCategory = c"
          >
            {{ c }}
          </button>
        </div>

        <p class="tr-count" role="status" aria-live="polite">
          {{ filteredTours.length }} {{ filteredTours.length === 1 ? "journey" : "journeys" }}
        </p>

        <!-- Cards -->
        <div v-if="filteredTours.length" class="tr-grid">
          <article class="tr-card" v-for="t in filteredTours" :key="t.slug">
            <div class="tr-card-photo-wrap">
              <div class="tr-card-photo" :style="{ backgroundImage: `url(${t.src})` }" role="img" :aria-label="t.alt"></div>
              <span class="tr-card-badge">{{ dayLabel(t.days) }}</span>
            </div>

            <div class="tr-card-body">
              <p class="tr-card-category">{{ t.category }}</p>
              <h3 class="tr-card-title">{{ t.title }}</h3>
              <p class="tr-card-route">{{ t.route }}</p>
              <p class="tr-card-text">{{ t.text }}</p>

              <ul class="tr-card-highlights">
                <li v-for="h in t.highlights" :key="h">{{ h }}</li>
              </ul>

              <div class="tr-card-actions">
                <a
                  :href="whatsappFor(t)"
                  class="tr-btn tr-btn-solid"
                  target="_blank"
                  rel="noopener noreferrer"
                >Ask about this trip</a>
                <a href="/plan-your-trip" class="tr-card-link">Plan it my way</a>
              </div>
            </div>
          </article>
        </div>

        <!-- Empty state -->
        <div v-else class="tr-empty">
          <h3 class="tr-empty-title">No tours match that search</h3>
          <p class="tr-empty-text">
            Try a different place or clear the filters. If you have something else in mind, we can
            plan it from scratch.
          </p>
          <div class="tr-empty-actions">
            <button type="button" class="tr-btn tr-btn-solid" @click="resetFilters">Clear filters</button>
            <a href="/plan-your-trip" class="tr-btn tr-btn-outline">Plan my own trip</a>
          </div>
        </div>
      </div>
    </section>

    <!-- ===== What every tour includes ===== -->
    <section class="tr-section tr-sand" aria-labelledby="tr-incl-heading">
      <div class="tr-wrap">
        <div class="tr-head">
          <p class="tr-eyebrow">Every journey</p>
          <h2 id="tr-incl-heading" class="tr-h2">What stays the same on every tour</h2>
        </div>

        <div class="tr-incl-grid">
          <article class="tr-incl" v-for="(p, i) in promises" :key="p.title">
            <span class="tr-incl-num">{{ i + 1 }}</span>
            <h3 class="tr-incl-title">{{ p.title }}</h3>
            <p class="tr-incl-text">{{ p.text }}</p>
          </article>
        </div>
      </div>
    </section>

    <!-- ===== CTA ===== -->
    <section class="tr-cta" aria-labelledby="tr-cta-heading">
      <span class="tr-cta-bg" :style="{ backgroundImage: `url(${ctaBg})` }" aria-hidden="true"></span>
      <div class="tr-wrap tr-cta-inner">
        <h2 id="tr-cta-heading" class="tr-cta-title">Tell us when you are coming. We will plan the rest.</h2>
        <div class="tr-cta-actions">
          <a href="/plan-your-trip" class="tr-btn tr-btn-sun">Plan my trip</a>
          <a :href="`tel:${phoneRaw}`" class="tr-btn tr-btn-line">Call {{ phoneDisplay }}</a>
          <a
            :href="generalWhatsapp"
            class="tr-btn tr-btn-whatsapp"
            target="_blank"
            rel="noopener noreferrer"
          >WhatsApp us</a>
        </div>
      </div>
    </section>

    <!-- ===== Quick links ===== -->
    <section class="tr-more" aria-label="More pages">
      <div class="tr-wrap">
        <p class="tr-more-label">Looking for something else?</p>
        <div class="tr-more-links">
          <a href="/" class="tr-pill">Back to home</a>
          <a href="/destinations" class="tr-pill">Destinations</a>
          <a href="/about" class="tr-pill">About us</a>
          <a href="/contact" class="tr-pill">Contact us</a>
        </div>
      </div>
    </section>
  </main>

  <ArrowButton></ArrowButton>
  <FooterPage></FooterPage>
</template>

<script>
import LankaNavBar from "./Lankanavbar.vue";
import FooterPage from "./FooterPage.vue";
import ArrowButton from "./ArrowButton.vue";

// Same local images the home page uses (src/assets/carousel)
import imgAmbuluwawa from "@/assets/carousel/Ambuluwawa.jpg";
import imgBeach from "@/assets/carousel/Beach.jpg";
import imgGalle from "@/assets/carousel/Gallefort.jpg";
import imgMirissa from "@/assets/carousel/Mirissa.jpg";
import imgNineArch from "@/assets/carousel/Ninearch.jpg";
import imgSigiriya from "@/assets/carousel/Seegiriya.png";

export default {
  name: "ToursPage",
  components: {
    LankaNavBar,
    FooterPage,
    ArrowButton,
  },
  data() {
    return {
      // ---- EDIT: brand details (keep in sync with the nav bar, home page and footer) ----
      brandName: "Ceylon Escapes",
      phoneRaw: "+94776809002",
      phoneDisplay: "+94 77 680 9002",
      // WhatsApp needs the number in international format with no + and no spaces
      whatsappNumber: "94776809002",
      // -----------------------------------------------------------------------------------

      heroBg: imgNineArch,
      ctaBg: imgBeach,

      query: "",
      activeCategory: "All",
      sortBy: "featured",

      // Placeholder tours: adjust days, routes, text and highlights to your real packages.
      // `slug` must be unique. `category` must match one of the filter names shown on the page.
      tours: [
        {
          slug: "ancient-cities-and-kandy",
          days: 5,
          category: "Culture & Heritage",
          title: "Ancient cities and Kandy",
          route: "Negombo, Sigiriya, Kandy",
          text: "Rock fortress, cave temples and the hill capital, with time to wander between them.",
          highlights: ["Sigiriya at sunrise", "Cave temple visit", "Temple of the Tooth"],
          src: imgSigiriya,
          alt: "Sigiriya rock fortress",
        },
        {
          slug: "tea-country-by-train",
          days: 4,
          category: "Hill Country",
          title: "Tea country by train",
          route: "Kandy, Nuwara Eliya, Ella",
          text: "Slow rail journeys, plantation walks and sunrise viewpoints in the cool hills.",
          highlights: ["Scenic train ride", "Tea estate visit", "Nine Arch Bridge"],
          src: imgNineArch,
          alt: "Train on the Nine Arch Bridge in Ella",
        },
        {
          slug: "south-coast-and-wildlife",
          days: 6,
          category: "Wildlife",
          title: "South coast and wildlife",
          route: "Galle, Mirissa, Yala",
          text: "Fort town mornings, beach afternoons and a safari at first light.",
          highlights: ["Yala jeep safari", "Galle Fort walk", "Time on the beach"],
          src: imgBeach,
          alt: "Palm trees on a southern beach",
        },
        {
          slug: "whale-watching-weekend",
          days: 3,
          category: "Wildlife",
          title: "Whale watching weekend",
          route: "Galle, Mirissa",
          text: "A short coastal escape built around an early boat trip in whale season, from December to March.",
          highlights: ["Morning boat trip", "Sunset at Galle Fort", "Beach time in Mirissa"],
          src: imgMirissa,
          alt: "Beach at Mirissa",
        },
        {
          slug: "beach-days-south-coast",
          days: 5,
          category: "Beach & Coast",
          title: "Beach days on the south coast",
          route: "Galle, Mirissa",
          text: "Unhurried days by the sea, with Galle Fort in the cool of the afternoon and nothing booked in the middle.",
          highlights: ["Beachfront stay", "Snorkelling or surf lesson", "Fort town evenings"],
          src: imgMirissa,
          alt: "Palm-lined beach in Mirissa",
        },
        {
          slug: "adventure-in-the-hills",
          days: 7,
          category: "Adventure",
          title: "Adventure in the hills",
          route: "Kandy, Ambuluwawa, Ella, Nuwara Eliya",
          text: "An active week of viewpoints, waterfalls and walks, with train rides to rest between them.",
          highlights: ["Ambuluwawa viewpoint", "Hikes around Ella", "Hill-country train"],
          src: imgAmbuluwawa,
          alt: "Ambuluwawa tower above green hills",
        },
        {
          slug: "honeymoon-island-escape",
          days: 8,
          category: "Honeymoon",
          title: "Honeymoon island escape",
          route: "Sigiriya, Ella, Galle, Mirissa",
          text: "Boutique stays, quiet mornings and a few well-chosen experiences, ending with days by the sea.",
          highlights: ["Boutique hotels", "Sunset on the Galle ramparts", "Private transfers"],
          src: imgBeach,
          alt: "Quiet beach at sunset",
        },
        {
          slug: "island-highlights",
          days: 10,
          category: "Culture & Heritage",
          title: "Island highlights",
          route: "Negombo, Sigiriya, Kandy, Ella, Yala, Galle",
          text: "The classic loop at a relaxed pace: ancient cities, tea country by train, a safari and the southern coast.",
          highlights: ["Two UNESCO sites", "Scenic train ride", "Safari and fort town"],
          src: imgGalle,
          alt: "Galle Fort lighthouse by the sea",
        },
        {
          slug: "grand-island-circuit",
          days: 14,
          category: "Adventure",
          title: "Grand island circuit",
          route: "Negombo, Sigiriya, Kandy, Nuwara Eliya, Ella, Yala, Mirissa, Galle",
          text: "Two weeks with room to breathe: the cultural north, the hills, the wildlife parks and a long finish on the coast.",
          highlights: ["Time at every stop", "Rest days built in", "Fully tailored route"],
          src: imgAmbuluwawa,
          alt: "Green hills near Kandy",
        },
      ],

      promises: [
        {
          title: "Private and flexible",
          text: "Trips run for your group only, and every day can be moved, swapped or dropped.",
        },
        {
          title: "Local guides",
          text: "Licensed guides who know the stories, the shortcuts and the best time of day for each place.",
        },
        {
          title: "Comfortable stays",
          text: "Hotels, villas and guesthouses chosen for location and character, across several budgets.",
        },
        {
          title: "Support on the road",
          text: "A real person to call during your trip, in case plans change.",
        },
      ],
    };
  },
  computed: {
    categories() {
      const seen = [];
      this.tours.forEach((t) => {
        if (!seen.includes(t.category)) seen.push(t.category);
      });
      return ["All", ...seen];
    },
    filteredTours() {
      const q = this.query.toLowerCase();
      let list = this.tours.filter((t) => {
        const inCategory = this.activeCategory === "All" || t.category === this.activeCategory;
        const haystack = `${t.title} ${t.route} ${t.text} ${t.category}`.toLowerCase();
        return inCategory && (!q || haystack.includes(q));
      });
      if (this.sortBy === "shortest") {
        list = [...list].sort((a, b) => a.days - b.days);
      } else if (this.sortBy === "longest") {
        list = [...list].sort((a, b) => b.days - a.days);
      }
      return list;
    },
    generalWhatsapp() {
      const text = `Hello ${this.brandName}, I would like to plan a trip to Sri Lanka.`;
      return `https://wa.me/${this.whatsappNumber}?text=${encodeURIComponent(text)}`;
    },
  },
  methods: {
    dayLabel(days) {
      return `${days} ${days === 1 ? "day" : "days"}`;
    },
    whatsappFor(tour) {
      const text = `Hello ${this.brandName}, I would like to know more about the "${tour.title}" trip (${this.dayLabel(tour.days)}).`;
      return `https://wa.me/${this.whatsappNumber}?text=${encodeURIComponent(text)}`;
    },
    resetFilters() {
      this.query = "";
      this.activeCategory = "All";
      this.sortBy = "featured";
    },
  },
};
</script>

<style scoped>
/*
  All class names on this page start with "tr-" so they can never clash with the
  nav bar's global "lk-" styles.
*/

/* ===== TOKENS (same blue theme as the home page and footer) ===== */
.tr-page {
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

.tr-wrap {
  position: relative;
  width: 100%;
  max-width: 1200px;
  margin: 0 auto;
  padding: 0 max(24px, 4vw);
  box-sizing: border-box;
}

.tr-section {
  position: relative;
  padding: 80px 0 96px;
  background: #fff;
}
.tr-sand {
  background: var(--sand);
}

.tr-visually-hidden {
  position: absolute;
  width: 1px;
  height: 1px;
  margin: -1px;
  padding: 0;
  overflow: hidden;
  clip: rect(0, 0, 0, 0);
  white-space: nowrap;
  border: 0;
}

/* ===== TYPE ===== */
.tr-h2,
.tr-hero-title,
.tr-cta-title {
  font-family: var(--font-display);
  font-weight: 400;
  text-wrap: balance;
}
.tr-h2 {
  margin: 0 0 10px;
  max-width: 24ch;
  font-size: clamp(1.7rem, 3vw, 2.4rem);
  line-height: 1.15;
  text-align: left;
  text-transform: none;
  color: var(--navy);
}
.tr-eyebrow {
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
.tr-eyebrow::before {
  content: "";
  width: 28px;
  height: 2px;
  background: var(--sun);
  border-radius: 2px;
}
.tr-sub {
  margin: 0;
  max-width: 60ch;
  font-size: 1.05rem;
  line-height: 1.6;
  color: var(--muted);
}
.tr-head {
  margin-bottom: 36px;
}

/* ===== HERO ===== */
.tr-hero {
  position: relative;
  overflow: hidden;
  padding: 72px 0 130px;
  color: #fff;
  background: var(--navy);
}
.tr-hero-photo {
  position: absolute;
  inset: 0;
  background-size: cover;
  background-position: center;
  /* the one deliberate motion on this page: a slow settle on load */
  animation: tr-drift 14s ease-out forwards;
}
@keyframes tr-drift {
  from { transform: scale(1.08); }
  to { transform: scale(1); }
}
.tr-hero-shade {
  position: absolute;
  inset: 0;
  background: linear-gradient(135deg, rgba(11, 42, 74, 0.9) 0%, rgba(21, 101, 192, 0.6) 100%);
}
.tr-hero-inner {
  position: relative;
  z-index: 2;
}
.tr-crumbs {
  display: flex;
  align-items: center;
  gap: 10px;
  margin-bottom: 26px;
  font-size: 0.9rem;
  font-weight: 600;
  color: rgba(255, 255, 255, 0.8);
}
.tr-crumbs a {
  color: var(--sky);
  text-decoration: none;
}
.tr-crumbs a:hover {
  text-decoration: underline;
}
.tr-hero-title {
  margin: 0 0 18px;
  max-width: 16ch;
  font-size: clamp(2.2rem, 5.2vw, 4rem);
  line-height: 1.08;
  letter-spacing: -0.01em;
  color: #fff;
  text-align: left;
  text-transform: none;
}
.tr-hero-lead {
  margin: 0 0 30px;
  max-width: 54ch;
  font-size: 1.15rem;
  line-height: 1.7;
  color: rgba(255, 255, 255, 0.92);
}
.tr-hero-actions {
  display: flex;
  flex-wrap: wrap;
  gap: 12px;
}
.tr-hero-wave {
  position: absolute;
  left: 0;
  bottom: -1px;
  z-index: 3;
  width: 100%;
  height: 60px;
  display: block;
  pointer-events: none;
}

/* ===== TOOLBAR ===== */
.tr-toolbar {
  display: flex;
  flex-wrap: wrap;
  align-items: flex-end;
  gap: 16px;
  margin-bottom: 20px;
}
.tr-search {
  flex: 1 1 320px;
}
.tr-search input,
.tr-sort select {
  width: 100%;
  padding: 13px 16px;
  font: 500 0.95rem var(--font-body);
  color: var(--text);
  background: #fff;
  border: 1.5px solid #cfe0f3;
  border-radius: 999px;
  box-sizing: border-box;
  transition: border-color 0.2s ease, box-shadow 0.2s ease;
}
.tr-search input:focus,
.tr-sort select:focus {
  outline: none;
  border-color: var(--blue-bright);
  box-shadow: 0 0 0 3px rgba(30, 136, 229, 0.18);
}
.tr-search input::placeholder {
  color: #93a6c0;
}
.tr-sort {
  display: flex;
  flex-direction: column;
  gap: 6px;
  flex: 0 0 200px;
}
.tr-sort label {
  padding-left: 6px;
  font-size: 0.8rem;
  font-weight: 700;
  color: var(--navy);
}

/* ===== FILTER CHIPS ===== */
.tr-filters {
  display: flex;
  flex-wrap: wrap;
  gap: 8px;
}
.tr-filter {
  padding: 9px 18px;
  font: 600 0.9rem var(--font-body);
  color: var(--blue);
  background: #fff;
  border: 1.5px solid #cfe0f3;
  border-radius: 999px;
  cursor: pointer;
  transition: background-color 0.2s ease, color 0.2s ease, border-color 0.2s ease;
}
.tr-filter:hover {
  border-color: var(--blue-bright);
}
.tr-filter.is-active {
  color: #fff;
  background: var(--blue);
  border-color: var(--blue);
}
.tr-filter:focus-visible {
  outline: 3px solid var(--sky);
  outline-offset: 2px;
}
.tr-count {
  margin: 22px 0 24px;
  font-size: 0.9rem;
  font-weight: 600;
  color: var(--muted);
}

/* ===== CARDS ===== */
.tr-grid {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 28px;
}
.tr-card {
  display: flex;
  flex-direction: column;
  overflow: hidden;
  background: #fff;
  border: 1px solid #e3dccb;
  border-radius: 20px;
  box-shadow: var(--shadow-sm);
  transition: transform 0.3s ease, box-shadow 0.3s ease;
}
.tr-card:hover {
  transform: translateY(-6px);
  box-shadow: var(--shadow-md);
}
.tr-card-photo-wrap {
  position: relative;
  overflow: hidden;
}
.tr-card-photo {
  height: 220px;
  background-size: cover;
  background-position: center;
  transition: transform 0.7s ease;
}
.tr-card:hover .tr-card-photo {
  transform: scale(1.06);
}
.tr-card-badge {
  position: absolute;
  top: 16px;
  left: 16px;
  padding: 6px 14px;
  font-size: 0.85rem;
  font-weight: 700;
  color: var(--navy);
  background: var(--sun);
  border-radius: 999px;
  box-shadow: 0 4px 10px rgba(0, 0, 0, 0.18);
}
.tr-card-body {
  display: flex;
  flex-direction: column;
  flex: 1;
  padding: 24px;
}
.tr-card-category {
  margin: 0 0 6px;
  font-size: 0.8rem;
  font-weight: 700;
  color: var(--blue);
}
.tr-card-title {
  margin: 0 0 4px;
  font-family: var(--font-display);
  font-weight: 400;
  font-size: 1.4rem;
  line-height: 1.2;
  color: var(--navy);
}
.tr-card-route {
  margin: 0 0 12px;
  font-size: 0.92rem;
  font-weight: 600;
  line-height: 1.4;
  color: var(--muted);
}
.tr-card-text {
  margin: 0 0 16px;
  line-height: 1.6;
  color: var(--muted);
}
.tr-card-highlights {
  display: flex;
  flex-wrap: wrap;
  gap: 8px;
  margin: 0 0 22px;
  padding: 0;
  list-style: none;
}
.tr-card-highlights li {
  padding: 5px 12px;
  font-size: 0.8rem;
  font-weight: 600;
  color: var(--blue);
  background: #e6f1fd;
  border-radius: 999px;
}
.tr-card-actions {
  display: flex;
  flex-wrap: wrap;
  align-items: center;
  gap: 14px;
  margin-top: auto;
}
.tr-card-link {
  font-size: 0.9rem;
  font-weight: 700;
  color: var(--blue);
  text-decoration: none;
  border-bottom: 2px solid var(--sun);
}
.tr-card-link:hover {
  color: var(--navy);
}

/* ===== EMPTY STATE ===== */
.tr-empty {
  padding: 56px 28px;
  text-align: center;
  background: #f2f8ff;
  border: 1.5px dashed #cfe0f3;
  border-radius: 20px;
}
.tr-empty-title {
  margin: 0 0 8px;
  font-family: var(--font-display);
  font-weight: 400;
  font-size: 1.5rem;
  color: var(--navy);
}
.tr-empty-text {
  max-width: 52ch;
  margin: 0 auto 22px;
  line-height: 1.6;
  color: var(--muted);
}
.tr-empty-actions {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  gap: 12px;
}

/* ===== INCLUDED ===== */
.tr-incl-grid {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  gap: 24px;
}
.tr-incl {
  padding: 28px 24px;
  background: #fff;
  border: 1px solid #e3dccb;
  border-radius: 18px;
  box-shadow: var(--shadow-sm);
}
.tr-incl-num {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  width: 44px;
  height: 44px;
  margin-bottom: 16px;
  font-family: var(--font-display);
  font-size: 1.2rem;
  color: #fff;
  background: linear-gradient(135deg, var(--blue) 0%, var(--blue-bright) 100%);
  border-radius: 14px;
  box-shadow: 0 6px 14px rgba(21, 101, 192, 0.35);
}
.tr-incl-title {
  margin: 0 0 8px;
  font-size: 1.1rem;
  font-weight: 600;
  color: var(--navy);
}
.tr-incl-text {
  margin: 0;
  line-height: 1.6;
  color: var(--muted);
}

/* ===== CTA ===== */
.tr-cta {
  position: relative;
  overflow: hidden;
  padding: 96px 0;
  background: var(--navy);
}
.tr-cta-bg {
  position: absolute;
  inset: 0;
  background-size: cover;
  background-position: center;
  opacity: 0.35;
}
.tr-cta::after {
  content: "";
  position: absolute;
  inset: 0;
  background: linear-gradient(135deg, rgba(11, 42, 74, 0.92) 0%, rgba(21, 101, 192, 0.78) 100%);
}
.tr-cta-inner {
  position: relative;
  z-index: 1;
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: 32px;
}
.tr-cta-title {
  margin: 0;
  max-width: 24ch;
  font-size: clamp(1.7rem, 3.2vw, 2.6rem);
  line-height: 1.15;
  color: #fff;
}
.tr-cta-actions {
  display: flex;
  flex-wrap: wrap;
  gap: 12px;
}

/* ===== QUICK LINKS ===== */
.tr-more {
  padding: 48px 0 8px;
  text-align: center;
  background: #fff;
}
.tr-more-label {
  margin: 0 0 14px;
  font-size: 0.95rem;
  color: var(--muted);
}
.tr-more-links {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  gap: 10px;
}
.tr-pill {
  padding: 8px 18px;
  font-size: 0.9rem;
  font-weight: 600;
  color: var(--blue);
  text-decoration: none;
  border: 1.5px solid #cfe0f3;
  border-radius: 999px;
  transition: background-color 0.25s ease, color 0.25s ease, border-color 0.25s ease;
}
.tr-pill:hover {
  color: #fff;
  background: var(--blue);
  border-color: var(--blue);
}

/* ===== BUTTONS ===== */
.tr-btn {
  display: inline-block;
  padding: 13px 28px;
  font-family: var(--font-body);
  font-size: 0.95rem;
  font-weight: 600;
  text-decoration: none;
  border: 0;
  border-radius: 999px;
  cursor: pointer;
  transition: background-color 0.25s ease, color 0.25s ease, border-color 0.25s ease,
    transform 0.25s ease;
}
.tr-btn:hover {
  transform: translateY(-2px);
}
.tr-btn-sun {
  color: var(--navy);
  background: var(--sun);
  box-shadow: 0 8px 20px rgba(245, 185, 74, 0.4);
}
.tr-btn-sun:hover {
  background: #fff;
}
.tr-btn-solid {
  padding: 11px 22px;
  color: #fff;
  background: var(--blue);
  box-shadow: 0 6px 16px rgba(21, 101, 192, 0.3);
}
.tr-btn-solid:hover {
  background: var(--navy);
}
.tr-btn-outline {
  color: var(--blue);
  background: #fff;
  border: 1.5px solid var(--blue);
  padding: 11.5px 26px;
}
.tr-btn-outline:hover {
  color: #fff;
  background: var(--blue);
}
.tr-btn-line {
  color: #fff;
  border: 1.5px solid rgba(255, 255, 255, 0.6);
}
.tr-btn-line:hover {
  border-color: #fff;
  background: rgba(255, 255, 255, 0.12);
}
.tr-btn-whatsapp {
  color: #fff;
  background: #128c7e;
  box-shadow: 0 6px 16px rgba(18, 140, 126, 0.35);
}
.tr-btn-whatsapp:hover {
  background: #0e7266;
}
.tr-btn:focus-visible,
.tr-pill:focus-visible,
.tr-card-link:focus-visible,
.tr-crumbs a:focus-visible {
  outline: 3px solid var(--sky);
  outline-offset: 3px;
}

/* ===== RESPONSIVE ===== */
@media (max-width: 1024px) {
  .tr-grid {
    grid-template-columns: repeat(2, 1fr);
  }
  .tr-incl-grid {
    grid-template-columns: repeat(2, 1fr);
  }
}

@media (max-width: 768px) {
  .tr-section {
    padding: 56px 0 64px;
  }
  .tr-hero {
    padding: 48px 0 110px;
  }
  .tr-cta {
    padding: 60px 0;
  }
  .tr-cta-inner {
    flex-direction: column;
    align-items: flex-start;
  }
  .tr-sort {
    flex: 1 1 100%;
  }
}

@media (max-width: 640px) {
  .tr-grid,
  .tr-incl-grid {
    grid-template-columns: 1fr;
  }
  .tr-filters {
    flex-wrap: nowrap;
    overflow-x: auto;
    padding-bottom: 6px;
  }
  .tr-filter {
    flex: 0 0 auto;
  }
}

@media (prefers-reduced-motion: reduce) {
  .tr-hero-photo {
    animation: none;
  }
  .tr-card,
  .tr-card-photo,
  .tr-btn,
  .tr-pill,
  .tr-filter {
    transition: none;
  }
  .tr-card:hover,
  .tr-btn:hover,
  .tr-card:hover .tr-card-photo {
    transform: none;
  }
}
</style>