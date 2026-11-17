<template>
  <div>
    <!-- The ONLY nav bar on this page. Do not add another one here or in a parent layout. -->
    <LankaNavBar />

    <main class="gl-page">
      <!-- ===== Header ===== -->
      <header class="gl-hero">
        <p class="gl-eyebrow">{{ mainSection === 'gallery' ? 'Travel photos' : 'Travel films' }}</p>
        <h1 class="gl-title">
          {{ mainSection === 'gallery' ? 'Our gallery' : 'Our travel films' }}
        </h1>
        <p class="gl-lead">
          {{ mainSection === 'gallery'
            ? 'Rock fortresses, tea trains, leopard country and long beaches. See where our guests have been.'
            : 'Watch short films from the road, from sunrise climbs to safari mornings.' }}
        </p>
      </header>

      <!-- ===== Gallery / Films switcher (hidden until you add a video) ===== -->
      <div v-if="travelVideos.length" class="gl-switch" role="tablist" aria-label="Choose what to view">
        <button
          type="button"
          role="tab"
          class="gl-switch-btn"
          :class="{ 'is-active': mainSection === 'gallery' }"
          :aria-selected="mainSection === 'gallery' ? 'true' : 'false'"
          @click="setMainSection('gallery')"
        >
          <i class="material-icons" aria-hidden="true">photo_library</i>
          <span>
            <strong>Gallery</strong>
            <small>Photos from the trip</small>
          </span>
        </button>
        <button
          type="button"
          role="tab"
          class="gl-switch-btn"
          :class="{ 'is-active': mainSection === 'films' }"
          :aria-selected="mainSection === 'films' ? 'true' : 'false'"
          @click="setMainSection('films')"
        >
          <i class="material-icons" aria-hidden="true">smart_display</i>
          <span>
            <strong>Films</strong>
            <small>Watch the videos</small>
          </span>
        </button>
      </div>

      <!-- ================= GALLERY SECTION ================= -->
      <template v-if="mainSection === 'gallery'">
        <!-- Marquee: photos + films -->
        <section v-if="marqueeRows.length" class="gl-mq" aria-labelledby="gl-mq-heading">
          <div class="gl-mq-head">
            <div>
              <h2 id="gl-mq-heading" class="gl-mq-title">Moments from the road</h2>
              <p class="gl-mq-sub">
                A moving wall of our latest photos. Hover to pause, and click any card to open it.
              </p>
            </div>

            <button
              type="button"
              class="gl-mq-toggle"
              :aria-pressed="marqueePaused ? 'true' : 'false'"
              :aria-label="marqueePaused ? 'Play slider' : 'Pause slider'"
              @click="marqueePaused = !marqueePaused"
            >
              <i class="material-icons" aria-hidden="true">{{ marqueePaused ? 'play_arrow' : 'pause' }}</i>
              {{ marqueePaused ? 'Play' : 'Pause' }}
            </button>
          </div>

          <div class="gl-mq-rows">
            <div
              v-for="row in marqueeRows"
              :key="row.key"
              class="gl-mq-marquee"
              :class="{ 'is-paused': marqueePaused }"
            >
              <div
                class="gl-mq-track"
                :class="{ 'is-reverse': row.reverse }"
                :style="{ animationDuration: row.duration + 's' }"
              >
                <button
                  v-for="(item, i) in row.items"
                  :key="row.key + '-' + i"
                  type="button"
                  class="gl-mq-card"
                  :class="{ 'is-video': item.type === 'video' }"
                  :aria-hidden="i >= row.count ? 'true' : null"
                  :tabindex="i >= row.count ? -1 : null"
                  :aria-label="i >= row.count ? null : (item.type === 'video' ? 'Play film: ' : 'View photo: ') + item.title"
                  @click="openMarqueeItem(item)"
                >
                  <img :src="item.src" :alt="i >= row.count ? '' : item.title" loading="lazy" class="gl-mq-img" />

                  <span v-if="item.type === 'video'" class="gl-mq-tag">Film</span>
                  <span v-if="item.type === 'video'" class="gl-mq-play" aria-hidden="true">
                    <i class="material-icons">play_arrow</i>
                  </span>

                  <span class="gl-mq-overlay">
                    <span class="gl-mq-card-title">{{ item.title }}</span>
                    <span class="gl-mq-card-year">{{ item.year }}</span>
                  </span>
                </button>
              </div>
            </div>
          </div>
        </section>

        <!-- Filters -->
        <div v-if="categories.length > 2" class="gl-filters" role="group" aria-label="Filter photos by type">
          <button
            v-for="cat in categories"
            :key="cat.key"
            type="button"
            class="gl-filter"
            :class="{ 'is-active': activeCategory === cat.key }"
            :aria-pressed="activeCategory === cat.key ? 'true' : 'false'"
            @click="setCategory(cat.key)"
          >
            {{ cat.label }}
            <span class="gl-filter-count">{{ getCategoryCount(cat.key) }}</span>
          </button>
        </div>

        <!-- Photo groups -->
        <div class="gl-groups">
          <section v-for="group in filteredGroups" :key="group.groupKey" class="gl-group">
            <div class="gl-group-head">
              <span class="gl-group-icon" aria-hidden="true">
                <i class="material-icons">{{ group.icon }}</i>
              </span>
              <div>
                <h2 class="gl-group-title">{{ group.groupLabel }}</h2>
                <span class="gl-group-meta">
                  {{ group.categoryLabel }} · {{ group.year }} ·
                  {{ group.items.length }} photo{{ group.items.length > 1 ? 's' : '' }}
                </span>
              </div>
            </div>

            <!-- shows max 3 tiles -->
            <div class="gl-grid" :class="'gl-grid--' + Math.min(group.items.length, 3)">
              <button
                v-for="item in group.items.slice(0, 2)"
                :key="item.id"
                type="button"
                class="gl-tile"
                :aria-label="'View photo: ' + item.title"
                @click="openLightbox(item, group.items)"
              >
                <img :src="item.src" :alt="item.title" class="gl-tile-img" loading="lazy" />
                <span class="gl-tile-overlay">
                  <span class="gl-tile-title">{{ item.title }}</span>
                  <i class="material-icons" aria-hidden="true">zoom_in</i>
                </span>
              </button>

              <template v-if="group.items.length >= 3">
                <button
                  v-if="group.items.length === 3"
                  type="button"
                  class="gl-tile"
                  :aria-label="'View photo: ' + group.items[2].title"
                  @click="openLightbox(group.items[2], group.items)"
                >
                  <img :src="group.items[2].src" :alt="group.items[2].title" class="gl-tile-img" loading="lazy" />
                  <span class="gl-tile-overlay">
                    <span class="gl-tile-title">{{ group.items[2].title }}</span>
                    <i class="material-icons" aria-hidden="true">zoom_in</i>
                  </span>
                </button>

                <button
                  v-else
                  type="button"
                  class="gl-tile gl-tile--more"
                  :aria-label="'See all ' + group.items.length + ' photos from ' + group.groupLabel"
                  @click="openGroupModal(group)"
                >
                  <img :src="group.items[2].src" alt="" class="gl-tile-img" loading="lazy" />
                  <span class="gl-more">
                    <span class="gl-more-count">+{{ group.items.length - 2 }}</span>
                    <span class="gl-more-label">more photos</span>
                  </span>
                </button>
              </template>
            </div>
          </section>

          <div v-if="filteredGroups.length === 0" class="gl-empty">
            <i class="material-icons" aria-hidden="true">image_not_supported</i>
            <p>No photos in this category yet.</p>
          </div>
        </div>
      </template>

      <!-- ================= FILMS SECTION ================= -->
      <template v-else>
        <div class="gl-films">
          <div v-if="travelVideos.length" class="gl-films-grid">
            <button
              v-for="video in travelVideos"
              :key="video.id"
              type="button"
              class="gl-film"
              :aria-label="'Play film: ' + video.title"
              @click="openVideoModal(video)"
            >
              <span class="gl-film-thumb">
                <img :src="videoThumb(video)" :alt="video.title" loading="lazy" />
                <span class="gl-film-play" aria-hidden="true"><i class="material-icons">play_arrow</i></span>
                <span v-if="video.badge" class="gl-film-badge">{{ video.badge }}</span>
              </span>
              <span class="gl-film-info">
                <span class="gl-film-title">{{ video.title }}</span>
                <span v-if="video.date" class="gl-film-date">{{ video.date }}</span>
              </span>
            </button>
          </div>

          <div v-else class="gl-empty">
            <i class="material-icons" aria-hidden="true">smart_display</i>
            <p>No films yet. Check back soon.</p>
          </div>
        </div>
      </template>

      <!-- ===== Call to action ===== -->
      <section class="gl-cta" aria-labelledby="gl-cta-heading">
        <div class="gl-cta-inner">
          <h2 id="gl-cta-heading" class="gl-cta-title">Want photos like these of your own trip?</h2>
          <div class="gl-cta-actions">
            <a href="/plan-your-trip" class="gl-btn gl-btn-sun">Plan my trip</a>
            <a :href="generalWhatsapp" class="gl-btn gl-btn-line" target="_blank" rel="noopener noreferrer">WhatsApp us</a>
          </div>
        </div>
      </section>
    </main>

    <!-- ===== Film modal ===== -->
    <transition name="gl-fade">
      <div v-if="videoModal" class="gl-backdrop" @click.self="closeVideoModal">
        <div class="gl-video-modal" role="dialog" aria-modal="true" :aria-label="videoModal.title">
          <button type="button" class="gl-lb-close" aria-label="Close film" @click="closeVideoModal">
            <i class="material-icons">close</i>
          </button>
          <div class="gl-video-frame">
            <iframe
              :src="'https://www.youtube.com/embed/' + cleanVideoId(videoModal.youtubeId) + '?autoplay=1&rel=0'"
              title="YouTube video player"
              frameborder="0"
              allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture"
              allowfullscreen
            ></iframe>
          </div>
          <div class="gl-lb-caption">
            <span class="gl-lb-cat">Film</span>
            <h3 class="gl-lb-title">{{ videoModal.title }}</h3>
            <span v-if="videoModal.date" class="gl-lb-year">{{ videoModal.date }}</span>
          </div>
        </div>
      </div>
    </transition>

    <!-- ===== All-photos modal ===== -->
    <transition name="gl-fade">
      <div v-if="groupModal" class="gl-backdrop gl-backdrop--light" @click.self="closeGroupModal">
        <div class="gl-modal" role="dialog" aria-modal="true" :aria-label="groupModal.groupLabel">
          <div class="gl-modal-head">
            <div class="gl-modal-head-left">
              <span class="gl-group-icon" aria-hidden="true">
                <i class="material-icons">{{ groupModal.icon }}</i>
              </span>
              <div>
                <h2 class="gl-modal-title">{{ groupModal.groupLabel }}</h2>
                <span class="gl-group-meta">{{ groupModal.year }} · {{ groupModal.items.length }} photos</span>
              </div>
            </div>
            <button type="button" class="gl-modal-close" aria-label="Close" @click="closeGroupModal">
              <i class="material-icons">close</i>
            </button>
          </div>

          <div class="gl-modal-grid">
            <button
              v-for="(item, idx) in groupModal.items"
              :key="item.id"
              type="button"
              class="gl-modal-card"
              :aria-label="'View photo ' + (idx + 1) + ': ' + item.title"
              @click="openLightboxFromModal(item, groupModal.items)"
            >
              <img :src="item.src" :alt="item.title" loading="lazy" />
              <span class="gl-modal-card-num">{{ idx + 1 }}</span>
            </button>
          </div>
        </div>
      </div>
    </transition>

    <!-- ===== Lightbox ===== -->
    <transition name="gl-fade">
      <div v-if="lightboxItem" class="gl-backdrop" @click.self="closeLightbox">
        <div class="gl-lightbox" role="dialog" aria-modal="true" :aria-label="lightboxItem.title">
          <button type="button" class="gl-lb-close" aria-label="Close lightbox" @click="closeLightbox">
            <i class="material-icons">close</i>
          </button>
          <button type="button" class="gl-lb-nav gl-lb-nav--prev" aria-label="Previous photo" @click="lightboxPrev">
            <i class="material-icons">chevron_left</i>
          </button>

          <div class="gl-lb-image-wrap">
            <transition name="gl-lb-slide" mode="out-in">
              <img :key="lightboxItem.id" :src="lightboxItem.src" :alt="lightboxItem.title" class="gl-lb-image" />
            </transition>
          </div>

          <button type="button" class="gl-lb-nav gl-lb-nav--next" aria-label="Next photo" @click="lightboxNext">
            <i class="material-icons">chevron_right</i>
          </button>

          <div class="gl-lb-caption">
            <span class="gl-lb-cat">{{ lightboxItem.category }}</span>
            <h3 class="gl-lb-title">{{ lightboxItem.title }}</h3>
            <span class="gl-lb-year">{{ lightboxItem.year }}</span>
          </div>
          <div class="gl-lb-counter">{{ lightboxIndex + 1 }} / {{ lightboxSet.length }}</div>
        </div>
      </div>
    </transition>

    <ArrowButton></ArrowButton>
    <FooterPage></FooterPage>
  </div>
</template>

<script>
import LankaNavBar from "./Lankanavbar.vue";
import FooterPage from "./FooterPage.vue";
import ArrowButton from "./ArrowButton.vue";

// Local images (src/assets/carousel). Check the file names match your folder.
import imgSigiriya from "@/assets/carousel/Seegiriya.png";
import imgKandy from "@/assets/carousel/Kandy.jpg";
import imgNineArch from "@/assets/carousel/Ninearch.jpg";
import imgYala from "@/assets/carousel/Yala.jpg";
import imgGalle from "@/assets/carousel/Gallefort.jpg";
import imgMirissa from "@/assets/carousel/Mirissa.jpg";
import imgBeach from "@/assets/carousel/Beach.jpg";
import imgAmbuluwawa from "@/assets/carousel/Ambuluwawa.jpg";

export default {
  name: "GalleryPage",
  components: { LankaNavBar, FooterPage, ArrowButton },

  data() {
    return {
      // ---- EDIT: brand details (keep in sync with the nav bar, tours page and footer) ----
      brandName: "Avatar Tours",
      whatsappNumber: "94776809002", // international format, no + and no spaces
      // -------------------------------------------------------------------------------------

      mainSection: "gallery", // 'gallery' | 'films'

      activeCategory: "all",
      lightboxItem: null,
      lightboxIndex: 0,
      lightboxSet: [],
      groupModal: null,
      videoModal: null,

      marqueePaused: false,

      // ── Films ────────────────────────────────────
      // The Films tab stays hidden while this list is empty.
      // `youtubeId` is the ID part of the link: for https://youtu.be/dQw4w9WgXcQ it is dQw4w9WgXcQ
      // Example:
      // { id: 1, youtubeId: 'dQw4w9WgXcQ', title: 'Sunrise at Sigiriya', date: '2026', badge: 'New',
      //   thumbnail: 'https://example.com/thumb.jpg' }   // thumbnail is optional
      travelVideos: [],

      // ── Photo groups ─────────────────────────────
      // One group per trip, place or theme. `categoryLabel` creates the filter buttons.
      // Add more photos by adding more items; with 4 or more, the card shows "+N more photos".
      // Photos below are placeholders from your carousel folder. Replace with your own.
      groups: [
        {
          groupKey: "sigiriya-kandy",
          groupLabel: "Sigiriya and Kandy",
          categoryKey: "culture",
          categoryLabel: "Culture & Heritage",
          year: "2026",
          icon: "temple_buddhist",
          items: [
            { id: 1001, src: imgSigiriya, title: "Sigiriya Rock Fortress", category: "Culture & Heritage", year: "2026" },
            { id: 1002, src: imgKandy, title: "Temple of the Sacred Tooth Relic, Kandy", category: "Culture & Heritage", year: "2026" },
          ],
        },
        {
          groupKey: "hill-country",
          groupLabel: "Ella and the hill country",
          categoryKey: "hills",
          categoryLabel: "Hill Country",
          year: "2026",
          icon: "train",
          items: [
            { id: 2001, src: imgNineArch, title: "Blue train on the Nine Arch Bridge, Ella", category: "Hill Country", year: "2026" },
            { id: 2002, src: imgAmbuluwawa, title: "Ambuluwawa tower above the hills", category: "Hill Country", year: "2026" },
          ],
        },
        {
          groupKey: "yala-safari",
          groupLabel: "Yala safari",
          categoryKey: "wildlife",
          categoryLabel: "Wildlife",
          year: "2026",
          icon: "pets",
          items: [
            { id: 3001, src: imgYala, title: "Leopard country in Yala National Park", category: "Wildlife", year: "2026" },
          ],
        },
        {
          groupKey: "south-coast",
          groupLabel: "Galle and the south coast",
          categoryKey: "coast",
          categoryLabel: "Beach & Coast",
          year: "2026",
          icon: "beach_access",
          items: [
            { id: 4001, src: imgGalle, title: "Galle Fort lighthouse by the sea", category: "Beach & Coast", year: "2026" },
            { id: 4002, src: imgMirissa, title: "Mirissa beach", category: "Beach & Coast", year: "2026" },
            { id: 4003, src: imgBeach, title: "Palm-lined bay on the south coast", category: "Beach & Coast", year: "2026" },
          ],
        },
      ],
    };
  },

  computed: {
    // "All" plus one button for each category used by a group
    categories() {
      const seen = [];
      this.groups.forEach((g) => {
        if (!seen.find((c) => c.key === g.categoryKey)) {
          seen.push({ key: g.categoryKey, label: g.categoryLabel });
        }
      });
      return [{ key: "all", label: "All" }, ...seen];
    },

    filteredGroups() {
      if (this.activeCategory === "all") return this.groups;
      return this.groups.filter((g) => g.categoryKey === this.activeCategory);
    },

    // Photos used by the marquee: the first 4 of every group.
    featuredPhotos() {
      const out = [];
      this.groups.forEach((g) => {
        g.items.slice(0, 4).forEach((item) => out.push(item));
      });
      return out;
    },

    // Horizontal rows (left, right, left, right). Photos and films are dealt out
    // round-robin across the rows, then films are mixed in evenly.
    // Each list is doubled so the loop is seamless.
    marqueeRows() {
      const ROWS = 4;

      const photos = this.featuredPhotos.map((p) => ({
        type: "photo",
        src: p.src,
        title: p.title,
        year: p.year,
        raw: p,
      }));

      const videos = this.travelVideos.map((v) => ({
        type: "video",
        src: this.videoThumb(v),
        title: v.title,
        year: v.date || "",
        raw: v,
      }));

      const deal = (list) =>
        Array.from({ length: ROWS }, (unused, c) => list.filter((item, i) => i % ROWS === c));

      const photoRows = deal(photos);
      const videoRows = deal(videos);

      const rows = [];
      for (let c = 0; c < ROWS; c++) {
        let list = this.mixIn(photoRows[c], videoRows[c]);
        if (!list.length) continue;
        // A short row would leave a gap in the loop, so repeat it until it is long enough
        const base = list.slice();
        while (list.length < 8) list = list.concat(base);
        rows.push({
          key: "row-" + c,
          reverse: c % 2 === 1,
          count: list.length,
          items: [...list, ...list],
          // Slightly different speed per row so they don't move in lockstep
          duration: Math.max(40, list.length * 5 + c * 8),
        });
      }
      return rows;
    },

    generalWhatsapp() {
      const text = `Hello ${this.brandName}, I would like to plan a trip to Sri Lanka.`;
      return `https://wa.me/${this.whatsappNumber}?text=${encodeURIComponent(text)}`;
    },
  },

  methods: {
    setMainSection(section) {
      this.mainSection = section;
      this.closeLightbox();
      this.closeGroupModal();
      this.closeVideoModal();
    },

    setCategory(key) {
      this.activeCategory = key;
      this.closeLightbox();
      this.closeGroupModal();
    },
    getCategoryCount(key) {
      const list = key === "all" ? this.groups : this.groups.filter((g) => g.categoryKey === key);
      return list.reduce((sum, g) => sum + g.items.length, 0);
    },

    // ── Marquee helpers ─────────────────────────
    // Spreads `extras` (films) evenly between the `base` items (photos)
    mixIn(base, extras) {
      if (!extras.length) return base.slice();
      const gap = Math.max(1, Math.floor(base.length / extras.length));
      const out = [];
      let e = 0;
      base.forEach((item, i) => {
        out.push(item);
        if ((i + 1) % gap === 0 && e < extras.length) out.push(extras[e++]);
      });
      while (e < extras.length) out.push(extras[e++]);
      return out;
    },
    openMarqueeItem(item) {
      if (item.type === "video") {
        this.openVideoModal(item.raw);
      } else {
        this.openLightbox(item.raw, this.featuredPhotos);
      }
    },

    // Strips things like "?si=xxxx" so the YouTube URLs stay valid
    cleanVideoId(id) {
      return String(id || "").split("?")[0];
    },
    videoThumb(video) {
      return video.thumbnail || "https://img.youtube.com/vi/" + this.cleanVideoId(video.youtubeId) + "/hqdefault.jpg";
    },

    // ── Group modal ─────────────────────────────
    openGroupModal(group) {
      this.groupModal = group;
      document.body.style.overflow = "hidden";
    },
    closeGroupModal() {
      this.groupModal = null;
      if (!this.lightboxItem && !this.videoModal) document.body.style.overflow = "";
    },

    // ── Lightbox ────────────────────────────────
    openLightbox(item, groupItems) {
      this.lightboxSet = groupItems;
      this.lightboxIndex = Math.max(0, groupItems.findIndex((i) => i.id === item.id));
      this.lightboxItem = item;
      document.body.style.overflow = "hidden";
    },
    openLightboxFromModal(item, groupItems) {
      this.groupModal = null;
      this.$nextTick(() => {
        this.lightboxSet = groupItems;
        this.lightboxIndex = Math.max(0, groupItems.findIndex((i) => i.id === item.id));
        this.lightboxItem = item;
        document.body.style.overflow = "hidden";
      });
    },
    closeLightbox() {
      this.lightboxItem = null;
      if (!this.groupModal && !this.videoModal) document.body.style.overflow = "";
    },
    lightboxNext() {
      this.lightboxIndex = (this.lightboxIndex + 1) % this.lightboxSet.length;
      this.lightboxItem = this.lightboxSet[this.lightboxIndex];
    },
    lightboxPrev() {
      this.lightboxIndex = (this.lightboxIndex - 1 + this.lightboxSet.length) % this.lightboxSet.length;
      this.lightboxItem = this.lightboxSet[this.lightboxIndex];
    },

    // ── Film modal ──────────────────────────────
    openVideoModal(video) {
      this.videoModal = video;
      document.body.style.overflow = "hidden";
    },
    closeVideoModal() {
      this.videoModal = null;
      if (!this.groupModal && !this.lightboxItem) document.body.style.overflow = "";
    },

    handleKeydown(e) {
      if (this.lightboxItem) {
        if (e.key === "ArrowRight") this.lightboxNext();
        if (e.key === "ArrowLeft") this.lightboxPrev();
        if (e.key === "Escape") this.closeLightbox();
      } else if (this.videoModal) {
        if (e.key === "Escape") this.closeVideoModal();
      } else if (this.groupModal) {
        if (e.key === "Escape") this.closeGroupModal();
      }
    },
  },

  mounted() {
    window.addEventListener("keydown", this.handleKeydown);
  },
  beforeUnmount() {
    window.removeEventListener("keydown", this.handleKeydown);
    document.body.style.overflow = "";
  },
};
</script>

<style scoped>
/*
  All class names on this page start with "gl-" so they can never clash with the
  nav bar's global "lk-" styles or the tours page's "tr-" styles.
*/

/* ===== TOKENS (same blue + sand theme as the tours page) ===== */
.gl-page {
  --navy: #0b2a4a;
  --blue: #1565c0;
  --blue-bright: #1e88e5;
  --sky: #8ec9ff;
  --sand: #f6f1e7;
  --sun: #f5b94a;
  --text: #1f2933;
  --muted: #52606d;
  --line: #cfe0f3;
  --shadow-sm: 0 4px 14px rgba(11, 42, 74, 0.08);
  --shadow-md: 0 14px 34px rgba(11, 42, 74, 0.16);
  --font-display: "Young Serif", Georgia, "Times New Roman", serif;
  --font-body: "Figtree", system-ui, -apple-system, "Segoe UI", Roboto, sans-serif;

  min-height: 100vh;
  padding: 64px 0 0;
  font-family: var(--font-body);
  color: var(--text);
  background: var(--sand);
  overflow-x: hidden;
}

.gl-page .material-icons {
  font-size: 20px;
  line-height: 1;
}

/* ===== HEADER ===== */
.gl-hero {
  max-width: 1200px;
  margin: 0 auto 36px;
  padding: 0 max(24px, 4vw);
}
.gl-eyebrow {
  display: inline-flex;
  align-items: center;
  gap: 10px;
  margin: 0 0 14px;
  font-size: 0.85rem;
  font-weight: 700;
  color: var(--blue);
}
.gl-eyebrow::before {
  content: "";
  width: 28px;
  height: 2px;
  background: var(--sun);
  border-radius: 2px;
}
.gl-title {
  margin: 0 0 14px;
  font-family: var(--font-display);
  font-weight: 400;
  font-size: clamp(2.2rem, 5vw, 3.6rem);
  line-height: 1.08;
  letter-spacing: -0.01em;
  color: var(--navy);
  text-align: left;
  text-transform: none;
  text-wrap: balance;
}
.gl-lead {
  margin: 0;
  max-width: 56ch;
  font-size: 1.1rem;
  line-height: 1.7;
  color: var(--muted);
}

/* ===== GALLERY / FILMS SWITCH ===== */
.gl-switch {
  display: flex;
  flex-wrap: wrap;
  gap: 12px;
  max-width: 1200px;
  margin: 0 auto 36px;
  padding: 0 max(24px, 4vw);
}
.gl-switch-btn {
  display: flex;
  align-items: center;
  gap: 12px;
  min-width: 210px;
  padding: 12px 22px;
  text-align: left;
  font-family: var(--font-body);
  color: var(--navy);
  background: #fff;
  border: 1.5px solid var(--line);
  border-radius: 16px;
  cursor: pointer;
  transition: background-color 0.25s ease, border-color 0.25s ease, color 0.25s ease;
}
.gl-switch-btn span {
  display: flex;
  flex-direction: column;
}
.gl-switch-btn strong {
  font-size: 0.98rem;
}
.gl-switch-btn small {
  font-size: 0.75rem;
  font-weight: 500;
  color: var(--muted);
}
.gl-switch-btn:hover {
  border-color: var(--blue-bright);
}
.gl-switch-btn.is-active {
  color: #fff;
  background: var(--blue);
  border-color: var(--blue);
}
.gl-switch-btn.is-active small {
  color: rgba(255, 255, 255, 0.85);
}

/* ===== MARQUEE ===== */
.gl-mq {
  position: relative;
  margin: 0 0 56px;
  padding: 56px 0 40px;
  overflow: hidden;
  background: var(--navy);
}
.gl-mq-head {
  display: flex;
  align-items: flex-end;
  justify-content: space-between;
  gap: 24px;
  max-width: 1200px;
  margin: 0 auto 28px;
  padding: 0 max(24px, 4vw);
  box-sizing: border-box;
}
.gl-mq-title {
  margin: 0 0 8px;
  font-family: var(--font-display);
  font-weight: 400;
  font-size: clamp(1.6rem, 3vw, 2.3rem);
  line-height: 1.15;
  color: #fff;
  text-align: left;
  text-transform: none;
}
.gl-mq-sub {
  margin: 0;
  max-width: 56ch;
  font-size: 1rem;
  line-height: 1.6;
  color: rgba(255, 255, 255, 0.8);
}
.gl-mq-toggle {
  display: inline-flex;
  align-items: center;
  gap: 8px;
  flex: 0 0 auto;
  padding: 10px 22px;
  font: 600 0.9rem var(--font-body);
  color: #fff;
  background: rgba(255, 255, 255, 0.08);
  border: 1.5px solid rgba(255, 255, 255, 0.45);
  border-radius: 999px;
  cursor: pointer;
  transition: border-color 0.25s ease, background-color 0.25s ease;
}
.gl-mq-toggle .material-icons {
  color: var(--sun);
}
.gl-mq-toggle:hover {
  border-color: var(--sun);
}
.gl-mq-toggle:focus-visible,
.gl-mq-card:focus-visible {
  outline: 3px solid var(--sky);
  outline-offset: 3px;
}

.gl-mq-rows {
  display: flex;
  flex-direction: column;
  gap: 16px;
}
.gl-mq-marquee {
  overflow: hidden;
  padding: 10px 0;
  -webkit-mask-image: linear-gradient(90deg, transparent, #000 6%, #000 94%, transparent);
  mask-image: linear-gradient(90deg, transparent, #000 6%, #000 94%, transparent);
}
.gl-mq-track {
  display: flex;
  align-items: stretch;
  width: max-content;
  will-change: transform;
  animation: gl-scroll-left 80s linear infinite;
}
.gl-mq-track.is-reverse {
  animation-name: gl-scroll-right;
}
.gl-mq-marquee:hover .gl-mq-track,
.gl-mq-marquee:focus-within .gl-mq-track,
.gl-mq-marquee.is-paused .gl-mq-track {
  animation-play-state: paused;
}
@keyframes gl-scroll-left {
  from { transform: translateX(0); }
  to { transform: translateX(-50%); }
}
@keyframes gl-scroll-right {
  from { transform: translateX(-50%); }
  to { transform: translateX(0); }
}

.gl-mq-card {
  position: relative;
  flex: 0 0 clamp(170px, 17vw, 230px);
  height: clamp(110px, 11vw, 145px);
  margin: 0 16px 0 0;
  padding: 0;
  overflow: hidden;
  border: 0;
  border-radius: 16px;
  background: #16406b;
  box-shadow: 0 12px 28px rgba(0, 0, 0, 0.4);
  cursor: pointer;
  transition: transform 0.4s ease, box-shadow 0.4s ease;
}
.gl-mq-card:hover,
.gl-mq-card:focus-visible {
  transform: translateY(-6px);
  box-shadow: 0 18px 36px rgba(30, 136, 229, 0.4);
}
.gl-mq-img {
  display: block;
  width: 100%;
  height: 100%;
  object-fit: cover;
  transition: transform 0.7s ease;
}
.gl-mq-card:hover .gl-mq-img,
.gl-mq-card:focus-visible .gl-mq-img {
  transform: scale(1.08);
}
.gl-mq-overlay {
  position: absolute;
  inset: 0;
  display: flex;
  flex-direction: column;
  justify-content: flex-end;
  gap: 2px;
  padding: 12px 14px;
  text-align: left;
  background: linear-gradient(to top, rgba(11, 42, 74, 0.92) 0%, rgba(11, 42, 74, 0.2) 55%, transparent 100%);
  opacity: 0;
  transition: opacity 0.35s ease;
}
.gl-mq-card:hover .gl-mq-overlay,
.gl-mq-card:focus-visible .gl-mq-overlay {
  opacity: 1;
}
.gl-mq-card-title {
  display: -webkit-box;
  -webkit-line-clamp: 2;
  -webkit-box-orient: vertical;
  overflow: hidden;
  font-size: 0.85rem;
  font-weight: 700;
  line-height: 1.3;
  color: #fff;
}
.gl-mq-card-year {
  font-size: 0.75rem;
  font-weight: 600;
  color: var(--sun);
}
.gl-mq-card.is-video {
  box-shadow: 0 12px 28px rgba(0, 0, 0, 0.4), 0 0 0 2px var(--sun);
}
.gl-mq-card.is-video .gl-mq-img {
  filter: brightness(0.82);
}
.gl-mq-tag {
  position: absolute;
  top: 10px;
  left: 10px;
  z-index: 2;
  padding: 3px 10px;
  font-size: 0.72rem;
  font-weight: 700;
  color: var(--navy);
  background: var(--sun);
  border-radius: 999px;
}
.gl-mq-play {
  position: absolute;
  top: 50%;
  left: 50%;
  z-index: 2;
  display: flex;
  align-items: center;
  justify-content: center;
  width: 54px;
  height: 54px;
  margin: -27px 0 0 -27px;
  color: #fff;
  background: rgba(21, 101, 192, 0.92);
  border-radius: 50%;
  box-shadow: 0 6px 20px rgba(0, 0, 0, 0.4);
  transition: transform 0.25s ease;
}
.gl-mq-play .material-icons {
  font-size: 28px;
}
.gl-mq-card.is-video:hover .gl-mq-play,
.gl-mq-card.is-video:focus-visible .gl-mq-play {
  transform: scale(1.12);
}
.gl-mq-card.is-video .gl-mq-overlay {
  z-index: 1;
}

/* ===== FILTERS ===== */
.gl-filters {
  display: flex;
  flex-wrap: wrap;
  gap: 8px;
  max-width: 1200px;
  margin: 0 auto 36px;
  padding: 0 max(24px, 4vw);
  box-sizing: border-box;
}
.gl-filter {
  display: inline-flex;
  align-items: center;
  gap: 8px;
  padding: 9px 18px;
  font: 600 0.9rem var(--font-body);
  color: var(--blue);
  background: #fff;
  border: 1.5px solid var(--line);
  border-radius: 999px;
  cursor: pointer;
  white-space: nowrap;
  transition: background-color 0.2s ease, color 0.2s ease, border-color 0.2s ease;
}
.gl-filter:hover {
  border-color: var(--blue-bright);
}
.gl-filter.is-active {
  color: #fff;
  background: var(--blue);
  border-color: var(--blue);
}
.gl-filter-count {
  min-width: 22px;
  padding: 1px 7px;
  font-size: 0.75rem;
  font-weight: 700;
  text-align: center;
  color: var(--muted);
  background: #e6f1fd;
  border-radius: 999px;
}
.gl-filter.is-active .gl-filter-count {
  color: #fff;
  background: rgba(255, 255, 255, 0.25);
}
.gl-filter:focus-visible,
.gl-switch-btn:focus-visible,
.gl-btn:focus-visible,
.gl-tile:focus-visible,
.gl-film:focus-visible,
.gl-modal-card:focus-visible {
  outline: 3px solid var(--blue-bright);
  outline-offset: 3px;
}

/* ===== PHOTO GROUPS ===== */
.gl-groups {
  display: flex;
  flex-direction: column;
  gap: 36px;
  max-width: 1280px;
  margin: 0 auto;
  padding: 0 max(24px, 4vw) 72px;
  box-sizing: border-box;
}
.gl-group {
  overflow: hidden;
  background: #fff;
  border: 1px solid #e3dccb;
  border-radius: 20px;
  box-shadow: var(--shadow-sm);
}
.gl-group-head {
  display: flex;
  align-items: center;
  gap: 12px;
  padding: 16px 20px;
  border-bottom: 1px solid #efe9da;
}
.gl-group-icon {
  display: flex;
  align-items: center;
  justify-content: center;
  flex-shrink: 0;
  width: 36px;
  height: 36px;
  color: var(--blue);
  background: #e6f1fd;
  border-radius: 10px;
}
.gl-group-title {
  margin: 0 0 2px;
  font-family: var(--font-display);
  font-weight: 400;
  font-size: 1.15rem;
  line-height: 1.3;
  color: var(--navy);
  text-align: left;
  text-transform: none;
}
.gl-group-meta {
  font-size: 0.8rem;
  font-weight: 500;
  color: var(--muted);
}

.gl-grid {
  display: grid;
  gap: 4px;
  padding: 4px;
  background: #efe9da;
}
.gl-grid--1 { grid-template-columns: 1fr; grid-template-rows: 340px; }
.gl-grid--2 { grid-template-columns: 1fr 1fr; grid-template-rows: 280px; }
.gl-grid--3 { grid-template-columns: 2fr 1fr; grid-template-rows: 155px 155px; }
.gl-grid--3 .gl-tile:first-child { grid-column: 1; grid-row: 1 / 3; }

.gl-tile {
  position: relative;
  display: block;
  padding: 0;
  overflow: hidden;
  border: 0;
  border-radius: 8px;
  background: #d9e6f5;
  cursor: pointer;
}
.gl-tile-img {
  display: block;
  width: 100%;
  height: 100%;
  object-fit: cover;
  transition: transform 0.5s cubic-bezier(0.165, 0.84, 0.44, 1);
}
.gl-tile:hover .gl-tile-img,
.gl-tile:focus-visible .gl-tile-img {
  transform: scale(1.07);
}
.gl-tile-overlay {
  position: absolute;
  inset: 0;
  display: flex;
  align-items: flex-end;
  justify-content: space-between;
  gap: 8px;
  padding: 12px;
  text-align: left;
  color: #fff;
  background: linear-gradient(to top, rgba(11, 42, 74, 0.9) 0%, rgba(21, 101, 192, 0.35) 55%, transparent 100%);
  opacity: 0;
  transition: opacity 0.35s ease;
}
.gl-tile:hover .gl-tile-overlay,
.gl-tile:focus-visible .gl-tile-overlay {
  opacity: 1;
}
.gl-tile-title {
  flex: 1;
  font-size: 0.85rem;
  font-weight: 700;
  line-height: 1.3;
}

.gl-tile--more .gl-tile-img {
  filter: brightness(0.3);
}
.gl-tile--more:hover .gl-tile-img {
  transform: scale(1.05);
  filter: brightness(0.24);
}
.gl-more {
  position: absolute;
  inset: 0;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  gap: 6px;
  color: #fff;
  text-align: center;
}
.gl-more-count {
  font-family: var(--font-display);
  font-size: 2.6rem;
  line-height: 1;
  text-shadow: 0 2px 16px rgba(0, 0, 0, 0.4);
}
.gl-more-label {
  font-size: 0.8rem;
  font-weight: 600;
  opacity: 0.9;
}

.gl-empty {
  padding: 72px 20px;
  text-align: center;
  color: var(--muted);
}
.gl-empty .material-icons {
  font-size: 56px;
  color: var(--line);
}
.gl-empty p {
  margin: 8px 0 0;
  font-size: 1rem;
}

/* ===== FILMS ===== */
.gl-films {
  max-width: 1280px;
  margin: 0 auto;
  padding: 0 max(24px, 4vw) 72px;
  box-sizing: border-box;
}
.gl-films-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(300px, 1fr));
  gap: 24px;
}
.gl-film {
  display: block;
  padding: 0;
  overflow: hidden;
  text-align: left;
  font-family: var(--font-body);
  background: #fff;
  border: 1px solid #e3dccb;
  border-radius: 16px;
  box-shadow: var(--shadow-sm);
  cursor: pointer;
  transition: box-shadow 0.3s ease, transform 0.3s ease;
}
.gl-film:hover {
  transform: translateY(-3px);
  box-shadow: var(--shadow-md);
}
.gl-film-thumb {
  position: relative;
  display: block;
  width: 100%;
  aspect-ratio: 16 / 9;
  overflow: hidden;
  background: #0b2a4a;
}
.gl-film-thumb img {
  display: block;
  width: 100%;
  height: 100%;
  object-fit: cover;
  transition: transform 0.5s ease;
}
.gl-film:hover .gl-film-thumb img {
  transform: scale(1.06);
}
.gl-film-play {
  position: absolute;
  top: 50%;
  left: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  width: 58px;
  height: 58px;
  margin: -29px 0 0 -29px;
  color: #fff;
  background: rgba(21, 101, 192, 0.92);
  border-radius: 50%;
  box-shadow: 0 6px 20px rgba(0, 0, 0, 0.35);
  transition: transform 0.25s ease;
}
.gl-film-play .material-icons {
  font-size: 30px;
}
.gl-film:hover .gl-film-play {
  transform: scale(1.1);
}
.gl-film-badge {
  position: absolute;
  top: 10px;
  left: 10px;
  padding: 3px 10px;
  font-size: 0.72rem;
  font-weight: 700;
  color: var(--navy);
  background: var(--sun);
  border-radius: 999px;
}
.gl-film-info {
  display: block;
  padding: 14px 16px;
}
.gl-film-title {
  display: block;
  margin: 0 0 4px;
  font-size: 0.95rem;
  font-weight: 700;
  line-height: 1.35;
  color: var(--navy);
}
.gl-film-date {
  font-size: 0.8rem;
  font-weight: 500;
  color: var(--muted);
}

/* ===== CTA ===== */
.gl-cta {
  padding: 80px 0;
  background: linear-gradient(135deg, var(--navy) 0%, var(--blue) 100%);
}
.gl-cta-inner {
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: 32px;
  max-width: 1200px;
  margin: 0 auto;
  padding: 0 max(24px, 4vw);
  box-sizing: border-box;
}
.gl-cta-title {
  margin: 0;
  max-width: 24ch;
  font-family: var(--font-display);
  font-weight: 400;
  font-size: clamp(1.7rem, 3.2vw, 2.6rem);
  line-height: 1.15;
  color: #fff;
  text-align: left;
  text-transform: none;
}
.gl-cta-actions {
  display: flex;
  flex-wrap: wrap;
  gap: 12px;
}

/* ===== BUTTONS ===== */
.gl-btn {
  display: inline-block;
  padding: 13px 28px;
  font-family: var(--font-body);
  font-size: 0.95rem;
  font-weight: 600;
  text-decoration: none;
  border-radius: 999px;
  transition: background-color 0.25s ease, border-color 0.25s ease, transform 0.25s ease;
}
.gl-btn:hover {
  transform: translateY(-2px);
}
.gl-btn-sun {
  color: var(--navy);
  background: var(--sun);
  box-shadow: 0 8px 20px rgba(245, 185, 74, 0.4);
}
.gl-btn-sun:hover {
  background: #fff;
}
.gl-btn-line {
  color: #fff;
  border: 1.5px solid rgba(255, 255, 255, 0.6);
}
.gl-btn-line:hover {
  border-color: #fff;
  background: rgba(255, 255, 255, 0.12);
}

/* ===== ALL-PHOTOS MODAL ===== */
.gl-backdrop {
  position: fixed;
  inset: 0;
  z-index: 9999;
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 20px;
  background: rgba(7, 20, 36, 0.94);
  backdrop-filter: blur(8px);
}
.gl-backdrop--light {
  z-index: 8888;
  background: rgba(11, 42, 74, 0.6);
  backdrop-filter: blur(6px);
}
.gl-modal {
  display: flex;
  flex-direction: column;
  width: 100%;
  max-width: 920px;
  max-height: 88vh;
  overflow: hidden;
  background: #fff;
  border-radius: 20px;
  box-shadow: 0 32px 80px rgba(0, 0, 0, 0.28);
  animation: gl-modal-in 0.28s ease;
}
@keyframes gl-modal-in {
  from { opacity: 0; transform: translateY(18px) scale(0.98); }
  to { opacity: 1; transform: none; }
}
.gl-modal-head {
  display: flex;
  align-items: center;
  justify-content: space-between;
  flex-shrink: 0;
  padding: 18px 22px;
  border-bottom: 1px solid #efe9da;
}
.gl-modal-head-left {
  display: flex;
  align-items: center;
  gap: 12px;
}
.gl-modal-title {
  margin: 0 0 2px;
  font-family: var(--font-display);
  font-weight: 400;
  font-size: 1.15rem;
  color: var(--navy);
  text-align: left;
  text-transform: none;
}
.gl-modal-close {
  display: flex;
  align-items: center;
  justify-content: center;
  flex-shrink: 0;
  width: 40px;
  height: 40px;
  color: var(--navy);
  background: #e6f1fd;
  border: 0;
  border-radius: 50%;
  cursor: pointer;
  transition: background-color 0.2s ease;
}
.gl-modal-close:hover {
  background: var(--sun);
}
.gl-modal-grid {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  flex: 1;
  gap: 6px;
  padding: 6px;
  overflow-y: auto;
  background: #efe9da;
}
.gl-modal-card {
  position: relative;
  display: block;
  padding: 0;
  overflow: hidden;
  aspect-ratio: 4 / 3;
  border: 0;
  border-radius: 8px;
  cursor: pointer;
}
.gl-modal-card img {
  display: block;
  width: 100%;
  height: 100%;
  object-fit: cover;
  transition: transform 0.4s ease;
}
.gl-modal-card:hover img {
  transform: scale(1.06);
}
.gl-modal-card-num {
  position: absolute;
  left: 8px;
  bottom: 8px;
  padding: 2px 8px;
  font-size: 0.72rem;
  font-weight: 700;
  color: #fff;
  background: rgba(11, 42, 74, 0.7);
  border-radius: 999px;
}

/* ===== LIGHTBOX + FILM MODAL ===== */
.gl-lightbox,
.gl-video-modal {
  position: relative;
  display: flex;
  flex-direction: column;
  align-items: center;
  width: 100%;
  max-width: 1000px;
}
.gl-video-modal {
  max-width: 900px;
}
.gl-lb-close,
.gl-lb-nav {
  position: absolute;
  z-index: 2;
  display: flex;
  align-items: center;
  justify-content: center;
  color: #fff;
  background: rgba(255, 255, 255, 0.12);
  border: 0;
  border-radius: 50%;
  cursor: pointer;
  transition: background-color 0.25s ease, color 0.25s ease;
}
.gl-lb-close .material-icons,
.gl-lb-nav .material-icons {
  font-size: 28px;
}
.gl-lb-close {
  top: -52px;
  right: 0;
  width: 44px;
  height: 44px;
}
.gl-lb-nav {
  top: 50%;
  width: 52px;
  height: 52px;
  transform: translateY(-50%);
}
.gl-lb-close:hover,
.gl-lb-nav:hover {
  color: var(--navy);
  background: var(--sun);
}
.gl-lb-close:focus-visible,
.gl-lb-nav:focus-visible,
.gl-modal-close:focus-visible,
.gl-mq-toggle:focus-visible {
  outline: 3px solid var(--sky);
  outline-offset: 3px;
}
.gl-lb-nav--prev { left: -70px; }
.gl-lb-nav--next { right: -70px; }
.gl-lb-image-wrap {
  display: flex;
  align-items: center;
  justify-content: center;
  width: 100%;
  max-height: 72vh;
  overflow: hidden;
  background: #0b2a4a;
  border-radius: 12px;
  box-shadow: 0 30px 80px rgba(0, 0, 0, 0.6);
}
.gl-lb-image {
  display: block;
  max-width: 100%;
  max-height: 72vh;
  object-fit: contain;
}
.gl-lb-caption {
  margin-top: 20px;
  text-align: center;
  color: #fff;
}
.gl-lb-cat {
  display: block;
  margin-bottom: 6px;
  font-size: 0.8rem;
  font-weight: 700;
  color: var(--sun);
}
.gl-lb-title {
  margin: 0;
  font-family: var(--font-display);
  font-weight: 400;
  font-size: 1.4rem;
  color: #fff;
}
.gl-lb-year {
  display: block;
  margin-top: 4px;
  font-size: 0.85rem;
  color: rgba(255, 255, 255, 0.6);
}
.gl-lb-counter {
  position: absolute;
  bottom: -36px;
  font-size: 0.8rem;
  color: rgba(255, 255, 255, 0.55);
}
.gl-video-frame {
  width: 100%;
  aspect-ratio: 16 / 9;
  overflow: hidden;
  background: #0b2a4a;
  border-radius: 12px;
  box-shadow: 0 30px 80px rgba(0, 0, 0, 0.6);
}
.gl-video-frame iframe {
  display: block;
  width: 100%;
  height: 100%;
  border: 0;
}

.gl-fade-enter-active,
.gl-fade-leave-active {
  transition: opacity 0.3s ease;
}
.gl-fade-enter-from,
.gl-fade-leave-to {
  opacity: 0;
}
.gl-lb-slide-enter-active,
.gl-lb-slide-leave-active {
  transition: opacity 0.22s ease, transform 0.22s ease;
}
.gl-lb-slide-enter-from {
  opacity: 0;
  transform: scale(0.96);
}
.gl-lb-slide-leave-to {
  opacity: 0;
  transform: scale(1.04);
}

/* ===== RESPONSIVE ===== */
@media (max-width: 1100px) {
  .gl-lb-nav--prev { left: 8px; }
  .gl-lb-nav--next { right: 8px; }
}

@media (max-width: 768px) {
  .gl-page { padding-top: 40px; }
  .gl-grid--1 { grid-template-rows: 240px; }
  .gl-grid--2 { grid-template-rows: 200px; }
  .gl-grid--3 { grid-template-rows: 130px 130px; }
  .gl-modal-grid { grid-template-columns: repeat(2, 1fr); }
  .gl-switch-btn { min-width: 160px; padding: 10px 18px; }
  .gl-mq { padding: 44px 0 32px; margin-bottom: 40px; }
  .gl-mq-head { flex-direction: column; align-items: flex-start; margin-bottom: 20px; }
  .gl-mq-card { border-radius: 12px; }
  .gl-cta { padding: 56px 0; }
  .gl-cta-inner { flex-direction: column; align-items: flex-start; }
  .gl-filters {
    flex-wrap: nowrap;
    overflow-x: auto;
    padding-bottom: 6px;
  }
  .gl-filter { flex: 0 0 auto; }
}

@media (max-width: 480px) {
  .gl-groups { gap: 20px; }
  .gl-grid--1 { grid-template-rows: 200px; }
  .gl-grid--2 { grid-template-rows: 160px; }
  .gl-grid--3 { grid-template-rows: 110px 110px; }
  .gl-more-count { font-size: 2rem; }
  .gl-films-grid { grid-template-columns: 1fr; }
  .gl-lb-nav { width: 40px; height: 40px; }
  .gl-lb-close { top: -48px; width: 38px; height: 38px; }
  .gl-lb-title { font-size: 1.1rem; }
  .gl-switch-btn { min-width: 140px; padding: 9px 14px; gap: 8px; }
}

/* Touch devices have no hover, so show the captions */
@media (hover: none) and (pointer: coarse) {
  .gl-tile-overlay { opacity: 0.6; }
  .gl-mq-overlay { opacity: 1; }
}

/* Respect "reduce motion": stop the animation and let people scroll by hand */
@media (prefers-reduced-motion: reduce) {
  .gl-mq-track { animation: none; }
  .gl-mq-marquee { overflow-x: auto; }
  .gl-modal { animation: none; }
  .gl-mq-card,
  .gl-mq-img,
  .gl-mq-play,
  .gl-tile-img,
  .gl-film,
  .gl-film-thumb img,
  .gl-btn { transition: none; }
  .gl-mq-card:hover,
  .gl-mq-card:hover .gl-mq-img,
  .gl-film:hover,
  .gl-btn:hover { transform: none; }
}
</style>