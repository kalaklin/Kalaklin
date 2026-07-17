# Requirements — Kalaklin Landing Page (Pre-Launch Waitlist)

## Overview

Landing page satu halaman untuk validasi minat pasar Kalaklin — layanan laundry berlangganan
ramah lingkungan berbasis WhatsApp di Jakarta Barat. Tujuan utama: mengumpulkan leads
(Nama + Akun Instagram) sebelum layanan resmi diluncurkan.

---

## Functional Requirements

### FR-1: Navigasi Anchor (Sticky Header)

- **WHEN** pengguna mengklik tautan navigasi (Cara Kerja, Paket, Tentang Kalaklin, FAQ),
  **THE SYSTEM SHALL** melakukan smooth-scroll ke section dengan `id` yang sesuai.
- **WHEN** pengguna berada di halaman mana pun,
  **THE SYSTEM SHALL** selalu menampilkan header di posisi sticky (tetap di atas layar).
- **WHEN** lebar viewport kurang dari 1024px,
  **THE SYSTEM SHALL** mengganti nav links dengan hamburger menu yang dapat dibuka/ditutup.
- **WHEN** hamburger menu terbuka,
  **THE SYSTEM SHALL** menyediakan mekanisme untuk menutupnya (klik luar, tombol close, atau klik item menu).

### FR-2: Formulir Lead Capture

- **WHEN** pengguna mengisi field Nama dan Akun Instagram lalu mengklik "Daftar sekarang →",
  **THE SYSTEM SHALL** menampilkan modal sukses dengan pesan:
  _"Terima kasih! Kamu telah terdaftar. Pantau DM Instagram mu, penawaran early-bird
  eksklusif akan dikirim sebelum launching."_
- **WHEN** pengguna mencoba submit dengan salah satu field kosong,
  **THE SYSTEM SHALL** menampilkan pesan validasi inline di bawah field yang kosong
  (bukan `alert()` browser).
- **WHEN** modal sukses sudah ditampilkan,
  **THE SYSTEM SHALL** mencegah submit ganda (tombol dinonaktifkan atau form direset
  setelah submit berhasil).
- **WHEN** form submission terjadi,
  **THE SYSTEM SHALL** memanggil placeholder endpoint (dapat berupa `mailto:`,
  webhook kosong, atau fungsi in-memory) yang mudah diganti dengan integrasi nyata
  (Google Sheet / CRM) di kemudian hari.

### FR-3: Modal Kebijakan Privasi

- **WHEN** pengguna mengklik tautan "Kebijakan Privasi",
  **THE SYSTEM SHALL** membuka modal terpisah yang berisi teks kebijakan privasi placeholder.
- **WHEN** modal Kebijakan Privasi terbuka,
  **THE SYSTEM SHALL** menampilkan tombol "Saya Mengerti" yang menutup modal tanpa
  men-submit form.
- **WHEN** modal Kebijakan Privasi terbuka,
  **THE SYSTEM SHALL** mencegah scroll pada halaman di belakang modal (body scroll lock).

### FR-4: CTA Paket → Scroll ke Form

- **WHEN** pengguna mengklik tombol "Mau Paket Ini" (pada card Eco-Simpel atau
  Eco-Family & Premium),
  **THE SYSTEM SHALL** melakukan smooth-scroll ke section form pendaftaran (Section 2).
- **WHEN** pengguna mengklik tombol "Claim Sekarang" (promo callout early-bird),
  **THE SYSTEM SHALL** melakukan smooth-scroll ke section form pendaftaran.
- **THE SYSTEM SHALL NOT** membuka halaman checkout atau flow pembayaran apa pun.

### FR-5: Tombol "Hubungi Kami"

- **WHEN** pengguna mengklik tombol "Hubungi Kami" di header,
  **THE SYSTEM SHALL** membuka tab/window baru ke URL WhatsApp:
  `https://wa.me/6281234567890?text=Halo%20Kalaklin%2C%20saya%20ingin%20cek%20apakah%20alamat%20saya%20masuk%20area%20layanan%20dan%20mengetahui%20paket%20langganan%20yang%20tersedia.`
- Nomor `6281234567890` **HARUS** ditandai sebagai placeholder yang mudah ditemukan
  dan diganti (didefinisikan sebagai konstanta/env variable).

### FR-6: FAQ Accordion

- **WHEN** pengguna mengklik header item FAQ,
  **THE SYSTEM SHALL** membuka/menutup konten item tersebut dengan animasi
  expand/collapse yang halus.
- **WHEN** item FAQ dibuka,
  **THE SYSTEM SHALL** menerima input keyboard (Enter / Space) untuk toggle.
- **WHEN** satu item FAQ terbuka dan pengguna membuka item lain,
  **THE SYSTEM SHALL** menutup item yang sebelumnya terbuka (single-open accordion).

### FR-7: Analytics Hooks (Placeholder)

**THE SYSTEM SHALL** menyertakan komentar placeholder event-tracking pada titik-titik berikut,
sehingga mudah dihubungkan ke GA4 / Meta Pixel di kemudian hari:

| Event Name | Trigger |
|---|---|
| `cta_whatsapp_click` | Klik "Hubungi Kami" |
| `form_submit_success` | Submit form berhasil |
| `cta_package_click` | Klik "Mau Paket Ini" atau "Claim Sekarang" |
| `faq_open` | Buka item FAQ (sertakan indeks/label item) |
| `scroll_depth_pricing` | Scroll mencapai section #paket |
| `scroll_depth_faq` | Scroll mencapai section #faq |

### FR-8: Single-Page Constraint

- **THE SYSTEM SHALL** menyajikan seluruh konten dalam satu halaman dengan navigasi anchor.
- **THE SYSTEM SHALL NOT** mengimplementasikan client-side routing multi-halaman.
- **THE SYSTEM SHALL NOT** mengandung flow pembayaran, login akun, atau ajakan
  mengunduh aplikasi di mana pun dalam halaman.

---

## Non-Functional Requirements

### NFR-1: Responsiveness

- **THE SYSTEM SHALL** mengimplementasikan layout mobile-first dengan breakpoint:
  - Mobile: < 640px
  - Tablet: 640px – 1023px
  - Desktop: ≥ 1024px
- **THE SYSTEM SHALL** memastikan semua tap target (tombol, accordion header, nav link)
  memiliki area klik minimal 44 × 44px di mobile.
- **THE SYSTEM SHALL** menampilkan layout dua kolom (VP+Form, About) sebagai satu kolom
  pada breakpoint mobile dan tablet.
- **THE SYSTEM SHALL** menampilkan card pricing dan card masalah secara horizontal di
  desktop dan stacked vertikal di mobile (tanpa horizontal scroll).
- **THE SYSTEM SHALL** menampilkan alur Cara Kerja 6-langkah secara horizontal di desktop
  dan stacked vertikal di mobile.

### NFR-2: Performa

- **THE SYSTEM SHALL** memuat Largest Contentful Paint (LCP) < 2,5 detik pada koneksi 4G
  (simulasi Lighthouse).
- **THE SYSTEM SHALL** menggunakan format gambar modern (WebP) atau SVG untuk aset
  non-foto; gambar raster dikompresi.
- **THE SYSTEM SHALL** lazy-load gambar yang berada di bawah fold.

### NFR-3: Aksesibilitas

- **THE SYSTEM SHALL** memenuhi standar WCAG 2.1 Level AA untuk kontras warna.
- **THE SYSTEM SHALL** menyertakan atribut `alt` yang deskriptif pada semua gambar.
- **THE SYSTEM SHALL** menggunakan elemen HTML semantik (`<header>`, `<nav>`, `<main>`,
  `<section>`, `<footer>`, `<h1>`–`<h3>`).
- **THE SYSTEM SHALL** memastikan modal dapat diakses dengan keyboard (focus trap,
  Escape untuk menutup).

### NFR-4: Maintainability

- **THE SYSTEM SHALL** mendefinisikan semua design token (warna, font-size, spacing)
  sebagai CSS custom properties atau Tailwind config — tidak ada nilai hardcode berulang.
- **THE SYSTEM SHALL** mendefinisikan nomor WhatsApp dan pesan pre-filled sebagai konstanta
  di satu lokasi (mudah diganti tanpa mencari di seluruh kode).
- **THE SYSTEM SHALL** menggunakan komponen reusable: `Accordion`, `Modal`, `Card`,
  `Section`, `CTAButton`.

---

## Constraints

1. Urutan section **TIDAK BOLEH** diubah dari wireframe yang telah disetujui.
2. Copy Bahasa Indonesia yang sudah final **TIDAK BOLEH** diparafrase atau diubah.
3. Tidak ada checkout / flow pembayaran.
4. Tidak ada routing multi-halaman.
5. Tidak ada kewajiban membuat akun atau mengunduh aplikasi.

---

## Acceptance Criteria (Page-Level)

- [ ] Nilai Kalaklin, model WhatsApp-only, dan radius 10 km Jakarta Barat terlihat
      jelas di hero / above-the-fold.
- [ ] 5 USP (garansi tepat waktu, tracking WhatsApp, 24/7, bebas plastik,
      eco/hypoallergenic) tampil sebelum section Paket.
- [ ] Semua CTA berfungsi sesuai FR-2, FR-4, FR-5.
- [ ] Form validasi inline bekerja tanpa `alert()`.
- [ ] Modal sukses muncul setelah submit dan mencegah submit ganda.
- [ ] Modal Kebijakan Privasi terpisah dari modal sukses.
- [ ] FAQ accordion keyboard-accessible.
- [ ] Semua anchor nav bekerja dengan smooth-scroll.
- [ ] Halaman usable dan benar secara visual di 375px, 768px, dan 1440px.
- [ ] Tidak ada flow pembayaran atau ajakan install aplikasi.
- [ ] Placeholder analytics event tersedia di semua titik yang didefinisikan di FR-7.
- [ ] Nomor WhatsApp dan placeholder aset mudah ditemukan dan diganti.
