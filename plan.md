# Bandung Sport — Rencana Belajar FE Lewat Produk Nyata

> Terakhir diperbarui: 2026-10-03
> Konteks: rencana 5 bulan belajar FE, dengan produk nyata sebagai kendaraan belajar.
> Prinsip: 70% membangun / 30% course sebagai referensi. Course itu peta, hands-on itu wilayahnya.

## Produk: apa dan kenapa

**Bandung Sport** — direktori olahraga di Bandung dengan filter budget, dari gratis sampai mahal:
- Cari tempat olga (gym, futsal, yoga, senam hamil, dll) di Bandung.
- Filter berdasarkan jenis olga, tier budget (gratis / murah / mahal), dan area.
- Detail per tempat: kisaran harga (termasuk estimasi "kalau ambil PT keluar berapa"), fasilitas, kontak/IG.
- v2 (FLAGSHIP): personalized suggestion — input param (kondisi, budget,
  area) → rekomendasi olahraga + venue + estimasi biaya. Contoh: ibu hamil
  → kelas yoga/senam hamil di mana + berapa. Engine v1-nya = filter yang
  dipandu (wizard); butuh data `tags` per venue (cocok-buat-siapa,
  ada-kelas, intensitas) yang dikurasi mulai sekarang.
  Guardrail: framing hati-hati ("opsi yang biasanya cocok — konsultasi
  dokter"), bukan klaim medis.

Terinspirasi dari aplikasi meal-planner di IG: pilih store → pilih budget → dapat menu + bahan belanja sebulan. Bandung Sport memakai pola yang sama untuk olahraga.

### Kenapa produk ini untuk belajar FE
- **Real user**: diri sendiri (pakai tiap hari), teman yang cari gym murah, komunitas olga Bandung di IG/Twitter. Tidak butuh ribuan user — puluhan orang yang balik lagi cukup untuk feedback loop.
- **Bukan kerjaan kantor**, tapi polanya identik dengan FE kantor: ini pada dasarnya mini-SRP (search, filter, listing card, detail page). Skill yang dibangun di sini langsung transfer untuk menge-vet FE di kerjaan.
- **Moat-nya data, bukan kode**: harga gym/PT/kelas tidak ada API-nya — dikurasi manual. Itu yang susah ditiru orang.

## Definisi v1 (target 1–2 minggu)

Ruang lingkup ketat, jangan melebar:
- [x] Halaman list: card per tempat + filter (jenis olga, tier budget, area).
- [x] Detail page per tempat: harga, fasilitas, kontak/IG.
  (Catatan 2026-10-04: halaman + harga + kontak/IG sudah jalan dengan
  routing; field "fasilitas" menyusul saat kurasi tags — schema belum
  punya field-nya.)
- [ ] Mobile-first. Tanpa login, tanpa booking, tanpa peta.
- [ ] Data dari file JSON dulu (belum perlu database).

Isi data awal: **satu kategori + satu area saja**, misal 20–30 tempat gym/futsal. Jangan janji komprehensif — tambah kategori bertahap setelah v1 dipakai.

## Peta 5 bulan

### Bulan 1 — Fondasi + versi statis produk
- Dasar HTML/CSS/JS modern, DOM, DevTools.
- Vite + React dasar.
- **Output produk**: versi statis halaman list Bandung Sport (data hardcode).
- Course sebagai peta saja; jangan binge. Setiap konsep langsung dipakai di produk.

### Bulan 2 — Filter + data fetching + BFF sendiri
- State management untuk filter (cukup state React + URL param dulu).
- Data fetching: loading/error state, caching dasar (TanStack Query).
- **Output produk**: tulis BFF Go kecil (kekuatan existing: backend) yang menyajikan data JSON; FE memanggil BFF-nya. Filter berfungsi penuh.

### Bulan 3 — Membaca FE produksi + polish
- Baca satu codebase FE kantor (portal-srp / customer-dashboard) untuk menyerap pattern nyata.
- Routing (detail page), responsive, accessibility dasar, error/loading state yang benar.
- **Output produk**: halaman detail + polish list page; form "usulkan tempat" (opsional).

### Bulan 4 — Deployment + composition
- Nginx, domain, HTTPS; konsep composition ala micro-frontend (replika kecil untuk produk sendiri).
- **Output produk**: Bandung Sport live di domain sendiri.

### Bulan 5 — Launch + iterasi + karier
- Launch ke komunitas olga Bandung (IG dll). Kumpulkan masukan, iterasi.
- Mulai interview sambil tetap bekerja; portofolio + cerita produk nyata (produk, data kurasi, keputusan FE) jadi bahan interview.
- **Output produk**: pengguna asli pertama + iterasi berbasis masukan.

## Revisi peta (2026-10-03) — milestone-based, evidence-driven

Evidence: proyek sebelumnya (koperasi-react) menunjukkan kapasitas eksekusi
3 minggu sampai beta deployed (auth + CRUD + pinjaman) — timeline bulanan
lama ternyata terlalu konservatif. Tapi koperasi juga membuktikan: deployed
tanpa value density + distribusi = proyek mati (gap 5,5 bulan). Maka:

1. **Struktur diganti milestone-based, bukan bulan-based:**
   - v1.0 "worth showing" — detail page + data 40–60 venue terkurasi
     (dengan `tags`) + deploy statis (Vercel/Netlify) + dipakai 5–10
     orang terdekat (soft validation).
   - v1.5 "worth sharing" — feedback 5–10 orang dipakai untuk perbaikan,
     baru distribusi publik (IG).
   - BFF Go + fetching = jalur belajar, jalan paralel, bukan blocker produk.
2. **Deploy ngikutin bar "worth it", bukan tanggal.**
3. **Guardrail tetap:** fitur baru (search, login, peta, booking) tetap
   dilarang sampai ada user asli. Yang digelar: kedalaman (data + tags,
   detail, polish) dan distribusi — pelajaran langsung dari koperasi.
4. **Checklist review ulang:** pas v1 feature-complete (detail page jadi),
   evaluasi ulang timeline pakai data kecepatan aktual termasuk kerja BFF.

## Hasil board review wizard (2026-10-11) — dewan GO-WITH-CONDITIONS

Board review (5 role: 2 investor, CEO, Product Lead, CTO) atas spec
wizard "Rekomendasi buat saya": **GO-WITH-CONDITIONS, konsensus.**
Spec final RATIFIED: `docs/spec/wizard-rekomendasi.md` v0.3 + Logic
Spec v1 (pure matching module, teruji, portabel ke BFF).

Keputusan kunci:
1. **General first** — segmentasi orang (usia/kondisi) ditunda sampai
   tags terkurasi + bukti concierge; usia TIDAK jadi input MVP.
2. **Matching MVP**: rank by jarak dari area tinggal user (coords
   di-parse dari sourceUrl Google Maps) + budget ternormalisasi
   (aturan konversi WAJIB terkunci+tested sebelum logic ditulis).
3. **Demand gate**: concierge test — kirim rekomendasi MANUAL via WA
   ke 5 orang; ≥3 bertindak → wizard dibangun. Gagal → persevere di
   directory, wizard dibunuh murah.
4. **Launch blockers**: data validation script (tier↔harga, koordinat,
   lastVerified), empty-state wizard, event logging minimal client-side.
5. **Urutan kerja (serial, founder solo):** minggu 1–3 = data pass 40–60
   venue + deploy + intro 5–10 teman + 5 wawancara + concierge WA →
   demand gate → wizard build (2–3 minggu).
6. **Density beats breadth**: refresh bulanan diukur; >2 malam per
   refresh → persempit coverage, jangan tambah wilayah.
7. Wizard output shareable via URL. WHO/guidelines = konten dikutip,
   tidak pernah logic; "describe venues, never assess a person".

## Arah baru v2 (2026-10-13) — SUMBER KEBENARAN SEKARANG: `docs/plan-v2.md`

- Pemilik repo tidak puas dengan arah wizard v0.3 + plan polish UI yang
  terlalu berat. Hasil brainstorm (fasilitator + 3 agen: produk/pasar,
  CTO data model, CEO sequencing) ditulis di **`docs/plan-v2.md`**.
  Dokumen itu **menggantikan** § "Definisi v1" dan spec wizard
  `docs/spec/wizard-rekomendasi.md` (diarsipkan, tidak dibangun).
- Visi: **pemandu olahraga Bandung untuk semua orang**, multi-mode
  (`/sendiri` Hitung biaya, `/bareng` Patungan, `/komunitas` Mabar minggu
  ini, `/anak`, `/khusus`). Data v2 = Tempat + Penyelenggara + Penawaran
  (`venues/offerings.json`; `organizers.json` baru di Fase 3), tier
  dihitung dari harga mentah per `(sport, priceUnit)` bukan disimpan,
  `priceVisibility public|on-request|unknown`. Metrik tunggal: klik
  "hubungi/gabung" per minggu (baru terukur Fase 1; Fase 0 = balasan
  teman).
- Guardrail direvisi: usia boleh sebagai atribut penawaran
  (`ageMin/ageMax`), mode medis-ish dibingkai "program yang tersedia";
  prinsip tetap "describe offerings, never assess a person"; tanpa
  login/booking sampai ada trigger terukur.
- Roadmap: Fase 0 Fondasi (12–16 malam, urutan: copy+outreach harga →
  token → formatPrice → skema → migrasi → list v2 → card → detail →
  input data → deploy → kirim ke 5 teman)
  → 1 Sendiri → 2 Bareng → 3 Komunitas → 4 Anak → 5 Khusus → 6 BFF Go
  (trigger-based). Mode baru hanya dibangun setelah ≥15 penawaran nyata.
- Eksekusi harian pindah ke tool AI yang lebih murah dalam **mode
  latihan** — instruksi untuk tool itu ada di `docs/plan-v2.md` § 9.
- Review UI 5 detik (board #1) tersimpan di
  `docs/review/ui-5-detik-board-review.md`; bagian yang masih dipakai
  sudah dilebur ke Fase 0.
- Plan v2 sudah direview ulang (2026-10-13): urutan malam dibalik (data
  dulu, polish belakangan), outreach harga ke venue mulai malam 1 karena
  asinkron (hanya 8/22 punya harga), tier dari harga mentah (bukan
  `monthlyCost`, biar futsal tidak kehilangan tier), organizers ditunda.
- **Tugas berikutnya: Fase 0 malam 1** (a: copy pass — tagline,
  `lang="id"`, title, label dimensi filter; b: kirim ≤10 pesan tanya
  harga ke venue) — lihat `docs/plan-v2.md` § 8.
- Belum ada yang di-commit/push dari sesi ini (docs di root bukan bagian
  repo `fe/`).

## Status (2026-10-17) — SUMBER KEBENARAN SEKARANG: `docs/plan-v3-delta.md`

> Tool AI: bagian ini yang kamu update tiap akhir sesi (3–6 baris).
> Sesi aktif = checkbox pertama yang kosong di `docs/plan-v3-delta.md` § 2.

- Konteks baru: pemilik repo di-PHK 15 Okt 2026, sedang cari kerja.
  Proyek ini sekarang punya tujuan ganda: belajar FE + bukti full-stack
  (FE React + BE Go live) dalam ≤4 minggu. Jatah proyek = setelah 75 menit
  wajib lamaran + English per hari.
- Board review final (5 peran, 2026-10-17) → plan-v3-delta disepakati:
  BFF Go dimajukan ke sesi 6–8 (langsung setelah data v2), dua mode
  dikunci 3 bulan (Sendiri, Bareng), concierge jadi side task bukan gate,
  hasil alat bantu = URL tanpa harga + CTA di halaman tujuan, coming-soon
  di bulan ke-2. Urutan malam plan-v2 § 8 digantikan checklist sesi v3 § 2.
- Laptop baru: `fe/` di-clone ulang (HEAD `0e0faf2`, bersih). `be/` belum
  ada. Docs root belum di-commit.
- **Sesi berikutnya: S0** (concierge 5 orang, 30 menit, di luar sesi) lalu
  **S1** (copy pass + outreach harga ke ≤10 venue) — detail di
  `docs/plan-v3-delta.md` § 2 dan § 6.
- **Sesi 2026-10-18 — S2 Token CSS SELESAI** (`fe@b61ec6e`): 14 token di
  `fe/src/styles/tokens.css`; literal warna/shadow → `var()`, `:root`
  duplikat + `.modal*` (kode mati) dihapus. Materi FE: design token (peran
  vs nilai), custom property/cascade, kode mati, "build hijau ≠ CSS benar"
  (2 bug lolos grep: `white` keyword + `rgba()` ganda). Verifikasi: 0 hex di
  luar `:root`, 14/14 token terpakai, lint+build hijau; lockfile yang
  sempat ter-churn dikembalikan. Utang: S0 (→S15) & S1 ditunda; titik koma
  `main.jsx:4`. Sesi berikutnya: **S3 `src/lib/formatPrice.js`**.
- **Sesi 2026-10-21 — S3 `formatPrice` SELESAI** (`fe@2f8f528`): fungsi
  murni `src/lib/formatPrice.js` + `PRICE_UNIT_LABEL` (6/7 unit; `free`
  tanpa label karena harga 0 → "Gratis"), `Intl.NumberFormat('id-ID')`
  eksplisit. 6 kasus uji di `scripts/check-format-price.mjs` (disposable,
  diganti Vitest di S17). 3 cabang duplikat di `Place.jsx` &
  `PlaceDetail.jsx` → 1 panggilan; shim `priceVisibility` ditandai
  `// temporary until v2 (S5)`. Materi FE: pure function, DRY,
  Intl/locale eksplisit, sinkron enum↔key map.
  Utang: `Intl.NumberFormat` dibuat per-call; `PlaceDetail` menghitung
  `priceFormatted` sebelum guard `!place`. Sesi berikutnya: **S4 — skema
  v2 (`types.js` + `tiers.js`)**.
- **Sesi 2026-10-21 — S4 Skema v2 SELESAI** (`fe@c435385`): `types.js` =
  typedef `Venue` + `Offering` + `ScheduleSlot` + enum terkunci (union
  literal); `tiers.js` = `TIER_RULES` (5 sport, ambang tebakan awal).
  Founder meminta AI menulis file-nya (kontrak data, bukan kode render);
  alasan field dibahas bersama: `sports` array, `lat/lng` `number|null`,
  `priceVisibility` 3 status, `venueId`/`organizerId` minimal satu,
  `lastVerified` tanggal. Keputusan: tetap JS + JSDoc, TS ditinjau
  setelah Fase 2. Sesi berikutnya: **S5 — migrasi + validator** (AI tulis
  script, founder jalankan & baca diff).
- **Sesi 2026-10-22 — S5 Migrasi + Validasi SELESAI** (`fe@bdc0462`):
  `venues.json` (22) + `offerings.json` (8) menggantikan bentuk v1; 11
  venue berkoordinat (regex `!3d…!4d…`), 11 null sampai S14; 14 venue
  tanpa offering = warning. `scripts/validate-data.mjs` + `npm run
  validate` jadi pre-hook `build` (validator hijau, 16 warning). Script
  migrasi dijalankan sekali lalu dihapus (disposable). `places.json`
  dibiarkan sampai S9 supaya list page masih render. Materi FE: migrasi
  data vs ubah kode, normalisasi, validator sebagai jaring pengaman
  sebelum BFF. Sesi berikutnya: **S6 — repo `be/`, struct Go + loader +
  `Validate()`** (founder tulis Go, AI review).
- **Sesi 2026-10-22 — S6 Katalog Go SELESAI** (`be@79f59db`): repo `be/`
  di-`git init` + remote `jakskal/bandung-sport-be` (sudah di-push;
  modul `github.com/jakskal/bandung-sport-be`). `internal/catalog`:
  `model.go`, `load.go`
  (`Load(fs.FS)` + indeks), `validate.go` (`Validate` + `Warnings`, port
  validator Node — 0 error/16 warning, paritas), `tier.go` (`TIER_RULES`,
  `Tier`, `PriceFrom`); `data/embed.go` + 2 JSON. Tes 21 kasus, `go test`
  + `go vet` hijau. Materi: pointer untuk null, `go:embed`, struct tag,
  tabel tes. Catatan: kode Go ditulis AI atas permintaan founder; review
  founder belum dilakukan. Sesi
  berikutnya: **S7 — HTTP server + CORS + Dockerfile**.
- **Sesi 2026-10-22 — S7 HTTP API SELESAI** (`be@c471fa2`): 3 endpoint
  (`/healthz`, `/api/venues`, `/api/venues/{id}`), JSON error body,
  405 + `Allow`, CORS preflight 204 hanya untuk `ALLOWED_ORIGINS`,
  `Validate()` saat startup (fail fast), `log/slog`, graceful shutdown.
  Dockerfile multi-stage golang→distroless nonroot, `CGO_ENABLED=0`;
  `Makefile run/build`; README endpoint + env. Tes 9 kasus `httptest`.
  Verifikasi: `go test`/`go vet` hijau; container jalan, `curl
  /api/venues` = 22, detail FTL `Sedang`. Materi: graceful shutdown,
  pattern routing `{id}`, CORS preflight. **S8 ditunda** (keputusan
  2026-10-22): deploy BE digeser ke sebelum S13, karena S9–S12 cukup
  pakai API lokal. Sesi berikutnya: **S9 — FE baca API**.
- **Sesi 2026-10-26 — S9 FE baca API SELESAI** (`fe@d28e599`):
  `api/client.js` + `hooks/useFetch.js` (3 fase, `AbortController`,
  `retry`; `loading` diturunkan saat render + jebakan state fungsi
  `() => …`), `ListPage` skeleton + error \"Coba lagi\", filter URL
  `olahraga`/`tier`/`area`, card pakai `priceFrom`/`tier` API,
  `Place.jsx` v2, route `/tempat/:venueId`, `PlaceDetail` basic dari API.
  Bersih-bersih: 3 JSON, `tiers.js`, `validate-data.mjs` + hook `validate`
  di build dihapus; `src/data/` tinggal `types.js`. Verifikasi: lint+build
  hijau, `grep 'data/.*\\.json' src` kosong, 404 asing OK. Sesi
  berikutnya: **S10 — detail page v2 dari API**.
- **Sesi 2026-10-28 — S10 Detail page v2 SELESAI** (`fe@51252d5`):
  `PriceTag` (satu sumber format harga), `OfferingRow` (judul + harga +
  "Harga dicek {bulan tahun}" hanya bila `lastVerified` != null), CTA WA
  full-width, Maps/IG/Website berlabel, empty state "Belum ada paket
  terverifikasi". Data BE: `lastVerified = 2026-10-22` untuk 8 penawaran
  berharga (`be@fa1e254`). Jebakan yang dilewati: `formatPrice` menerima
  objek (bukan positional args), `priceFrom` objek harus di-spread,
  `priceVisibility` undefined = fallback 'unknown'. Lint+build hijau.
  Sesi berikutnya: **S11 — hierarki card**.
- **Sesi 2026-10-28 — S11 Hierarki card SELESAI** (`fe@dea16b7`):
  token `--brand`/`--brand-soft`/`--fs-price`/`--fs-meta`/`--radius-card`,
  komponen `TierBadge` (pill), kartu disusun ulang: nama + badge → harga
  hero (`--fs-price`, 700) → meta "Olahraga · Area" kecil. Chip aktif
  pakai `--brand`. Aturan yang disepakati: `PriceTag` mengurus bentuk
  dirinya (weight/margin), halaman mengurus ukuran (`.place-card
  .price-tag`), supaya baris paket di detail tidak ikut jadi hero.
  Lint+build hijau. Sesi berikutnya: **S12 — header "ini apa"**.
- **Sesi 2026-10-29 — S12 BERJALAN, tes 5 detik belum lulus.** Kode header
  selesai (`fe@45d2b14`..`3738531`): komponen `Header/` + token `--fs-hero`,
  CTA anchor ke `<section id="tempat-list">`, `lang="id"` + `<title>` + meta
  description, jebakan `VITE_API_BASE_URL` → `VITE_API_URL` beres
  (`fe@7481833`). Copy dikunci di `docs/copy/header.md`: tagline "Bandung
  Sport — Direktori olahraga Bandung", subline "Kita bantu cari tempat
  olahraga — kamu tinggal pilih" (founder menolak kata "harga" di header —
  alasannya: user lihat harga di kartu). **Hasil tes 1 orang: "oh list
  doang"** = Q1 lulus (paham bentuknya), Q3 gagal (tidak terasa beda dari
  Google). Keputusan founder: tes screenshot **ditunda, dipindah ke S15**
  (reaksi pada link live lebih jujur daripada pada screenshot). S12 sengaja
  dibiarkan belum dicentang — kalau S15 nanti gagal, yang direvisi subline,
  bukan komponen. **S8 & S13 sudah live DAN dicentang** (diverifikasi
  2026-10-29 lewat curl + tes refresh HP oleh founder): FE
  `bandung-sport-web.vercel.app` (root 200, deep link `/tempat/v-urban-gym`
  200 = rewrite SPA jalan, bundle panggil BE production, `<title>`/meta
  identik dengan build HEAD) + BE `bandung-sport-be.vercel.app` (`/healthz`
  200). Target 4 minggu "FE React + BE Go live" tercapai hari ke-12.
  Keputusan founder: `ALLOWED_ORIGINS` yang masih ikut localhost dibiarkan
  (bukan utang). Vercel Analytics terpasang 2026-10-30 (`fe@bea6084`,
  `@vercel/analytics` + `<Analytics />`); pageview sampai & terlihat di
  dashboard. Sesi aktif: **S14 (input data)
  → S15 (kirim ke orang)**. Lint+build+`go test` hijau.
- **Sesi 2026-10-30 — badge tier bohong saat harga ada** (`be@bf45525` +
  `fe@8a79956`): `Tier()` dulu cuma menerima satu satuan per sport
  (`fitness` = monthly), jadi gym yang harganya cuma `per-visit` dapat tier
  null → badge mencetak "Belum dicek" padahal harga tampil. Sekarang band
  per `(sport, priceUnit)` sesuai plan-v2 § 6, `fitness` dapat band
  `per-visit` (35rb/75rb, tebakan) → Gym Pod, Komodoz, Diraya jadi `Murah`.
  `TierBadge` berhenti merender apa pun saat tier null (harga tetap tampil).
  Masih tidak bertier: Urban Gym (PT 12 sesi) & Peak Padel (per sesi) —
  satuannya `per-session-pack` dan durasi sesi belum diketahui.
- **Sesi 2026-10-30 — fix tampilan paket multi-sesi** (`be@a0ee5d8` +
  `fe@df49e44`): `Price.SessionsPerPack` ikut di `/api/venues`, `formatPrice`
  menerima `sessionsPerPack` dan mencetak "mulai Rp700rb /12 sesi". Urban Gym
  PT 12 sesi dikembalikan ke data (27 offering, 17 venue berharga).
  Sangkuriang 3 bulan tetap di luar karena enum belum punya `quarterly`.
- **Sesi 2026-10-30 — S14 bagian B sebagian terisi** (`be@7cca157`): 18
  offering baru dari riset web (total 26 offering; venue berharga 8 → 16 dari
  22), provenance di `docs/data/harga-riset-2026-10-30.md`. Semua
  `lastVerified` = null (scrape ≠ verifikasi) → harga tampil tanpa badge
  "Harga dicek". 3 baris dicoret saat review: DPC (sumber agregator, kutipan
  terpotong), Sangkuriang 3 bulan & Urban Gym PT 12 sesi (akan tampil "/sesi"
  padahal multi-sesi). Live terverifikasi: `/healthz` offerings 26, 16 venue
  punya `priceFrom` (angka ini naik jadi 27/17 setelah fix di atas). Belum dicentang: menunggu konfirmasi vendor untuk
  `lastVerified` (dan status Eldorado).
- **Sesi 2026-10-30 — S14 bagian A SELESAI: koordinat 22/22.** Semua venue
  punya `lat`/`lng` (`be@b8f977b` 10 venue dari link Google Maps founder,
  `be@9cd3107` Oasis Siliwangi setelah dipastikan sama dengan pin "Siliwangi
  Swimming Pool"). Live terverifikasi: `/api/venues` → 22 venue, 22
  berkoordinat. `go test` + `go vet` hijau. **S14 belum dicentang**: bagian B
  (penawaran baru dari outreach) masih menunggu balasan vendor. Keputusan
  founder: Lembang & Cimahi tetap dianggap "Bandung" untuk `area` — jangan
  diusulkan diganti/dipindah.

## Nanti (ide yang sengaja ditunda — jangan dikerjakan)

- Enum `priceUnit` belum punya `quarterly` (paket 3 bulan) dan
  `per-session` (court padel "per sesi"). Akibatnya Sangkuriang 3 bulan
  630rb belum bisa masuk data — kalau dipaksa `per-session-pack` ia tampil
  "/3 sesi" padahal 3 bulan. Saat enum ditambah, masukkan baris itu.
  (`sessionsPerPack` sendiri sudah didukung: `be@a0ee5d8` + `fe@df49e44`.)

## Aturan main (mencegah kegagalan yang lalu)

1. **Jangan nunggu "ngerti dulu baru bikin"** — bikin dulu, yang nggak ngerti dicari pas macet.
2. AI dipakai sebagai tutor (minta latihan, minta penjelasan), bukan implementer — dengan satu pengecualian: boleh minta AI setup tooling awal (Vite, dsb).
3. Kalau stuck >30 menit di satu hal, tulis pertanyaan spesifik, minta AI jelaskan, lanjut.
4. Scope v1 tidak boleh ditambah sebelum v1 dipakai orang.
5. Ukuran waktu: konsisten kecil (misal 45–60 menit/hari) lebih baik daripada weekend marathon.

## Catatan pribadi

- Kelas semacam "Frontend Master" pernah dicoba dan tidak "kena" tanpa hands-on — valid, dan memang metode ini menaruh hands-on di depan.
- Kebiasaan lama: merasa harus paham konsep penuh sebelum mulai. Terapi: mulai dari versi paling kecil yang bisa dilihat di browser.
