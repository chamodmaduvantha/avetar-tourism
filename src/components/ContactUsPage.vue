<template>
  <!-- The ONLY nav bar on this page. Do not add another one here or in a parent layout. -->
  <LankaNavBar />

  <main class="ct-page">
    <!-- ===== Hero ===== -->
    <section class="ct-hero" aria-labelledby="ct-title">
      <span class="ct-hero-photo" :style="{ backgroundImage: `url(${heroBg})` }" aria-hidden="true"></span>
      <span class="ct-hero-shade" aria-hidden="true"></span>

      <div class="ct-wrap ct-hero-inner">
        <nav class="ct-crumbs" aria-label="Breadcrumb">
          <a href="/">Home</a>
          <span aria-hidden="true">/</span>
          <span aria-current="page">Contact us</span>
        </nav>

        <h1 id="ct-title" class="ct-hero-title">Tell us about your trip</h1>
        <p class="ct-hero-lead">
          Questions about a route, a stay or the best time to go? Send us a message and a real
          person at {{ brandName }} will reply, usually within 24 hours.
        </p>

        <div class="ct-hero-actions">
          <a :href="`tel:${phoneRaw}`" class="ct-btn ct-btn-sun">Call {{ phoneDisplay }}</a>
          <a :href="whatsappLink" class="ct-btn ct-btn-line" target="_blank" rel="noopener noreferrer">WhatsApp us</a>
        </div>
      </div>

      <svg class="ct-hero-wave" viewBox="0 0 1440 80" preserveAspectRatio="none" aria-hidden="true">
        <path d="M0,40 C240,90 480,0 720,36 C960,72 1200,10 1440,44 L1440,80 L0,80 Z" fill="#ffffff" />
      </svg>
    </section>

    <!-- ===== Contact cards ===== -->
    <section class="ct-section ct-cards-section" aria-label="Ways to reach us">
      <div class="ct-wrap">
        <div class="ct-cards">
          <article class="ct-card">
            <span class="ct-icon" aria-hidden="true">&#9742;</span>
            <h2 class="ct-card-title">Call us</h2>
            <p class="ct-card-text"><a :href="`tel:${phoneRaw}`">{{ phoneDisplay }}</a></p>
            <p class="ct-card-note">Hot line, {{ openHours }}</p>
          </article>

          <article class="ct-card">
            <span class="ct-icon" aria-hidden="true">&#9993;</span>
            <h2 class="ct-card-title">Email us</h2>
            <p class="ct-card-text"><a :href="`mailto:${email}`">{{ email }}</a></p>
            <p class="ct-card-note">We reply within 24 hours</p>
          </article>

          <article class="ct-card">
            <span class="ct-icon" aria-hidden="true">&#128172;</span>
            <h2 class="ct-card-title">WhatsApp</h2>
            <p class="ct-card-text">
              <a :href="whatsappLink" target="_blank" rel="noopener noreferrer">Message {{ phoneDisplay }}</a>
            </p>
            <p class="ct-card-note">Quick questions and photos welcome</p>
          </article>

          <article class="ct-card">
            <span class="ct-icon" aria-hidden="true">&#128205;</span>
            <h2 class="ct-card-title">Visit us</h2>
            <p class="ct-card-text">{{ address }}</p>
            <p class="ct-card-note">{{ openHours }}</p>
          </article>
        </div>
      </div>
    </section>

    <!-- ===== Form + side panel ===== -->
    <section id="ct-form" class="ct-section ct-sand" aria-labelledby="ct-form-heading">
      <div class="ct-wrap ct-form-grid">
        <div class="ct-form-card">
          <p class="ct-eyebrow">Send a message</p>
          <h2 id="ct-form-heading" class="ct-h2">How can we help?</h2>
          <p class="ct-sub">
            Fill in what you know. Everything except your name, email and message is optional.
          </p>

          <form class="ct-form" novalidate @submit.prevent="sendEmail">
            <div class="ct-field">
              <label for="ct-name">Name</label>
              <input
                id="ct-name"
                v-model.trim="form.name"
                :class="{ 'is-invalid': errors.name }"
                type="text"
                autocomplete="name"
                placeholder="Your full name"
              />
              <p v-if="errors.name" class="ct-error" role="alert">{{ errors.name }}</p>
            </div>

            <div class="ct-field">
              <label for="ct-email">Email</label>
              <input
                id="ct-email"
                v-model.trim="form.email"
                :class="{ 'is-invalid': errors.email }"
                type="text"
                inputmode="email"
                autocomplete="email"
                placeholder="you@example.com"
              />
              <p v-if="errors.email" class="ct-error" role="alert">{{ errors.email }}</p>
            </div>

            <div class="ct-field">
              <label for="ct-phone">Phone or WhatsApp (optional)</label>
              <input
                id="ct-phone"
                v-model.trim="form.phone"
                type="tel"
                autocomplete="tel"
                placeholder="+44 7xxx xxxxxx"
              />
            </div>

            <div class="ct-field">
              <label for="ct-topic">What is this about?</label>
              <select id="ct-topic" v-model="form.topic">
                <option v-for="t in topics" :key="t" :value="t">{{ t }}</option>
              </select>
            </div>

            <div class="ct-field">
              <label for="ct-arrival">Arrival date (optional)</label>
              <input id="ct-arrival" v-model="form.arrival" type="date" :min="today" />
            </div>

            <div class="ct-field">
              <label for="ct-people">Number of travellers</label>
              <input id="ct-people" v-model.number="form.people" type="number" min="1" max="50" />
            </div>

            <div class="ct-field ct-field-full">
              <label for="ct-message">Your message</label>
              <textarea
                id="ct-message"
                v-model.trim="form.message"
                :class="{ 'is-invalid': errors.message }"
                rows="5"
                placeholder="Places you would like to visit, special occasions, pace of travel…"
              ></textarea>
              <p v-if="errors.message" class="ct-error" role="alert">{{ errors.message }}</p>
            </div>

            <div class="ct-field-full ct-form-actions">
              <button type="submit" class="ct-btn ct-btn-solid">Send by email</button>
              <button type="button" class="ct-btn ct-btn-whatsapp" @click="sendWhatsApp">Send on WhatsApp</button>
            </div>

            <p class="ct-status" role="status" aria-live="polite">{{ status }}</p>
          </form>
        </div>

        <aside class="ct-side" aria-label="What happens next">
          <div class="ct-side-card">
            <h3 class="ct-side-title">What happens next</h3>
            <ol class="ct-next">
              <li>
                <strong>We read your message</strong>
                <span>Someone on our team goes through it, not an automatic reply.</span>
              </li>
              <li>
                <strong>You hear back in 24 hours</strong>
                <span>Usually sooner during office hours, Monday to Saturday.</span>
              </li>
              <li>
                <strong>We send a draft route</strong>
                <span>Day by day, with stays, transport and guides. Change anything you like.</span>
              </li>
            </ol>
          </div>

          <div class="ct-side-card ct-side-dark">
            <h3 class="ct-side-title">Office hours</h3>
            <p class="ct-hours">{{ openHours }}</p>
            <p class="ct-side-note">{{ topbarNote }}</p>
            <div class="ct-social">
              <a
                v-for="s in socials"
                :key="s.name"
                :href="s.link"
                class="ct-social-link"
                target="_blank"
                rel="noopener noreferrer"
              >{{ s.name }}</a>
            </div>
          </div>
        </aside>
      </div>
    </section>

    <!-- ===== Map ===== -->
    <section class="ct-section" aria-labelledby="ct-map-heading">
      <div class="ct-wrap ct-map-grid">
        <div class="ct-map-text">
          <p class="ct-eyebrow">Find us</p>
          <h2 id="ct-map-heading" class="ct-h2">Our office</h2>
          <span class="ct-rule" aria-hidden="true"></span>
          <p class="ct-lead">{{ address }}</p>
          <p class="ct-lead">{{ openHours }}</p>
          <a :href="mapsLink" class="ct-btn ct-btn-solid" target="_blank" rel="noopener noreferrer">
            Open in Google Maps
          </a>
        </div>

        <div class="ct-map-frame">
          <iframe
            :src="mapsEmbed"
            title="Map showing our office location"
            loading="lazy"
            referrerpolicy="no-referrer-when-downgrade"
            allowfullscreen
          ></iframe>
        </div>
      </div>
    </section>

    <!-- ===== Quick links ===== -->
    <section class="ct-more" aria-label="More pages">
      <div class="ct-wrap">
        <p class="ct-more-label">Looking for something else?</p>
        <div class="ct-more-links">
          <a href="/" class="ct-pill">Back to home</a>
          <a href="/about" class="ct-pill">About us</a>
          <a href="/destinations" class="ct-pill">Destinations</a>
          <a href="/gallery" class="ct-pill">Gallery</a>
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
import imgNineArch from "@/assets/carousel/Ninearch.jpg";

// Must have text, one @, a domain and a dot ending, e.g. name@example.com
const EMAIL_REGEX = /^[^\s@]+@[^\s@]+\.[^\s@]{2,}$/;

const todayISO = () => {
  const d = new Date();
  d.setMinutes(d.getMinutes() - d.getTimezoneOffset());
  return d.toISOString().slice(0, 10);
};

const emptyForm = () => ({
  name: "",
  email: "",
  phone: "",
  topic: "Planning a trip",
  arrival: "",
  people: 2,
  message: "",
});

export default {
  name: "ContactUsPage",
  components: {
    LankaNavBar,
    FooterPage,
    ArrowButton,
  },
  data() {
    return {
      // ---- EDIT: contact details (keep in sync with the nav bar and footer) ----
      brandName: "Ceylon Escapes",
      phoneRaw: "+94776809002",
      phoneDisplay: "+94 77 680 9002",
      // WhatsApp needs the number in international format with no + and no spaces
      whatsappNumber: "94776809002",
      email: "lankaroundtrip@gmail.com",
      address: "No 350/2, Ambalama Mountain, Kapuhempala, Galle, Sri Lanka",
      openHours: "Mon – Sat, 8:30 am – 6:00 pm",
      topbarNote: "Local guides · Free trip planning",
      // Social links: replace with your real pages
      socials: [
        { name: "Instagram", link: "https://www.instagram.com/" },
        { name: "Facebook", link: "https://www.facebook.com/" },
        { name: "Telegram", link: "https://t.me/Kavee9902" },
      ],
      // ---------------------------------------------------------------------------

      heroBg: imgNineArch,
      today: todayISO(),
      topics: ["Planning a trip", "Day trip or excursion", "Hotels and stays", "A trip I have already booked", "Something else"],
      form: emptyForm(),
      errors: {},
      status: "",
    };
  },
  computed: {
    whatsappLink() {
      const text = `Hello ${this.brandName}, I would like to plan a trip to Sri Lanka.`;
      return `https://wa.me/${this.whatsappNumber}?text=${encodeURIComponent(text)}`;
    },
    mapsLink() {
      return `https://www.google.com/maps/search/?api=1&query=${encodeURIComponent(this.address)}`;
    },
    mapsEmbed() {
      return `https://www.google.com/maps?q=${encodeURIComponent(this.address)}&output=embed`;
    },
  },
  methods: {
    validate() {
      const errors = {};
      if (!this.form.name) {
        errors.name = "Please enter your name.";
      }
      if (!EMAIL_REGEX.test(this.form.email || "")) {
        errors.email = "Enter a valid email, for example name@example.com";
      }
      if (!this.form.message) {
        errors.message = "Please tell us a little about what you need.";
      }
      this.errors = errors;
      return Object.keys(errors).length === 0;
    },
    buildMessage() {
      const f = this.form;
      const lines = [
        `Hello ${this.brandName},`,
        "",
        `Name: ${f.name}`,
        `Email: ${f.email}`,
        `Phone: ${f.phone || "-"}`,
        `Topic: ${f.topic}`,
        `Arrival date: ${f.arrival || "Not decided"}`,
        `Travellers: ${f.people || 1}`,
        "",
        f.message,
      ];
      return lines.join("\n");
    },
    sendWhatsApp() {
      if (!this.validate()) {
        this.status = "";
        return;
      }
      const url = `https://wa.me/${this.whatsappNumber}?text=${encodeURIComponent(this.buildMessage())}`;
      window.open(url, "_blank", "noopener");
      this.status = "WhatsApp is opening with your message ready to send.";
    },
    sendEmail() {
      if (!this.validate()) {
        this.status = "";
        return;
      }
      const subject = `${this.form.topic} - ${this.form.name}`;
      const url = `mailto:${this.email}?subject=${encodeURIComponent(subject)}&body=${encodeURIComponent(this.buildMessage())}`;
      window.location.href = url;
      this.status = "Your email app is opening with your message ready to send.";
    },
  },
};
</script>

<style scoped>
/*
  All class names on this page start with "ct-" so they can never clash with the
  nav bar's global "lk-" styles.
*/

/* ===== TOKENS (same blue theme as the home page and footer) ===== */
.ct-page {
  --navy: #0b2a4a;
  --blue: #1565c0;
  --blue-bright: #1e88e5;
  --sky: #8ec9ff;
  --sand: #f6f1e7;
  --sun: #f5b94a;
  --text: #1f2933;
  --muted: #52606d;
  --danger: #b42318;
  --shadow-sm: 0 4px 14px rgba(11, 42, 74, 0.08);
  --shadow-md: 0 14px 34px rgba(11, 42, 74, 0.16);
  --font-display: "Young Serif", Georgia, "Times New Roman", serif;
  --font-body: "Figtree", system-ui, -apple-system, "Segoe UI", Roboto, sans-serif;

  font-family: var(--font-body);
  color: var(--text);
  overflow-x: hidden;
}

.ct-wrap {
  position: relative;
  width: 100%;
  max-width: 1200px;
  margin: 0 auto;
  padding: 0 max(24px, 4vw);
  box-sizing: border-box;
}

.ct-section {
  position: relative;
  padding: 96px 0;
  background: #fff;
}
.ct-sand {
  background: var(--sand);
}

/* ===== TYPE ===== */
.ct-h2,
.ct-hero-title {
  font-family: var(--font-display);
  font-weight: 400;
  text-wrap: balance;
}
.ct-h2 {
  margin: 0 0 10px;
  font-size: clamp(1.7rem, 3vw, 2.4rem);
  line-height: 1.15;
  text-align: left;
  text-transform: none;
  color: var(--navy);
}
.ct-eyebrow {
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
.ct-eyebrow::before {
  content: "";
  width: 28px;
  height: 2px;
  background: var(--sun);
  border-radius: 2px;
}
.ct-rule {
  display: block;
  width: 72px;
  height: 4px;
  margin: 22px 0;
  border-radius: 4px;
  background: linear-gradient(90deg, var(--sun), var(--blue-bright));
}
.ct-lead {
  margin: 0 0 14px;
  max-width: 50ch;
  font-size: 1.1rem;
  line-height: 1.7;
  color: var(--muted);
}
.ct-sub {
  margin: 0 0 28px;
  max-width: 56ch;
  font-size: 1.05rem;
  line-height: 1.6;
  color: var(--muted);
}

/* ===== HERO ===== */
.ct-hero {
  position: relative;
  overflow: hidden;
  padding: 72px 0 130px;
  color: #fff;
  background: var(--navy);
}
.ct-hero-photo {
  position: absolute;
  inset: 0;
  background-size: cover;
  background-position: center;
  /* the one deliberate motion on this page: a slow settle on load */
  animation: ct-drift 14s ease-out forwards;
}
@keyframes ct-drift {
  from { transform: scale(1.08); }
  to { transform: scale(1); }
}
.ct-hero-shade {
  position: absolute;
  inset: 0;
  background: linear-gradient(135deg, rgba(11, 42, 74, 0.9) 0%, rgba(21, 101, 192, 0.6) 100%);
}
.ct-hero-inner {
  position: relative;
  z-index: 2;
}
.ct-crumbs {
  display: flex;
  align-items: center;
  gap: 10px;
  margin-bottom: 26px;
  font-size: 0.9rem;
  font-weight: 600;
  color: rgba(255, 255, 255, 0.8);
}
.ct-crumbs a {
  color: var(--sky);
  text-decoration: none;
}
.ct-crumbs a:hover {
  text-decoration: underline;
}
.ct-hero-title {
  margin: 0 0 18px;
  max-width: 16ch;
  font-size: clamp(2.2rem, 5.2vw, 4rem);
  line-height: 1.08;
  letter-spacing: -0.01em;
  color: #fff;
  text-align: left;
  text-transform: none;
}
.ct-hero-lead {
  margin: 0 0 30px;
  max-width: 54ch;
  font-size: 1.15rem;
  line-height: 1.7;
  color: rgba(255, 255, 255, 0.92);
}
.ct-hero-actions {
  display: flex;
  flex-wrap: wrap;
  gap: 12px;
}
.ct-hero-wave {
  position: absolute;
  left: 0;
  bottom: -1px;
  z-index: 3;
  width: 100%;
  height: 60px;
  display: block;
  pointer-events: none;
}

/* ===== CONTACT CARDS ===== */
.ct-cards-section {
  padding: 0 0 80px;
  margin-top: -20px;
}
.ct-cards {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  gap: 20px;
}
.ct-card {
  padding: 28px 24px;
  background: #fff;
  border: 1px solid #e3dccb;
  border-top: 5px solid var(--sun);
  border-radius: 18px;
  box-shadow: var(--shadow-sm);
}
.ct-icon {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  width: 46px;
  height: 46px;
  margin-bottom: 16px;
  font-size: 1.3rem;
  color: #fff;
  background: linear-gradient(135deg, var(--blue) 0%, var(--blue-bright) 100%);
  border-radius: 14px;
  box-shadow: 0 6px 14px rgba(21, 101, 192, 0.35);
}
.ct-card-title {
  margin: 0 0 8px;
  font-size: 1.1rem;
  font-weight: 600;
  color: var(--navy);
}
.ct-card-text {
  margin: 0 0 6px;
  line-height: 1.5;
  font-weight: 600;
  color: var(--text);
  overflow-wrap: anywhere;
}
.ct-card-text a {
  color: var(--blue);
  text-decoration: none;
}
.ct-card-text a:hover {
  text-decoration: underline;
}
.ct-card-note {
  margin: 0;
  font-size: 0.88rem;
  line-height: 1.45;
  color: var(--muted);
}

/* ===== FORM ===== */
.ct-form-grid {
  display: grid;
  grid-template-columns: 1.4fr 1fr;
  gap: 40px;
  align-items: start;
}
.ct-form-card {
  padding: 40px 36px;
  background: #fff;
  border: 1px solid #e3dccb;
  border-radius: 22px;
  box-shadow: var(--shadow-md);
}
.ct-form {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 18px 16px;
}
.ct-field {
  display: flex;
  flex-direction: column;
  gap: 6px;
  min-width: 0;
}
.ct-field-full {
  grid-column: 1 / -1;
}
.ct-field label {
  font-size: 0.85rem;
  font-weight: 700;
  color: var(--navy);
}
.ct-field input,
.ct-field select,
.ct-field textarea {
  width: 100%;
  padding: 12px 14px;
  font: 500 0.95rem var(--font-body);
  color: var(--text);
  background: #fff;
  border: 1.5px solid #cfe0f3;
  border-radius: 12px;
  box-sizing: border-box;
  transition: border-color 0.2s ease, box-shadow 0.2s ease;
}
.ct-field textarea {
  min-height: 130px;
  resize: vertical;
}
.ct-field input:focus,
.ct-field select:focus,
.ct-field textarea:focus {
  outline: none;
  border-color: var(--blue-bright);
  box-shadow: 0 0 0 3px rgba(30, 136, 229, 0.18);
}
.ct-field input::placeholder,
.ct-field textarea::placeholder {
  color: #93a6c0;
}
.ct-field .is-invalid {
  border-color: var(--danger);
  box-shadow: 0 0 0 3px rgba(180, 35, 24, 0.12);
}
.ct-error {
  margin: 0;
  font-size: 0.82rem;
  font-weight: 600;
  color: var(--danger);
}
.ct-form-actions {
  display: flex;
  flex-wrap: wrap;
  gap: 12px;
  margin-top: 4px;
}
.ct-status {
  grid-column: 1 / -1;
  min-height: 1.2em;
  margin: 0;
  font-size: 0.9rem;
  font-weight: 600;
  color: var(--blue);
}

/* ===== SIDE PANEL ===== */
.ct-side {
  display: grid;
  gap: 20px;
}
.ct-side-card {
  padding: 30px 28px;
  background: #fff;
  border: 1px solid #e3dccb;
  border-radius: 20px;
  box-shadow: var(--shadow-sm);
}
.ct-side-dark {
  color: #fff;
  background: linear-gradient(135deg, var(--navy) 0%, #12457a 100%);
  border-color: var(--navy);
}
.ct-side-title {
  margin: 0 0 16px;
  font-family: var(--font-display);
  font-weight: 400;
  font-size: 1.35rem;
  color: var(--navy);
}
.ct-side-dark .ct-side-title {
  color: #fff;
}
.ct-next {
  display: grid;
  gap: 18px;
  margin: 0;
  padding: 0;
  list-style: none;
  counter-reset: ct-step;
}
.ct-next li {
  position: relative;
  display: flex;
  flex-direction: column;
  gap: 2px;
  padding-left: 46px;
  counter-increment: ct-step;
}
.ct-next li::before {
  content: counter(ct-step);
  position: absolute;
  left: 0;
  top: 0;
  display: flex;
  align-items: center;
  justify-content: center;
  width: 32px;
  height: 32px;
  font-family: var(--font-display);
  color: var(--navy);
  background: var(--sun);
  border-radius: 50%;
}
.ct-next strong {
  font-size: 1rem;
  color: var(--navy);
}
.ct-next span {
  font-size: 0.92rem;
  line-height: 1.5;
  color: var(--muted);
}
.ct-hours {
  margin: 0 0 6px;
  font-size: 1.1rem;
  font-weight: 600;
  color: var(--sun);
}
.ct-side-note {
  margin: 0 0 18px;
  font-size: 0.92rem;
  color: rgba(255, 255, 255, 0.82);
}
.ct-social {
  display: flex;
  flex-wrap: wrap;
  gap: 8px;
}
.ct-social-link {
  padding: 7px 16px;
  font-size: 0.85rem;
  font-weight: 600;
  color: #fff;
  text-decoration: none;
  border: 1.5px solid rgba(255, 255, 255, 0.4);
  border-radius: 999px;
  transition: background-color 0.25s ease, border-color 0.25s ease;
}
.ct-social-link:hover {
  background: rgba(255, 255, 255, 0.14);
  border-color: var(--sun);
}

/* ===== MAP ===== */
.ct-map-grid {
  display: grid;
  grid-template-columns: 0.9fr 1.3fr;
  gap: 56px;
  align-items: center;
}
.ct-map-frame {
  overflow: hidden;
  border: 1px solid #e3dccb;
  border-radius: 22px;
  box-shadow: var(--shadow-md);
  background: #e6f1fd;
}
.ct-map-frame iframe {
  display: block;
  width: 100%;
  height: 380px;
  border: 0;
}

/* ===== QUICK LINKS ===== */
.ct-more {
  padding: 8px 0 8px;
  text-align: center;
  background: #fff;
}
.ct-more-label {
  margin: 0 0 14px;
  font-size: 0.95rem;
  color: var(--muted);
}
.ct-more-links {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  gap: 10px;
}
.ct-pill {
  padding: 8px 18px;
  font-size: 0.9rem;
  font-weight: 600;
  color: var(--blue);
  text-decoration: none;
  border: 1.5px solid #cfe0f3;
  border-radius: 999px;
  transition: background-color 0.25s ease, color 0.25s ease, border-color 0.25s ease;
}
.ct-pill:hover {
  color: #fff;
  background: var(--blue);
  border-color: var(--blue);
}

/* ===== BUTTONS ===== */
.ct-btn {
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
.ct-btn:hover {
  transform: translateY(-2px);
}
.ct-btn-sun {
  color: var(--navy);
  background: var(--sun);
  box-shadow: 0 8px 20px rgba(245, 185, 74, 0.4);
}
.ct-btn-sun:hover {
  background: #fff;
}
.ct-btn-solid {
  color: #fff;
  background: var(--blue);
  box-shadow: 0 6px 16px rgba(21, 101, 192, 0.3);
}
.ct-btn-solid:hover {
  background: var(--navy);
}
.ct-btn-line {
  color: #fff;
  border: 1.5px solid rgba(255, 255, 255, 0.6);
}
.ct-btn-line:hover {
  border-color: #fff;
  background: rgba(255, 255, 255, 0.12);
}
.ct-btn-whatsapp {
  color: #fff;
  background: #128c7e;
  box-shadow: 0 6px 16px rgba(18, 140, 126, 0.35);
}
.ct-btn-whatsapp:hover {
  background: #0e7266;
}
.ct-btn:focus-visible,
.ct-pill:focus-visible,
.ct-social-link:focus-visible,
.ct-crumbs a:focus-visible,
.ct-card-text a:focus-visible {
  outline: 3px solid var(--sky);
  outline-offset: 3px;
}

/* ===== RESPONSIVE ===== */
@media (max-width: 1100px) {
  .ct-cards {
    grid-template-columns: repeat(2, 1fr);
  }
}

@media (max-width: 1024px) {
  .ct-form-grid,
  .ct-map-grid {
    grid-template-columns: 1fr;
    gap: 36px;
  }
}

@media (max-width: 768px) {
  .ct-section {
    padding: 60px 0;
  }
  .ct-cards-section {
    padding: 0 0 56px;
  }
  .ct-hero {
    padding: 48px 0 110px;
  }
  .ct-form-card {
    padding: 28px 20px;
  }
  .ct-map-frame iframe {
    height: 300px;
  }
}

@media (max-width: 560px) {
  .ct-cards,
  .ct-form {
    grid-template-columns: 1fr;
  }
  .ct-form-actions .ct-btn {
    width: 100%;
    text-align: center;
  }
}

@media (prefers-reduced-motion: reduce) {
  .ct-hero-photo {
    animation: none;
  }
  .ct-btn,
  .ct-pill,
  .ct-social-link {
    transition: none;
  }
  .ct-btn:hover {
    transform: none;
  }
}
</style>