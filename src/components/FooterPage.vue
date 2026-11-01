<template>
    <div>
        <!-- Footer -->
        <footer class="footer-container" aria-label="Site footer">
            <!-- Wave divider: sits on top edge of the footer -->
            <svg class="footer-wave" viewBox="0 0 1440 80" preserveAspectRatio="none" aria-hidden="true">
                <path d="M0,40 C240,90 480,0 720,36 C960,72 1200,10 1440,44 L1440,80 L0,80 Z" fill="#0b2a4a" />
                <path d="M0,52 C260,86 500,18 740,48 C980,78 1220,26 1440,58 L1440,80 L0,80 Z" fill="#0b2a4a" opacity="0.55" />
            </svg>

            <div class="footer-content">
                <!-- Brand row -->
                <div class="brand-row">
                    <div class="brand-block">
                        <strong class="title-footer">{{ tagline }}</strong>
                        <p class="brand-blurb">
                            From the hill country mists of Ella to the golden shores of Mirissa,
                            we plan every journey across the island with local guides and honest advice.
                        </p>
                    </div>
                    <div aria-label="Social media links" class="social-links">
                        <button
                            v-for="icon in icons"
                            :key="icon.name"
                            class="social-btn"
                            @click="navigate(icon.link)"
                            :aria-label="`Visit our ${icon.name} page`"
                            :title="icon.name"
                        >
                            <img :src="icon.icon" :alt="`${icon.name} icon`" width="24" height="24" />
                        </button>
                    </div>
                </div>

                <div class="gold-rule" aria-hidden="true"></div>

                <!-- Link columns -->
                <div class="footer-columns">
                    <nav class="footer-col" aria-label="Quick links">
                        <h3 class="col-title">Explore</h3>
                        <ul>
                            <li v-for="item in quickLinks" :key="item.label">
                                <a :href="item.href">{{ item.label }}</a>
                            </li>
                        </ul>
                    </nav>

                    <nav class="footer-col" aria-label="Popular destinations">
                        <h3 class="col-title">Popular destinations</h3>
                        <ul>
                            <li v-for="place in destinations" :key="place.label">
                                <a :href="place.href">{{ place.label }}</a>
                            </li>
                        </ul>
                    </nav>

                    <div class="footer-col contact-info">
                        <h3 class="col-title">Talk to a travel expert</h3>
                        <ul class="contact-list" aria-label="Contact information">
                            <li>
                                <span class="contact-icon" aria-hidden="true">&#128222;</span>
                                <span><a :href="`tel:${phoneRaw}`">{{ phoneDisplay }}</a> (Hot Line)</span>
                            </li>
                            <li>
                                <span class="contact-icon" aria-hidden="true">&#9993;</span>
                                <span><a :href="`mailto:${email}`">{{ email }}</a></span>
                            </li>
                            <li>
                                <span class="contact-icon" aria-hidden="true">&#128205;</span>
                                <span>{{ address }}</span>
                            </li>
                            <li>
                                <span class="contact-icon" aria-hidden="true">&#128339;</span>
                                <span>{{ openHours }}</span>
                            </li>
                        </ul>
                    </div>
                </div>

                <!-- Logos: your brand + optional partner / accreditation logos -->
                <div class="logos-container">
                    <div class="logos-grid">
                        <div class="logo-item">
                            <img :src="logo" :alt="`${brandName} Logo`" width="200" height="150" />
                        </div>
                        <div class="logo-item" v-for="partner in partners" :key="partner.name">
                            <img :src="partner.src" class="responsive-logo" :alt="partner.name" width="200" height="150" />
                        </div>
                    </div>
                </div>

                <div class="bottom-bar">
                    <div class="policy-links">
                        <a href="#" @click.prevent="showPrivacyPolicy = true" class="privacy-link">Privacy Policy</a> |
                        <a href="#" @click.prevent="showTermsOfService = true" class="privacy-link">Terms of Service</a>
                    </div>
                    <div class="footer-text">
                        At {{ brandName }}, we are committed to helping travellers discover great destinations, plan with
                        confidence and travel responsibly.
                        <br />Copyright © {{ currentYear }} {{ brandName }}. All rights reserved.
                    </div>
                    <button class="to-top" @click="scrollToTop" aria-label="Back to top" title="Back to top">&#8593;</button>
                </div>
            </div>
        </footer>

        <!-- Privacy Policy Modal -->
        <div class="modal-overlay" :class="{ active: showPrivacyPolicy }" @click="closeModalOnOverlay">
            <div class="modal" style="max-width: 800px;" @click.stop>
                <div class="modal-header">
                    <h2 class="modal-title">Privacy Policy</h2>
                </div>
                <div class="modal-content">
                    <p>
                        At {{ brandName }}, we are committed to protecting your privacy and
                        ensuring the security of your personal information. This Privacy Policy explains how we collect,
                        use, disclose, and safeguard the information you provide when using our website, booking
                        platform, and services. By accessing or using our services, you consent to the practices described in this policy.
                    </p>

                    <h4 class="policy-heading">1. Information We Collect</h4>
                    <strong>a. Personal Information</strong>
                    <ul>
                        <li>Full Name</li>
                        <li>Email Address</li>
                        <li>Phone Number</li>
                        <li>Address</li>
                        <li>Travel Preferences and Booking Details</li>
                    </ul>
                    <strong>b. Payment Information</strong>
                    <ul>
                        <li>Payment Card Details</li>
                        <li>Billing Address</li>
                        <li>Transaction History</li>
                    </ul>
                    <p>We do not store payment details directly on our servers. Payments are securely processed through
                        trusted third-party payment gateways.</p>

                    <h4 class="policy-heading">2. How We Use Your Information</h4>
                    <ul>
                        <li>Booking and Service Delivery</li>
                        <li>Personalization</li>
                        <li>Communication</li>
                        <li>Marketing</li>
                        <li>Security</li>
                        <li>Research and Improvement</li>
                    </ul>

                    <h4 class="policy-heading">3. Information Sharing and Disclosure</h4>
                    <strong>a. Service Providers</strong>
                    <ul>
                        <li>Hotels, Transport and Tour Operators (to fulfil your booking)</li>
                        <li>Payment Processing</li>
                        <li>Data Analysis</li>
                        <li>Platform Maintenance</li>
                        <li>Email and Communication</li>
                    </ul>
                    <strong>b. Legal Requirements</strong>
                    <p>We may disclose your information if required by law, legal process, or regulatory authorities.</p>

                    <h4 class="policy-heading">4. Data Security</h4>
                    <p>We use SSL encryption and other technical measures to protect your data. However, no platform can
                        guarantee 100% security.</p>

                    <h4 class="policy-heading">5. Retention of Information</h4>
                    <p>We retain your information only as long as necessary for operational and legal purposes.</p>

                    <h4 class="policy-heading">6. Your Rights</h4>
                    <ul>
                        <li>Access</li>
                        <li>Correction</li>
                        <li>Deletion</li>
                        <li>Opt-Out</li>
                    </ul>

                    <h4 class="policy-heading">7. Cookies and Tracking Technologies</h4>
                    <p>We use cookies to improve your browsing experience. You can manage cookie settings in your browser.
                    </p>

                    <h4 class="policy-heading">8. Third-Party Links</h4>
                    <p>We are not responsible for third-party privacy practices. Please review their policies.</p>

                    <h4 class="policy-heading">9. Children's Privacy</h4>
                    <p>Our services are not intended for individuals under 13 without parental consent.</p>

                    <h4 class="policy-heading">10. Changes to the Privacy Policy</h4>
                    <p>We may update this policy periodically. Please review it regularly for changes.</p>

                    <h4 class="policy-heading">11. Contact Us</h4>
                    <p>Tel No:<a :href="`tel:${phoneRaw}`"> {{ phoneDisplay }}</a> (Hot Line)</p>
                    <p>Email:<a :href="`mailto:${email}`"> {{ email }}</a></p>
                </div>
                <div class="modal-actions">
                    <button class="btn btn-text" @click="showPrivacyPolicy = false" aria-label="Close privacy policy dialog">Close</button>
                </div>
            </div>
        </div>

        <!-- Terms of Service Modal -->
        <div class="modal-overlay" :class="{ active: showTermsOfService }" @click="closeModalOnOverlay">
            <div class="modal" style="max-width: 600px;" @click.stop>
                <div class="modal-header">
                    <h2 class="modal-title">Terms & Conditions</h2>
                </div>
                <div class="modal-content">
                    <h4>Welcome to {{ siteDomain }}!</h4>
                    <p>
                        These terms and conditions outline the rules and regulations for the use of our website. By
                        accessing this website, we assume you accept these terms and conditions. Do not continue to use
                        {{ siteDomain }} if you do not agree to all the terms and conditions stated on this page.
                    </p>

                    <h4 class="policy-heading">1. Use of Site</h4>
                    <ul>
                        <li>Use the site for any illegal activities or violate any local, state, or international laws.</li>
                        <li>Attempt to interfere with the site's security features or its operation.</li>
                        <li>Use the site to distribute harmful content, such as malware or spam.</li>
                    </ul>

                    <h4 class="policy-heading">2. Intellectual Property Rights</h4>
                    <p>
                        All content on {{ siteDomain }}, including text, graphics, logos, photographs, and any other material, is the
                        property of {{ brandName }} (or its licensors) and is protected by copyright and other
                        intellectual property rights. Unauthorized use or reproduction is prohibited.
                    </p>
                    <ul>
                        <li>You may not modify, distribute, or reproduce any content from this site without our express
                            written consent.</li>
                        <li>Any trademarks or logos displayed on this site are the property of {{ brandName }}
                            and may not be used without permission.</li>
                    </ul>

                    <h4 class="policy-heading">3. Privacy Policy</h4>
                    <p>
                        Your privacy is very important to us. Please review our Privacy Policy to understand our practices
                        regarding personal information.
                    </p>
                    <ul>
                        <li>We use SSL encryption and industry-standard security measures to protect your data.</li>
                        <li>We will not share your personal information with third parties without your consent, except as
                            required by law or as needed to fulfil your booking.</li>
                    </ul>

                    <h4 class="policy-heading">4. Links to Third-Party Websites</h4>
                    <p>
                        {{ siteDomain }} may include links to third-party websites that are not controlled by us. We do not:
                    </p>
                    <ul>
                        <li>Endorse the content of these third-party websites.</li>
                        <li>Assume any responsibility for the content or practices of third-party websites.</li>
                    </ul>
                    <p>You access these links at your own risk.</p>

                    <h4 class="policy-heading">5. Bookings and Payment Terms</h4>
                    <ul>
                        <li>Payments are securely processed through trusted third-party gateways.</li>
                        <li>We do not store payment details directly on our servers.</li>
                        <li>Any cancellations, changes or refunds are governed by our Cancellation and Refund Policy.</li>
                        <li>Prices, itineraries and availability may change until a booking is confirmed.</li>
                    </ul>

                    <h4 class="policy-heading">6. Limitation of Liability</h4>
                    <p>
                        {{ brandName }} is not liable for any damages or losses related to the use or
                        inability to use the website. This includes, but is not limited to:
                    </p>
                    <ul>
                        <li>Errors, interruptions, or technical issues on the site.</li>
                        <li>Unauthorized access to user data.</li>
                        <li>Loss of data or profits arising from the use of the site.</li>
                        <li>Changes or disruptions to travel caused by weather, transport or other events beyond our control.</li>
                    </ul>

                    <h4 class="policy-heading">7. Changes to Terms</h4>
                    <p>
                        We reserve the right to update or change these terms and conditions at any time without prior
                        notice.
                    </p>
                    <ul>
                        <li>Any changes will be posted on this page.</li>
                        <li>Your continued use of the site after changes are posted constitutes your acceptance of the
                            updated terms.</li>
                    </ul>

                    <h4 class="policy-heading">8. Governing Law</h4>
                    <p>
                        These terms and conditions are governed by and construed in accordance with the laws of {{ governingLaw }}.
                    </p>
                    <ul>
                        <li>Any disputes will be resolved under these laws.</li>
                        <li>Any legal action arising out of the use of this site shall be brought exclusively in the courts
                            of {{ governingLaw }}.</li>
                    </ul>

                    <h4 class="policy-heading">9. Contact Information</h4>
                    <p>
                        If you have any questions about these Terms and Conditions, please contact us at:
                    </p>
                    <ul>
                        <li><p>Tel No:<a :href="`tel:${phoneRaw}`"> {{ phoneDisplay }}</a> (Hot Line)</p></li>
                        <li><p>Email:<a :href="`mailto:${email}`"> {{ email }}</a></p></li>
                    </ul>
                </div>
                <div class="modal-actions">
                    <button class="btn btn-text" @click="showTermsOfService = false">Close</button>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    name: 'FooterPage',
    data() {
        return {
            // ---- EDIT THESE: your tourism brand details ----
            brandName: 'Avatar Tours',
            siteDomain: 'yourtourismsite.com',
            tagline: 'Discover More. Travel Better.',
            phoneRaw: '+94 77 680 9002',
            phoneDisplay: '+94 77 680 9002',
            email: 'lankaroundtrip@gmail.com',
            address: 'No. 350/2, Ambalan Mountain, Kapuhenpola, Galle, Sri Lanka',
            openHours: 'Mon – Sat, 8:30 am – 6:00 pm',
            governingLaw: 'Sri Lanka',
            currentYear: new Date().getFullYear(),
            // -------------------------------------------------
            logo: require('@/assets/Logo.png'),
            // Optional partner / accreditation logos, e.g.:
            // { name: 'Tourism Board', src: require('@/assets/partner1.png') }
            partners: [],
            // Update the hrefs to match your router paths
            quickLinks: [
                { label: 'Home', href: '/' },
                { label: 'Tour packages', href: '/packages' },
                { label: 'Day trips', href: '/day-trips' },
                { label: 'Hotels & stays', href: '/stays' },
                { label: 'Plan my trip', href: '/plan' },
                { label: 'Contact us', href: '/contact' },
            ],
            destinations: [
                { label: 'Sigiriya & Dambulla', href: '/destinations/sigiriya' },
                { label: 'Kandy & Nuwara Eliya', href: '/destinations/hill-country' },
                { label: 'Ella & Haputale', href: '/destinations/ella' },
                { label: 'Galle & Mirissa', href: '/destinations/south-coast' },
                { label: 'Yala & Udawalawe', href: '/destinations/safari' },
                { label: 'Trincomalee & Arugam Bay', href: '/destinations/east-coast' },
            ],
            icons: [
                {
                    name: 'instagram',
                    link: 'https://www.instagram.com/',
                    icon: require('@/assets/social media icon/instagram.png')
                },
                {
                    name: 'facebook',
                    link: 'https://www.facebook.com/',
                    icon: require('@/assets/social media icon/facebook.png')
                },
                {
                    name: 'youtube',
                    link: 'https://www.youtube.com/',
                    icon: require('@/assets/social media icon/youtube.png')
                },
                {
                    name: 'tiktok',
                    link: 'https://www.tiktok.com/',
                    icon: require('@/assets/social media icon/tiktok.png')
                },
            ],
            showPrivacyPolicy: false,
            showTermsOfService: false,
        };
    },
    methods: {
        navigate(link) {
            window.open(link, '_blank');
        },
        scrollToTop() {
            window.scrollTo({ top: 0, behavior: 'smooth' });
        },
        closeModalOnOverlay(event) {
            if (event.target.classList.contains('modal-overlay')) {
                this.showPrivacyPolicy = false;
                this.showTermsOfService = false;
            }
        },
        handleEscape(e) {
            if (e.key === 'Escape') {
                this.showPrivacyPolicy = false;
                this.showTermsOfService = false;
            }
        }
    },
    mounted() {
        // Add keyboard event listener for Escape key
        document.addEventListener('keydown', this.handleEscape);
    },
    beforeUnmount() {
        document.removeEventListener('keydown', this.handleEscape);
    }
};
</script>

<style scoped>
* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
    font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
}

/* ===== INDIAN OCEAN BLUE + SAPPHIRE GOLD THEME ===== */
.footer-container {
    --blue-900: #0b2a4a;
    --blue-800: #08203a;
    --blue-700: #1565c0;
    --blue-500: #1e88e5;
    --blue-200: #8ec9ff;
    --gold: #f2b33d;
    --ink-on-blue: #eaf4ff;

    position: relative;
    background-color: var(--blue-900);
    background-image:
        radial-gradient(circle at 12% 0%, rgba(30, 136, 229, 0.28), transparent 45%),
        radial-gradient(circle at 92% 100%, rgba(242, 179, 61, 0.10), transparent 40%),
        linear-gradient(180deg, var(--blue-900) 0%, var(--blue-800) 100%);
    color: var(--ink-on-blue);
    padding: 70px 20px 24px;
    margin-top: 80px;
    text-align: left;
}

/* Wave sits on the top edge, bleeding up into the page above */
.footer-wave {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 60px;
    transform: translateY(-99%);
    display: block;
    pointer-events: none;
}

.footer-content {
    max-width: 1200px;
    margin: 0 auto;
}

/* ----- Brand row ----- */
.brand-row {
    display: flex;
    justify-content: space-between;
    align-items: center;
    gap: 24px;
    flex-wrap: wrap;
}

.brand-block {
    max-width: 560px;
}

.title-footer {
    font-family: Georgia, 'Times New Roman', serif;
    font-size: 30px;
    letter-spacing: 0.3px;
    display: block;
    color: #ffffff;
}

.brand-blurb {
    margin-top: 10px;
    font-size: 15px;
    line-height: 1.7;
    color: var(--ink-on-blue);
    opacity: 0.85;
}

.gold-rule {
    height: 3px;
    width: 100%;
    margin: 28px 0 30px;
    border-radius: 3px;
    background: linear-gradient(90deg, var(--gold) 0%, rgba(242, 179, 61, 0.35) 35%, rgba(142, 201, 255, 0.15) 100%);
}

/* ----- Social ----- */
.social-links {
    display: flex;
    justify-content: center;
    gap: 14px;
}

.social-btn {
    background: #ffffff;
    border: 2px solid transparent;
    cursor: pointer;
    padding: 9px;
    border-radius: 50%;
    transition: background-color 0.3s ease, transform 0.2s ease, border-color 0.3s ease;
}

.social-btn:hover {
    background-color: var(--blue-200);
    border-color: var(--gold);
    transform: translateY(-3px);
}

.social-btn:focus-visible,
.privacy-link:focus-visible,
.footer-col a:focus-visible,
.to-top:focus-visible {
    outline: 3px solid var(--blue-200);
    outline-offset: 2px;
}

.social-btn img {
    width: 24px;
    height: 24px;
    display: block;
}

/* ----- Link columns ----- */
.footer-columns {
    display: grid;
    grid-template-columns: 1fr 1.2fr 1.5fr;
    gap: 40px;
}

.col-title {
    font-family: Georgia, 'Times New Roman', serif;
    font-size: 18px;
    font-weight: 600;
    color: #ffffff;
    margin-bottom: 14px;
    padding-bottom: 8px;
    border-bottom: 2px solid var(--gold);
    display: inline-block;
}

.footer-col ul {
    list-style: none;
}

.footer-col li {
    margin-bottom: 9px;
    font-size: 15px;
    line-height: 1.5;
}

.footer-col a {
    color: var(--ink-on-blue);
    text-decoration: none;
    transition: color 0.25s ease, padding-left 0.25s ease;
}

.footer-col a:hover {
    color: var(--gold);
    padding-left: 4px;
}

.contact-info {
    font-size: 16px;
    color: var(--ink-on-blue);
}

.contact-info a {
    color: var(--blue-200);
    text-decoration: none;
}

.contact-info a:hover {
    color: #ffffff;
    text-decoration: underline;
    padding-left: 0;
}

.contact-list li {
    display: flex;
    align-items: flex-start;
    gap: 12px;
}

.contact-icon {
    flex: 0 0 34px;
    height: 34px;
    display: flex;
    align-items: center;
    justify-content: center;
    border-radius: 50%;
    background: rgba(242, 179, 61, 0.16);
    color: var(--gold);
    font-size: 15px;
}

.contact-list li > span:last-child {
    padding-top: 6px;
}

/* ----- Logos ----- */
.logos-container {
    width: 100%;
    background: white;
    padding: 20px;
    border-radius: 14px;
    border-top: 4px solid var(--gold);
    box-shadow: 0 6px 18px rgba(0, 0, 0, 0.3);
    margin: 36px 0 20px;
}

.logos-grid {
    display: flex;
    flex-wrap: wrap;
    justify-content: center;
    gap: 20px;
}

.logo-item {
    flex: 0 0 auto;
    display: flex;
    align-items: center;
    justify-content: center;
    text-align: center;
}

.responsive-logo {
    max-width: 100%;
    height: auto;
    margin: 0 auto;
    display: block;
    padding: 5px;
    max-height: 150px;
}

/* ----- Bottom bar ----- */
.bottom-bar {
    position: relative;
    text-align: center;
    padding-top: 10px;
}

.policy-links {
    margin: 10px 0;
}

.privacy-link {
    color: var(--ink-on-blue);
    font-size: 14px;
    text-decoration: none;
    font-weight: 500;
    padding: 4px 8px;
    border-radius: 4px;
    transition: background-color 0.3s ease, color 0.3s ease;
    display: inline-block;
}

.privacy-link:hover {
    background-color: rgba(255, 255, 255, 0.14);
    color: var(--gold);
    text-decoration: underline;
}

.footer-text {
    font-size: 14px;
    line-height: 1.6;
    margin-top: 12px;
    color: var(--ink-on-blue);
    opacity: 0.9;
    max-width: 800px;
    margin-left: auto;
    margin-right: auto;
}

.to-top {
    position: absolute;
    right: 0;
    bottom: 0;
    width: 42px;
    height: 42px;
    border-radius: 50%;
    border: 2px solid var(--gold);
    background: transparent;
    color: var(--gold);
    font-size: 20px;
    cursor: pointer;
    transition: background-color 0.3s ease, color 0.3s ease, transform 0.2s ease;
}

.to-top:hover {
    background: var(--gold);
    color: var(--blue-900);
    transform: translateY(-3px);
}

/* Modal/Dialog Styles */
.modal-overlay {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background-color: rgba(5, 20, 40, 0.6);
    display: none;
    justify-content: center;
    align-items: center;
    z-index: 1000;
}

.modal-overlay.active {
    display: flex;
}

.modal {
    background-color: white;
    border-radius: 8px;
    box-shadow: 0 4px 20px rgba(0, 0, 0, 0.25);
    max-width: 90%;
    max-height: 90vh;
    display: flex;
    flex-direction: column;
    text-align: left;
    color: #1f2933;
}

.modal-header {
    padding: 20px 24px 10px;
    border-bottom: 2px solid #1565c0;
}

.modal-title {
    font-size: 1.5rem;
    font-weight: 600;
    color: #0b2a4a;
}

.modal-content {
    padding: 20px 24px;
    overflow-y: auto;
    max-height: 500px;
    flex: 1;
}

.modal-content a {
    color: #1565c0;
}

.modal-actions {
    padding: 8px 16px 16px;
    display: flex;
    justify-content: flex-end;
    border-top: 1px solid #e0e0e0;
}

.btn {
    padding: 8px 16px;
    border-radius: 4px;
    border: none;
    cursor: pointer;
    font-weight: 500;
    font-size: 14px;
    transition: background-color 0.3s ease;
}

.btn-primary {
    background-color: #1976d2;
    color: white;
}

.btn-primary:hover {
    background-color: #1565c0;
}

.btn-text {
    background: none;
    color: #1565c0;
}

.btn-text:hover {
    background-color: rgba(21, 101, 192, 0.1);
}

.policy-heading {
    text-align: left;
    font-weight: bold;
    margin: 16px 0 8px;
    color: #0b2a4a;
}

.modal-content p, .modal-content ul {
    margin-bottom: 12px;
    line-height: 1.6;
}

.modal-content ul {
    padding-left: 20px;
}

.modal-content strong {
    display: block;
    margin-top: 10px;
}

/* Responsive Design */
@media (max-width: 900px) {
    .footer-columns {
        grid-template-columns: 1fr 1fr;
    }

    .contact-info {
        grid-column: 1 / -1;
    }
}

@media (max-width: 768px) {
    .brand-row {
        flex-direction: column;
        text-align: center;
    }

    .logos-grid {
        flex-direction: column;
        align-items: center;
    }

    .logo-item {
        width: 100%;
        max-width: 250px;
    }

    .social-links {
        gap: 10px;
    }

    .title-footer {
        font-size: 24px;
    }

    .modal {
        width: 95%;
        max-height: 85vh;
    }

    .to-top {
        position: static;
        margin-top: 16px;
    }
}

@media (max-width: 480px) {
    .footer-container {
        padding: 60px 14px 18px;
    }

    .footer-columns {
        grid-template-columns: 1fr;
        gap: 28px;
    }

    .title-footer {
        font-size: 21px;
    }

    .contact-info {
        font-size: 14px;
    }
}

@media (prefers-reduced-motion: reduce) {
    .social-btn,
    .to-top,
    .footer-col a {
        transition: none;
    }

    .social-btn:hover,
    .to-top:hover {
        transform: none;
    }
}
</style>