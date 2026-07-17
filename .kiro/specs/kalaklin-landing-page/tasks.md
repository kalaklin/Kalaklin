# Tasks — Kalaklin Landing Page (Pre-Launch Waitlist)

Implementation task list berurutan. Setiap task memiliki kriteria selesai (done criteria)
yang jelas agar mudah diverifikasi sebelum lanjut ke task berikutnya.

---

## Phase 1 — Project Scaffolding & Foundation

### Task 1.1 — Inisialisasi Proyek Vite + React + TypeScript

**Steps:**
- Jalankan `npm create vite@latest kalaklin-landing -- --template react-ts`
- Install dependencies: `tailwindcss`, `postcss`, `autoprefixer`, `lucide-react`
- Jalankan `npx tailwindcss init -p` untuk generate `tailwind.config.ts` dan `postcss.config.js`

**Done criteria:**
- [ ] `npm run dev` berjalan tanpa error
- [ ] `npm run build` menghasilkan folder `dist/` tanpa error
- [ ] TypeScript strict mode aktif di `tsconfig.json`

---

### Task 1.2 — Konfigurasi Design Tokens di Tailwind

**Steps:**
- Edit `tailwind.config.ts`: tambahkan `content` paths untuk semua file `src/**/*.{ts,tsx}`
- Definisikan semua warna brand sebagai custom color tokens:
  - `brand.primary: '#00544D'`
  - `brand.accent: '#FF7A45'`
  - `brand.link: '#3C85D4'`
  - `brand.surface: '#E8EFED'`
- Definisikan `fontFamily.sans: ['Montserrat', 'sans-serif']`
- Definisikan custom `fontSize` untuk hero-xl, hero-md, hero-sm, tagline, price
- Definisikan `borderRadius.cta: '6px'`

**Done criteria:**
- [ ] Kelas `bg-brand-accent` dan `text-brand-primary` bekerja di komponen test
- [ ] Font Montserrat (dari Google Fonts via `<link>` di `index.html`) terload di browser
- [ ] Tidak ada warna hex hardcode di luar `tailwind.config.ts`

---

### Task 1.3 — Setup File Constants & Analytics Placeholder

**Steps:**
- Buat `src/constants/content.ts`:
  - `WHATSAPP_NUMBER`, `WHATSAPP_MESSAGE`, `WHATSAPP_URL`
  - Array data: `VALUE_PROPS` (6 item), `PROBLEM_CARDS` (3 item), `WHY_KALAKLIN` (4 item)
  - Array data: `PACKAGES` (2 item), `HOW_IT_WORKS_STEPS` (6 item), `FAQ_ITEMS` (5 item)
  - `SOCIAL_LINKS` object (instagram, facebook, linkedin)
  - Semua copy Bahasa Indonesia persis seperti di spec (verbatim)
- Buat `src/analytics/events.ts`:
  - Fungsi `trackEvent(eventName, params?)` dengan `console.debug` dan komentar TODO GA4/Meta Pixel
  - Konstanta `EVENTS` untuk semua 6 event name

**Done criteria:**
- [ ] `WHATSAPP_URL` menghasilkan URL yang valid saat ditest di browser
- [ ] Semua 5 item FAQ tersedia di `FAQ_ITEMS` dengan `id`, `question`, `answer`
- [ ] `trackEvent` bisa dipanggil tanpa error dan muncul di console

---

### Task 1.4 — Setup Folder Struktur & Salin Aset

**Steps:**
- Buat folder struktur sesuai design.md:
  `src/components/ui/`, `src/components/layout/`, `src/components/sections/`, `src/hooks/`
- Salin aset dari `Creative-Content/` ke `public/assets/`:
  - Logo → `public/assets/logo/`
  - Mascot → `public/assets/mascot/`
  - Icons → `public/assets/icons/`
  - Images → `public/assets/images/`
- Buat `src/hooks/useScrollTo.ts`: helper function smooth-scroll ke element by id

**Done criteria:**
- [ ] Semua folder `src/components/` ada (boleh berisi file kosong/placeholder)
- [ ] File aset dapat diakses via `/assets/logo/logo-kalaklin.png` di browser dev server
- [ ] `useScrollTo` dapat dipanggil dengan id string dan browser scroll ke elemen terkait

---

## Phase 2 — Komponen UI Reusable

### Task 2.1 — Komponen `CTAButton`

**File:** `src/components/ui/CTAButton.tsx`

**Steps:**
- Implementasikan props: `children`, `onClick`, `href`, `variant` (primary/secondary), `fullWidth`, `disabled`
- `primary`: `bg-brand-accent text-white font-bold rounded-cta hover:brightness-90 hover:scale-[1.02]`
- Pastikan min height/width 44px untuk aksesibilitas mobile
- Jika `href` tersedia, render sebagai `<a target="_blank" rel="noopener noreferrer">`
- Jika `disabled`, tambahkan `opacity-50 cursor-not-allowed`

**Done criteria:**
- [ ] Variant primary dan secondary tampil sesuai design system
- [ ] Hover state berfungsi di desktop
- [ ] Link eksternal (WhatsApp) membuka tab baru
- [ ] Disabled state mencegah klik

---

### Task 2.2 — Komponen `Modal`

**File:** `src/components/ui/Modal.tsx`

**Steps:**
- Props: `isOpen`, `onClose`, `title`, `children`
- Render overlay `fixed inset-0 bg-black/50 z-50` saat `isOpen: true`
- Implementasikan focus trap: saat modal terbuka, `Tab` bersirkulasi di dalam elemen fokusable modal
- Implementasikan body scroll lock: `document.body.style.overflow = 'hidden'` saat buka, restore saat tutup
- Klik overlay atau tekan `Escape` memanggil `onClose`
- Animasi: fade-in + scale-up (CSS transition atau Tailwind `transition`)
- ARIA: `role="dialog"`, `aria-modal="true"`, `aria-labelledby`

**Done criteria:**
- [ ] Modal muncul dan hilang dengan animasi
- [ ] Klik di luar modal / Escape menutup modal
- [ ] `Tab` tidak keluar dari modal saat terbuka
- [ ] Scroll halaman terkunci saat modal terbuka, pulih saat ditutup
- [ ] Accessible: role dialog, aria-modal, label terhubung

---

### Task 2.3 — Komponen `Accordion`

**File:** `src/components/ui/Accordion.tsx`

**Steps:**
- Props: `items: AccordionItem[]` dengan `id`, `question`, `answer`
- State: `activeId: string | null`
- Toggle: klik item → buka; klik item yang sama → tutup; klik item lain → tutup yang lama, buka yang baru
- Keyboard: `onKeyDown` handle Enter dan Space
- Animasi konten: `max-height` transition dari `0` ke nilai tinggi konten
- ARIA: header sebagai `<button>` dengan `aria-expanded`, `aria-controls`; panel dengan `id` matching
- Ikon chevron: rotate `180deg` saat expanded

**Done criteria:**
- [ ] Single-open accordion berfungsi (item lama menutup saat item baru dibuka)
- [ ] Enter/Space pada item header toggle accordion
- [ ] Animasi expand/collapse halus
- [ ] `aria-expanded` berubah sesuai state

---

### Task 2.4 — Komponen `Card`

**File:** `src/components/ui/Card.tsx`

**Steps:**
- Props: `children`, `variant` (default/pricing/problem), `className`
- `default`: `bg-white border border-brand-surface rounded-xl shadow-sm`
- `pricing`: `bg-white border-2 border-brand-primary rounded-xl p-8`
- `problem`: `bg-brand-surface rounded-xl p-6`

**Done criteria:**
- [ ] Ketiga variant tampil berbeda sesuai spesifikasi
- [ ] `className` prop bisa menambahkan kelas tambahan tanpa konflik

---

## Phase 3 — Layout Komponen

### Task 3.1 — Komponen `Header`

**File:** `src/components/layout/Header.tsx`

**Steps:**
- Sticky: `fixed top-0 left-0 right-0 z-50`
- Scroll effect: tambahkan `bg-white shadow-md` saat `scrollY > 80` (via `useEffect` + `scroll` event listener)
- **Desktop (≥1024px):** Logo kiri, nav links tengah-kanan, "Hubungi Kami" button kanan
- **Mobile (<1024px):** Logo kiri, "Hubungi Kami" mini button, hamburger icon kanan
- Hamburger state: `isMenuOpen: boolean`
- Mobile drawer: overlay full-screen atau slide-in dari kanan, nav links vertikal
- Menutup drawer: klik item menu, klik overlay, atau tombol close (×)
- Nav links scroll ke section dengan `useScrollTo`
- "Hubungi Kami" → `WHATSAPP_URL` buka tab baru + `trackEvent(EVENTS.CTA_WHATSAPP_CLICK)`

**Done criteria:**
- [ ] Header tetap di atas saat scroll
- [ ] Background berubah saat halaman di-scroll
- [ ] Nav links smooth-scroll ke section yang benar
- [ ] Hamburger menu berfungsi di viewport 375px
- [ ] "Hubungi Kami" membuka WhatsApp URL yang benar
- [ ] Analytics event terpanggil saat klik "Hubungi Kami"

---

### Task 3.2 — Komponen `Footer`

**File:** `src/components/layout/Footer.tsx`

**Steps:**
- Desktop: `grid grid-cols-4 gap-8`; Mobile: `flex flex-col gap-8`
- Kolom 1: Logo + tagline singkat brand
- Kolom 2: Heading "Navigasi" + 4 anchor links (Cara Kerja, Paket, Tentang Kalaklin, FAQ)
- Kolom 3: Heading "Hubungi Kami" + ikon sosial (Instagram `@kala_klin`, Facebook `kalaklin`, LinkedIn `kalaklin`)
- Kolom 4: Info legal
- Bottom bar: `border-t mt-8 pt-4 text-center text-sm` + "© 2024 Kalaklin. All rights reserved."

**Done criteria:**
- [ ] 4 kolom di desktop, stack di mobile
- [ ] Semua sosial media link tersambung ke URL yang benar (placeholder boleh #)
- [ ] Copyright text tampil di bottom bar

---

## Phase 4 — Section Komponen

### Task 4.1 — `HeroSection`

**File:** `src/components/sections/HeroSection.tsx`

**Steps:**
- `id="hero"`, `min-h-screen` (desktop), flex column center
- H1: copy headline dari `content.ts`, ukuran responsif (hero-sm → hero-xl)
- Tagline: copy dari `content.ts`, opacity 80%
- Location badge: ikon pin (Lucide `MapPin`) + "Jakarta Barat"
- Mascot: `<img src="/assets/mascot/mascot-coming-soon.png" alt="Maskot Kala memegang papan Segera Hadir" />`
  atau placeholder div jika file tidak ada

**Done criteria:**
- [ ] Headline terlihat jelas di hero, font ExtraBold, warna brand-primary
- [ ] Tagline muncul di bawah headline, opacity lebih rendah
- [ ] Location badge dengan ikon pin terlihat
- [ ] Mascot atau placeholder terlihat jelas dan tidak broken
- [ ] Di mobile (375px), semua elemen fit dalam viewport tanpa overflow horizontal

---

### Task 4.2 — `ValuePropFormSection`

**File:** `src/components/sections/ValuePropFormSection.tsx`

**Steps:**
- `id="daftar"`, padding top cukup untuk offset sticky header
- **Desktop:** `grid grid-cols-2 gap-12`; **Mobile:** `flex flex-col`
- **Kiri:** intro teks + 6 bullet `VALUE_PROPS` (ikon dari `public/assets/icons/` atau Lucide fallback + bold label + deskripsi)
- **Kanan — LeadForm:**
  - State: `name`, `instagram`, `errors`, `isLoading`, `isSubmitted`
  - Input Nama: label "Nama", placeholder, error state
  - Input Instagram: label "Akun Instagram", placeholder "@username", error state
  - Validasi inline: field kosong → tampilkan pesan error merah di bawah input
  - Tombol "Daftar sekarang →": `CTAButton` primary, `disabled` saat loading
  - Microcopy italic: "Belum perlu bayar sekarang"
  - Link "Kebijakan Privasi" → `setPrivacyModalOpen(true)` + BUKAN submit form
  - Submit handler:
    1. Validasi → jika gagal, tampilkan errors dan stop
    2. Set `isLoading: true`
    3. Panggil `submitLead(data)` (placeholder async function, resolve setelah 800ms)
    4. `trackEvent(EVENTS.FORM_SUBMIT_SUCCESS, { name, instagram })`
    5. Set `isSubmitted: true`, buka `SuccessModal`
- `SuccessModal`: judul "Terima kasih!", body copy dari spec, tombol "Tutup" yang reset form
- `PrivacyPolicyModal`: judul "Kebijakan Privasi", konten teks placeholder, tombol "Saya Mengerti"

**Done criteria:**
- [ ] Validasi inline muncul tanpa `alert()` saat field kosong
- [ ] Modal sukses muncul setelah submit valid
- [ ] Tombol submit disabled saat loading dan setelah sukses
- [ ] Modal Kebijakan Privasi terbuka via link, bukan submit form
- [ ] Kedua modal berfungsi secara independen
- [ ] Layout 2-kolom di desktop, stack di mobile
- [ ] Analytics event terpanggil saat submit berhasil

---

### Task 4.3 — `ProblemWhySection`

**File:** `src/components/sections/ProblemWhySection.tsx`

**Steps:**
- `id="mengapa"`
- **Sub-section 1** "Sering Mengalami Masalah Seperti Ini?":
  - 3 `Card` (problem variant) dari `PROBLEM_CARDS`
  - `grid-cols-3` desktop, `grid-cols-1` mobile
  - Setiap card: ikon/ilustrasi atas + teks bawah
- **Sub-section 2** "Kenapa Harus Kalaklin?":
  - 4 baris dari `WHY_KALAKLIN`
  - Layout: `flex items-start gap-4` (ikon kiri, teks kanan)
  - Bold label + deskripsi

**Done criteria:**
- [ ] 3 problem card tampil horizontal di desktop, stack di mobile
- [ ] 4 why-kalaklin rows tampil dengan ikon dan teks yang benar
- [ ] Copy verbatim sesuai spec

---

### Task 4.4 — `PricingSection`

**File:** `src/components/sections/PricingSection.tsx`

**Steps:**
- `id="paket"`, offset anchor untuk sticky header
- 2 `Card` (pricing variant) dari `PACKAGES`:
  - Nama paket (bold 20px)
  - Harga (ExtraBold 28px, warna brand-accent)
  - Detail kuota (body text)
  - Tombol "Mau Paket Ini" → `useScrollTo('daftar')` + `trackEvent(EVENTS.CTA_PACKAGE_CLICK, { package: name })`
- `grid-cols-2 gap-6` desktop, `grid-cols-1` mobile
- Promo callout:
  - Background `brand-accent/10`, border `brand-accent`
  - Teks promo 10 pendaftar pertama
  - Tombol "Claim Sekarang" → `useScrollTo('daftar')` + `trackEvent`
  - Mascot promo sebagai ilustrasi pendukung

**Done criteria:**
- [ ] 2 pricing card dengan info lengkap (nama, harga, detail, CTA)
- [ ] Tombol "Mau Paket Ini" dan "Claim Sekarang" scroll ke form, bukan checkout
- [ ] Promo callout tampil dengan styling accent
- [ ] Analytics event terpanggil di setiap klik CTA
- [ ] `IntersectionObserver` memanggil `trackEvent(EVENTS.SCROLL_PRICING)` saat section masuk viewport

---

### Task 4.5 — `HowItWorksSection`

**File:** `src/components/sections/HowItWorksSection.tsx`

**Steps:**
- `id="cara-kerja"`
- 6 langkah dari `HOW_IT_WORKS_STEPS`, setiap item: nomor urut, ikon, judul bold, deskripsi
- **Desktop:** `grid grid-cols-3 gap-8` (2 baris × 3 kolom)
- **Mobile:** `flex flex-col gap-6` (list vertikal)
- Koneksi visual antar langkah di desktop: garis horizontal atau panah (CSS atau border)

**Done criteria:**
- [ ] 6 langkah tampil dengan nomor, ikon, judul, deskripsi
- [ ] Grid 3-3 di desktop, list vertikal di mobile
- [ ] Copy verbatim sesuai spec

---

### Task 4.6 — `AboutSection`

**File:** `src/components/sections/AboutSection.tsx`

**Steps:**
- `id="tentang-kalaklin"`
- **Desktop:** `grid grid-cols-2 gap-12` — teks kiri, gambar kanan
- **Mobile:** `flex flex-col` — teks dulu, gambar di bawah
- Teks: copy brand story verbatim dari spec
- Gambar: `<img src="/assets/images/kalaklin-workshop.png">` atau placeholder bertuliskan
  "[PLACEHOLDER: Workshop Kalaklin]"

**Done criteria:**
- [ ] Brand story copy tampil lengkap dan verbatim
- [ ] Layout 2-kolom di desktop, stack di mobile (teks di atas)
- [ ] Gambar atau placeholder terlihat dengan ukuran proporsional

---

### Task 4.7 — `FAQSection`

**File:** `src/components/sections/FAQSection.tsx`

**Steps:**
- `id="faq"`, offset anchor
- Render `<Accordion items={FAQ_ITEMS} />`
- Setiap item dibuka: `trackEvent(EVENTS.FAQ_OPEN, { question: item.id })`
  → Accordion perlu menerima callback `onOpen` atau `onToggle`
- `IntersectionObserver` memanggil `trackEvent(EVENTS.SCROLL_FAQ)` saat section masuk viewport

**Done criteria:**
- [ ] 5 item FAQ tampil dengan pertanyaan dan jawaban verbatim
- [ ] Accordion single-open, keyboard-accessible
- [ ] Analytics event terpanggil saat item dibuka
- [ ] `trackEvent` scroll depth terpanggil saat section terlihat

---

## Phase 5 — Assembly & Integrasi

### Task 5.1 — Assembly `App.tsx`

**File:** `src/App.tsx`

**Steps:**
- Import semua section dan layout
- Struktur:
  ```tsx
  <Header />
  <main>
    <HeroSection />
    <ValuePropFormSection />
    <ProblemWhySection />
    <PricingSection />
    <HowItWorksSection />
    <AboutSection />
    <FAQSection />
  </main>
  <Footer />
  ```
- Pastikan urutan section TIDAK berubah dari wireframe
- Tambahkan `pt-[80px]` atau equivalent pada `<main>` untuk offset sticky header

**Done criteria:**
- [ ] Semua section tampil dalam urutan yang benar
- [ ] Tidak ada section yang hilang atau duplikat
- [ ] Halaman dapat di-scroll penuh dari Hero sampai Footer tanpa layout break

---

### Task 5.2 — Anchor Navigation End-to-End Test

**Steps:**
- Verifikasi setiap nav link di header mengarah ke section yang benar:
  - "Cara Kerja" → `#cara-kerja`
  - "Paket" → `#paket`
  - "Tentang Kalaklin" → `#tentang-kalaklin`
  - "FAQ" → `#faq`
- Verifikasi smooth-scroll berjalan (bukan jump)
- Verifikasi offset memperhitungkan tinggi sticky header

**Done criteria:**
- [ ] Semua 4 nav link scroll ke section yang benar
- [ ] Scroll behavior terasa smooth
- [ ] Section heading tidak tertutup sticky header setelah scroll

---

### Task 5.3 — Responsive QA

**Steps:**
- Test di 3 viewport:
  - **375px** (iPhone SE): semua konten fit, tidak ada overflow horizontal, tap targets ≥ 44px
  - **768px** (iPad): layout transisi dengan benar
  - **1440px** (Desktop): layout 2-kolom/multi-kolom tampil
- Khusus mobile: verifikasi hamburger menu, accordion, dan modals
- Gunakan browser DevTools responsive mode

**Done criteria:**
- [ ] Tidak ada horizontal scroll di 375px
- [ ] Hamburger menu berfungsi di 375px dan 768px
- [ ] Modals full-screen atau centered dan scrollable di 375px
- [ ] Semua text readable (tidak terlalu kecil) di mobile
- [ ] Hero headline tetap bold dan proporsional di semua breakpoint

---

### Task 5.4 — Aksesibilitas & Markup Semantik

**Steps:**
- Verifikasi elemen semantik: `<header>`, `<nav>`, `<main>`, `<section>`, `<footer>`, satu `<h1>` per halaman
- Verifikasi semua `<img>` punya `alt` yang deskriptif
- Verifikasi semua form input punya `<label>` yang terhubung via `htmlFor`/`id`
- Verifikasi kontras warna memenuhi WCAG AA (rasio ≥ 4.5:1 untuk body text)
- Verifikasi modal focus trap dan Escape key
- Verifikasi accordion keyboard navigation

**Done criteria:**
- [ ] Tidak ada gambar dengan `alt=""` yang seharusnya deskriptif
- [ ] Semua input punya label yang visible atau accessible
- [ ] Modal bisa ditutup dengan Escape
- [ ] Heading hierarchy logis (h1 → h2 → h3)

---

### Task 5.5 — Build & Deploy Check

**Steps:**
- Jalankan `npm run build` — pastikan tidak ada error TypeScript atau Vite
- Preview build: `npm run preview`
- Verifikasi semua aset ter-resolve dengan benar di build output
- Tambahkan `README.md` singkat berisi:
  - Cara run dev: `npm run dev`
  - Cara build: `npm run build`
  - Cara ganti nomor WhatsApp: lokasi konstanta di `src/constants/content.ts`
  - Cara ganti aset: path `public/assets/`

**Done criteria:**
- [ ] `npm run build` selesai tanpa error
- [ ] `npm run preview` menampilkan halaman yang benar di browser
- [ ] Tidak ada broken image atau 404 di build preview
- [ ] README menjelaskan cara ganti nomor WA dan aset

---

## Ringkasan Task & Dependencies

```
Phase 1 (Foundation)
  1.1 Scaffolding
  1.2 Tailwind Config      ← depends on 1.1
  1.3 Constants & Analytics ← depends on 1.1
  1.4 Folder & Assets      ← depends on 1.1

Phase 2 (UI Primitives)   ← depends on Phase 1
  2.1 CTAButton
  2.2 Modal
  2.3 Accordion
  2.4 Card

Phase 3 (Layout)          ← depends on Phase 2
  3.1 Header
  3.2 Footer

Phase 4 (Sections)        ← depends on Phase 2 + 3
  4.1 HeroSection
  4.2 ValuePropFormSection  ← depends on Modal (2.2)
  4.3 ProblemWhySection
  4.4 PricingSection
  4.5 HowItWorksSection
  4.6 AboutSection
  4.7 FAQSection            ← depends on Accordion (2.3)

Phase 5 (Integration & QA) ← depends on Phase 4
  5.1 App.tsx Assembly
  5.2 Anchor Nav Test
  5.3 Responsive QA
  5.4 Aksesibilitas
  5.5 Build & Deploy
```

**Total: 18 tasks** — estimasi implementasi 2–3 hari kerja full-stack.
