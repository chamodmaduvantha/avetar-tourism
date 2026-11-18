<template>
  <LankaNavBar :over-hero="true" />

  <main class="pt">
    <!-- ================= HERO ================= -->
    <section class="pt-hero" aria-labelledby="pt-title">
      <span class="pt-hero-bg" :style="{ backgroundImage: `url(${heroImg})` }" aria-hidden="true"></span>
      <span class="pt-hero-shade" aria-hidden="true"></span>

      <div class="pt-wrap pt-hero-inner">
        <nav class="pt-crumbs" aria-label="Breadcrumb">
          <a href="/">Home</a>
          <span aria-hidden="true">/</span>
          <span class="pt-crumb-now">Plan my trip</span>
        </nav>
        <p class="pt-eyebrow">Free trip planning</p>
        <h1 id="pt-title" class="pt-h1">Tell us your dates. We will plan the rest.</h1>
        <p class="pt-sub">
          Share a few details about your trip and our team will reply with a tailored plan,
          usually within 24 hours.
        </p>
      </div>

      <svg class="pt-wave" viewBox="0 0 1440 80" preserveAspectRatio="none" aria-hidden="true">
        <path d="M0,40 C240,90 480,0 720,36 C960,72 1200,10 1440,44 L1440,80 L0,80 Z" fill="#f6f1e7" />
      </svg>
    </section>

    <!-- ================= FORM + SIDE ================= -->
    <section class="pt-section" aria-label="Travel inquiry">
      <div class="pt-wrap pt-layout">
        <!-- Form card -->
        <div class="pt-card">
          <div class="pt-card-head">
            <h2 class="pt-h2">Travel inquiry</h2>
            <p class="pt-card-sub">The more you tell us, the better we can tailor your trip.</p>
          </div>

          <!-- Success state -->
          <div v-if="submitted" class="pt-success" role="status">
            <i class="material-icons">check_circle</i>
            <h3>Thank you, {{ submittedName }}!</h3>
            <p>Your inquiry has been received. Our team will contact you shortly.</p>
            <button type="button" class="pt-btn pt-btn-solid" @click="startAgain">Send another inquiry</button>
          </div>

          <!-- Travel Inquiry form -->
          <form v-else novalidate class="pt-form" @submit.prevent="submitInquiry">
            <div class="pt-field">
              <label for="p-name">Name</label>
              <input id="p-name" v-model.trim="travel.name" type="text" required placeholder="Your full name" />
            </div>

            <div class="pt-field">
              <label for="p-email">Email</label>
              <input
                id="p-email"
                v-model.trim="travel.email"
                :class="{ 'is-invalid': travelEmailError }"
                type="text"
                inputmode="email"
                autocomplete="email"
                required
                placeholder="you@example.com"
              />
              <p v-if="travelEmailError" class="pt-hint-error" role="alert">{{ travelEmailError }}</p>
            </div>

            <div class="pt-field">
              <label for="p-phone">Phone / WhatsApp</label>
              <input id="p-phone" v-model.trim="travel.phone" type="tel" placeholder="+44 7xxx xxxxxx" />
            </div>

            <div class="pt-field">
              <label for="p-country">Country</label>
              <select id="p-country" v-model="travel.country">
                <option value="">Select country</option>
                <option v-for="c in countries" :key="c" :value="c">{{ c }}</option>
              </select>
            </div>

            <div class="pt-field">
              <label for="p-arrival">Arrival Date</label>
              <input id="p-arrival" v-model="travel.arrival" type="date" :min="today" required />
            </div>

            <div class="pt-field">
              <label for="p-departure">Departure Date</label>
              <input id="p-departure" v-model="travel.departure" type="date" :min="travel.arrival || today" required />
            </div>

            <div class="pt-field">
              <label for="p-rooms">Number of Rooms</label>
              <input id="p-rooms" v-model.number="travel.rooms" type="number" min="1" max="25" required />
            </div>

            <div class="pt-field">
              <label for="p-adults">Adults</label>
              <input id="p-adults" v-model.number="travel.adults" type="number" min="1" max="50" required />
            </div>

            <div class="pt-field">
              <label for="p-children">Children</label>
              <input id="p-children" v-model.number="travel.children" type="number" min="0" max="50" />
            </div>

            <div class="pt-field">
              <label for="p-hotel">Hotel Category</label>
              <select id="p-hotel" v-model="travel.hotel" required>
                <option value="" disabled>Select category</option>
                <option v-for="h in hotelCategories" :key="h" :value="h">{{ h }}</option>
              </select>
            </div>

            <div class="pt-field">
              <label for="p-meal">Meal Plan</label>
              <select id="p-meal" v-model="travel.mealPlan" required>
                <option value="" disabled>Select meal plan</option>
                <option v-for="m in mealPlans" :key="m" :value="m">{{ m }}</option>
              </select>
            </div>

            <div class="pt-field">
              <label for="p-budget">Budget per person (USD)</label>
              <select id="p-budget" v-model="travel.budget">
                <option value="">Not sure yet</option>
                <option v-for="b in budgetRanges" :key="b" :value="b">{{ b }}</option>
              </select>
            </div>

            <fieldset class="pt-field pt-full pt-chips">
              <legend>What interests you?</legend>
              <label v-for="opt in interestOptions" :key="opt" class="pt-chip">
                <input type="checkbox" :value="opt" v-model="travel.interests" />
                <span>{{ opt }}</span>
              </label>
            </fieldset>

            <div class="pt-field pt-full">
              <label for="p-message">Tell us about your trip</label>
              <textarea
                id="p-message"
                v-model.trim="travel.message"
                rows="5"
                placeholder="Places you'd like to visit, special occasions, pace of travel…"
              ></textarea>
            </div>

            <p v-if="errorMsg" class="pt-error pt-full" role="alert">{{ errorMsg }}</p>

            <div class="pt-full pt-foot">
              <button type="submit" class="pt-submit" :disabled="submitting">
                {{ submitting ? "Sending…" : "Send Travel Inquiry" }}
                <i class="material-icons">send</i>
              </button>
              <p class="pt-note">We only use your details to reply to this inquiry.</p>
            </div>
          </form>
        </div>

        <!-- Side column -->
        <aside class="pt-side">
          <div class="pt-panel pt-steps">
            <p class="pt-kicker"><i class="material-icons">route</i> How it works</p>
            <ol class="pt-step-list">
              <li v-for="(s, i) in steps" :key="s.title">
                <span class="pt-step-num">{{ i + 1 }}</span>
                <span>
                  <strong>{{ s.title }}</strong>
                  <small>{{ s.text }}</small>
                </span>
              </li>
            </ol>
          </div>

          <div class="pt-panel pt-contact">
            <p class="pt-kicker pt-kicker-light"><i class="material-icons">support_agent</i> Prefer to talk?</p>
            <p class="pt-contact-text">Call or message us and a real person will help.</p>
            <a :href="`tel:${phoneRaw}`" class="pt-contact-row">
              <i class="material-icons">phone</i> {{ phoneDisplay }}
            </a>
            <a :href="`mailto:${email}`" class="pt-contact-row">
              <i class="material-icons">email</i> {{ email }}
            </a>
            <span class="pt-contact-row pt-contact-plain">
              <i class="material-icons">schedule</i> {{ openHours }}
            </span>
          </div>
        </aside>
      </div>
    </section>

    <!-- ================= REASSURANCE ================= -->
    <section class="pt-trust" aria-label="Why plan with us">
      <div class="pt-wrap pt-trust-grid">
        <div class="pt-trust-item" v-for="t in trust" :key="t.title">
          <span class="pt-trust-icon"><i class="material-icons">{{ t.icon }}</i></span>
          <h3 class="pt-trust-title">{{ t.title }}</h3>
          <p class="pt-trust-text">{{ t.text }}</p>
        </div>
      </div>
    </section>
  </main>

  <ArrowButton></ArrowButton>
  <FooterPage></FooterPage>
</template>

<script>
import emailjs from "@emailjs/browser";
import LankaNavBar from "./Lankanavbar.vue";
import FooterPage from "./FooterPage.vue";
import ArrowButton from "./ArrowButton.vue";
import imgBeach from "@/assets/carousel/Beach.jpg";

// Same EmailJS settings as the navbar's Travel Inquiry.
// Public key + service/template IDs are safe in browser code.
// NEVER put the EmailJS private key here.
const EMAILJS = {
  publicKey: "UoAQPbgdpIPj5eGN6",
  serviceId: "service_of2u8m9",
  travelTemplateId: "template_xxkq1e7",
};

const COUNTRIES = [
  "Afghanistan", "Albania", "Algeria", "Andorra", "Angola", "Antigua and Barbuda",
  "Argentina", "Armenia", "Australia", "Austria", "Azerbaijan", "Bahamas",
  "Bahrain", "Bangladesh", "Barbados", "Belarus", "Belgium", "Belize",
  "Benin", "Bhutan", "Bolivia", "Bosnia and Herzegovina", "Botswana", "Brazil",
  "Brunei", "Bulgaria", "Burkina Faso", "Burundi", "Cambodia", "Cameroon",
  "Canada", "Cape Verde", "Central African Republic", "Chad", "Chile", "China",
  "Colombia", "Comoros", "Congo (DRC)", "Congo (Republic)", "Costa Rica", "Croatia",
  "Cuba", "Cyprus", "Czech Republic", "Denmark", "Djibouti", "Dominica",
  "Dominican Republic", "Ecuador", "Egypt", "El Salvador", "Equatorial Guinea", "Eritrea",
  "Estonia", "Eswatini", "Ethiopia", "Fiji", "Finland", "France",
  "Gabon", "Gambia", "Georgia", "Germany", "Ghana", "Greece",
  "Grenada", "Guatemala", "Guinea", "Guinea-Bissau", "Guyana", "Haiti",
  "Honduras", "Hong Kong", "Hungary", "Iceland", "India", "Indonesia",
  "Iran", "Iraq", "Ireland", "Israel", "Italy", "Ivory Coast",
  "Jamaica", "Japan", "Jordan", "Kazakhstan", "Kenya", "Kiribati",
  "Kuwait", "Kyrgyzstan", "Laos", "Latvia", "Lebanon", "Lesotho",
  "Liberia", "Libya", "Liechtenstein", "Lithuania", "Luxembourg", "Macau",
  "Madagascar", "Malawi", "Malaysia", "Maldives", "Mali", "Malta",
  "Marshall Islands", "Mauritania", "Mauritius", "Mexico", "Micronesia", "Moldova",
  "Monaco", "Mongolia", "Montenegro", "Morocco", "Mozambique", "Myanmar",
  "Namibia", "Nauru", "Nepal", "Netherlands", "New Zealand", "Nicaragua",
  "Niger", "Nigeria", "North Korea", "North Macedonia", "Norway", "Oman",
  "Pakistan", "Palau", "Palestine", "Panama", "Papua New Guinea", "Paraguay",
  "Peru", "Philippines", "Poland", "Portugal", "Qatar", "Romania",
  "Russia", "Rwanda", "Saint Kitts and Nevis", "Saint Lucia", "Saint Vincent and the Grenadines", "Samoa",
  "San Marino", "Sao Tome and Principe", "Saudi Arabia", "Senegal", "Serbia", "Seychelles",
  "Sierra Leone", "Singapore", "Slovakia", "Slovenia", "Solomon Islands", "Somalia",
  "South Africa", "South Korea", "South Sudan", "Spain", "Sri Lanka", "Sudan",
  "Suriname", "Sweden", "Switzerland", "Syria", "Taiwan", "Tajikistan",
  "Tanzania", "Thailand", "Timor-Leste", "Togo", "Tonga", "Trinidad and Tobago",
  "Tunisia", "Turkey", "Turkmenistan", "Tuvalu", "Uganda", "Ukraine",
  "United Arab Emirates", "United Kingdom", "United States", "Uruguay", "Uzbekistan", "Vanuatu",
  "Vatican City", "Venezuela", "Vietnam", "Yemen", "Zambia", "Zimbabwe",
  "Other",
];

// Must have text, one @, a domain and a dot ending, e.g. name@example.com
const EMAIL_REGEX = /^[^\s@]+@[^\s@]+\.[^\s@]{2,}$/;

const todayISO = () => {
  const d = new Date();
  d.setMinutes(d.getMinutes() - d.getTimezoneOffset());
  return d.toISOString().slice(0, 10);
};

const emptyTravel = () => ({
  name: "",
  email: "",
  phone: "",
  country: "",
  arrival: "",
  departure: "",
  rooms: 1,
  adults: 2,
  children: 0,
  hotel: "",
  mealPlan: "",
  budget: "",
  interests: [],
  message: "",
});

export default {
  name: "PlanYourTripPage",
  components: { LankaNavBar, FooterPage, ArrowButton },

  data() {
    return {
      heroImg: imgBeach,

      // ---- keep in sync with the navbar / footer ----
      phoneRaw: "+94776809002",
      phoneDisplay: "+94 77 680 9002",
      email: "lankaroundtrip@gmail.com",
      openHours: "Mon – Sat, 8:30 am – 6:00 pm",
      // -----------------------------------------------

      submitting: false,
      submitted: false,
      submittedName: "",
      errorMsg: "",
      today: todayISO(),
      travel: emptyTravel(),

      hotelCategories: ["5 Star", "4 Star", "3 Star", "Budget", "Boutique (Luxury)"],
      countries: COUNTRIES,
      mealPlans: ["Bed & Breakfast", "Half Board", "Full Board"],
      budgetRanges: ["$1 - $500", "$500 - $1000", "$1000 - $2000", "$2000 - $3500", "$3500 - $6000", "$6000+"],
      interestOptions: [
        "Culture & Heritage",
        "Nature",
        "Ancient & Archaeology",
        "Wildlife",
        "Adventure",
        "Beach",
        "Shopping",
      ],

      steps: [
        { title: "Send your inquiry", text: "Fill in the form with your dates, group size and interests." },
        { title: "We design your plan", text: "Our team builds a route with stays, guides and transfers." },
        { title: "Review and adjust", text: "We reply within 24 hours and tweak the plan until it feels right." },
      ],

      trust: [
        { icon: "payments", title: "Free planning", text: "Sending an inquiry costs nothing and carries no obligation." },
        { icon: "badge", title: "Local guides", text: "Licensed guides who know the stories and the best time of day." },
        { icon: "tune", title: "Your pace", text: "Private or small-group trips, as relaxed or as busy as you like." },
        { icon: "support_agent", title: "Support on the road", text: "A real person to call during your trip if plans change." },
      ],
    };
  },

  computed: {
    travelEmailError() {
      return this.emailMessage(this.travel.email);
    },
  },

  mounted() {
    document.title = "Plan My Trip | Ceylon Escapes";
  },

  methods: {
    // Live inline hint under the email field
    emailMessage(value) {
      if (!value) return "";
      return EMAIL_REGEX.test(value) ? "" : "Enter a valid email, for example name@example.com";
    },

    // Same validation rules as the navbar's inquiry form
    validate(form) {
      if (!form.name) {
        return "Please enter your name.";
      }
      if (!EMAIL_REGEX.test(form.email || "")) {
        return "Please enter a valid email address, for example name@example.com.";
      }
      if (!form.arrival || !form.departure) {
        return "Please choose your arrival and departure dates.";
      }
      if (!form.hotel || !form.hotel.length) {
        return "Please select a hotel category.";
      }
      if ("mealPlan" in form && !form.mealPlan.length) {
        return "Please select a meal plan.";
      }
      if (!(form.persons ?? form.adults) || (form.persons ?? form.adults) < 1) {
        return "Please enter at least 1 person.";
      }
      if (form.arrival && form.departure && form.departure < form.arrival) {
        return "Departure date must be on or after the arrival date.";
      }
      return "";
    },

    async submitInquiry() {
      const form = this.travel;

      const problem = this.validate(form);
      if (problem) {
        this.errorMsg = problem;
        return;
      }

      this.errorMsg = "";
      this.submitting = true;

      try {
        const params = {
          from_name: form.name,
          reply_to: form.email,
          phone: form.phone || "—",
          country: form.country || "—",
          arrival: form.arrival,
          departure: form.departure,
          rooms: form.rooms,
          adults: form.adults,
          children: form.children || 0,
          hotel: form.hotel,
          meal_plan: form.mealPlan,
          budget: form.budget || "Not specified",
          interests: form.interests.length ? form.interests.join(", ") : "—",
          message: form.message || "—",
        };

        await emailjs.send(EMAILJS.serviceId, EMAILJS.travelTemplateId, params, {
          publicKey: EMAILJS.publicKey,
        });

        this.submittedName = form.name;
        this.submitted = true;
        this.$emit("inquiry-sent", { type: "travel", ...form });
        window.scrollTo({ top: 0, behavior: "smooth" });
      } catch (e) {
        this.errorMsg = "Sorry, something went wrong. Please try again or email us directly.";
      } finally {
        this.submitting = false;
      }
    },

    startAgain() {
      this.travel = emptyTravel();
      this.submitted = false;
      this.errorMsg = "";
      this.today = todayISO();
    },
  },
};
</script>

<style scoped>
/* ===== TOKENS (same blue + gold theme as the rest of the site) ===== */
.pt {
  --navy: #0b2a4a;
  --blue: #1565c0;
  --blue-bright: #1e88e5;
  --sky: #8ec9ff;
  --sand: #f6f1e7;
  --sun: #f5b94a;
  --sun-deep: #d99512;
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
.pt .material-icons {
  font-size: 22px;
  line-height: 1;
}
.pt-wrap {
  position: relative;
  width: 100%;
  max-width: 1200px;
  margin: 0 auto;
  padding: 0 max(24px, 4vw);
  box-sizing: border-box;
}

/* ===== HERO ===== */
.pt-hero {
  position: relative;
  display: flex;
  align-items: flex-end;
  min-height: 58vh;
  padding: 190px 0 120px;
  overflow: hidden;
  background: var(--navy);
}
.pt-hero-bg {
  position: absolute;
  inset: 0;
  background-size: cover;
  background-position: center;
}
.pt-hero-shade {
  position: absolute;
  inset: 0;
  background: linear-gradient(to top, rgba(11, 42, 74, 0.92) 0%, rgba(11, 42, 74, 0.45) 60%, rgba(11, 42, 74, 0.6) 100%);
}
.pt-hero-inner {
  z-index: 1;
}
.pt-wave {
  position: absolute;
  left: 0;
  bottom: -1px;
  width: 100%;
  height: 70px;
  pointer-events: none;
}
.pt-crumbs {
  display: flex;
  flex-wrap: wrap;
  gap: 8px;
  margin-bottom: 24px;
  font-size: 0.88rem;
  color: rgba(255, 255, 255, 0.75);
}
.pt-crumbs a {
  color: inherit;
  text-decoration: none;
  transition: color 0.2s ease;
}
.pt-crumbs a:hover {
  color: var(--sun);
}
.pt-crumb-now {
  color: #fff;
  font-weight: 600;
}
.pt-eyebrow {
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
.pt-eyebrow::before {
  content: "";
  width: 28px;
  height: 2px;
  background: var(--sun);
  border-radius: 2px;
}
.pt-h1 {
  margin: 0;
  max-width: 18ch;
  font-family: var(--font-display);
  font-weight: 400;
  font-size: clamp(2.2rem, 5.4vw, 4rem);
  line-height: 1.08;
  color: #fff;
  text-wrap: balance;
}
.pt-sub {
  max-width: 48ch;
  margin: 18px 0 0;
  font-size: 1.15rem;
  line-height: 1.6;
  color: rgba(255, 255, 255, 0.9);
}

/* ===== FORM LAYOUT ===== */
.pt-section {
  padding: 8px 0 96px;
  background: var(--sand);
}
.pt-layout {
  display: grid;
  grid-template-columns: 1.7fr 1fr;
  gap: 32px;
  align-items: start;
}
.pt-card {
  padding: 36px 36px 34px;
  background: #fff;
  border: 1px solid #e3dccb;
  border-top: 5px solid var(--sun);
  border-radius: 22px;
  box-shadow: var(--shadow-md);
}
.pt-card-head {
  margin-bottom: 26px;
}
.pt-h2 {
  margin: 0 0 6px;
  font-family: var(--font-display);
  font-weight: 400;
  font-size: clamp(1.6rem, 3vw, 2.1rem);
  line-height: 1.15;
  color: var(--navy);
}
.pt-card-sub {
  margin: 0;
  color: var(--muted);
  line-height: 1.5;
}

/* ===== FORM ===== */
.pt-form {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 18px 18px;
}
.pt-field {
  display: flex;
  flex-direction: column;
  gap: 6px;
  min-width: 0;
  margin: 0;
  padding: 0;
  border: 0;
}
.pt-full {
  grid-column: 1 / -1;
}
.pt-field label,
.pt-field legend {
  padding: 0;
  font-size: 0.82rem;
  font-weight: 700;
  color: var(--navy);
}
.pt-field input,
.pt-field select,
.pt-field textarea {
  width: 100%;
  box-sizing: border-box;
  padding: 12px 14px;
  font: 500 0.97rem var(--font-body);
  color: var(--text);
  background: #fff;
  border: 1.5px solid #cfe0f3;
  border-radius: 12px;
  transition: border-color 0.2s ease, box-shadow 0.2s ease;
}
.pt-field textarea {
  min-height: 120px;
  resize: vertical;
}
.pt-field input:focus,
.pt-field select:focus,
.pt-field textarea:focus {
  outline: none;
  border-color: var(--blue-bright);
  box-shadow: 0 0 0 3px rgba(30, 136, 229, 0.18);
}
.pt-field input::placeholder,
.pt-field textarea::placeholder {
  color: #93a6c0;
}

/* Invalid email state + inline hint */
.pt-field input.is-invalid {
  border-color: #b42318;
  box-shadow: 0 0 0 3px rgba(180, 35, 24, 0.12);
}
.pt-hint-error {
  margin: 0;
  font-size: 0.78rem;
  font-weight: 600;
  color: #b42318;
}

/* Interest chips */
.pt-chips {
  flex-direction: row;
  flex-wrap: wrap;
  gap: 8px;
}
.pt-chips legend {
  width: 100%;
  margin-bottom: 8px;
}
.pt-chip {
  position: relative;
  cursor: pointer;
}
.pt-chip input {
  position: absolute;
  width: 1px;
  height: 1px;
  opacity: 0;
  pointer-events: none;
}
.pt-chip span {
  display: inline-block;
  padding: 8px 16px;
  font-size: 0.88rem;
  font-weight: 600;
  color: var(--blue);
  border: 1.5px solid #cfe0f3;
  border-radius: 999px;
  transition: background-color 0.2s ease, border-color 0.2s ease, color 0.2s ease;
}
.pt-chip:hover span {
  border-color: var(--blue-bright);
}
.pt-chip input:checked + span {
  color: #fff;
  background: var(--blue);
  border-color: var(--blue);
}
.pt-chip input:focus-visible + span {
  outline: 3px solid var(--sky);
  outline-offset: 2px;
}

.pt-error {
  margin: 0;
  padding: 11px 15px;
  font-size: 0.88rem;
  font-weight: 600;
  color: #b42318;
  background: #fdecec;
  border-radius: 10px;
}
.pt-foot {
  margin-top: 4px;
}
.pt-submit {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  gap: 10px;
  width: 100%;
  padding: 15px 24px;
  font: 800 1rem var(--font-body);
  color: var(--navy);
  cursor: pointer;
  background: linear-gradient(135deg, #ffc857 0%, var(--sun) 55%, var(--sun-deep) 100%);
  border: 0;
  border-radius: 999px;
  box-shadow: 0 8px 20px rgba(217, 149, 18, 0.35);
  transition: transform 0.2s ease, box-shadow 0.2s ease, filter 0.2s ease;
}
.pt-submit:hover {
  transform: translateY(-2px);
  box-shadow: 0 14px 26px rgba(217, 149, 18, 0.45);
  filter: brightness(1.04);
}
.pt-submit:disabled {
  opacity: 0.65;
  cursor: not-allowed;
  transform: none;
}
.pt-note {
  margin: 12px 0 0;
  font-size: 0.85rem;
  text-align: center;
  color: var(--muted);
}

/* Success */
.pt-success {
  padding: 30px 10px 10px;
  text-align: center;
}
.pt-success .material-icons {
  font-size: 68px;
  color: #22a06b;
}
.pt-success h3 {
  margin: 10px 0 8px;
  font-family: var(--font-display);
  font-weight: 400;
  font-size: 1.6rem;
  color: var(--navy);
}
.pt-success p {
  margin: 0 0 24px;
  color: var(--muted);
}

/* Buttons */
.pt-btn {
  display: inline-block;
  padding: 13px 28px;
  font: 600 0.95rem var(--font-body);
  text-decoration: none;
  cursor: pointer;
  border: 0;
  border-radius: 999px;
  transition: background-color 0.25s ease, transform 0.25s ease;
}
.pt-btn:hover {
  transform: translateY(-2px);
}
.pt-btn-solid {
  color: #fff;
  background: var(--blue);
  box-shadow: 0 6px 16px rgba(21, 101, 192, 0.3);
}
.pt-btn-solid:hover {
  background: var(--navy);
}

/* ===== SIDE ===== */
.pt-side {
  position: sticky;
  top: 110px;
  display: flex;
  flex-direction: column;
  gap: 20px;
}
.pt-panel {
  padding: 28px;
  border-radius: 20px;
  box-shadow: var(--shadow-sm);
}
.pt-kicker {
  display: flex;
  align-items: center;
  gap: 8px;
  margin: 0 0 16px;
  font-size: 0.78rem;
  font-weight: 700;
  letter-spacing: 0.12em;
  text-transform: uppercase;
  color: var(--blue);
}
.pt-kicker-light {
  color: var(--sun);
}
.pt-steps {
  background: #fff;
  border: 1px solid #e3dccb;
}
.pt-step-list {
  display: flex;
  flex-direction: column;
  gap: 18px;
  margin: 0;
  padding: 0;
  list-style: none;
}
.pt-step-list li {
  display: flex;
  align-items: flex-start;
  gap: 14px;
}
.pt-step-num {
  flex: 0 0 auto;
  display: grid;
  place-items: center;
  width: 36px;
  height: 36px;
  font-family: var(--font-display);
  color: #fff;
  background: linear-gradient(135deg, var(--blue) 0%, var(--blue-bright) 100%);
  border-radius: 12px;
  box-shadow: 0 6px 14px rgba(21, 101, 192, 0.3);
}
.pt-step-list strong {
  display: block;
  color: var(--navy);
}
.pt-step-list small {
  display: block;
  margin-top: 2px;
  font-size: 0.9rem;
  line-height: 1.5;
  color: var(--muted);
}
.pt-contact {
  color: #fff;
  background: linear-gradient(135deg, var(--navy) 0%, #12457a 100%);
}
.pt-contact-text {
  margin: 0 0 14px;
  line-height: 1.55;
  color: rgba(255, 255, 255, 0.85);
}
.pt-contact-row {
  display: flex;
  align-items: center;
  gap: 10px;
  padding: 8px 0;
  font-weight: 600;
  color: #fff;
  text-decoration: none;
  word-break: break-word;
}
.pt-contact-row .material-icons {
  color: var(--sun);
}
a.pt-contact-row:hover {
  color: var(--sun);
}
.pt-contact-plain {
  font-weight: 500;
  color: rgba(255, 255, 255, 0.85);
}

/* ===== TRUST ===== */
.pt-trust {
  padding: 80px 0;
  background: #fff;
}
.pt-trust-grid {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  gap: 24px;
}
.pt-trust-item {
  padding: 28px 24px;
  background: #fff;
  border: 1px solid #e3dccb;
  border-radius: 18px;
  box-shadow: var(--shadow-sm);
  transition: transform 0.3s ease, box-shadow 0.3s ease;
}
.pt-trust-item:hover {
  transform: translateY(-6px);
  box-shadow: var(--shadow-md);
}
.pt-trust-icon {
  display: grid;
  place-items: center;
  width: 50px;
  height: 50px;
  margin-bottom: 16px;
  color: var(--navy);
  background: var(--sun);
  border-radius: 15px;
  box-shadow: 0 6px 14px rgba(245, 185, 74, 0.45);
}
.pt-trust-title {
  margin: 0 0 8px;
  font-size: 1.1rem;
  font-weight: 600;
  color: var(--navy);
}
.pt-trust-text {
  margin: 0;
  line-height: 1.6;
  color: var(--muted);
}

/* ===== RESPONSIVE ===== */
@media (max-width: 1024px) {
  .pt-layout {
    grid-template-columns: 1fr;
  }
  .pt-side {
    position: static;
  }
  .pt-trust-grid {
    grid-template-columns: repeat(2, 1fr);
  }
}
@media (max-width: 768px) {
  .pt-hero {
    min-height: 50vh;
    padding: 150px 0 100px;
  }
  .pt-card {
    padding: 28px 22px 26px;
  }
  .pt-trust {
    padding: 60px 0;
  }
}
@media (max-width: 560px) {
  .pt-form {
    grid-template-columns: 1fr;
  }
  .pt-trust-grid {
    grid-template-columns: 1fr;
  }
}
@media (prefers-reduced-motion: reduce) {
  .pt-submit,
  .pt-btn,
  .pt-trust-item {
    transition: none;
  }
  .pt-submit:hover,
  .pt-btn:hover,
  .pt-trust-item:hover {
    transform: none;
  }
}
</style>