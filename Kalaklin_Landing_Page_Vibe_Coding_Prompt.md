# Vibe Coding AI Prompt — Kalaklin Landing Page

> Copy everything below this line into your Vibe Coding AI tool (Lovable / Bolt / v0 / Cursor / Windsurf, etc.) as the build prompt.

---

## ROLE & OBJECTIVE

You are building a **single-page marketing landing page** for **Kalaklin**, an eco-friendly, WhatsApp-based subscription laundry pickup & delivery service operating in West Jakarta. This is a **pre-launch validation landing page** — its core business goal is to convert visitors into Instagram sign-ups via a short form, so the team can build an early-bird waitlist before the official launch.

Build a **fully responsive (mobile, tablet, desktop), production-quality, single-page website** using semantic HTML/React + Tailwind CSS (or the framework native to this tool). Follow the wireframe layout, design system, and content specifications below **exactly** — do not invent new sections, reorder sections, or change the copy unless explicitly marked as placeholder.

---

## 1. BRAND CONTEXT (for tone & copy fidelity — do not paraphrase provided copy)

- **Brand name meaning:** "Kala" (Sanskrit, "time") + "Clean" → punctuality + cleanliness.
- **Tagline:** "Bersih Bajunya, Hijau Buminya, Berseri Hatinya."
- **Positioning:** Modern, eco-friendly, plastic-free, hyperlocal (max 10 km radius), 100% operated via WhatsApp chat — no app download required.
- **Service area:** West Jakarta only — Kebon Jeruk, Tanjung Duren, Palmerah, Puri, Pesanggrahan, Kemanggisan.
- **5 USPs:** On-time guarantee (time-slot pickup with compensation if late), transparent WhatsApp tracking, 24/7 operations, 100% single-use-plastic-free (reusable canvas laundry bag), biodegradable/hypoallergenic detergent.
- **Target audience:** Urban residents 18–55 (students, office workers, housewives/families) in kos/apartments, time-poor, environmentally conscious.
- **Mascot:** "Kala" — a character mascot used throughout the page (treat mascot image assets as fixed/unmodified assets to be placed in designated slots — use placeholder illustration if asset not supplied).
- **Primary business goal of this page:** Drive sign-ups to the short lead form (Name + Instagram handle), which should feel like it leads to Instagram DM follow-up, not an app or paid checkout.

---

## 2. DESIGN SYSTEM

**Colors**
| Token | Hex | Usage |
|---|---|---|
| Primary Brand | `#00544D` (deep teal green) | Headings, nav text, body copy, logo color |
| Accent / CTA | `#FF7A45` (coral orange) | All CTA buttons, price highlights, promo callouts |
| Secondary Action / Link | `#3C85D4` (blue) | Text links (e.g., "Kebijakan Privasi", FAQ icon color) |
| Input/Card Background | `#E8EFED` (light muted green-gray) | Form input backgrounds, card fills |
| White | `#FFFFFF` | CTA button text, section backgrounds |

**Typography — Montserrat (Google Font) throughout**
- Hero Highlight: Montserrat ExtraBold, 64px, line-height 72px, letter-spacing -2%, color `#00544D` (scale down responsively on mobile — see Section 6).
- Hero Tagline: Montserrat Regular, 20px, line-height 28px, color `#00544D` at 80% opacity.
- Nav links: Montserrat Regular, 16px, color `#00544D`.
- Body text: Montserrat Regular, 16px, line-height 24px, color `#00544D`.
- Form labels/inputs: Montserrat Regular, 14–16px, color `#00544D`.
- CTA button text: Montserrat Bold, 16px, white.
- Pricing package name: Montserrat Bold, 20px, `#00544D`.
- Pricing amount: Montserrat ExtraBold, 28px, `#00544D` or accent.
- Popup title: Montserrat Bold, 20px, `#00544D`. Popup body: Regular, 15px.
- Links (Privacy Policy, FAQ accents): 14px Regular, `#3C85D4`, underline on hover.

**Buttons**
- Primary CTA: rectangular with small rounded corners, background `#FF7A45`, white bold text. On hover: slight darken/scale.
- Consistent CTA styling across header, hero, package cards, and closing section.

**Iconography/Imagery**
- Use simple line-style icons (48–56px) for the "How It Works" 6-step process and the 4 "Why Kalaklin" value props.
- Use a mascot illustration placeholder (friendly laundry-basket character named "Kala") in: Hero (holding "Segera Hadir / Coming Soon" sign), Value Proposition section, Problem/Why section, Pricing promo callout, How It Works intro, FAQ icon, About section.
- If real image assets aren't provided to the tool, use clearly labeled placeholder illustrations/emoji-style SVGs in the brand colors — do not leave broken image tags.

---

## 3. PAGE STRUCTURE (follow this exact section order — based on approved wireframe)

### Header (sticky, full width)
- Left: Logo (~40px height, proportional).
- Right: Anchor nav links — **Cara Kerja | Paket | Tentang Kalaklin | FAQ**
- Far right: "Hubungi Kami" button (secondary style, links to WhatsApp CTA or scrolls to form).
- On mobile: collapse nav into a hamburger menu; keep logo + a single CTA button visible.

### 1. Hero (above the fold, full viewport height on desktop)
- Centered layout, background full-bleed.
- **Highlight headline:** "24 jam siap melayani, 100% bersih, 0% plastik!" (large, bold, centered).
- **Tagline** below: "Bersih Bajunya, Hijau Buminya, Berseri Hatinya" (smaller, regular weight).
- **Location badge:** pin icon + "Jakarta Barat" centered below tagline.
- **Mascot Kala** holding a sign that says **"Segera Hadir"** (Coming Soon), centered, prominent.
- No CTA button required in hero itself (per wireframe) — form CTA lives in the next section.

### 2. Value Proposition + Lead Form ("below the fold" — two-column on desktop, stacked on mobile)
- **Left column:** 
  - Intro line: "Baju bersih tepat waktu, tanpa rasa lelah. Kalaklin hadir untuk membantumu dengan memberikan:"
  - 6 value proposition bullet points, each with a small icon, bold label:
    1. Layanan 24 jam
    2. 24 jam antar jemput
    3. Garansi tepat waktu
    4. Aman untuk keluarga
    5. Ramah lingkungan
    6. Pesan praktis via WhatsApp
- **Right column:** Lead capture form, inside a bordered card:
  - Field 1: **Nama** (text input)
  - Field 2: **Akun Instagram** (text input)
  - **CTA button:** "Daftar sekarang →" (accent color `#FF7A45`)
  - Micro-copy under button (italic, small): "Belum perlu bayar sekarang"
  - Below that: "Dengan mendaftar, kamu setuju dengan [Kebijakan Privasi](#) kami." — "Kebijakan Privasi" is a clickable link (blue, underline on hover) that opens a **modal popup** styled to match the page, containing placeholder privacy policy text and a single button **"Saya Mengerti"** that closes the modal.
  - **On form submit:** trigger a **success modal popup**:
    - Title: "Terima kasih!"
    - Body: "Kamu telah terdaftar. Pantau DM Instagram mu, penawaran early-bird eksklusif akan dikirim sebelum launching."
    - Close/confirm button styled like the accent CTA.

### 3. Problem & Why Kalaklin (two sub-sections stacked)
- **Sub-head:** "Sering Mengalami Masalah Seperti Ini?"
  - 3 horizontal cards (stack vertically on mobile), each with an image/icon on top and short text below:
    1. Merasa capek dan tidak memiliki waktu untuk mencuci baju.
    2. Kekhawatiran tentang kesehatan kulit pada keluarga.
    3. Menumpuknya sampah plastik dari kemasan laundry.
- **Sub-head:** "Kenapa Harus Kalaklin?"
  - 4 rows, each with an icon on the left and text block on the right:
    1. **24 jam siap melayani** — Kalaklin hadir untuk membantu pelanggan yang memiliki keterbatasan waktu dalam mencuci pakaian.
    2. **24 jam antar jemput** — Kalaklin memberikan kemudahan layanan antar-jemput langsung di depan pintu rumah atau kos secara terjadwal.
    3. **0% plastik** — Kalaklin mengganti plastik dengan kemasan Reusable Canvas Laundry Bag yang digunakan setiap kali proses penjemputan dan pengantaran.
    4. **Pesan praktis via WhatsApp** — Kalaklin memproses seluruh pendaftaran, penjadwalan kurir, dan pelacakan status cucian terintegrasi secara otomatis dan praktis melalui WhatsApp.

### 4. Pricing / Paket Langganan (anchor: `#paket`)
- 2 pricing cards side by side on desktop, stacked on mobile. Each card: bordered, package name, price, quota detail, CTA button.
  1. **Eco-Simpel** — Rp 180.000/bulan — "Cuci Kering Lipat, maksimal 20kg atau 4x jemput-antar." CTA: **"Mau Paket Ini"**
  2. **Eco-Family & Premium** — Rp 240.000/bulan — "Cuci Kering Setrika, maksimal 20kg atau 4x jemput-antar, termasuk setrika uap rapi." CTA: **"Mau Paket Ini"**
  - Clicking either CTA **smooth-scrolls the page up to the lead form** in Section 2 (do not open WhatsApp here).
- **Promo callout** below the cards (mascot "Kala" handing out a discount), styled with accent color:
  - "10 orang pertama yang mendaftar mendapatkan potongan Rp 40.000 untuk bulan pertama Paket Eco-Family & Premium (dari Rp 240.000 menjadi Rp 200.000)."
  - CTA: **"Claim Sekarang"** → also scrolls to the lead form.

### 5. Cara Kerja / How Kalaklin Works (anchor: `#cara-kerja`)
- 6-step horizontal process flow (wraps to 2 rows or a vertical stack on mobile — wireframe shows a zigzag 3-3 layout on desktop), each step = icon + short label + one-line description:
  1. **Pesanan dibuat** — Pesanan masuk, kurir Kalaklin siap menerima penugasan.
  2. **Pakaian dijemput** — Kurir Kalaklin mengambil keranjang laundry langsung dari pintu rumah kamu.
  3. **Konsumen mendapat nota konfirmasi** — E-receipt berisi tanggal masuk, tanggal selesai, berat pakaian, dan jumlah pakaianmu.
  4. **Pakaian diproses** — Dicuci dan disetrika dengan bahan ramah lingkungan dan mesin berstandar profesional.
  5. **Notifikasi selesai** — Notifikasi WhatsApp saat pakaian sudah bersih dan siap dikirim.
  6. **Pakaian diantar** — Pakaian bersih dikembalikan ke rumahmu tepat waktu.

### 6. Tentang Kalaklin / About (anchor: `#tentang-kalaklin`)
- Two-column: text on the left, image (laundry-themed photo/illustration) on the right (stack on mobile, text first).
- Copy: "Kalaklin hadir sebagai solusi laundry digital hyperlocal berbasis WhatsApp tanpa ribet, siap melayani 24 jam dengan memadukan ketepatan waktu (Kala) dan memberikan jaminan kebersihan maksimal (Clean). Cukup lewat chat dari rumah, Anda mendapatkan layanan cuci praktis yang aman untuk kulit sensitif, sekaligus ikut menjaga bumi berkat penggunaan deterjen ramah lingkungan dan Reusable Canvas Laundry Bag."

### 7. FAQ (anchor: `#faq`)
- Accordion component, closed by default, one open at a time, smooth expand/collapse animation. 5 items in order:
  1. **Apakah Kalaklin tersedia di area saya?** — Saat ini tersedia di Jakarta Barat (Kebon Jeruk, Tanjung Duren, Palmerah, Puri, Pesanggrahan, dan Kemanggisan).
  2. **Apakah saya harus mengunduh aplikasi?** — Tidak perlu, hanya menggunakan chat WhatsApp.
  3. **Bagaimana sistem Reusable Canvas Laundry Bag bekerja?** — Kami menjamin 100% operasional pengemasan dan distribusi bebas dari kantong plastik sekali pakai. Setiap pelanggan paket langganan akan difasilitasi dengan Reusable Canvas Laundry Bag eksklusif yang ditukarkan secara sirkular pada setiap sesi antar-jemput.
  4. **Apa yang terjadi jika kurir terlambat?** — Kalaklin berkomitmen memberikan layanan antar-jemput terbaik dan tepat waktu. Jika kami terlambat menjemput, kami akan memberikan voucher khusus sebagai bentuk permohonan maaf dan tanggung jawab kami.
  5. **Berapa lama estimasi pengerjaan?** — Kalaklin memiliki waktu standar pengerjaan selama 2 hari.

### Footer
- 4-column layout on desktop (stack on mobile): Logo block | Nav links repeated | Contact/Social | Legal.
- Social icons linking to: Instagram `@kala_klin`, Facebook Page `kalaklin`, LinkedIn Page `kalaklin`.
- Bottom bar, centered, small text: "© 2024 Kalaklin. All rights reserved."

---

## 4. FUNCTIONAL REQUIREMENTS (must-have)

1. **Anchor navigation:** Header nav links (Cara Kerja, Paket, Tentang Kalaklin, FAQ) must smooth-scroll to their respective sections; add `id` attributes matching each section.
2. **Lead form → success modal:** Submitting Name + Instagram opens the "Terima kasih!" modal described in Section 3. Validate both fields are non-empty before allowing submit (simple client-side validation, inline error text in brand colors — no alert()).
3. **Privacy Policy modal:** Clicking "Kebijakan Privasi" opens a separate modal with placeholder policy text and a "Saya Mengerti" button that closes it. This must not submit the form.
4. **Pricing CTAs scroll to form:** Both "Mau Paket Ini" buttons and "Claim Sekarang" scroll smoothly back up to the lead form section — they do not open a separate checkout.
5. **FAQ accordion:** Expand/collapse interaction, keyboard accessible (Enter/Space to toggle), only one panel open at a time is acceptable but not required.
6. **"Hubungi Kami" header button:** Should open WhatsApp with a pre-filled message (use a `https://wa.me/<placeholder_number>?text=...` link) OR scroll to the lead form — pick WhatsApp deep-link behavior since the PRD requires all primary CTAs to eventually route to WhatsApp; pre-fill the message with the Thank-You copy: *"Halo Kalaklin, saya ingin cek apakah alamat saya masuk area layanan dan mengetahui paket langganan yang tersedia."* Use a clearly marked placeholder phone number (e.g., `6281234567890`) that's easy to find/replace.
7. **Sticky/consistent CTA presence:** Ensure a CTA (WhatsApp or "Daftar Sekarang") is reachable from header, hero area, pricing section, and footer, per PRD acceptance criteria.
8. **Analytics hooks:** Add clearly commented placeholder event-tracking calls (e.g., `// TODO: trackEvent('cta_whatsapp_click')`) at: WhatsApp/CTA clicks, form submission, pricing card CTA clicks, FAQ open events, and scroll-depth milestones (e.g., reaching Pricing and FAQ sections). Structure these so a real analytics SDK (GA4/Meta Pixel) can be dropped in later.
9. **No login/app download messaging anywhere** — reinforce "no app needed" positioning in hero/nav copy where natural.

---

## 5. RESPONSIVENESS REQUIREMENTS

- **Mobile-first build.** Breakpoints: mobile (<640px), tablet (640–1024px), desktop (>1024px).
- Hero highlight text scales down from 64px (desktop) to ~32–36px (mobile) while preserving bold weight and centered alignment.
- Two-column sections (VP+Form, About) stack to single column, form/image below text, on mobile and tablet.
- Pricing cards and "Sering Mengalami Masalah" cards: horizontal row on desktop → horizontal scroll or stacked cards on mobile (stacked preferred for accessibility).
- How It Works 6-step flow: horizontal with connecting arrows on desktop → vertical stacked list on mobile.
- Header collapses to a hamburger menu below tablet breakpoint; nav drawer/overlay must be dismissible.
- All tap targets (buttons, accordion headers, nav links) must be at least 44×44px on mobile.
- Test that the FAQ accordion and both modals render correctly and remain scrollable within small viewports (no content cut off).

---

## 6. ACCEPTANCE CRITERIA (page is "done" when all of these are true)

- [ ] Kalaklin's value message, WhatsApp-only service model, and the 10 km West Jakarta radius are all clearly visible within the first screen/hero.
- [ ] All CTAs route correctly — form CTA opens the success modal, pricing CTAs scroll to the form, header CTA opens WhatsApp with pre-filled text.
- [ ] Biodegradable detergent and reusable canvas bag information appears in at least two sections (Why Kalaklin + FAQ, minimum).
- [ ] The 5 USPs (on-time guarantee, WhatsApp tracking, 24/7, plastic-free, eco/hypoallergenic) are visible before the pricing section.
- [ ] Page is fully usable and visually correct on mobile (375px width), tablet (768px), and desktop (1440px).
- [ ] Pricing, service area, FAQ, and footer contact/social info are present and easy to find.
- [ ] All section anchors work from the header nav.
- [ ] No broken image placeholders left unstyled; all colors/fonts match the design system in Section 2.

---

## 7. WHAT NOT TO DO

- Do not add a payment/checkout flow — this is a lead-gen page only, no money changes hands here.
- Do not require account creation or app download anywhere.
- Do not alter the approved Indonesian copy — use it verbatim as given in Section 3 (this is final marketing copy, not a draft).
- Do not reorder sections from the structure in Section 3 — it reflects a wireframe already approved by stakeholders.
