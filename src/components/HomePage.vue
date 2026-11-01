<template>
  <LankaNavBar />

  <CaroselPage></CaroselPage>

  <main class="lk-home">
    <!-- Intro -->
    <section class="lk-section lk-intro" aria-labelledby="intro-heading">
      <span class="lk-blob lk-blob-a" aria-hidden="true"></span>
      <span class="lk-blob lk-blob-b" aria-hidden="true"></span>
      <div class="lk-wrap lk-intro-grid">
        <div>
          <p class="lk-eyebrow">Discover Sri Lank</p>
          <h1 id="intro-heading" class="lk-h1" data-aos="fade-up" data-aos-duration="1500">
            One island, a thousand ways to see it
          </h1>
          <span class="lk-rule" aria-hidden="true"></span>
        </div>
        <div>
          <p class="lk-lead">
            Sri Lanka packs ancient kingdoms, misty tea country, wild national
            parks and warm coastlines into an island you can cross in a day.
            {{ brandName }} plans the whole journey for you: local guides,
            comfortable stays and routes that avoid the crowds.
          </p>
          <p class="lk-lead">
            Whether you have four days or four weeks, tell us what you love and
            we will build a trip around it.
          </p>
        </div>
      </div>
    </section>

    <!-- Introduction about Sri Lanka -->
    <section id="about-sri-lanka" class="lk-section lk-sand" aria-labelledby="about-heading">
      <div class="lk-wrap">
        <div class="lk-head">
          <p class="lk-eyebrow">About Sri Lanka</p>
          <h2 id="about-heading" class="lk-h2">A small island with a big story</h2>
        </div>

        <div class="lk-about-grid">
          <div class="lk-about-text">
            <p class="lk-lead">
              Sri Lanka is a tropical island in the Indian Ocean, just south of
              India. Known for centuries as the "Pearl of the Indian Ocean", it
              is home to more than 2,500 years of recorded history, eight UNESCO
              World Heritage Sites and a rich mix of Buddhist, Hindu, Muslim and
              Christian traditions.
            </p>
            <p class="lk-lead">
              Within a few hours' drive you can move from golden beaches to
              rainforest, from ancient rock fortresses to cool tea-covered
              mountains. The island is also famous for its wildlife, including
              elephants, leopards and blue whales, as well as its spice-rich
              cuisine and famously warm hospitality.
            </p>
          </div>

          <div class="lk-fact-grid">
            <div class="lk-fact" v-for="f in aboutFacts" :key="f.label">
              <p class="lk-fact-label">{{ f.label }}</p>
              <p class="lk-fact-value">{{ f.value }}</p>
            </div>
          </div>
        </div>
      </div>
    </section>

    <!-- Stats band -->
    <section class="lk-stats" ref="statsSection" aria-label="Our experience in numbers">
      <div class="lk-wrap lk-stats-grid">
        <div class="lk-stat" v-for="stat in stats" :key="stat.label">
          <p class="lk-stat-value">{{ stat.value.toLocaleString() }}{{ stat.suffix }}</p>
          <p class="lk-stat-label">{{ stat.label }}</p>
        </div>
      </div>
    </section>

    <!-- Destinations -->
    <section id="destinations" class="lk-section" aria-labelledby="dest-heading">
      <div class="lk-wrap">
        <div class="lk-head">
          <p class="lk-eyebrow">Destinations</p>
          <h2 id="dest-heading" class="lk-h2">Places people come back for</h2>
          <p class="lk-sub">Start with these seven, then ask us about the quiet corners in between.</p>
        </div>

        <div class="lk-dest-grid">
          <a
            v-for="(d, i) in destinations"
            :key="d.name"
            :href="d.link"
            class="lk-dest"
            :class="d.size"
          >
            <span class="lk-dest-photo" :style="{ backgroundImage: `url(${d.src})` }"></span>
            <span class="lk-dest-shade"></span>
            <span class="lk-dest-num">{{ String(i + 1).padStart(2, "0") }}</span>
            <span class="lk-dest-text">
              <span class="lk-dest-region">{{ d.region }}</span>
              <span class="lk-dest-name">{{ d.name }}</span>
              <span class="lk-dest-tag">{{ d.tagline }}</span>
              <span class="lk-dest-more">Explore <i aria-hidden="true">&rarr;</i></span>
            </span>
          </a>
        </div>
      </div>
    </section>

    <!-- Our Destinations with Sri Lanka map -->
    <section id="our-destinations" class="lk-section lk-sand" aria-labelledby="map-heading">
      <div class="lk-wrap">
        <div class="lk-head">
          <p class="lk-eyebrow">Our Destinations</p>
          <h2 id="map-heading" class="lk-h2">Nine stops across the island</h2>
          <p class="lk-sub">
            From the capital to the ancient cities, the hills and the coast. Hover or tap a
            place on the map to see what waits there.
          </p>
        </div>

        <div class="lk-map-layout">
          <!-- Map -->
          <div class="lk-map-stage">
            <div class="lk-map-card">
              <svg
                class="lk-map-svg"
                viewBox="0 0 400 590"
                role="img"
                aria-label="Map of Sri Lanka showing our nine destinations"
              >
                <defs>
                  <path id="lk-island-shape" :d="islandPath" />
                  <linearGradient id="lk-top-grad" x1="0" y1="0" x2="1" y2="1">
                    <stop offset="0%" stop-color="#ffd824" />
                    <stop offset="100%" stop-color="#e2b300" />
                  </linearGradient>
                  <radialGradient id="lk-cloud-grad" cx="45%" cy="35%" r="75%">
                    <stop offset="0%" stop-color="#ffffff" />
                    <stop offset="100%" stop-color="#d9e0e8" />
                  </radialGradient>
                  <filter id="lk-blur" x="-20%" y="-20%" width="140%" height="140%">
                    <feGaussianBlur stdDeviation="8" />
                  </filter>

                  <!-- Cloud -->
                  <g id="lk-cloud-shape">
                    <circle cx="30" cy="46" r="22" />
                    <circle cx="56" cy="32" r="28" />
                    <circle cx="86" cy="40" r="24" />
                    <circle cx="102" cy="52" r="17" />
                    <circle cx="60" cy="52" r="24" />
                  </g>

                  <!-- Palm tree -->
                  <g id="lk-palm">
                    <path d="M0,0 Q4,-9 1,-19" class="lk-palm-trunk" />
                    <g transform="translate(1 -19)" class="lk-palm-leaves">
                      <ellipse cx="-7" cy="-2" rx="8" ry="2.6" transform="rotate(-20 -7 -2)" />
                      <ellipse cx="7" cy="-2" rx="8" ry="2.6" transform="rotate(20 7 -2)" />
                      <ellipse cx="-5" cy="3" rx="7" ry="2.4" transform="rotate(35 -5 3)" />
                      <ellipse cx="5" cy="3" rx="7" ry="2.4" transform="rotate(-35 5 3)" />
                      <ellipse cx="0" cy="-6" rx="2.6" ry="7" />
                    </g>
                  </g>
                </defs>

                <!-- Soft ground shadow -->
                <use
                  href="#lk-island-shape"
                  transform="translate(10 30)"
                  class="lk-island-shadow"
                  filter="url(#lk-blur)"
                />

                <!-- Extruded sides (3D depth) -->
                <use
                  v-for="n in 10"
                  :key="'side' + n"
                  href="#lk-island-shape"
                  :transform="`translate(0 ${n * 1.8})`"
                  class="lk-island-side"
                />

                <!-- Top surface -->
                <use href="#lk-island-shape" class="lk-island-top" />

                <!-- Palm trees -->
                <g
                  v-for="(pl, i) in mapPalms"
                  :key="'palm' + i"
                  :transform="`translate(${project(pl[0], pl[1]).join(' ')})`"
                  aria-hidden="true"
                >
                  <use href="#lk-palm" />
                </g>

                <!-- Sri Lanka flag -->
                <g transform="translate(246 328)" class="lk-flag" aria-hidden="true">
                  <ellipse cx="0" cy="1" rx="7" ry="2.6" class="lk-flag-base" />
                  <line x1="0" y1="0" x2="0" y2="-34" class="lk-flag-pole" />
                  <rect x="0" y="-34" width="34" height="21" rx="1.5" class="lk-flag-bg" />
                  <rect x="2" y="-32" width="5" height="17" class="lk-flag-green" />
                  <rect x="7" y="-32" width="5" height="17" class="lk-flag-orange" />
                  <rect x="15" y="-31" width="17" height="15" rx="1" class="lk-flag-lion" />
                </g>

                <!-- Clouds -->
                <g transform="translate(0 4) scale(0.8)" aria-hidden="true">
                  <use href="#lk-cloud-shape" class="lk-cloud lk-cloud-a" />
                </g>
                <g transform="translate(268 18) scale(0.95)" aria-hidden="true">
                  <use href="#lk-cloud-shape" class="lk-cloud lk-cloud-b" />
                </g>
                <g transform="translate(318 330) scale(0.62)" aria-hidden="true">
                  <use href="#lk-cloud-shape" class="lk-cloud lk-cloud-c" />
                </g>
                <g transform="translate(-4 505) scale(0.7)" aria-hidden="true">
                  <use href="#lk-cloud-shape" class="lk-cloud lk-cloud-d" />
                </g>

                <!-- Compass -->
                <g class="lk-compass" transform="translate(354 542)" aria-hidden="true">
                  <circle r="20" class="lk-compass-ring" />
                  <path d="M0,-15 L4.5,0 L0,15 L-4.5,0 Z" class="lk-compass-needle" />
                  <text y="-26" text-anchor="middle" class="lk-compass-n">N</text>
                </g>

                <!-- Pins -->
                <g
                  v-for="(p, i) in mapPoints"
                  :key="p.name"
                  :transform="`translate(${p.x} ${p.y})`"
                  class="lk-pin"
                  :class="{ 'is-active': activeIndex === i }"
                  role="button"
                  tabindex="0"
                  :aria-label="`${i + 1}. ${p.name}`"
                  @mouseenter="activeIndex = i"
                  @focus="activeIndex = i"
                  @click="activeIndex = i"
                  @keydown.enter="activeIndex = i"
                  @keydown.space.prevent="activeIndex = i"
                >
                  <circle v-if="activeIndex === i" r="11" class="lk-pin-pulse" />
                  <g class="lk-pin-body">
                    <circle r="11" class="lk-pin-dot" />
                    <text y="4" text-anchor="middle" class="lk-pin-num">{{ i + 1 }}</text>
                  </g>
                  <text
                    :x="p.side === 'l' ? -17 : 17"
                    y="4"
                    :text-anchor="p.side === 'l' ? 'end' : 'start'"
                    class="lk-pin-label"
                  >{{ p.mapLabel }}</text>
                </g>
              </svg>
            </div>
          </div>

          <!-- Details + list -->
          <div class="lk-map-side">
            <div class="lk-dm-panel" aria-live="polite">
              <p class="lk-dm-panel-region">
                <span class="lk-dm-panel-num">{{ String(activeIndex + 1).padStart(2, "0") }}</span>
                {{ activeDest.region }}
              </p>
              <h3 class="lk-dm-panel-name">{{ activeDest.name }}</h3>
              <p class="lk-dm-panel-text">{{ activeDest.text }}</p>
              <ul class="lk-dm-tags">
                <li v-for="t in activeDest.tags" :key="t">{{ t }}</li>
              </ul>
            </div>

            <div class="lk-dm-list">
              <button
                type="button"
                class="lk-dm-item"
                v-for="(p, i) in mapPoints"
                :key="'item' + p.name"
                :class="{ 'is-active': activeIndex === i }"
                @mouseenter="activeIndex = i"
                @focus="activeIndex = i"
                @click="activeIndex = i"
              >
                <span class="lk-dm-item-num">{{ i + 1 }}</span>
                <span class="lk-dm-item-name">{{ p.name }}</span>
              </button>
            </div>
          </div>
        </div>
      </div>
    </section>

    <!-- Gallery strip -->
    <section class="lk-gallery" aria-label="Photo gallery">
      <div class="lk-wrap">
        <div class="lk-head lk-head-light">
          <p class="lk-eyebrow lk-eyebrow-light">Gallery</p>
          <h2 class="lk-h2 lk-h2-light">Moments from the island</h2>
        </div>
      </div>
      <!-- Row 1 moves left -->
      <div class="lk-marquee">
        <div class="lk-track lk-track-left">
          <figure
            class="lk-shot"
            v-for="(g, i) in marqueeLeft"
            :key="'l' + i"
            :aria-hidden="i >= gallery.length"
          >
            <img :src="g.src" :alt="g.caption" loading="lazy" />
            <figcaption>{{ g.caption }}</figcaption>
          </figure>
        </div>
      </div>
      <!-- Row 2 moves right -->
      <div class="lk-marquee">
        <div class="lk-track lk-track-right">
          <figure
            class="lk-shot"
            v-for="(g, i) in marqueeRight"
            :key="'r' + i"
            :aria-hidden="i >= gallery.length"
          >
            <img :src="g.src" :alt="g.caption" loading="lazy" />
            <figcaption>{{ g.caption }}</figcaption>
          </figure>
        </div>
      </div>
    </section>

    <!-- Tours -->
    <section id="tours" class="lk-section lk-sand" aria-labelledby="tours-heading">
      <div class="lk-wrap">
        <div class="lk-head">
          <p class="lk-eyebrow">Itineraries</p>
          <h2 id="tours-heading" class="lk-h2">Popular trips to start from</h2>
          <p class="lk-sub">Every trip can be shortened, stretched or mixed with another.</p>
        </div>

        <div class="lk-tour-grid">
          <article
            class="lk-tour"
            v-for="t in tours"
            :key="t.title"
          >
            <div class="lk-tour-photo-wrap">
              <div class="lk-tour-photo" :style="{ backgroundImage: `url(${t.src})` }"></div>
              <span class="lk-tour-badge">{{ t.days }}</span>
            </div>
            <div class="lk-tour-body">
              <h3 class="lk-tour-title">{{ t.title }}</h3>
              <p class="lk-tour-route">{{ t.route }}</p>
              <p class="lk-tour-text">{{ t.text }}</p>
              <a :href="t.link" class="lk-btn lk-btn-solid">Ask about this trip</a>
            </div>
          </article>
        </div>
      </div>
    </section>

    <!-- Best time -->
    <section id="best-time" class="lk-section" aria-labelledby="time-heading">
      <div class="lk-wrap">
        <div class="lk-head">
          <p class="lk-eyebrow">Seasons</p>
          <h2 id="time-heading" class="lk-h2">When to go</h2>
          <p class="lk-sub">
            Sri Lanka has two monsoons, so there is always a sunny coast. Pick your region first, then your dates.
          </p>
        </div>

        <div class="lk-time-grid">
          <div
            class="lk-time"
            v-for="s in seasons"
            :key="s.where"
          >
            <p class="lk-time-months">{{ s.months }}</p>
            <h3 class="lk-time-where">{{ s.where }}</h3>
            <p class="lk-time-note">{{ s.note }}</p>
          </div>
        </div>
        <p class="lk-fine">Weather patterns vary year to year. We will confirm the best timing for your plans.</p>
      </div>
    </section>

    <!-- How we help -->
    <section class="lk-section lk-sand" aria-labelledby="help-heading">
      <div class="lk-wrap">
        <div class="lk-head">
          <p class="lk-eyebrow">Why us</p>
          <h2 id="help-heading" class="lk-h2">How we make it easy</h2>
        </div>
        <div class="lk-help-grid">
          <div
            class="lk-help"
            v-for="(h, i) in help"
            :key="h.title"
          >
            <span class="lk-help-num">{{ i + 1 }}</span>
            <h3 class="lk-help-title">{{ h.title }}</h3>
            <p class="lk-help-text">{{ h.text }}</p>
          </div>
        </div>
      </div>
    </section>

    <!-- CTA -->
    <section id="contact" class="lk-cta" aria-labelledby="cta-heading">
      <span class="lk-cta-bg" :style="{ backgroundImage: `url(${ctaBg})` }" aria-hidden="true"></span>
      <div class="lk-wrap lk-cta-inner">
        <h2 id="cta-heading" class="lk-cta-title">Tell us when you are coming. We will plan the rest.</h2>
        <div class="lk-cta-actions">
          <a href="#contact-form" class="lk-btn lk-btn-sun">Plan my trip</a>
          <a :href="`tel:${phoneRaw}`" class="lk-btn lk-btn-line">Call {{ phoneDisplay }}</a>
        </div>
      </div>
    </section>

    <!-- Our Vision & Our Mission (last section) -->
    <section id="vision-mission" class="lk-section lk-values" aria-labelledby="values-heading">
      <div class="lk-wrap">
        <div class="lk-head lk-head-center">
          <p class="lk-eyebrow">Who we are</p>
          <h2 id="values-heading" class="lk-h2">Our Vision &amp; Our Mission</h2>
        </div>

        <div class="lk-values-grid">
          <article class="lk-value" v-for="v in visionMission" :key="v.title">
            <span class="lk-value-icon" aria-hidden="true">{{ v.icon }}</span>
            <h3 class="lk-value-title">{{ v.title }}</h3>
            <blockquote class="lk-value-text">{{ v.text }}</blockquote>
          </article>
        </div>
      </div>
    </section>
  </main>

  <ArrowButton></ArrowButton>
  <FooterPage></FooterPage>
</template>

<script>
import LankaNavBar from "./Lankanavbar.vue";
import CaroselPage from "./CaroselPage.vue";
import FooterPage from "./FooterPage.vue";
import ArrowButton from "./ArrowButton.vue";

// Your local images (src/assets/carousel)
import imgAmbuluwawa from "@/assets/carousel/Ambuluwawa.jpg";
import imgBeach from "@/assets/carousel/Beach.jpg";
import imgGalle from "@/assets/carousel/Gallefort.jpg";
import imgMirissa from "@/assets/carousel/Mirissa.jpg";
import imgNineArch from "@/assets/carousel/Ninearch.jpg";
import imgSigiriya from "@/assets/carousel/Seegiriya.png";

export default {
  name: "HelloWorld",
  components: {
    LankaNavBar,
    CaroselPage,
    FooterPage,
    ArrowButton,
  },
  data() {
    return {
      // ---- EDIT: brand details (keep in sync with the footer) ----
      brandName: "Your Tourism Brand",
      phoneRaw: "+94000000000",
      phoneDisplay: "+94 00 000 0000",
      // -------------------------------------------------------------

      ctaBg: imgBeach,

      // Quick facts for the "About Sri Lanka" section
      aboutFacts: [
        { label: "Capital", value: "Sri Jayawardenepura Kotte" },
        { label: "Commercial hub", value: "Colombo" },
        { label: "Area", value: "About 65,600 km²" },
        { label: "Population", value: "About 22 million" },
        { label: "Languages", value: "Sinhala, Tamil, English" },
        { label: "Currency", value: "Sri Lankan Rupee (LKR)" },
        { label: "Time zone", value: "UTC +5:30" },
        { label: "Climate", value: "Tropical, warm all year" },
      ],

      // Our Vision & Our Mission
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

      // ---- Our Destinations map ----
      activeIndex: 0,

      // Palm tree positions [longitude, latitude] (decoration only)
      mapPalms: [
        [80.0, 7.5],
        [80.3, 8.0],
        [81.3, 7.4],
        [80.9, 6.6],
        [80.35, 6.45],
        [81.45, 8.4],
      ],

      // Approximate island outline as [longitude, latitude] points (clockwise from the north).
      islandOutline: [
        [80.25, 9.82],
        [80.55, 9.72],
        [80.82, 9.3],
        [81.0, 8.95],
        [81.23, 8.55],
        [81.45, 8.05],
        [81.72, 7.7],
        [81.86, 7.25],
        [81.87, 6.85],
        [81.62, 6.45],
        [81.35, 6.2],
        [81.05, 6.08],
        [80.7, 5.95],
        [80.59, 5.92],
        [80.2, 6.02],
        [79.99, 6.4],
        [79.85, 6.9],
        [79.82, 7.25],
        [79.78, 7.6],
        [79.76, 8.0],
        [79.74, 8.25],
        [79.9, 8.55],
        [79.8, 9.0],
        [79.95, 9.4],
        [80.08, 9.62],
      ],

      // Pins: lat/lng are projected onto the SVG automatically. `side` is the label side.
      mapDestinations: [
        {
          name: "Colombo",
          mapLabel: "Colombo",
          region: "Western coast",
          lat: 6.93,
          lng: 79.85,
          side: "l",
          text: "Sri Lanka's lively commercial capital, with colonial buildings, Galle Face Green, busy markets and some of the island's best street food.",
          tags: ["City life", "Shopping", "Food"],
        },
        {
          name: "Galle",
          mapLabel: "Galle",
          region: "South coast",
          lat: 6.03,
          lng: 80.22,
          side: "l",
          text: "A UNESCO-listed Dutch fort town of ramparts, lighthouse sunsets, boutique cafés and cobbled lanes.",
          tags: ["Heritage", "Sunset walks", "Cafés"],
        },
        {
          name: "Mirissa",
          mapLabel: "Mirissa",
          region: "South coast",
          lat: 5.95,
          lng: 80.45,
          side: "r",
          text: "Palm-lined beaches, surf breaks and whale watching boat trips in season.",
          tags: ["Beaches", "Whale watching", "Surf"],
        },
        {
          name: "Yala",
          mapLabel: "Yala",
          region: "Southeast",
          lat: 6.37,
          lng: 81.4,
          side: "r",
          text: "Early-morning jeep safaris through leopard country, with elephants, crocodiles and rich birdlife.",
          tags: ["Safari", "Wildlife", "Nature"],
        },
        {
          name: "Ella",
          mapLabel: "Ella",
          region: "Hill country",
          lat: 6.87,
          lng: 81.05,
          side: "r",
          text: "Scenic train rides, tea hills, waterfalls and the famous Nine Arch Bridge.",
          tags: ["Train ride", "Hiking", "Tea hills"],
        },
        {
          name: "Kandy",
          mapLabel: "Kandy",
          region: "Central highlands",
          lat: 7.29,
          lng: 80.63,
          side: "r",
          text: "The hill capital, home to the Temple of the Tooth, a peaceful lake and misty mountain views.",
          tags: ["Culture", "Temples", "Gardens"],
        },
        {
          name: "Sigiriya",
          mapLabel: "Sigiriya",
          region: "Cultural Triangle",
          lat: 7.96,
          lng: 80.76,
          side: "r",
          text: "Climb the ancient rock fortress at sunrise and look out over jungle and lakes.",
          tags: ["UNESCO site", "Sunrise climb", "History"],
        },
        {
          name: "Anuradhapura / Polonnaruwa",
          mapLabel: "Anuradhapura",
          region: "Ancient cities",
          lat: 8.35,
          lng: 80.4,
          side: "l",
          text: "Two ancient capitals of great dagobas, sacred Bodhi tree, royal ruins and carved Buddha statues, best explored by bicycle.",
          tags: ["UNESCO sites", "Ruins", "Cycling"],
        },
        {
          name: "Trincomalee",
          mapLabel: "Trincomalee",
          region: "East coast",
          lat: 8.57,
          lng: 81.23,
          side: "r",
          text: "A deep natural harbour with calm blue beaches, snorkelling at Pigeon Island and the Koneswaram temple.",
          tags: ["Beaches", "Snorkelling", "Temple"],
        },
      ],

      // Placeholder numbers: replace with your real figures.
      stats: [
        { label: "Happy travellers", target: 5000, suffix: "+", value: 0, duration: 2000 },
        { label: "Trips completed", target: 1200, suffix: "+", value: 0, duration: 1600 },
        { label: "Years of experience", target: 10, suffix: "+", value: 0, duration: 1000 },
        { label: "UNESCO World Heritage Sites on our routes", target: 8, suffix: "", value: 0, duration: 900 },
      ],
      animationFrames: [],
      observer: null,

      // Yala and Nuwara Eliya still use placeholders: add Yala.jpg / Nuwaraeliya.jpg
      // to src/assets/carousel, import them above and swap the `src` values.
      destinations: [
        {
          name: "Sigiriya",
          region: "Cultural Triangle",
          tagline: "Climb the ancient rock fortress at sunrise.",
          src: imgSigiriya,
          link: "#sigiriya",
          size: "is-big",
        },
        {
          name: "Ella",
          region: "Hill country",
          tagline: "Train rides, tea hills and Nine Arch Bridge.",
          src: imgNineArch,
          link: "#ella",
          size: "",
        },
        {
          name: "Kandy",
          region: "Central highlands",
          tagline: "Temple of the Tooth and misty hill views.",
          src: imgAmbuluwawa,
          link: "#kandy",
          size: "",
        },
        {
          name: "Galle",
          region: "South coast",
          tagline: "Walk the old Dutch fort ramparts at sunset.",
          src: imgGalle,
          link: "#galle",
          size: "",
        },
        {
          name: "Yala",
          region: "Southeast",
          tagline: "Early-morning safaris in leopard country.",
          src: "https://picsum.photos/seed/lk-yala/800/600",
          link: "#yala",
          size: "",
        },
        {
          name: "Mirissa",
          region: "South coast",
          tagline: "Whale watching, surf and palm-lined beaches.",
          src: imgMirissa,
          link: "#mirissa",
          size: "is-wide",
        },
        {
          name: "Nuwara Eliya",
          region: "Tea country",
          tagline: "Cool air, colonial charm and rolling estates.",
          src: "https://picsum.photos/seed/lk-nuwara/1200/600",
          link: "#nuwara-eliya",
          size: "is-wide",
        },
      ],

      gallery: [
        { src: imgSigiriya, caption: "Sigiriya" },
        { src: imgNineArch, caption: "Nine Arch Bridge" },
        { src: imgAmbuluwawa, caption: "Ambuluwawa" },
        { src: imgGalle, caption: "Galle Fort" },
        { src: imgMirissa, caption: "Mirissa" },
        { src: imgBeach, caption: "Southern shores" },
      ],

      // Placeholder trips: adjust durations, routes and links to your real packages.
      tours: [
        {
          days: "5 days",
          title: "Ancient cities and Kandy",
          route: "Negombo, Sigiriya, Kandy",
          text: "Rock fortress, cave temples and the hill capital, with time to wander between them.",
          src: imgSigiriya,
          link: "#tour-cultural",
        },
        {
          days: "4 days",
          title: "Tea country by train",
          route: "Kandy, Nuwara Eliya, Ella",
          text: "Slow rail journeys, plantation walks and sunrise viewpoints in the cool hills.",
          src: imgNineArch,
          link: "#tour-hills",
        },
        {
          days: "6 days",
          title: "South coast and wildlife",
          route: "Galle, Mirissa, Yala",
          text: "Fort town mornings, beach afternoons and a safari at first light.",
          src: imgBeach,
          link: "#tour-south",
        },
      ],

      seasons: [
        {
          months: "Dec – Mar",
          where: "South and west coasts",
          note: "Calm seas and dry weather for beaches, Galle and whale watching.",
        },
        {
          months: "Apr – Sep",
          where: "East coast",
          note: "The drier season for Trincomalee and the eastern beaches.",
        },
        {
          months: "Jan – Apr",
          where: "Hill country",
          note: "Clearer skies for Ella, Nuwara Eliya and the tea estates.",
        },
      ],

      help: [
        {
          title: "Local guides",
          text: "Licensed guides who know the stories, the shortcuts and the best time of day for each place.",
        },
        {
          title: "Your pace",
          text: "Private or small-group trips, with as much or as little planned as you like.",
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
    // The list is doubled so the loop is seamless (track slides exactly -50%)
    marqueeLeft() {
      return [...this.gallery, ...this.gallery];
    },
    marqueeRight() {
      const reversed = [...this.gallery].reverse();
      return [...reversed, ...reversed];
    },

    // Destinations with their projected map positions
    mapPoints() {
      return this.mapDestinations.map((d) => {
        const [x, y] = this.project(d.lng, d.lat);
        return { ...d, x, y };
      });
    },
    activeDest() {
      return this.mapDestinations[this.activeIndex] || this.mapDestinations[0];
    },

    // Smooth closed outline of the island (Catmull-Rom converted to Bezier curves)
    islandPath() {
      const pts = this.islandOutline.map(([lng, lat]) => this.project(lng, lat));
      const n = pts.length;
      const f = (v) => v.toFixed(1);
      let d = `M${f(pts[0][0])},${f(pts[0][1])}`;
      for (let i = 0; i < n; i++) {
        const p0 = pts[(i - 1 + n) % n];
        const p1 = pts[i];
        const p2 = pts[(i + 1) % n];
        const p3 = pts[(i + 2) % n];
        const c1x = p1[0] + (p2[0] - p0[0]) / 6;
        const c1y = p1[1] + (p2[1] - p0[1]) / 6;
        const c2x = p2[0] - (p3[0] - p1[0]) / 6;
        const c2y = p2[1] - (p3[1] - p1[1]) / 6;
        d += `C${f(c1x)},${f(c1y)} ${f(c2x)},${f(c2y)} ${f(p2[0])},${f(p2[1])}`;
      }
      return d + "Z";
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
    // Convert longitude/latitude to SVG coordinates (viewBox 0 0 400 590)
    project(lng, lat) {
      const scale = 130; // pixels per degree
      return [(lng - 79.6) * scale + 70, (9.95 - lat) * scale + 14];
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
/* ===== TOKENS (blue theme, matches the footer) ===== */
.lk-home {
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

.lk-wrap {
  position: relative;
  width: 100%;
  max-width: 1200px;
  margin: 0 auto;
  padding: 0 max(24px, 4vw);
  box-sizing: border-box;
}

.lk-section {
  position: relative;
  padding: 96px 0;
  background: #fff;
}
.lk-sand {
  background: var(--sand);
}

/* ===== TYPE ===== */
.lk-h1,
.lk-h2,
.lk-cta-title {
  font-family: var(--font-display);
  font-weight: 400;
  color: var(--navy);
  text-wrap: balance;
}
.lk-h1 {
  margin: 0;
  font-size: clamp(2rem, 4.2vw, 3.2rem);
  line-height: 1.12;
  text-align: left;
  text-transform: none;
}
.lk-h2 {
  margin: 0 0 10px;
  font-size: clamp(1.7rem, 3vw, 2.4rem);
  line-height: 1.15;
  text-align: left;
  text-transform: none;
}
.lk-h2-light {
  color: #fff;
}
.lk-eyebrow {
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
.lk-eyebrow::before {
  content: "";
  width: 28px;
  height: 2px;
  background: var(--sun);
  border-radius: 2px;
}
.lk-eyebrow-light {
  color: var(--sky);
}
.lk-rule {
  display: block;
  width: 72px;
  height: 4px;
  margin-top: 22px;
  border-radius: 4px;
  background: linear-gradient(90deg, var(--sun), var(--blue-bright));
}
.lk-lead {
  margin: 0 0 16px;
  max-width: 60ch;
  font-size: 1.1rem;
  line-height: 1.7;
  color: var(--muted);
}
.lk-sub {
  margin: 0;
  max-width: 60ch;
  font-size: 1.05rem;
  line-height: 1.6;
  color: var(--muted);
}
.lk-head {
  margin-bottom: 44px;
}
.lk-fine {
  margin: 28px 0 0;
  font-size: 0.9rem;
  color: var(--muted);
}

/* ===== INTRO ===== */
.lk-intro {
  overflow: hidden;
  background: linear-gradient(180deg, #f2f8ff 0%, #ffffff 70%);
}
.lk-blob {
  position: absolute;
  border-radius: 50%;
  filter: blur(60px);
  pointer-events: none;
}
.lk-blob-a {
  top: -80px;
  right: -60px;
  width: 340px;
  height: 340px;
  background: rgba(142, 201, 255, 0.45);
}
.lk-blob-b {
  bottom: -100px;
  left: -80px;
  width: 280px;
  height: 280px;
  background: rgba(245, 185, 74, 0.25);
}
.lk-intro-grid {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 56px;
  align-items: start;
}

/* ===== ABOUT SRI LANKA ===== */
.lk-about-grid {
  display: grid;
  grid-template-columns: 1.1fr 1fr;
  gap: 56px;
  align-items: start;
}
.lk-about-text .lk-lead:last-child {
  margin-bottom: 0;
}
.lk-fact-grid {
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  gap: 16px;
}
.lk-fact {
  padding: 18px 20px;
  background: #fff;
  border: 1px solid #e3dccb;
  border-left: 4px solid var(--sun);
  border-radius: 0 14px 14px 0;
  box-shadow: var(--shadow-sm);
  transition: transform 0.3s ease, box-shadow 0.3s ease;
}
.lk-fact:hover {
  transform: translateY(-4px);
  box-shadow: var(--shadow-md);
}
.lk-fact-label {
  margin: 0 0 4px;
  font-size: 0.75rem;
  font-weight: 700;
  letter-spacing: 0.12em;
  text-transform: uppercase;
  color: var(--blue);
}
.lk-fact-value {
  margin: 0;
  font-family: var(--font-display);
  font-size: 1.05rem;
  line-height: 1.3;
  color: var(--navy);
}

/* ===== STATS ===== */
.lk-stats {
  position: relative;
  background: linear-gradient(135deg, var(--navy) 0%, #12457a 100%);
  color: #fff;
  padding: 52px 0;
}
.lk-stats::before {
  content: "";
  position: absolute;
  inset: 0;
  background-image: radial-gradient(rgba(255, 255, 255, 0.07) 1.5px, transparent 1.5px);
  background-size: 22px 22px;
  pointer-events: none;
}
.lk-stats-grid {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  gap: 24px;
}
.lk-stat {
  padding-left: 18px;
  border-left: 3px solid var(--sun);
}
.lk-stat-value {
  margin: 0;
  font-family: var(--font-display);
  font-size: clamp(2rem, 4vw, 3rem);
  line-height: 1;
  color: var(--sun);
}
.lk-stat-label {
  margin: 8px 0 0;
  max-width: 22ch;
  font-size: 0.95rem;
  line-height: 1.4;
  color: rgba(255, 255, 255, 0.85);
}

/* ===== DESTINATIONS ===== */
.lk-dest-grid {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  grid-auto-rows: 250px;
  gap: 18px;
}
.lk-dest {
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
.lk-dest:hover,
.lk-dest:focus-visible {
  transform: translateY(-6px);
  box-shadow: var(--shadow-md);
}
.lk-dest.is-big {
  grid-column: span 2;
  grid-row: span 2;
}
.lk-dest.is-wide {
  grid-column: span 2;
}
.lk-dest-photo {
  position: absolute;
  inset: 0;
  background-size: cover;
  background-position: center;
  transition: transform 0.7s ease;
}
.lk-dest:hover .lk-dest-photo,
.lk-dest:focus-visible .lk-dest-photo {
  transform: scale(1.08);
}
.lk-dest-shade {
  position: absolute;
  inset: 0;
  background: linear-gradient(to top, rgba(11, 42, 74, 0.92) 0%, rgba(11, 42, 74, 0.25) 45%, rgba(11, 42, 74, 0) 70%);
}
.lk-dest-num {
  position: absolute;
  top: 14px;
  right: 16px;
  font-family: var(--font-display);
  font-size: 1.1rem;
  color: rgba(255, 255, 255, 0.85);
  padding: 4px 12px;
  border-radius: 999px;
  background: rgba(11, 42, 74, 0.45);
  backdrop-filter: blur(6px);
}
.lk-dest-text {
  position: absolute;
  left: 22px;
  right: 22px;
  bottom: 20px;
  display: flex;
  flex-direction: column;
  gap: 4px;
}
.lk-dest-region {
  font-size: 0.8rem;
  font-weight: 600;
  letter-spacing: 0.08em;
  text-transform: uppercase;
  color: var(--sun);
}
.lk-dest-name {
  font-family: var(--font-display);
  font-size: 1.5rem;
  line-height: 1.15;
}
.lk-dest.is-big .lk-dest-name {
  font-size: 2.4rem;
}
.lk-dest-tag {
  font-size: 0.95rem;
  line-height: 1.4;
  color: rgba(255, 255, 255, 0.92);
}
.lk-dest-more {
  max-height: 0;
  opacity: 0;
  overflow: hidden;
  font-size: 0.9rem;
  font-weight: 600;
  color: var(--sky);
  transition: max-height 0.35s ease, opacity 0.35s ease, margin 0.35s ease;
}
.lk-dest-more i {
  font-style: normal;
  display: inline-block;
  transition: transform 0.3s ease;
}
.lk-dest:hover .lk-dest-more,
.lk-dest:focus-visible .lk-dest-more {
  max-height: 28px;
  opacity: 1;
  margin-top: 6px;
}
.lk-dest:hover .lk-dest-more i {
  transform: translateX(5px);
}

/* ===== OUR DESTINATIONS MAP ===== */
.lk-map-layout {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 48px;
  align-items: center;
}
.lk-map-stage {
  perspective: 1400px;
}
.lk-map-card {
  position: relative;
  max-width: 520px;
  margin: 0 auto;
  padding: 18px 10px 6px;
  border-radius: 28px;
  background: linear-gradient(160deg, #f4f6f8 0%, #e3e8ee 100%);
  box-shadow: 0 26px 50px rgba(11, 42, 74, 0.18);
  transform: rotateX(14deg);
  transform-origin: 50% 85%;
  transition: transform 0.6s ease;
}
.lk-map-card:hover {
  transform: rotateX(4deg);
}
.lk-map-svg {
  display: block;
  width: 100%;
  height: auto;
  overflow: visible;
}
.lk-island-shadow {
  fill: rgba(11, 42, 74, 0.28);
}
.lk-island-side {
  fill: #b98f00;
  stroke: #b98f00;
  stroke-width: 1;
  stroke-linejoin: round;
}
.lk-island-top {
  fill: url(#lk-top-grad);
  stroke: #fff0a0;
  stroke-width: 1.4;
  stroke-linejoin: round;
}
.lk-compass-ring {
  fill: rgba(255, 255, 255, 0.7);
  stroke: rgba(11, 42, 74, 0.35);
  stroke-width: 1.2;
}
.lk-compass-needle {
  fill: var(--navy);
}
.lk-compass-n {
  font-family: var(--font-body);
  font-size: 12px;
  font-weight: 700;
  fill: var(--navy);
}

/* Clouds, palms and flag */
.lk-cloud {
  fill: url(#lk-cloud-grad);
  filter: drop-shadow(0 6px 6px rgba(11, 42, 74, 0.18));
  animation: lk-float 7s ease-in-out infinite;
}
.lk-cloud-b { animation-duration: 9s; animation-delay: -2s; }
.lk-cloud-c { animation-duration: 8s; animation-delay: -4s; }
.lk-cloud-d { animation-duration: 10s; animation-delay: -1s; }
@keyframes lk-float {
  0%, 100% { transform: translateX(0); }
  50% { transform: translateX(12px); }
}
.lk-palm-trunk {
  fill: none;
  stroke: #7a5a33;
  stroke-width: 2;
  stroke-linecap: round;
}
.lk-palm-leaves ellipse {
  fill: #2f9e4f;
}
.lk-flag-base { fill: #cfd6de; }
.lk-flag-pole { stroke: #e9edf1; stroke-width: 2.2; stroke-linecap: round; }
.lk-flag-bg { fill: #8d153a; }
.lk-flag-green { fill: #00534e; }
.lk-flag-orange { fill: #eb7400; }
.lk-flag-lion { fill: #ffbe29; }

/* Pins */
.lk-pin {
  cursor: pointer;
  outline: none;
}
.lk-pin-body {
  transform-box: fill-box;
  transform-origin: center;
  transition: transform 0.25s ease;
}
.lk-pin-dot {
  fill: #fff;
  stroke: var(--navy);
  stroke-width: 2.5;
  transition: fill 0.25s ease;
}
.lk-pin-num {
  font-family: var(--font-body);
  font-size: 12px;
  font-weight: 700;
  fill: var(--navy);
  pointer-events: none;
}
.lk-pin-label {
  font-family: var(--font-body);
  font-size: 13px;
  font-weight: 700;
  fill: var(--navy);
  stroke: rgba(255, 255, 255, 0.95);
  stroke-width: 4px;
  paint-order: stroke;
  pointer-events: none;
}
.lk-pin:hover .lk-pin-body,
.lk-pin:focus-visible .lk-pin-body,
.lk-pin.is-active .lk-pin-body {
  transform: scale(1.3);
}
.lk-pin.is-active .lk-pin-dot,
.lk-pin:hover .lk-pin-dot {
  fill: var(--navy);
}
.lk-pin.is-active .lk-pin-num,
.lk-pin:hover .lk-pin-num {
  fill: #fff;
}
.lk-pin:focus-visible .lk-pin-dot {
  stroke: var(--sky);
  stroke-width: 3.5;
}
.lk-pin-pulse {
  fill: none;
  stroke: var(--navy);
  stroke-width: 2.5;
  transform-box: fill-box;
  transform-origin: center;
  animation: lk-pulse 1.8s ease-out infinite;
  pointer-events: none;
}
@keyframes lk-pulse {
  from {
    transform: scale(1);
    opacity: 0.9;
  }
  to {
    transform: scale(2.6);
    opacity: 0;
  }
}

/* Detail panel + list */
.lk-dm-panel {
  min-height: 210px;
  padding: 30px 30px 26px;
  background: #fff;
  border: 1px solid #e3dccb;
  border-top: 5px solid var(--sun);
  border-radius: 20px;
  box-shadow: var(--shadow-sm);
}
.lk-dm-panel-region {
  display: flex;
  align-items: center;
  gap: 10px;
  margin: 0 0 8px;
  font-size: 0.8rem;
  font-weight: 700;
  letter-spacing: 0.12em;
  text-transform: uppercase;
  color: var(--blue);
}
.lk-dm-panel-num {
  padding: 3px 10px;
  font-family: var(--font-display);
  font-size: 0.9rem;
  letter-spacing: 0;
  color: var(--navy);
  background: var(--sun);
  border-radius: 999px;
}
.lk-dm-panel-name {
  margin: 0 0 10px;
  font-family: var(--font-display);
  font-weight: 400;
  font-size: 1.9rem;
  line-height: 1.15;
  color: var(--navy);
}
.lk-dm-panel-text {
  margin: 0 0 16px;
  line-height: 1.7;
  color: var(--muted);
}
.lk-dm-tags {
  display: flex;
  flex-wrap: wrap;
  gap: 8px;
  margin: 0;
  padding: 0;
  list-style: none;
}
.lk-dm-tags li {
  padding: 5px 14px;
  font-size: 0.82rem;
  font-weight: 600;
  color: var(--blue);
  background: #e6f1fd;
  border-radius: 999px;
}
.lk-dm-list {
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  gap: 10px;
  margin-top: 18px;
}
.lk-dm-item {
  display: flex;
  align-items: center;
  gap: 12px;
  padding: 10px 14px;
  font-family: var(--font-body);
  font-size: 0.95rem;
  font-weight: 600;
  text-align: left;
  color: var(--navy);
  background: #fff;
  border: 1px solid #e3dccb;
  border-radius: 14px;
  cursor: pointer;
  transition: transform 0.25s ease, box-shadow 0.25s ease, background-color 0.25s ease,
    border-color 0.25s ease;
}
.lk-dm-item:hover,
.lk-dm-item:focus-visible {
  transform: translateY(-2px);
  box-shadow: var(--shadow-sm);
}
.lk-dm-item.is-active {
  background: var(--navy);
  border-color: var(--navy);
  color: #fff;
}
.lk-dm-item:focus-visible {
  outline: 3px solid var(--sky);
  outline-offset: 2px;
}
.lk-dm-item-num {
  flex: 0 0 auto;
  display: inline-flex;
  align-items: center;
  justify-content: center;
  width: 28px;
  height: 28px;
  font-family: var(--font-display);
  font-size: 0.85rem;
  color: #fff;
  background: linear-gradient(135deg, var(--blue) 0%, var(--blue-bright) 100%);
  border-radius: 9px;
}
.lk-dm-item.is-active .lk-dm-item-num {
  color: var(--navy);
  background: var(--sun);
}
.lk-dm-item-name {
  line-height: 1.25;
}

/* ===== GALLERY ===== */
.lk-gallery {
  padding: 88px 0 96px;
  background: linear-gradient(180deg, var(--navy) 0%, #0e3560 100%);
}
/* Auto-moving news-ticker style rows */
.lk-marquee {
  overflow: hidden;
  padding: 10px 0;
  -webkit-mask-image: linear-gradient(90deg, transparent, #000 8%, #000 92%, transparent);
  mask-image: linear-gradient(90deg, transparent, #000 8%, #000 92%, transparent);
}
.lk-track {
  display: flex;
  width: max-content;
  will-change: transform;
}
.lk-track-left {
  animation: lk-scroll-left 45s linear infinite;
}
.lk-track-right {
  animation: lk-scroll-right 55s linear infinite;
}
.lk-marquee:hover .lk-track {
  animation-play-state: paused;
}
@keyframes lk-scroll-left {
  from {
    transform: translateX(0);
  }
  to {
    transform: translateX(-50%);
  }
}
@keyframes lk-scroll-right {
  from {
    transform: translateX(-50%);
  }
  to {
    transform: translateX(0);
  }
}
.lk-shot {
  position: relative;
  flex: 0 0 clamp(220px, 22vw, 300px);
  height: 230px;
  margin: 0 16px 0 0;
  overflow: hidden;
  border-radius: 18px;
  box-shadow: 0 12px 30px rgba(0, 0, 0, 0.35);
}
.lk-shot img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  display: block;
  transition: transform 0.7s ease;
}
.lk-shot:hover img {
  transform: scale(1.08);
}
.lk-shot figcaption {
  position: absolute;
  left: 0;
  right: 0;
  bottom: 0;
  padding: 36px 18px 16px;
  font-family: var(--font-display);
  font-size: 1.15rem;
  color: #fff;
  background: linear-gradient(to top, rgba(11, 42, 74, 0.9), rgba(11, 42, 74, 0));
}

/* ===== TOURS ===== */
.lk-tour-grid {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 28px;
}
.lk-tour {
  display: flex;
  flex-direction: column;
  background: #fff;
  border-radius: 20px;
  overflow: hidden;
  border: 1px solid #e3dccb;
  box-shadow: var(--shadow-sm);
  transition: transform 0.35s ease, box-shadow 0.35s ease;
}
.lk-tour:hover {
  transform: translateY(-8px);
  box-shadow: var(--shadow-md);
}
.lk-tour-photo-wrap {
  position: relative;
  overflow: hidden;
}
.lk-tour-photo {
  height: 230px;
  background-size: cover;
  background-position: center;
  transition: transform 0.7s ease;
}
.lk-tour:hover .lk-tour-photo {
  transform: scale(1.07);
}
.lk-tour-badge {
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
.lk-tour-body {
  display: flex;
  flex-direction: column;
  align-items: flex-start;
  flex: 1;
  padding: 24px;
}
.lk-tour-title {
  margin: 0 0 4px;
  font-family: var(--font-display);
  font-weight: 400;
  font-size: 1.4rem;
  line-height: 1.2;
  color: var(--navy);
}
.lk-tour-route {
  margin: 0 0 12px;
  font-size: 0.95rem;
  font-weight: 600;
  color: var(--blue);
}
.lk-tour-text {
  margin: 0 0 22px;
  flex: 1;
  line-height: 1.6;
  color: var(--muted);
}

/* ===== BEST TIME ===== */
.lk-time-grid {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 24px;
}
.lk-time {
  padding: 30px;
  border-left: 5px solid var(--blue-bright);
  background: linear-gradient(135deg, #f2f8ff 0%, #e6f1fd 100%);
  border-radius: 0 16px 16px 0;
  box-shadow: var(--shadow-sm);
  transition: transform 0.3s ease, box-shadow 0.3s ease;
}
.lk-time:hover {
  transform: translateY(-5px);
  box-shadow: var(--shadow-md);
}
.lk-time-months {
  margin: 0 0 6px;
  font-family: var(--font-display);
  font-size: 2rem;
  line-height: 1.1;
  color: var(--navy);
}
.lk-time-where {
  margin: 0 0 8px;
  font-size: 1.05rem;
  font-weight: 600;
  color: var(--blue);
}
.lk-time-note {
  margin: 0;
  line-height: 1.6;
  color: var(--muted);
}

/* ===== HOW WE HELP ===== */
.lk-help-grid {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  gap: 24px;
}
.lk-help {
  padding: 28px 24px;
  background: #fff;
  border-radius: 18px;
  border: 1px solid #e3dccb;
  box-shadow: var(--shadow-sm);
  transition: transform 0.3s ease, box-shadow 0.3s ease;
}
.lk-help:hover {
  transform: translateY(-6px);
  box-shadow: var(--shadow-md);
}
.lk-help-num {
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
.lk-help-title {
  margin: 0 0 8px;
  font-size: 1.1rem;
  font-weight: 600;
  color: var(--navy);
}
.lk-help-text {
  margin: 0;
  line-height: 1.6;
  color: var(--muted);
}

/* ===== CTA ===== */
.lk-cta {
  position: relative;
  overflow: hidden;
  padding: 96px 0;
  background: var(--navy);
}
.lk-cta-bg {
  position: absolute;
  inset: 0;
  background-size: cover;
  background-position: center;
  opacity: 0.35;
}
.lk-cta::after {
  content: "";
  position: absolute;
  inset: 0;
  background: linear-gradient(135deg, rgba(11, 42, 74, 0.92) 0%, rgba(21, 101, 192, 0.78) 100%);
}
.lk-cta-inner {
  position: relative;
  z-index: 1;
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: 32px;
}
.lk-cta-title {
  margin: 0;
  max-width: 24ch;
  font-size: clamp(1.7rem, 3.2vw, 2.6rem);
  line-height: 1.15;
  color: #fff;
}
.lk-cta-actions {
  display: flex;
  flex-wrap: wrap;
  gap: 12px;
}

/* ===== VISION & MISSION ===== */
.lk-values {
  background: linear-gradient(180deg, #f2f8ff 0%, #ffffff 100%);
}
.lk-head-center {
  text-align: center;
}
.lk-head-center .lk-h2 {
  text-align: center;
}
.lk-values-grid {
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  gap: 28px;
}
.lk-value {
  position: relative;
  padding: 40px 36px 36px;
  background: #fff;
  border: 1px solid #dbe7f5;
  border-top: 5px solid var(--sun);
  border-radius: 20px;
  box-shadow: var(--shadow-sm);
  transition: transform 0.35s ease, box-shadow 0.35s ease;
}
.lk-value:hover {
  transform: translateY(-8px);
  box-shadow: var(--shadow-md);
}
.lk-value-icon {
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
.lk-value-title {
  margin: 0 0 14px;
  font-family: var(--font-display);
  font-weight: 400;
  font-size: 1.8rem;
  line-height: 1.15;
  color: var(--navy);
}
.lk-value-text {
  margin: 0;
  font-size: 1.05rem;
  line-height: 1.75;
  font-style: italic;
  color: var(--muted);
}

/* ===== BUTTONS ===== */
.lk-btn {
  display: inline-block;
  padding: 13px 28px;
  font-size: 0.95rem;
  font-weight: 600;
  text-decoration: none;
  border-radius: 999px;
  transition: background-color 0.25s ease, color 0.25s ease, border-color 0.25s ease,
    transform 0.25s ease, box-shadow 0.25s ease;
}
.lk-btn:hover {
  transform: translateY(-2px);
}
.lk-btn-solid {
  background: var(--blue);
  color: #fff;
  box-shadow: 0 6px 16px rgba(21, 101, 192, 0.3);
}
.lk-btn-solid:hover {
  background: var(--navy);
}
.lk-btn-sun {
  background: var(--sun);
  color: var(--navy);
  box-shadow: 0 8px 20px rgba(245, 185, 74, 0.4);
}
.lk-btn-sun:hover {
  background: #fff;
}
.lk-btn-line {
  color: #fff;
  border: 1.5px solid rgba(255, 255, 255, 0.6);
}
.lk-btn-line:hover {
  border-color: #fff;
  background: rgba(255, 255, 255, 0.12);
}
.lk-btn:focus-visible,
.lk-dest:focus-visible {
  outline: 3px solid var(--sky);
  outline-offset: 3px;
}

/* ===== RESPONSIVE ===== */
@media (max-width: 1024px) {
  .lk-dest-grid {
    grid-template-columns: repeat(2, 1fr);
    grid-auto-rows: 230px;
  }
  .lk-dest.is-big {
    grid-row: span 2;
  }
  .lk-tour-grid,
  .lk-time-grid {
    grid-template-columns: 1fr;
  }
  .lk-help-grid {
    grid-template-columns: repeat(2, 1fr);
  }
  .lk-stats-grid {
    grid-template-columns: repeat(2, 1fr);
  }
  .lk-about-grid {
    grid-template-columns: 1fr;
    gap: 36px;
  }
  .lk-map-layout {
    grid-template-columns: 1fr;
    gap: 36px;
  }
}

@media (max-width: 768px) {
  .lk-section {
    padding: 60px 0;
  }
  .lk-gallery {
    padding: 60px 0 64px;
  }
  .lk-intro-grid {
    grid-template-columns: 1fr;
    gap: 22px;
  }
  .lk-cta {
    padding: 60px 0;
  }
  .lk-cta-inner {
    flex-direction: column;
    align-items: flex-start;
  }
  .lk-shot {
    height: 190px;
    margin-right: 12px;
  }
  .lk-values-grid {
    grid-template-columns: 1fr;
  }
  .lk-value {
    padding: 32px 24px 28px;
  }
  .lk-map-card,
  .lk-map-card:hover {
    transform: rotateX(6deg);
  }
  .lk-dm-panel {
    padding: 24px 22px 22px;
  }
}

@media (max-width: 560px) {
  .lk-dest-grid {
    grid-template-columns: 1fr;
    grid-auto-rows: 220px;
  }
  .lk-dest.is-big,
  .lk-dest.is-wide {
    grid-column: span 1;
  }
  .lk-dest.is-big {
    grid-row: span 1;
  }
  .lk-dest.is-big .lk-dest-name {
    font-size: 1.8rem;
  }
  .lk-help-grid {
    grid-template-columns: 1fr;
  }
  .lk-fact-grid {
    grid-template-columns: 1fr;
  }
  .lk-dm-list {
    grid-template-columns: 1fr;
  }
}

@media (prefers-reduced-motion: reduce) {
  .lk-track-left,
  .lk-track-right {
    animation: none;
  }
  .lk-marquee {
    overflow-x: auto;
  }
  .lk-dest,
  .lk-tour,
  .lk-time,
  .lk-help,
  .lk-fact,
  .lk-value,
  .lk-dm-item,
  .lk-map-card,
  .lk-pin-body,
  .lk-btn,
  .lk-dest-photo,
  .lk-tour-photo,
  .lk-shot img {
    transition: none;
  }
  .lk-pin-pulse {
    animation: none;
    opacity: 0.6;
  }
  .lk-cloud {
    animation: none;
  }
  .lk-dest:hover,
  .lk-tour:hover,
  .lk-time:hover,
  .lk-help:hover,
  .lk-fact:hover,
  .lk-value:hover,
  .lk-dm-item:hover,
  .lk-btn:hover,
  .lk-dest:hover .lk-dest-photo,
  .lk-tour:hover .lk-tour-photo,
  .lk-shot:hover img {
    transform: none;
  }
}
</style>