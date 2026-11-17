<template>
  <!-- The ONLY nav bar on this page. Do not add another one here or in a parent layout. -->
  <LankaNavBar />

  <main class="ab-page">
    <!-- ===== Hero ===== -->
    <section class="ab-hero" aria-labelledby="ab-title">
      <span class="ab-hero-photo" :style="{ backgroundImage: `url(${heroBg})` }" aria-hidden="true"></span>
      <span class="ab-hero-shade" aria-hidden="true"></span>

      <div class="ab-wrap ab-hero-inner">
        <nav class="ab-crumbs" aria-label="Breadcrumb">
          <a href="/">Home</a>
          <span aria-hidden="true">/</span>
          <span aria-current="page">About us</span>
        </nav>

        <h1 id="ab-title" class="ab-hero-title">Local people who know the island by heart</h1>
        <p class="ab-hero-lead">
          {{ brandName }} is a Sri Lankan travel company. We plan trips with local guides,
          honest advice and a real person on the phone from your first message to your last day.
        </p>

        <div class="ab-hero-actions">
          <a href="/plan-your-trip" class="ab-btn ab-btn-sun">Plan my trip</a>
          <a href="#ab-story" class="ab-btn ab-btn-line">Read our story</a>
        </div>
      </div>

      <ul class="ab-hero-facts" aria-label="At a glance">
        <li v-for="f in heroFacts" :key="f.label">
          <strong>{{ f.value }}</strong>
          <span>{{ f.label }}</span>
        </li>
      </ul>

      <svg class="ab-hero-wave" viewBox="0 0 1440 80" preserveAspectRatio="none" aria-hidden="true">
        <path d="M0,40 C240,90 480,0 720,36 C960,72 1200,10 1440,44 L1440,80 L0,80 Z" fill="#ffffff" />
      </svg>
    </section>

    <!-- ===== Our story ===== -->
    <section id="ab-story" class="ab-section" aria-labelledby="ab-story-heading">
      <span class="ab-blob" aria-hidden="true"></span>
      <div class="ab-wrap ab-story-grid">
        <div class="ab-story-text">
          <p class="ab-eyebrow">Our story</p>
          <h2 id="ab-story-heading" class="ab-h2">We started with one idea: show visitors the Sri Lanka we grew up in</h2>
          <span class="ab-rule" aria-hidden="true"></span>
          <p class="ab-lead">
            Most visitors see the same five stops from the back of a coach. We wanted to offer
            something different: quieter roads, family-run guesthouses, a driver who knows which
            temple is calm at 7 am, and a guide who can tell you the story behind what you are
            looking at.
          </p>
          <p class="ab-lead">
            Today we plan private and small-group trips across the whole island, from the Cultural
            Triangle to the hill country and the southern coast. Every route is built around the
            traveller, and every trip is checked by someone on our team who has been there.
          </p>
        </div>

        <div class="ab-collage">
          <figure class="ab-collage-a">
            <img :src="storyImages[0]" alt="Sigiriya rock fortress" loading="lazy" />
          </figure>
          <figure class="ab-collage-b">
            <img :src="storyImages[1]" alt="Galle Fort ramparts" loading="lazy" />
          </figure>
          <figure class="ab-collage-c">
            <img :src="storyImages[2]" alt="Mirissa beach" loading="lazy" />
          </figure>
          <p class="ab-collage-badge">
            <strong>{{ foundedLabel }}</strong>
            <span>Planning trips across Sri Lanka</span>
          </p>
        </div>
      </div>
    </section>

    <!-- ===== Stats band ===== -->
    <section class="ab-stats" ref="statsSection" aria-label="Our experience in numbers">
      <div class="ab-wrap ab-stats-grid">
        <div class="ab-stat" v-for="stat in stats" :key="stat.label">
          <p class="ab-stat-value">{{ stat.value.toLocaleString() }}{{ stat.suffix }}</p>
          <p class="ab-stat-label">{{ stat.label }}</p>
        </div>
      </div>
    </section>

    <!-- ===== Vision & Mission ===== -->
    <section id="ab-vision-mission" class="ab-section ab-sand" aria-labelledby="ab-vm-heading">
      <div class="ab-wrap">
        <div class="ab-head">
          <p class="ab-eyebrow">Who we are</p>
          <h2 id="ab-vm-heading" class="ab-h2">Our vision and our mission</h2>
        </div>

        <div class="ab-vm-grid">
          <article class="ab-vm" v-for="v in visionMission" :key="v.title">
            <span class="ab-icon-tile" aria-hidden="true">{{ v.icon }}</span>
            <h3 class="ab-vm-title">{{ v.title }}</h3>
            <blockquote class="ab-vm-text">{{ v.text }}</blockquote>
          </article>
        </div>
      </div>
    </section>

    <!-- ===== What we stand for ===== -->
    <section class="ab-section" aria-labelledby="ab-values-heading">
      <div class="ab-wrap">
        <div class="ab-head">
          <p class="ab-eyebrow">What we stand for</p>
          <h2 id="ab-values-heading" class="ab-h2">Four promises behind every trip</h2>
          <p class="ab-sub">These guide how we plan, who we work with and how we treat the places you visit.</p>
        </div>

        <div class="ab-values-grid">
          <article class="ab-value" v-for="p in promises" :key="p.title">
            <span class="ab-icon-tile ab-icon-tile-sm" aria-hidden="true">{{ p.icon }}</span>
            <h3 class="ab-value-title">{{ p.title }}</h3>
            <p class="ab-value-text">{{ p.text }}</p>
          </article>
        </div>
      </div>
    </section>

    <!-- ===== What we arrange ===== -->
    <section class="ab-section ab-sand" aria-labelledby="ab-arrange-heading">
      <div class="ab-wrap ab-arrange-grid">
        <div class="ab-arrange-intro">
          <p class="ab-eyebrow">What we arrange</p>
          <h2 id="ab-arrange-heading" class="ab-h2">Everything between the airport and the beach</h2>
          <p class="ab-sub">
            Take one piece or the whole journey. Tell us what you need and we will price it as a
            single plan.
          </p>
          <a href="/plan-your-trip" class="ab-btn ab-btn-solid">Plan my trip</a>
        </div>

        <ul class="ab-services">
          <li class="ab-service" v-for="s in services" :key="s.title">
            <span class="ab-service-mark" aria-hidden="true"></span>
            <div>
              <h3 class="ab-service-title">{{ s.title }}</h3>
              <p class="ab-service-text">{{ s.text }}</p>
            </div>
          </li>
        </ul>
      </div>
    </section>

    <!-- ===== Where we work ===== -->
    <section class="ab-section" aria-labelledby="ab-where-heading">
      <div class="ab-wrap">
        <div class="ab-head">
          <p class="ab-eyebrow">Where we work</p>
          <h2 id="ab-where-heading" class="ab-h2">From the ancient cities to the southern coast</h2>
          <p class="ab-sub">Pick a place to read more, or ask us about the quiet corners in between.</p>
        </div>

        <div class="ab-places">
          <a v-for="d in places" :key="d.name" :href="d.link" class="ab-place">
            <span class="ab-place-photo" :style="{ backgroundImage: `url(${d.src})` }"></span>
            <span class="ab-place-shade"></span>
            <span class="ab-place-text">
              <span class="ab-place-region">{{ d.region }}</span>
              <span class="ab-place-name">{{ d.name }}</span>
            </span>
          </a>
        </div>

        <p class="ab-places-more">
          <a href="/destinations">See all destinations</a>
        </p>
      </div>
    </section>

    <!-- ===== How it works (a real sequence, so numbered) ===== -->
    <section class="ab-section ab-sand" aria-labelledby="ab-steps-heading">
      <div class="ab-wrap">
        <div class="ab-head">
          <p class="ab-eyebrow">How it works</p>
          <h2 id="ab-steps-heading" class="ab-h2">From first message to the last day</h2>
          <p class="ab-sub">Four steps, and you can talk to the same person at each one.</p>
        </div>

        <ol class="ab-steps">
          <li class="ab-step" v-for="(s, i) in steps" :key="s.title">
            <span class="ab-step-num">{{ i + 1 }}</span>
            <h3 class="ab-step-title">{{ s.title }}</h3>
            <p class="ab-step-text">{{ s.text }}</p>
          </li>
        </ol>
      </div>
    </section>

    <!-- ===== Team ===== -->
    <section class="ab-section" aria-labelledby="ab-team-heading">
      <div class="ab-wrap">
        <div class="ab-head">
          <p class="ab-eyebrow">Our team</p>
          <h2 id="ab-team-heading" class="ab-h2">The people you will talk to</h2>
          <p class="ab-sub">A small team that plans your trip and stays reachable while you travel.</p>
        </div>

        <div class="ab-team-grid">
          <article class="ab-member" v-for="m in team" :key="m.name">
            <span class="ab-member-photo">
              <img v-if="m.photo" :src="m.photo" :alt="m.name" loading="lazy" />
              <span v-else aria-hidden="true">{{ initials(m.name) }}</span>
            </span>
            <h3 class="ab-member-name">{{ m.name }}</h3>
            <p class="ab-member-role">{{ m.role }}</p>
            <p class="ab-member-text">{{ m.text }}</p>
          </article>
        </div>
      </div>
    </section>

    <!-- ===== FAQ ===== -->
    <section class="ab-section ab-sand" aria-labelledby="ab-faq-heading">
      <div class="ab-wrap ab-faq-grid">
        <div class="ab-faq-intro">
          <p class="ab-eyebrow">Questions</p>
          <h2 id="ab-faq-heading" class="ab-h2">Before you get in touch</h2>
          <p class="ab-sub">
            Cannot find your answer? Call {{ phoneDisplay }} or send an inquiry and we will reply
            within 24 hours.
          </p>
        </div>

        <div class="ab-faq-list">
          <div
            class="ab-faq"
            v-for="(f, i) in faqs"
            :key="f.q"
            :class="{ 'is-open': openFaq === i }"
          >
            <h3 class="ab-faq-q">
              <button
                type="button"
                class="ab-faq-btn"
                :id="`ab-faq-btn-${i}`"
                :aria-expanded="openFaq === i ? 'true' : 'false'"
                :aria-controls="`ab-faq-panel-${i}`"
                @click="toggleFaq(i)"
              >
                <span>{{ f.q }}</span>
                <span class="ab-faq-icon" aria-hidden="true"></span>
              </button>
            </h3>
            <div
              class="ab-faq-panel"
              :id="`ab-faq-panel-${i}`"
              role="region"
              :aria-labelledby="`ab-faq-btn-${i}`"
            >
              <p class="ab-faq-a">{{ f.a }}</p>
            </div>
          </div>
        </div>
      </div>
    </section>

    <!-- ===== CTA ===== -->
    <section class="ab-cta" aria-labelledby="ab-cta-heading">
      <span class="ab-cta-bg" :style="{ backgroundImage: `url(${ctaBg})` }" aria-hidden="true"></span>
      <div class="ab-wrap ab-cta-inner">
        <h2 id="ab-cta-heading" class="ab-cta-title">Tell us when you are coming. We will plan the rest.</h2>
        <div class="ab-cta-actions">
          <a href="/plan-your-trip" class="ab-btn ab-btn-sun">Plan my trip</a>
          <a :href="`tel:${phoneRaw}`" class="ab-btn ab-btn-line">Call {{ phoneDisplay }}</a>
          <a
            :href="whatsappLink"
            class="ab-btn ab-btn-whatsapp"
            target="_blank"
            rel="noopener noreferrer"
          >WhatsApp us</a>
        </div>
      </div>
    </section>

    <!-- ===== Quick links ===== -->
    <section class="ab-more" aria-label="More pages">
      <div class="ab-wrap">
        <p class="ab-more-label">Looking for something else?</p>
        <div class="ab-more-links">
          <a href="/" class="ab-pill">Back to home</a>
          <a href="/destinations" class="ab-pill">Destinations</a>
          <a href="/gallery" class="ab-pill">Gallery</a>
          <a href="/contact" class="ab-pill">Contact us</a>
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
  name: "AboutUsPage",
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
      whatsappMessage: "Hello, I would like to plan a trip to Sri Lanka.",
      // Shown on the badge in "Our story". Example: "Since 2015"
      foundedLabel: "Local since day one",
      // -----------------------------------------------------------------------------------

      heroBg: imgSigiriya,
      ctaBg: imgBeach,
      storyImages: [imgSigiriya, imgGalle, imgMirissa],

      heroFacts: [
        { value: "Local guides", label: "Licensed and experienced" },
        { value: "Free planning", label: "No charge for a trip plan" },
        { value: "24 hours", label: "Usual reply to inquiries" },
      ],

      visionMission: [
        {
          icon: "◎",
          title: "Our Vision",
          text: "“To become a globally recognized tourism destination management organization, showcasing the beauty, culture, heritage, and hospitality of Sri Lanka while creating unforgettable travel experiences for visitors from around the world.”",
        },
        {
          icon: "➤",
          title: "Our Mission",
          text: "“To promote and develop sustainable tourism in Sri Lanka by delivering exceptional travel experiences, preserving our natural and cultural heritage, supporting local communities, and providing high-quality, reliable, and personalized tourism services. We are committed to connecting visitors with the authentic beauty, traditions, and hospitality of Sri Lanka while contributing to the growth of the national tourism industry.”",
        },
      ],

      // Placeholder numbers: replace with your real figures (same as the home page).
      stats: [
        { label: "Happy travellers", target: 5000, suffix: "+", value: 0, duration: 2000 },
        { label: "Trips completed", target: 1200, suffix: "+", value: 0, duration: 1600 },
        { label: "Years of experience", target: 10, suffix: "+", value: 0, duration: 1000 },
        { label: "UNESCO World Heritage Sites on our routes", target: 8, suffix: "", value: 0, duration: 900 },
      ],
      animationFrames: [],
      observer: null,

      promises: [
        {
          icon: "✦",
          title: "Honest advice",
          text: "We tell you when a place is crowded, when it is not worth the drive and when the weather will get in the way.",
        },
        {
          icon: "♥",
          title: "Local first",
          text: "Family-run stays, village lunches and licensed local guides, so your money stays in the communities you visit.",
        },
        {
          icon: "☘",
          title: "Travel with care",
          text: "Small groups, respectful wildlife viewing and temple etiquette explained before you arrive.",
        },
        {
          icon: "✔",
          title: "Support on the road",
          text: "A real person to call or message during your trip if plans change or something goes wrong.",
        },
      ],

      services: [
        {
          title: "Tour packages",
          text: "Ready-made island itineraries that we can shorten, stretch or mix with another.",
        },
        {
          title: "Day trips",
          text: "A single sunrise climb, a safari or a train ride, with a driver and guide for the day.",
        },
        {
          title: "Hotels and stays",
          text: "Hotels, villas and guesthouses chosen for location and character, across several budgets.",
        },
        {
          title: "Guides and transport",
          text: "Licensed guides and comfortable vehicles, so you can focus on the view.",
        },
      ],

      // Links match the Destinations menu in the nav bar.
      places: [
        { name: "Sigiriya", region: "Cultural Triangle", src: imgSigiriya, link: "/destinations/sigiriya" },
        { name: "Ella", region: "Hill country", src: imgNineArch, link: "/destinations/ella" },
        { name: "Kandy", region: "Central highlands", src: imgAmbuluwawa, link: "/destinations/kandy" },
        { name: "Galle", region: "South coast", src: imgGalle, link: "/destinations/galle" },
        { name: "Mirissa", region: "South coast", src: imgMirissa, link: "/destinations/mirissa" },
      ],

      steps: [
        {
          title: "Tell us what you love",
          text: "Share your dates, interests and budget through the inquiry form, by phone or on WhatsApp.",
        },
        {
          title: "We draft your route",
          text: "You get a day-by-day plan with stays, transport and guides, usually within 24 hours.",
        },
        {
          title: "Adjust until it fits",
          text: "Swap a stop, slow the pace or add a beach day. We revise until you are happy with it.",
        },
        {
          title: "Travel with a guide",
          text: "Your driver and guide meet you on arrival, and our team stays reachable for the whole trip.",
        },
      ],

      // Placeholder team: replace names, roles and text. Add `photo: require('@/assets/team/name.jpg')`
      // to a member to show a photo instead of initials.
      team: [
        {
          name: "Founder Name",
          role: "Founder and lead planner",
          text: "Designs the island routes and checks every new stay in person.",
        },
        {
          name: "Guide Name",
          role: "Senior guide",
          text: "Licensed national guide with deep knowledge of the Cultural Triangle.",
        },
        {
          name: "Coordinator Name",
          role: "Guest support",
          text: "Your contact before and during the trip, in English and Sinhala.",
        },
      ],

      openFaq: 0,
      faqs: [
        {
          q: "Do you offer private trips or small groups?",
          a: "Both. Most trips are private for your own group, and we can also arrange small-group days. Tell us your preference and we will plan around it.",
        },
        {
          q: "Can I change the route after you send it?",
          a: "Yes. Every itinerary can be shortened, stretched or mixed with another. We revise it with you until it fits your dates, pace and budget.",
        },
        {
          q: "What do you arrange for me?",
          a: "Stays, transport, licensed guides and activities such as safaris, train rides and cultural visits. You can book the whole journey or just the parts you need.",
        },
        {
          q: "How quickly will I hear back?",
          a: "We usually reply to inquiries within 24 hours. Our office is open Monday to Saturday, 8:30 am to 6:00 pm, and you can also reach us on WhatsApp.",
        },
        {
          q: "When is the best time to visit?",
          a: "Sri Lanka has two monsoons, so there is always a sunny coast. The south and west are best from December to March, the east from April to September, and the hill country from January to April.",
        },
      ],
    };
  },
  computed: {
    whatsappLink() {
      return `https://wa.me/${this.whatsappNumber}?text=${encodeURIComponent(this.whatsappMessage)}`;
    },
  },
  mounted() {
    // Count the stats up once, when the band scrolls into view
    this.observer = new IntersectionObserver(
      (entries) => {
        if (entries.some((e) => e.isIntersecting)) {
          this.runCounters();
          this.observer.disconnect();
        }
      },
      { root: null, rootMargin: "0px", threshold: 0.4 }
    );
    if (this.$refs.statsSection) {
      this.observer.observe(this.$refs.statsSection);
    }
  },
  beforeUnmount() {
    if (this.observer) this.observer.disconnect();
    this.animationFrames.forEach((id) => cancelAnimationFrame(id));
  },
  methods: {
    initials(name) {
      return name
        .split(" ")
        .filter(Boolean)
        .slice(0, 2)
        .map((w) => w[0].toUpperCase())
        .join("");
    },
    toggleFaq(i) {
      this.openFaq = this.openFaq === i ? -1 : i;
    },
    runCounters() {
      const reduce = window.matchMedia("(prefers-reduced-motion: reduce)").matches;
      this.stats.forEach((stat) => {
        if (reduce) {
          stat.value = stat.target;
          return;
        }
        const start = performance.now();
        const tick = (now) => {
          const t = Math.min((now - start) / stat.duration, 1);
          const eased = 1 - Math.pow(1 - t, 3); // ease-out
          stat.value = Math.round(stat.target * eased);
          if (t < 1) {
            this.animationFrames.push(requestAnimationFrame(tick));
          }
        };
        this.animationFrames.push(requestAnimationFrame(tick));
      });
    },
  },
};
</script>

<style scoped>
/*
  All class names on this page start with "ab-" so they can never clash with the
  nav bar's global "lk-" styles.
*/

/* ===== TOKENS (same blue theme as the home page and footer) ===== */
.ab-page {
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

.ab-wrap {
  position: relative;
  width: 100%;
  max-width: 1200px;
  margin: 0 auto;
  padding: 0 max(24px, 4vw);
  box-sizing: border-box;
}

.ab-section {
  position: relative;
  padding: 96px 0;
  background: #fff;
}
.ab-sand {
  background: var(--sand);
}

/* ===== TYPE ===== */
.ab-h2,
.ab-hero-title,
.ab-cta-title {
  font-family: var(--font-display);
  font-weight: 400;
  text-wrap: balance;
}
.ab-h2 {
  margin: 0 0 10px;
  max-width: 24ch;
  font-size: clamp(1.7rem, 3vw, 2.4rem);
  line-height: 1.15;
  text-align: left;
  text-transform: none;
  color: var(--navy);
}
.ab-eyebrow {
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
.ab-eyebrow::before {
  content: "";
  width: 28px;
  height: 2px;
  background: var(--sun);
  border-radius: 2px;
}
.ab-rule {
  display: block;
  width: 72px;
  height: 4px;
  margin: 22px 0;
  border-radius: 4px;
  background: linear-gradient(90deg, var(--sun), var(--blue-bright));
}
.ab-lead {
  margin: 0 0 16px;
  max-width: 60ch;
  font-size: 1.1rem;
  line-height: 1.7;
  color: var(--muted);
}
.ab-sub {
  margin: 0;
  max-width: 60ch;
  font-size: 1.05rem;
  line-height: 1.6;
  color: var(--muted);
}
.ab-head {
  margin-bottom: 44px;
}

/* ===== HERO ===== */
.ab-hero {
  position: relative;
  overflow: hidden;
  min-height: clamp(520px, 78vh, 660px);
  display: flex;
  flex-direction: column;
  justify-content: center;
  padding: 72px 0 150px;
  color: #fff;
  background: var(--navy);
}
.ab-hero-photo {
  position: absolute;
  inset: 0;
  background-size: cover;
  background-position: center 30%;
  /* the one deliberate motion on this page: a slow settle on load */
  animation: ab-drift 14s ease-out forwards;
}
@keyframes ab-drift {
  from { transform: scale(1.08); }
  to { transform: scale(1); }
}
.ab-hero-shade {
  position: absolute;
  inset: 0;
  background:
    linear-gradient(to top, rgba(11, 42, 74, 0.9) 0%, rgba(11, 42, 74, 0) 40%),
    linear-gradient(to right, rgba(11, 42, 74, 0.85) 0%, rgba(11, 42, 74, 0.45) 55%, rgba(21, 101, 192, 0.2) 100%);
}
.ab-hero-inner {
  position: relative;
  z-index: 2;
}
.ab-crumbs {
  display: flex;
  align-items: center;
  gap: 10px;
  margin-bottom: 26px;
  font-size: 0.9rem;
  font-weight: 600;
  color: rgba(255, 255, 255, 0.8);
}
.ab-crumbs a {
  color: var(--sky);
  text-decoration: none;
}
.ab-crumbs a:hover {
  text-decoration: underline;
}
.ab-hero-title {
  margin: 0 0 18px;
  max-width: 18ch;
  font-size: clamp(2.2rem, 5.2vw, 4rem);
  line-height: 1.08;
  letter-spacing: -0.01em;
  color: #fff;
  text-align: left;
  text-transform: none;
}
.ab-hero-lead {
  margin: 0 0 30px;
  max-width: 54ch;
  font-size: 1.15rem;
  line-height: 1.7;
  color: rgba(255, 255, 255, 0.92);
}
.ab-hero-actions {
  display: flex;
  flex-wrap: wrap;
  gap: 12px;
}
.ab-hero-facts {
  position: absolute;
  left: 0;
  right: 0;
  bottom: 62px;
  z-index: 2;
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 24px;
  max-width: 1200px;
  margin: 0 auto;
  padding: 0 max(24px, 4vw);
  list-style: none;
  box-sizing: border-box;
}
.ab-hero-facts li {
  display: flex;
  flex-direction: column;
  gap: 2px;
  padding-left: 14px;
  border-left: 3px solid var(--sun);
}
.ab-hero-facts strong {
  font-family: var(--font-display);
  font-weight: 400;
  font-size: 1.2rem;
  color: #fff;
}
.ab-hero-facts span {
  font-size: 0.85rem;
  color: rgba(255, 255, 255, 0.82);
}
.ab-hero-wave {
  position: absolute;
  left: 0;
  bottom: -1px;
  z-index: 3;
  width: 100%;
  height: 60px;
  display: block;
  pointer-events: none;
}

/* ===== OUR STORY ===== */
.ab-blob {
  position: absolute;
  top: -60px;
  right: -60px;
  width: 320px;
  height: 320px;
  border-radius: 50%;
  background: rgba(142, 201, 255, 0.4);
  filter: blur(60px);
  pointer-events: none;
}
.ab-story-grid {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 64px;
  align-items: center;
}
.ab-story-text .ab-lead:last-child {
  margin-bottom: 0;
}
.ab-collage {
  position: relative;
  display: grid;
  grid-template-columns: 1.15fr 1fr;
  grid-template-rows: 1fr 1fr;
  gap: 16px;
  height: 480px;
}
.ab-collage figure {
  margin: 0;
  overflow: hidden;
  border-radius: 18px;
  box-shadow: var(--shadow-md);
}
.ab-collage img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  display: block;
}
.ab-collage-a {
  grid-row: span 2;
}
.ab-collage-badge {
  position: absolute;
  left: -18px;
  bottom: 28px;
  display: flex;
  flex-direction: column;
  gap: 2px;
  max-width: 190px;
  margin: 0;
  padding: 14px 18px;
  background: var(--sun);
  color: var(--navy);
  border-radius: 14px;
  box-shadow: var(--shadow-md);
}
.ab-collage-badge strong {
  font-family: var(--font-display);
  font-weight: 400;
  font-size: 1.25rem;
  line-height: 1.15;
}
.ab-collage-badge span {
  font-size: 0.85rem;
  font-weight: 600;
  line-height: 1.35;
}

/* ===== STATS ===== */
.ab-stats {
  position: relative;
  padding: 52px 0;
  color: #fff;
  background: linear-gradient(135deg, var(--navy) 0%, #12457a 100%);
}
.ab-stats::before {
  content: "";
  position: absolute;
  inset: 0;
  background-image: radial-gradient(rgba(255, 255, 255, 0.07) 1.5px, transparent 1.5px);
  background-size: 22px 22px;
  pointer-events: none;
}
.ab-stats-grid {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  gap: 24px;
}
.ab-stat {
  padding-left: 18px;
  border-left: 3px solid var(--sun);
}
.ab-stat-value {
  margin: 0;
  font-family: var(--font-display);
  font-size: clamp(2rem, 4vw, 3rem);
  line-height: 1;
  color: var(--sun);
}
.ab-stat-label {
  margin: 8px 0 0;
  max-width: 22ch;
  font-size: 0.95rem;
  line-height: 1.4;
  color: rgba(255, 255, 255, 0.85);
}

/* ===== ICON TILE (shared) ===== */
.ab-icon-tile {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  width: 52px;
  height: 52px;
  margin-bottom: 18px;
  font-size: 1.5rem;
  color: #fff;
  background: linear-gradient(135deg, var(--blue) 0%, var(--blue-bright) 100%);
  border-radius: 16px;
  box-shadow: 0 6px 14px rgba(21, 101, 192, 0.35);
}
.ab-icon-tile-sm {
  width: 44px;
  height: 44px;
  margin-bottom: 16px;
  font-size: 1.2rem;
  border-radius: 14px;
}

/* ===== VISION & MISSION ===== */
.ab-vm-grid {
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  gap: 28px;
}
.ab-vm {
  padding: 40px 36px 36px;
  background: #fff;
  border: 1px solid #e3dccb;
  border-top: 5px solid var(--sun);
  border-radius: 20px;
  box-shadow: var(--shadow-sm);
}
.ab-vm-title {
  margin: 0 0 14px;
  font-family: var(--font-display);
  font-weight: 400;
  font-size: 1.8rem;
  line-height: 1.15;
  color: var(--navy);
}
.ab-vm-text {
  margin: 0;
  font-size: 1.05rem;
  line-height: 1.75;
  font-style: italic;
  color: var(--muted);
}

/* ===== VALUES ===== */
.ab-values-grid {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  gap: 24px;
}
.ab-value {
  padding: 28px 24px;
  background: #f2f8ff;
  border-left: 5px solid var(--blue-bright);
  border-radius: 0 16px 16px 0;
  box-shadow: var(--shadow-sm);
}
.ab-value-title {
  margin: 0 0 8px;
  font-size: 1.1rem;
  font-weight: 600;
  color: var(--navy);
}
.ab-value-text {
  margin: 0;
  line-height: 1.6;
  color: var(--muted);
}

/* ===== WHAT WE ARRANGE ===== */
.ab-arrange-grid {
  display: grid;
  grid-template-columns: 0.9fr 1.1fr;
  gap: 64px;
  align-items: start;
}
.ab-arrange-intro .ab-sub {
  margin-bottom: 28px;
}
.ab-services {
  display: grid;
  gap: 14px;
  margin: 0;
  padding: 0;
  list-style: none;
}
.ab-service {
  display: flex;
  gap: 16px;
  padding: 20px 22px;
  background: #fff;
  border: 1px solid #e3dccb;
  border-radius: 16px;
  box-shadow: var(--shadow-sm);
}
.ab-service-mark {
  flex: 0 0 auto;
  width: 10px;
  height: 10px;
  margin-top: 8px;
  background: var(--sun);
  border-radius: 50%;
  box-shadow: 0 0 0 4px rgba(245, 185, 74, 0.25);
}
.ab-service-title {
  margin: 0 0 4px;
  font-size: 1.05rem;
  font-weight: 600;
  color: var(--navy);
}
.ab-service-text {
  margin: 0;
  line-height: 1.55;
  color: var(--muted);
}

/* ===== WHERE WE WORK ===== */
.ab-places {
  display: grid;
  grid-template-columns: repeat(5, 1fr);
  gap: 16px;
}
.ab-place {
  position: relative;
  display: block;
  height: 280px;
  overflow: hidden;
  border-radius: 18px;
  color: #fff;
  text-decoration: none;
  background: var(--navy);
  box-shadow: var(--shadow-sm);
}
.ab-place-photo {
  position: absolute;
  inset: 0;
  background-size: cover;
  background-position: center;
  transition: transform 0.6s ease;
}
.ab-place:hover .ab-place-photo,
.ab-place:focus-visible .ab-place-photo {
  transform: scale(1.07);
}
.ab-place-shade {
  position: absolute;
  inset: 0;
  background: linear-gradient(to top, rgba(11, 42, 74, 0.92) 0%, rgba(11, 42, 74, 0.1) 60%, rgba(11, 42, 74, 0) 100%);
}
.ab-place-text {
  position: absolute;
  left: 18px;
  right: 18px;
  bottom: 16px;
  display: flex;
  flex-direction: column;
  gap: 2px;
}
.ab-place-region {
  font-size: 0.75rem;
  font-weight: 600;
  color: var(--sun);
}
.ab-place-name {
  font-family: var(--font-display);
  font-size: 1.35rem;
  line-height: 1.15;
}
.ab-places-more {
  margin: 24px 0 0;
}
.ab-places-more a {
  font-weight: 700;
  color: var(--blue);
  text-decoration: none;
  border-bottom: 2px solid var(--sun);
}
.ab-places-more a:hover {
  color: var(--navy);
}

/* ===== STEPS ===== */
.ab-steps {
  position: relative;
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  gap: 24px;
  margin: 0;
  padding: 0;
  list-style: none;
}
/* dotted line joining the four numbers */
.ab-steps::before {
  content: "";
  position: absolute;
  top: 22px;
  left: 22px;
  right: 22px;
  height: 2px;
  background: repeating-linear-gradient(90deg, var(--sun) 0 10px, transparent 10px 18px);
}
.ab-step {
  position: relative;
}
.ab-step-num {
  position: relative;
  display: inline-flex;
  align-items: center;
  justify-content: center;
  width: 44px;
  height: 44px;
  margin: 0 0 18px -4px;
  font-family: var(--font-display);
  font-size: 1.2rem;
  color: var(--navy);
  background: var(--sun);
  border: 4px solid var(--sand);
  border-radius: 50%;
  box-sizing: content-box;
}
.ab-step-title {
  margin: 0 0 8px;
  font-size: 1.1rem;
  font-weight: 600;
  color: var(--navy);
}
.ab-step-text {
  margin: 0;
  max-width: 28ch;
  line-height: 1.6;
  color: var(--muted);
}

/* ===== TEAM ===== */
.ab-team-grid {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 28px;
}
.ab-member {
  padding: 32px 28px;
  text-align: center;
  background: #fff;
  border: 1px solid #e3dccb;
  border-radius: 20px;
  box-shadow: var(--shadow-sm);
}
.ab-member-photo {
  display: flex;
  align-items: center;
  justify-content: center;
  width: 112px;
  height: 112px;
  margin: 0 auto 18px;
  overflow: hidden;
  font-family: var(--font-display);
  font-size: 2rem;
  color: #fff;
  background: linear-gradient(135deg, var(--navy) 0%, var(--blue) 100%);
  border: 4px solid var(--sun);
  border-radius: 50%;
}
.ab-member-photo img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  display: block;
}
.ab-member-name {
  margin: 0 0 4px;
  font-family: var(--font-display);
  font-weight: 400;
  font-size: 1.3rem;
  color: var(--navy);
}
.ab-member-role {
  margin: 0 0 12px;
  font-size: 0.95rem;
  font-weight: 600;
  color: var(--blue);
}
.ab-member-text {
  margin: 0;
  line-height: 1.6;
  color: var(--muted);
}

/* ===== FAQ ===== */
.ab-faq-grid {
  display: grid;
  grid-template-columns: 0.8fr 1.2fr;
  gap: 64px;
  align-items: start;
}
.ab-faq-list {
  display: grid;
  gap: 12px;
}
.ab-faq {
  background: #fff;
  border: 1px solid #e3dccb;
  border-radius: 16px;
  box-shadow: var(--shadow-sm);
  overflow: hidden;
}
.ab-faq.is-open {
  border-color: var(--sun);
}
.ab-faq-q {
  margin: 0;
}
.ab-faq-btn {
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: 16px;
  width: 100%;
  padding: 18px 22px;
  font-family: var(--font-body);
  font-size: 1.05rem;
  font-weight: 600;
  text-align: left;
  color: var(--navy);
  background: none;
  border: 0;
  cursor: pointer;
}
.ab-faq-btn:focus-visible {
  outline: 3px solid var(--sky);
  outline-offset: -3px;
}
/* plus / minus built from two bars */
.ab-faq-icon {
  position: relative;
  flex: 0 0 auto;
  width: 22px;
  height: 22px;
}
.ab-faq-icon::before,
.ab-faq-icon::after {
  content: "";
  position: absolute;
  top: 50%;
  left: 50%;
  width: 14px;
  height: 2px;
  background: var(--blue);
  border-radius: 2px;
  transform: translate(-50%, -50%);
  transition: transform 0.25s ease;
}
.ab-faq-icon::after {
  transform: translate(-50%, -50%) rotate(90deg);
}
.ab-faq.is-open .ab-faq-icon::after {
  transform: translate(-50%, -50%) rotate(0deg);
}
/* panel opens by animating grid row height */
.ab-faq-panel {
  display: grid;
  grid-template-rows: 0fr;
  transition: grid-template-rows 0.3s ease;
}
.ab-faq.is-open .ab-faq-panel {
  grid-template-rows: 1fr;
}
.ab-faq-a {
  min-height: 0;
  margin: 0;
  overflow: hidden;
  padding: 0 22px;
  line-height: 1.7;
  color: var(--muted);
  transition: padding 0.3s ease;
}
.ab-faq.is-open .ab-faq-a {
  padding: 0 22px 20px;
}

/* ===== CTA ===== */
.ab-cta {
  position: relative;
  overflow: hidden;
  padding: 96px 0;
  background: var(--navy);
}
.ab-cta-bg {
  position: absolute;
  inset: 0;
  background-size: cover;
  background-position: center;
  opacity: 0.35;
}
.ab-cta::after {
  content: "";
  position: absolute;
  inset: 0;
  background: linear-gradient(135deg, rgba(11, 42, 74, 0.92) 0%, rgba(21, 101, 192, 0.78) 100%);
}
.ab-cta-inner {
  position: relative;
  z-index: 1;
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: 32px;
}
.ab-cta-title {
  margin: 0;
  max-width: 24ch;
  font-size: clamp(1.7rem, 3.2vw, 2.6rem);
  line-height: 1.15;
  color: #fff;
}
.ab-cta-actions {
  display: flex;
  flex-wrap: wrap;
  gap: 12px;
}

/* ===== QUICK LINKS ===== */
.ab-more {
  padding: 48px 0 8px;
  text-align: center;
  background: #fff;
}
.ab-more-label {
  margin: 0 0 14px;
  font-size: 0.95rem;
  color: var(--muted);
}
.ab-more-links {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  gap: 10px;
}
.ab-pill {
  padding: 8px 18px;
  font-size: 0.9rem;
  font-weight: 600;
  color: var(--blue);
  text-decoration: none;
  border: 1.5px solid #cfe0f3;
  border-radius: 999px;
  transition: background-color 0.25s ease, color 0.25s ease, border-color 0.25s ease;
}
.ab-pill:hover {
  color: #fff;
  background: var(--blue);
  border-color: var(--blue);
}

/* ===== BUTTONS ===== */
.ab-btn {
  display: inline-block;
  padding: 13px 28px;
  font-size: 0.95rem;
  font-weight: 600;
  text-decoration: none;
  border-radius: 999px;
  transition: background-color 0.25s ease, color 0.25s ease, border-color 0.25s ease,
    transform 0.25s ease;
}
.ab-btn:hover {
  transform: translateY(-2px);
}
.ab-btn-sun {
  color: var(--navy);
  background: var(--sun);
  box-shadow: 0 8px 20px rgba(245, 185, 74, 0.4);
}
.ab-btn-sun:hover {
  background: #fff;
}
.ab-btn-solid {
  color: #fff;
  background: var(--blue);
  box-shadow: 0 6px 16px rgba(21, 101, 192, 0.3);
}
.ab-btn-solid:hover {
  background: var(--navy);
}
.ab-btn-line {
  color: #fff;
  border: 1.5px solid rgba(255, 255, 255, 0.6);
}
.ab-btn-line:hover {
  border-color: #fff;
  background: rgba(255, 255, 255, 0.12);
}
.ab-btn-whatsapp {
  color: #fff;
  background: #128c7e;
  box-shadow: 0 6px 16px rgba(18, 140, 126, 0.35);
}
.ab-btn-whatsapp:hover {
  background: #0e7266;
}
.ab-btn:focus-visible,
.ab-pill:focus-visible,
.ab-place:focus-visible,
.ab-crumbs a:focus-visible,
.ab-places-more a:focus-visible {
  outline: 3px solid var(--sky);
  outline-offset: 3px;
}

/* ===== RESPONSIVE ===== */
@media (max-width: 1100px) {
  .ab-places {
    grid-template-columns: repeat(3, 1fr);
  }
  .ab-place {
    height: 240px;
  }
}

@media (max-width: 1024px) {
  .ab-story-grid,
  .ab-arrange-grid,
  .ab-faq-grid {
    grid-template-columns: 1fr;
    gap: 40px;
  }
  .ab-collage-badge {
    left: 12px;
  }
  .ab-stats-grid,
  .ab-values-grid {
    grid-template-columns: repeat(2, 1fr);
  }
  .ab-steps {
    grid-template-columns: repeat(2, 1fr);
    row-gap: 36px;
  }
  .ab-steps::before {
    display: none;
  }
  .ab-team-grid {
    grid-template-columns: repeat(2, 1fr);
  }
}

@media (max-width: 768px) {
  .ab-section {
    padding: 60px 0;
  }
  .ab-hero {
    padding: 48px 0 190px;
  }
  .ab-hero-facts {
    grid-template-columns: 1fr;
    gap: 12px;
    bottom: 56px;
  }
  .ab-hero-facts li:nth-child(n + 3) {
    display: none;
  }
  .ab-collage {
    height: 380px;
  }
  .ab-vm-grid {
    grid-template-columns: 1fr;
  }
  .ab-vm {
    padding: 32px 24px 28px;
  }
  .ab-places {
    grid-template-columns: repeat(2, 1fr);
  }
  .ab-cta {
    padding: 60px 0;
  }
  .ab-cta-inner {
    flex-direction: column;
    align-items: flex-start;
  }
}

@media (max-width: 560px) {
  .ab-values-grid,
  .ab-steps,
  .ab-team-grid {
    grid-template-columns: 1fr;
  }
  .ab-collage {
    height: 320px;
    gap: 10px;
  }
  .ab-collage-badge {
    max-width: 160px;
    padding: 10px 14px;
  }
  .ab-place {
    height: 200px;
  }
  .ab-hero-facts li:nth-child(n + 2) {
    display: none;
  }
  .ab-hero {
    padding-bottom: 150px;
  }
}

@media (prefers-reduced-motion: reduce) {
  .ab-hero-photo {
    animation: none;
  }
  .ab-place-photo,
  .ab-btn,
  .ab-pill,
  .ab-faq-panel,
  .ab-faq-a,
  .ab-faq-icon::before,
  .ab-faq-icon::after {
    transition: none;
  }
  .ab-btn:hover,
  .ab-place:hover .ab-place-photo {
    transform: none;
  }
}
</style>