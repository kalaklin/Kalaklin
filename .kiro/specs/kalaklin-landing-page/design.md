# Design — Kalaklin Landing Page (Pre-Launch Waitlist)

## Tech Stack

| Layer | Pilihan | Alasan |
|---|---|---|
| Framework | **Vite + React 18** | Optimal untuk static/marketing page — build lebih ringan, tidak perlu SSR, deploy ke static host (Vercel/Netlify/GitHub Pages) tanpa konfigurasi tambahan |
| Styling | **Tailwind CSS v3** | Utility-first, design token tersentralisasi di `tailwind.config.js`, responsive breakpoint built-in |
| Language | **TypeScript** | Type safety untuk props komponen dan event handler |
| Font | **Google Fonts — Montserrat** | Sesuai design system; dimuat via `<link>` di `index.html` |
| Icons | **Lucide React** | Tree-shakeable, line-style, cocok dengan estetika brand |
| Form | Native React state | Tidak perlu library form eksternal untuk form dua field |
| Routing | Tidak ada | Single-page, navigasi anchor murni |
| Analytics | Placeholder komentar | Mudah diganti GA4 / Meta Pixel |

---

## Struktur Proyek

```
kalaklin-landing/
├── public/
│   └── assets/
│       ├── logo/
│       │   ├── logo-kalaklin.png          # Logo utama (dark bg)
│       │   └── logo-kalaklin-white-bg.png # Logo untuk section terang
│       ├── mascot/
│       │   ├── mascot-coming-soon.png     # Hero — "Segera Hadir"
│       │   ├── mascot-greeting.png        # Value Prop section
│       │   ├── mascot-laundry.png         # Cara Kerja / About
│       │   ├── mascot-promo.png           # Pricing callout
│       │   └── mascot-faq.png             # FAQ
│       ├── icons/
│       │   ├── delivery.png
│       │   ├── pickup.png
│       │   ├── layanan-24jam.png
│       │   ├── garansi-tepat-waktu.png
│       │   ├── aman-keluarga.png
│       │   ├── ramah-lingkungan.png
│       │   ├── whatsapp-order.png
│       │   ├── no-plastic.png
│       │   ├── notifikasi-tracking.png
│       │   └── proses-laundry.png
│       └── images/
│           └── kalaklin-workshop.png      # About section
├── src/
│   ├── main.tsx                           # Entry point
│   ├── App.tsx                            # Root — merangkai semua Section
│   ├── constants/
│   │   └── content.ts                     # Semua copy, nomor WA, data FAQ, dll.
│   ├── components/
│   │   ├── ui/                            # Reusable primitif
│   │   │   ├── CTAButton.tsx
│   │   │   ├── Modal.tsx
│   │   │   ├── Accordion.tsx
│   │   │   └── Card.tsx
│   │   ├── layout/
│   │   │   ├── Header.tsx
│   │   │   └── Footer.tsx
│   │   └── sections/
│   │       ├── HeroSection.tsx
│   │       ├── ValuePropFormSection.tsx
│   │       ├── ProblemWhySection.tsx
│   │       ├── PricingSection.tsx
│   │       ├── HowItWorksSection.tsx
│   │       ├── AboutSection.tsx
│   │       └── FAQSection.tsx
│   ├── hooks/
│   │   └── useScrollTo.ts                 # Smooth-scroll helper
│   └── analytics/
│       └── events.ts                      # Placeholder event tracker
├── tailwind.config.ts
├── index.html
├── vite.config.ts
└── tsconfig.json
```

---

## Design System (Design Tokens)

Semua token didefinisikan di `tailwind.config.ts` — **tidak ada nilai warna/font
hardcode berulang di komponen**.

```ts
// tailwind.config.ts (excerpt)
theme: {
  extend: {
    colors: {
      brand: {
        primary:   '#00544D',  // headings, nav, body
        accent:    '#FF7A45',  // CTA buttons, harga, promo
        link:      '#3C85D4',  // text links, FAQ icon
        surface:   '#E8EFED',  // input bg, card fill
      },
    },
    fontFamily: {
      sans: ['Montserrat', 'sans-serif'],
    },
    fontSize: {
      'hero-xl':  ['64px', { lineHeight: '72px', letterSpacing: '-0.02em', fontWeight: '800' }],
      'hero-md':  ['40px', { lineHeight: '48px', letterSpacing: '-0.02em', fontWeight: '800' }],
      'hero-sm':  ['32px', { lineHeight: '40px', letterSpacing: '-0.02em', fontWeight: '800' }],
      'tagline':  ['20px', { lineHeight: '28px' }],
      'body':     ['16px', { lineHeight: '24px' }],
      'label':    ['14px', { lineHeight: '20px' }],
      'price':    ['28px', { lineHeight: '36px', fontWeight: '800' }],
    },
    borderRadius: {
      cta: '6px',   // tombol CTA — rounded tipis
    },
  },
}
```

---

## Komponen UI Reusable

### `CTAButton`

```tsx
interface CTAButtonProps {
  children: React.ReactNode;
  onClick?: () => void;
  href?: string;        // jika link eksternal (WhatsApp)
  variant?: 'primary' | 'secondary';
  fullWidth?: boolean;
  disabled?: boolean;
}
```

- `primary`: bg `brand-accent`, teks putih bold 16px, rounded-cta, hover: `brightness-90 scale-[1.02]`
- `secondary`: border `brand-primary`, teks `brand-primary`, bg transparan
- Min tap target 44 × 44px di semua ukuran

### `Modal`

```tsx
interface ModalProps {
  isOpen: boolean;
  onClose: () => void;
  title: string;
  children: React.ReactNode;
}
```

- Overlay semi-transparan, klik overlay menutup modal
- Focus trap saat terbuka (`Tab` bersirkulasi di dalam modal)
- `Escape` menutup modal
- Body scroll lock saat terbuka (`overflow-hidden` pada `document.body`)
- Animasi: fade-in + scale-up (150ms ease-out)
- Dua instance: `SuccessModal` dan `PrivacyPolicyModal` — keduanya menggunakan
  komponen `Modal` yang sama dengan konten berbeda

### `Accordion`

```tsx
interface AccordionItem {
  id: string;
  question: string;
  answer: string;
}

interface AccordionProps {
  items: AccordionItem[];
}
```

- State: `activeId: string | null` — single-open
- Toggle dengan klik atau keyboard (Enter / Space)
- Animasi tinggi konten: `max-height` transition (0 → auto via CSS atau framer-motion)
- ARIA: `role="button"`, `aria-expanded`, `aria-controls`

### `Card`

```tsx
interface CardProps {
  children: React.ReactNode;
  variant?: 'default' | 'pricing' | 'problem';
  className?: string;
}
```

- `default`: bg putih, border `brand-surface`, shadow ringan
- `pricing`: border lebih tebal, padding lebih besar
- `problem`: bg `brand-surface`, no border, rounded-xl

---

## Arsitektur Section

### Section Order (FIXED — sesuai wireframe)

```
<Header />          ← sticky, z-50
<main>
  <HeroSection />                    id="hero"
  <ValuePropFormSection />           id="daftar"
  <ProblemWhySection />              id="mengapa"
  <PricingSection />                 id="paket"
  <HowItWorksSection />              id="cara-kerja"
  <AboutSection />                   id="tentang-kalaklin"
  <FAQSection />                     id="faq"
</main>
<Footer />
```

### Header

- **Desktop (≥1024px):** Logo kiri | Nav center-right: Cara Kerja · Paket · Tentang Kalaklin · FAQ | "Hubungi Kami" button kanan
- **Mobile/Tablet (<1024px):** Logo kiri | "Hubungi Kami" mini-button | Hamburger icon kanan
- Hamburger menu: overlay drawer dari kanan, menampilkan nav links vertikal
- Scroll behavior: background transparan → bg putih + shadow saat scroll > 80px

### HeroSection

- Full viewport height (`min-h-screen`) di desktop, `min-h-[85vh]` di mobile
- Layout: flex column center-aligned
- Elements (atas ke bawah):
  1. Headline H1 (hero-xl → hero-sm di mobile)
  2. Tagline (tagline, opacity-80)
  3. Location badge (pin icon + "Jakarta Barat")
  4. Mascot "Kala" dengan papan "Segera Hadir" (`<img>` atau SVG placeholder)
- Background: putih atau gradient sangat halus brand-primary → putih

### ValuePropFormSection

- **Desktop:** 2-col grid (`grid-cols-2 gap-12`)
- **Mobile/Tablet:** single col, form di bawah value props
- Kiri: intro teks + 6 bullet value proposition (ikon + bold label + deskripsi)
- Kanan: `<LeadForm />` dalam `Card` dengan border `brand-primary`
  - Input styling: bg `brand-surface`, border transparan, focus ring `brand-primary`
  - Error state: border merah, teks error di bawah input, warna `#DC2626`
  - Submit state: tombol `disabled` + loading text saat proses

### ProblemWhySection

- Dua sub-section dalam satu `<section>`
- "Sering Mengalami Masalah": 3 `Card` (problem variant) — `grid-cols-3` desktop, `grid-cols-1` mobile
- "Kenapa Harus Kalaklin": 4 baris icon-left + text-right (`flex items-start gap-4`)

### PricingSection

- 2 `Card` (pricing variant) — `grid-cols-2` desktop, `grid-cols-1` mobile
- Promo callout: bg `brand-accent/10`, border `brand-accent`, rounded-xl
- Semua CTA scroll ke `#daftar`

### HowItWorksSection

- **Desktop:** `grid-cols-3` dengan koneksi visual (garis atau panah antar langkah)
- **Mobile:** `flex flex-col` numbered list
- Setiap langkah: ikon Lucide atau asset icon + nomor + judul bold + deskripsi

### AboutSection

- **Desktop:** `grid-cols-2` — teks kiri, gambar kanan
- **Mobile:** teks dulu, gambar di bawah
- Gambar: `kalaklin-workshop.png` atau placeholder bertuliskan "Workshop Kalaklin"

### FAQSection

- `<Accordion items={faqData} />` penuh lebar
- Icon chevron rotate 180° saat terbuka

### Footer

- **Desktop:** `grid-cols-4`
- **Mobile:** `flex flex-col gap-8`
- Kolom 1: Logo + tagline singkat
- Kolom 2: Nav links (Cara Kerja, Paket, Tentang Kalaklin, FAQ)
- Kolom 3: Kontak + ikon sosial (Instagram, Facebook, LinkedIn)
- Kolom 4: Info legal + copyright
- Bottom bar: "© 2024 Kalaklin. All rights reserved."

---

## Data & Constants (`src/constants/content.ts`)

Semua copy dan data dikentralisasi di satu file agar mudah diubah tanpa menyentuh
komponen:

```ts
// src/constants/content.ts

export const WHATSAPP_NUMBER = '6281234567890'; // TODO: ganti dengan nomor resmi
export const WHATSAPP_MESSAGE = 'Halo Kalaklin, saya ingin cek apakah alamat saya masuk area layanan dan mengetahui paket langganan yang tersedia.';
export const WHATSAPP_URL = `https://wa.me/${WHATSAPP_NUMBER}?text=${encodeURIComponent(WHATSAPP_MESSAGE)}`;

export const VALUE_PROPS = [ /* ... 6 item */ ];
export const PROBLEM_CARDS = [ /* ... 3 item */ ];
export const WHY_KALAKLIN = [ /* ... 4 item */ ];
export const PACKAGES = [ /* ... 2 item */ ];
export const HOW_IT_WORKS_STEPS = [ /* ... 6 item */ ];
export const FAQ_ITEMS = [ /* ... 5 item */ ];
export const SOCIAL_LINKS = { instagram, facebook, linkedin };
```

---

## Analytics Placeholder (`src/analytics/events.ts`)

```ts
// src/analytics/events.ts
// TODO: Replace with actual GA4 / Meta Pixel SDK calls

export const trackEvent = (eventName: string, params?: Record<string, unknown>) => {
  // TODO: window.gtag?.('event', eventName, params);
  // TODO: window.fbq?.('track', eventName, params);
  console.debug('[Analytics]', eventName, params);
};

export const EVENTS = {
  CTA_WHATSAPP_CLICK:   'cta_whatsapp_click',
  FORM_SUBMIT_SUCCESS:  'form_submit_success',
  CTA_PACKAGE_CLICK:    'cta_package_click',
  FAQ_OPEN:             'faq_open',
  SCROLL_PRICING:       'scroll_depth_pricing',
  SCROLL_FAQ:           'scroll_depth_faq',
} as const;
```

Scroll depth events diimplementasikan via `IntersectionObserver` pada `useEffect`
di `PricingSection` dan `FAQSection`.

---

## Form Submission Flow

```
User klik "Daftar sekarang →"
  │
  ├─ Validasi client-side
  │   ├─ Nama kosong?  → tampilkan error inline "Nama tidak boleh kosong"
  │   └─ Instagram kosong? → tampilkan error inline "Akun Instagram tidak boleh kosong"
  │
  ├─ Jika valid:
  │   ├─ Set loading state (disable tombol, tampilkan spinner)
  │   ├─ Panggil submitLead(data) → placeholder fetch/webhook
  │   ├─ trackEvent(EVENTS.FORM_SUBMIT_SUCCESS, { name, instagram })
  │   └─ Buka SuccessModal
  │
  └─ SuccessModal onClose:
      └─ Reset form ke state awal
```

---

## Responsiveness Detail

| Elemen | Mobile (<640px) | Tablet (640–1023px) | Desktop (≥1024px) |
|---|---|---|---|
| Header | Logo + CTA + Hamburger | Logo + CTA + Hamburger | Logo + Nav + "Hubungi Kami" |
| Hero headline | 32px | 40px | 64px |
| VP+Form | Stack (form di bawah) | Stack (form di bawah) | 2 kolom |
| Problem cards | 1 kolom | 1 kolom | 3 kolom |
| Pricing cards | 1 kolom | 2 kolom | 2 kolom |
| How It Works | List vertikal | List vertikal | Grid 3-3 |
| About | Stack (teks dulu) | Stack (teks dulu) | 2 kolom |
| Footer | Stack vertikal | Stack 2 kolom | 4 kolom |

---

## Aset & Placeholder Policy

- **Jika file aset tersedia** di `public/assets/`: gunakan path langsung.
- **Jika aset tidak tersedia**: render elemen `<div>` / `<svg>` placeholder dengan:
  - Background `#E8EFED` (brand-surface)
  - Border dashed `#00544D`
  - Teks tengah: `[PLACEHOLDER: nama-file.png]` warna `#00544D`
  - Dimensi sesuai slot yang dimaksud
- Placeholder **TIDAK BOLEH** dirender sebagai `<img src="">` yang broken.
- Semua placeholder harus mudah diganti cukup dengan menaruh file gambar di path yang benar.
