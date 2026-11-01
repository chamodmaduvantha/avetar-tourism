<template>
    <header class="lk-nav" :class="{ 'is-scrolled': scrolled, 'is-over-hero': overHero && !scrolled }">
        <!-- Top info strip (collapses once the page scrolls) -->
        <div class="lk-topbar">
            <div class="lk-topbar__inner">
                <div class="lk-topbar__group">
                    <a :href="`tel:${phoneRaw}`" class="lk-topbar__item">
                        <i class="material-icons">phone</i>{{ phoneDisplay }}
                    </a>
                    <a :href="`mailto:${email}`" class="lk-topbar__item">
                        <i class="material-icons">email</i>{{ email }}
                    </a>
                </div>
                <div class="lk-topbar__group">
                    <span class="lk-topbar__item lk-topbar__item--plain">
                        <i class="material-icons">schedule</i>{{ openHours }}
                    </span>
                    <span class="lk-topbar__item lk-topbar__item--plain lk-topbar__badge">
                        <i class="material-icons">verified</i>{{ topbarNote }}
                    </span>
                </div>
            </div>
        </div>

        <div class="lk-nav__bar">
            <!-- Brand -->
            <a href="/" class="lk-brand" aria-label="Ceylon Escapes home">
                <img :src="logo" class="lk-brand__mark" alt="" />
                <span class="lk-brand__text">
                    <span class="lk-brand__name">Ceylon Escapes</span>
                    <span class="lk-brand__tag">Discover Sri Lanka</span>
                </span>
            </a>

            <!-- Desktop links -->
            <nav class="lk-links" aria-label="Main navigation">
                <ul class="lk-links__list">
                    <li
                        v-for="(item, i) in navItems"
                        :key="item.text"
                        class="lk-item"
                        :class="{ 'has-menu': item.children, 'is-open': openMenu === i }"
                        @mouseenter="item.children && (openMenu = i)"
                        @mouseleave="item.children && (openMenu = -1)"
                    >
                        <a
                            v-if="!item.children"
                            :href="item.link"
                            class="lk-link"
                            :class="{ 'is-active': isActive(item.link) }"
                        >
                            {{ item.text }}
                        </a>

                        <template v-else>
                            <button
                                class="lk-link lk-link--button"
                                :aria-expanded="openMenu === i ? 'true' : 'false'"
                                @click="openMenu = openMenu === i ? -1 : i"
                                @keydown.esc="openMenu = -1"
                            >
                                {{ item.text }}
                                <i class="material-icons lk-link__chev">expand_more</i>
                            </button>

                            <div class="lk-menu" role="menu">
                                <a
                                    v-for="child in item.children"
                                    :key="child.text"
                                    :href="child.link"
                                    class="lk-menu__row"
                                    role="menuitem"
                                >
                                    <span class="lk-menu__icon"><i class="material-icons">{{ child.icon }}</i></span>
                                    <span class="lk-menu__copy">
                                        <strong>{{ child.text }}</strong>
                                        <small>{{ child.desc }}</small>
                                    </span>
                                    <i class="material-icons lk-menu__go">arrow_forward</i>
                                </a>
                            </div>
                        </template>
                    </li>
                </ul>
            </nav>

            <!-- Actions -->
            <div class="lk-actions">
                <a href="/contact" class="lk-ghost">
                    <i class="material-icons">phone</i>
                    <span>Contact</span>
                </a>
                <button class="lk-ghost" @click="openInquiryPopup">
                    <i class="material-icons">help_outline</i>
                    <span>Inquiry</span>
                </button>
                <a href="/plan-your-trip" class="lk-nav-cta">
                    Plan my trip
                    <i class="material-icons">flight_takeoff</i>
                </a>
            </div>

            <!-- Mobile toggle -->
            <button
                class="lk-burger"
                :class="{ 'is-open': isMenuOpen }"
                :aria-expanded="isMenuOpen ? 'true' : 'false'"
                aria-label="Toggle menu"
                @click="toggleMobileMenu"
            >
                <span></span><span></span><span></span>
            </button>
        </div>

        <!-- Mobile drawer -->
        <transition name="lk-fade">
            <div v-if="isMenuOpen" class="lk-scrim" @click="closeMobileMenu"></div>
        </transition>

        <aside class="lk-drawer" :class="{ 'is-open': isMenuOpen }" :aria-hidden="!isMenuOpen">
            <div class="lk-drawer__head">
                <span class="lk-drawer__brand">
                    <img :src="logo" class="lk-drawer__logo" alt="" />
                    <span class="lk-brand__name lk-brand__name--light">Ceylon Escapes</span>
                </span>
                <button class="lk-drawer__close" aria-label="Close menu" @click="closeMobileMenu">
                    <i class="material-icons">close</i>
                </button>
            </div>

            <div class="lk-drawer__body">
                <ul class="lk-drawer__list">
                    <li v-for="(item, i) in navItems" :key="item.text">
                        <a v-if="!item.children" :href="item.link" class="lk-drawer__link" @click="closeMobileMenu">
                            {{ item.text }}
                        </a>
                        <template v-else>
                            <button class="lk-drawer__link lk-drawer__link--toggle" @click="toggleAccordion(i)">
                                {{ item.text }}
                                <i class="material-icons" :class="{ 'is-flipped': accordion === i }">expand_more</i>
                            </button>
                            <ul class="lk-drawer__sub" :class="{ 'is-open': accordion === i }">
                                <li v-for="child in item.children" :key="child.text">
                                    <a :href="child.link" @click="closeMobileMenu">
                                        <i class="material-icons">{{ child.icon }}</i>{{ child.text }}
                                    </a>
                                </li>
                            </ul>
                        </template>
                    </li>
                </ul>

                <div class="lk-drawer__actions">
                    <a href="/plan-your-trip" class="lk-nav-cta lk-nav-cta--block" @click="closeMobileMenu">Plan my trip</a>
                    <button class="lk-drawer__outline" @click="openInquiryPopup(); closeMobileMenu();">
                        <i class="material-icons">help_outline</i> Inquiry
                    </button>
                    <a href="/contact" class="lk-drawer__outline" @click="closeMobileMenu">
                        <i class="material-icons">phone</i> Contact
                    </a>
                </div>

                <div class="lk-drawer__foot">
                    <span><i class="material-icons">email</i> {{ email }}</span>
                    <span><i class="material-icons">phone</i> {{ phoneDisplay }}</span>
                </div>
            </div>

            <!-- Decorative wave at the foot of the drawer -->
            <svg class="lk-drawer__wave" viewBox="0 0 400 60" preserveAspectRatio="none" aria-hidden="true">
                <path d="M0,30 C80,60 140,0 220,28 C290,52 340,10 400,32 L400,60 L0,60 Z" fill="#38bdf8" opacity="0.18" />
                <path d="M0,42 C90,64 150,16 230,40 C300,60 350,28 400,44 L400,60 L0,60 Z" fill="#f2b33d" opacity="0.22" />
            </svg>
        </aside>
    </header>
</template>

<script>
export default {
    name: 'LankaNavBar',

    props: {
        /** Let the bar sit transparent over a hero image until the page scrolls. */
        overHero: { type: Boolean, default: false },
        /** URL opened in the inquiry popup window. */
        inquiryUrl: { type: String, default: '/inquiry' },
    },

    data() {
        return {
            logo: require('@/assets/Logo.png'),
            scrolled: false,
            isMenuOpen: false,
            openMenu: -1,
            accordion: -1,

            // ---- EDIT THESE: contact details shown in the top strip & drawer ----
            phoneRaw: '+94 77 680 9002',
            phoneDisplay: '+94 77 680 9002',
            email: 'lankaroundtrip@gmail.com',
            openHours: 'Mon – Sat, 8:30 am – 6:00 pm',
            topbarNote: 'Local guides · Free trip planning',
            // ---------------------------------------------------------------------

            navItems: [
                { text: 'Home', link: '/' },
                {
                    text: 'Destinations',
                    link: '/destinations',
                    children: [
                        { text: 'Sigiriya & Cultural Triangle', desc: 'Rock fortress, ancient cities', icon: 'temple_buddhist', link: '/destinations/sigiriya' },
                        { text: 'Kandy & Hill Country', desc: 'Tea estates and misty trains', icon: 'terrain', link: '/destinations/hill-country' },
                        { text: 'Southern Coast', desc: 'Galle Fort, Mirissa, Unawatuna', icon: 'beach_access', link: '/destinations/south-coast' },
                        { text: 'Yala & Wildlife', desc: 'Leopards, elephants, safaris', icon: 'pets', link: '/destinations/wildlife' },
                    ],
                },
                {
                    text: 'Experiences',
                    link: '/experiences',
                    children: [
                        { text: 'Tours & Packages', desc: 'Ready-made island itineraries', icon: 'luggage', link: '/tours' },
                        { text: 'Whale Watching', desc: 'Blue whales off Mirissa', icon: 'sailing', link: '/experiences/whales' },
                        { text: 'Food & Culture', desc: 'Cooking classes, festivals', icon: 'restaurant', link: '/experiences/culture' },
                    ],
                },
                { text: 'Stay', link: '/stay' },
                { text: 'Gallery', link: '/gallery' },
                { text: 'About', link: '/about' },
            ],
        };
    },

    methods: {
        isActive(link) {
            return link === '/'
                ? window.location.pathname === '/'
                : window.location.pathname.startsWith(link);
        },
        onScroll() {
            this.scrolled = window.scrollY > 24;
        },
        toggleMobileMenu() {
            this.isMenuOpen ? this.closeMobileMenu() : this.openMobile();
        },
        openMobile() {
            this.isMenuOpen = true;
            document.body.style.overflow = 'hidden';
        },
        closeMobileMenu() {
            this.isMenuOpen = false;
            this.accordion = -1;
            document.body.style.overflow = '';
        },
        toggleAccordion(i) {
            this.accordion = this.accordion === i ? -1 : i;
        },
        openInquiryPopup() {
            const w = 720;
            const h = 800;
            const left = Math.round((screen.width - w) / 2);
            const top = Math.round((screen.height - h) / 2);
            window.open(
                this.inquiryUrl,
                'LankaInquiry',
                `width=${w},height=${h},left=${left},top=${top},resizable=yes,scrollbars=yes,toolbar=no,menubar=no,location=no,status=no`
            );
        },
        onKeydown(e) {
            if (e.key === 'Escape') {
                this.openMenu = -1;
                if (this.isMenuOpen) this.closeMobileMenu();
            }
        },
        onDocClick(e) {
            if (!e.target.closest('.lk-item')) this.openMenu = -1;
        },
    },

    mounted() {
        this.onScroll();
        window.addEventListener('scroll', this.onScroll, { passive: true });
        document.addEventListener('keydown', this.onKeydown);
        document.addEventListener('click', this.onDocClick);
    },

    beforeUnmount() {
        window.removeEventListener('scroll', this.onScroll);
        document.removeEventListener('keydown', this.onKeydown);
        document.removeEventListener('click', this.onDocClick);
        document.body.style.overflow = '';
    },
};
</script>

<style>
@import url('https://fonts.googleapis.com/icon?family=Material+Icons');
@import url('https://fonts.googleapis.com/css2?family=Fraunces:opsz,wght@9..144,600;9..144,700&family=Manrope:wght@500;600;700&display=swap');

.lk-nav {
    --ocean-900: #072a52;
    --ocean-700: #0b4a96;
    --ocean-500: #1475e1;
    --lagoon: #38bdf8;
    --foam: #e8f4ff;
    --gold: #f2b33d;
    --gold-deep: #d99512;
    --ink: #0c2340;
    --muted: #5b7191;

    position: sticky;
    top: 0;
    z-index: 1000;
    width: 100%;
    font-family: 'Manrope', system-ui, sans-serif;
    background: #fff;
    transition: background-color .3s ease, box-shadow .3s ease;
}

.lk-nav *,
.lk-nav *::before,
.lk-nav *::after { box-sizing: border-box; }

.lk-nav .material-icons { font-size: 20px; line-height: 1; }

/* Gold + lagoon ribbon along the bottom edge */
.lk-nav::after {
    content: '';
    position: absolute;
    left: 0; right: 0; bottom: 0;
    height: 3px;
    background: linear-gradient(90deg, var(--ocean-700) 0%, var(--lagoon) 35%, var(--gold) 70%, var(--ocean-700) 100%);
    opacity: 1;
    transition: opacity .3s ease;
}

.lk-nav.is-scrolled { box-shadow: 0 10px 30px rgba(7, 42, 82, .14); }

/* ---------- Top info strip ---------- */
.lk-topbar {
    background: linear-gradient(90deg, var(--ocean-900), var(--ocean-700));
    color: #d6ebff;
    font-size: .8rem;
    max-height: 44px;
    overflow: hidden;
    transition: max-height .35s ease, opacity .25s ease;
}
.lk-nav.is-scrolled .lk-topbar { max-height: 0; opacity: 0; }

.lk-topbar__inner {
    display: flex;
    align-items: center;
    justify-content: space-between;
    gap: 16px;
    max-width: 1320px;
    margin: 0 auto;
    padding: 8px 24px;
}
.lk-topbar__group { display: flex; align-items: center; gap: 22px; }
.lk-topbar__item {
    display: inline-flex;
    align-items: center;
    gap: 7px;
    color: inherit;
    text-decoration: none;
    font-weight: 600;
    transition: color .2s ease;
}
a.lk-topbar__item:hover { color: var(--gold); }
.lk-topbar__item .material-icons { font-size: 16px; color: var(--gold); }
.lk-topbar__badge {
    padding: 3px 12px 3px 8px;
    border-radius: 999px;
    background: rgba(255, 255, 255, .1);
}

/* ---------- Transparent state over a hero ---------- */
.lk-nav.is-over-hero {
    position: fixed;
    background: transparent;
}
.lk-nav.is-over-hero::after { opacity: 0; }
.lk-nav.is-over-hero .lk-topbar { background: rgba(7, 42, 82, .55); backdrop-filter: blur(6px); }
.lk-nav.is-over-hero .lk-brand__name,
.lk-nav.is-over-hero .lk-link,
.lk-nav.is-over-hero .lk-ghost { color: #fff; }
.lk-nav.is-over-hero .lk-brand__tag { color: rgba(255,255,255,.82); }
.lk-nav.is-over-hero .lk-burger span { background: #fff; }
.lk-nav.is-over-hero .lk-burger { background: rgba(255, 255, 255, .16); }

.lk-nav__bar {
    display: flex;
    align-items: center;
    gap: 28px;
    max-width: 1320px;
    margin: 0 auto;
    padding: 12px 24px 14px;
}

/* ---------- Brand ---------- */
.lk-brand {
    display: flex;
    align-items: center;
    gap: 12px;
    text-decoration: none;
    flex-shrink: 0;
}
.lk-brand__mark { height: 46px; width: auto; max-width: 140px; object-fit: contain; display: block; }
.lk-brand__text { display: flex; flex-direction: column; line-height: 1.1; padding-left: 12px; border-left: 2px solid var(--gold); }
.lk-brand__name {
    font-family: 'Fraunces', Georgia, serif;
    font-weight: 700;
    font-size: 1.3rem;
    color: var(--ocean-900);
    letter-spacing: -.01em;
}
.lk-brand__name--light { color: #fff; }
.lk-brand__tag { font-size: .72rem; font-weight: 600; color: var(--muted); margin-top: 3px; }

/* ---------- Links ---------- */
.lk-links { margin-left: auto; }
.lk-links__list { display: flex; align-items: center; gap: 2px; list-style: none; margin: 0; padding: 0; }
.lk-item { position: relative; }

.lk-link {
    display: inline-flex;
    align-items: center;
    gap: 4px;
    padding: 10px 14px;
    border: 0;
    background: none;
    font: 600 .95rem 'Manrope', sans-serif;
    color: var(--ink);
    text-decoration: none;
    cursor: pointer;
    border-radius: 10px;
    position: relative;
    transition: color .2s ease, background-color .2s ease;
}
.lk-link::after {
    content: '';
    position: absolute;
    left: 14px; right: 14px; bottom: 4px;
    height: 3px;
    background: var(--gold);
    border-radius: 3px;
    transform: scaleX(0);
    transform-origin: left;
    transition: transform .25s ease;
}
.lk-link:hover::after,
.lk-link.is-active::after,
.lk-item.is-open > .lk-link::after { transform: scaleX(1); }
.lk-link:hover { color: var(--ocean-700); }
.lk-nav.is-over-hero .lk-link:hover { color: var(--gold); }
.lk-link__chev { transition: transform .25s ease; font-size: 18px !important; }
.lk-item.is-open .lk-link__chev { transform: rotate(180deg); }

/* ---------- Dropdown ---------- */
.lk-menu {
    position: absolute;
    top: calc(100% + 10px);
    left: 50%;
    min-width: 340px;
    padding: 8px;
    background: #fff;
    border: 1px solid #dbe8f7;
    border-top: 3px solid var(--gold);
    border-radius: 16px;
    box-shadow: 0 24px 48px rgba(7, 42, 82, .2);
    opacity: 0;
    visibility: hidden;
    transform: translate(-50%, 8px);
    transition: opacity .2s ease, transform .2s ease, visibility .2s;
}
/* hover bridge so the menu doesn't close crossing the gap */
.lk-menu::before { content: ''; position: absolute; top: -16px; left: 0; right: 0; height: 16px; }
/* notch */
.lk-menu::after {
    content: '';
    position: absolute;
    top: -9px; left: 50%;
    width: 14px; height: 14px;
    background: var(--gold);
    transform: translateX(-50%) rotate(45deg);
    border-radius: 3px 0 0 0;
    z-index: -1;
}
.lk-item.is-open .lk-menu { opacity: 1; visibility: visible; transform: translate(-50%, 0); }

.lk-menu__row {
    display: flex;
    align-items: center;
    gap: 14px;
    padding: 10px 12px;
    border-radius: 12px;
    text-decoration: none;
    color: var(--ink);
    transition: background-color .15s ease;
}
.lk-menu__row:hover,
.lk-menu__row:focus-visible { background: var(--foam); }
.lk-menu__icon {
    display: grid;
    place-items: center;
    width: 42px; height: 42px;
    flex-shrink: 0;
    border-radius: 12px;
    background: var(--foam);
    color: var(--ocean-700);
    transition: background-color .2s ease, color .2s ease;
}
.lk-menu__row:hover .lk-menu__icon { background: var(--ocean-700); color: var(--gold); }
.lk-menu__copy { display: flex; flex-direction: column; flex: 1; }
.lk-menu__copy strong { font-size: .92rem; }
.lk-menu__copy small { font-size: .78rem; color: var(--muted); font-weight: 500; }
.lk-menu__go { font-size: 18px !important; color: var(--gold-deep); opacity: 0; transform: translateX(-6px); transition: opacity .2s ease, transform .2s ease; }
.lk-menu__row:hover .lk-menu__go { opacity: 1; transform: translateX(0); }

/* ---------- Actions ---------- */
.lk-actions { display: flex; align-items: center; gap: 6px; }
.lk-ghost {
    display: inline-flex;
    align-items: center;
    gap: 6px;
    padding: 10px 12px;
    border: 0;
    background: none;
    border-radius: 10px;
    font: 600 .9rem 'Manrope', sans-serif;
    color: var(--ink);
    text-decoration: none;
    cursor: pointer;
    transition: background-color .2s ease, color .2s ease;
}
.lk-ghost:hover { background: var(--foam); color: var(--ocean-700); }
.lk-nav.is-over-hero .lk-ghost:hover { background: rgba(255,255,255,.16); color: #fff; }

.lk-nav-cta {
    display: inline-flex;
    align-items: center;
    justify-content: center;
    gap: 8px;
    margin-left: 6px;
    padding: 12px 22px;
    border-radius: 999px;
    background: linear-gradient(135deg, #ffc857 0%, var(--gold) 55%, var(--gold-deep) 100%);
    color: var(--ocean-900);
    font: 800 .92rem 'Manrope', sans-serif;
    text-decoration: none;
    white-space: nowrap;
    box-shadow: 0 6px 16px rgba(217, 149, 18, .35);
    transition: transform .2s ease, box-shadow .2s ease, filter .2s ease;
}
.lk-nav-cta .material-icons { font-size: 18px; transition: transform .25s ease; }
.lk-nav-cta:hover { transform: translateY(-2px); box-shadow: 0 12px 24px rgba(217, 149, 18, .45); filter: brightness(1.04); }
.lk-nav-cta:hover .material-icons { transform: translate(3px, -3px); }
.lk-nav-cta--block { width: 100%; margin: 0; }

/* Focus */
.lk-nav a:focus-visible,
.lk-nav button:focus-visible { outline: 3px solid var(--lagoon); outline-offset: 2px; }

/* ---------- Burger ---------- */
.lk-burger {
    display: none;
    margin-left: auto;
    flex-direction: column;
    gap: 5px;
    padding: 12px;
    border: 0;
    border-radius: 12px;
    background: var(--foam);
    cursor: pointer;
}
.lk-burger span {
    width: 22px; height: 2.5px;
    background: var(--ocean-900);
    border-radius: 2px;
    transition: transform .3s ease, opacity .2s ease;
}
.lk-burger.is-open span:nth-child(1) { transform: translateY(7.5px) rotate(45deg); }
.lk-burger.is-open span:nth-child(2) { opacity: 0; }
.lk-burger.is-open span:nth-child(3) { transform: translateY(-7.5px) rotate(-45deg); }

/* ---------- Drawer ---------- */
.lk-scrim {
    position: fixed; inset: 0;
    background: rgba(7, 42, 82, .55);
    backdrop-filter: blur(4px);
    z-index: 1001;
}
.lk-fade-enter-active, .lk-fade-leave-active { transition: opacity .3s ease; }
.lk-fade-enter-from, .lk-fade-leave-to { opacity: 0; }

.lk-drawer {
    position: fixed;
    top: 0; right: 0; bottom: 0;
    width: min(88vw, 380px);
    display: flex;
    flex-direction: column;
    background:
        radial-gradient(circle at 100% 0%, rgba(56, 189, 248, .22), transparent 45%),
        var(--ocean-900);
    color: #fff;
    z-index: 1002;
    overflow: hidden;
    transform: translateX(100%);
    transition: transform .4s cubic-bezier(.4, 0, .2, 1);
}
.lk-drawer.is-open { transform: translateX(0); }

.lk-drawer__head {
    position: relative;
    z-index: 1;
    display: flex; align-items: center; justify-content: space-between;
    padding: 18px 22px;
    border-bottom: 2px solid var(--gold);
}
.lk-drawer__brand { display: flex; align-items: center; gap: 10px; }
.lk-drawer__logo { height: 38px; width: auto; max-width: 90px; object-fit: contain; background: #fff; border-radius: 10px; padding: 3px 6px; }
.lk-drawer__close {
    display: grid; place-items: center;
    width: 40px; height: 40px;
    border: 0; border-radius: 50%;
    background: rgba(255,255,255,.12);
    color: #fff; cursor: pointer;
    transition: background-color .2s ease, color .2s ease;
}
.lk-drawer__close:hover { background: var(--gold); color: var(--ocean-900); }
.lk-drawer__body { position: relative; z-index: 1; flex: 1; overflow-y: auto; padding: 14px 22px 70px; display: flex; flex-direction: column; gap: 22px; }

.lk-drawer__list, .lk-drawer__sub { list-style: none; margin: 0; padding: 0; }
.lk-drawer__link {
    display: flex; align-items: center; justify-content: space-between;
    width: 100%;
    padding: 15px 4px;
    border: 0;
    border-bottom: 1px solid rgba(255,255,255,.1);
    background: none;
    color: #fff;
    font: 600 1.05rem 'Manrope', sans-serif;
    text-decoration: none;
    text-align: left;
    cursor: pointer;
    transition: color .2s ease, padding-left .2s ease;
}
.lk-drawer__link:hover { color: var(--gold); padding-left: 10px; }
.lk-drawer__link .material-icons { transition: transform .25s ease; }
.lk-drawer__link .is-flipped { transform: rotate(180deg); }

.lk-drawer__sub { max-height: 0; overflow: hidden; transition: max-height .35s ease; }
.lk-drawer__sub.is-open { max-height: 320px; }
.lk-drawer__sub a {
    display: flex; align-items: center; gap: 12px;
    padding: 12px 8px 12px 14px;
    color: #bfe0ff;
    font-weight: 500; font-size: .95rem;
    text-decoration: none;
    border-left: 2px solid rgba(242, 179, 61, .45);
    margin-left: 6px;
}
.lk-drawer__sub a .material-icons { color: var(--gold); }
.lk-drawer__sub a:hover { color: #fff; background: rgba(255,255,255,.06); }

.lk-drawer__actions { display: flex; flex-direction: column; gap: 10px; }
.lk-drawer__outline {
    display: flex; align-items: center; justify-content: center; gap: 8px;
    padding: 13px;
    border: 1.5px solid rgba(255,255,255,.3);
    border-radius: 999px;
    background: none;
    color: #fff;
    font: 600 .95rem 'Manrope', sans-serif;
    text-decoration: none;
    cursor: pointer;
    transition: background-color .2s ease, border-color .2s ease;
}
.lk-drawer__outline:hover { background: rgba(255,255,255,.1); border-color: var(--gold); }

.lk-drawer__foot { margin-top: auto; display: flex; flex-direction: column; gap: 10px; font-size: .88rem; color: #bfe0ff; }
.lk-drawer__foot span { display: flex; align-items: center; gap: 10px; }
.lk-drawer__foot .material-icons { color: var(--gold); }

.lk-drawer__wave {
    position: absolute;
    left: 0; bottom: 0;
    width: 100%; height: 60px;
    pointer-events: none;
}

/* ---------- Responsive ---------- */
@media (max-width: 1180px) {
    .lk-ghost span { display: none; }
    .lk-ghost { padding: 10px; }
    .lk-topbar__badge { display: none; }
}
@media (max-width: 992px) {
    .lk-links, .lk-actions { display: none; }
    .lk-burger { display: flex; }
    .lk-brand__mark { height: 40px; }
    .lk-nav__bar { padding: 10px 16px 12px; }
    .lk-topbar__inner { padding: 7px 16px; justify-content: center; }
    .lk-topbar__group:last-child { display: none; }
}
@media (max-width: 560px) {
    .lk-topbar__item:nth-child(2) { display: none; }
}
@media (max-width: 420px) {
    .lk-brand__tag { display: none; }
    .lk-brand__name { font-size: 1.15rem; }
    .lk-brand__text { padding-left: 10px; }
}

@media (prefers-reduced-motion: reduce) {
    .lk-nav *, .lk-drawer { transition-duration: .01ms !important; }
}
</style>