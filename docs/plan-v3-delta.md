# Plan v3 (delta) — Bandung Sport: full-stack dalam 4 minggu, dua mode dalam 3 bulan

> Status: DISEPAKATI pemilik repo 2026-10-17 setelah board review final
> (5 peran). Dokumen ini **mengubah urutan** `docs/plan-v2.md`, bukan
> menggantinya: visi (§ 0–1), model data (§ 6), arsitektur FE (§ 7), dan
> ritual (§ 10) plan-v2 tetap berlaku. Kalau dua dokumen bertentangan,
> **dokumen ini menang**.
>
> Cara pakai untuk tool AI (VS Code, mode latihan): baca § 1, § 4, § 5,
> § 7, § 8 di sini, lalu `docs/plan-v2.md` § 6. Kerjakan **hanya sesi
> yang ditunjuk** di § 2 (checkbox pertama yang belum dicentang), satu
> sesi per percakapan.

## 0. Konteks yang mengubah rencana

Pemilik repo di-PHK 15 Okt 2026 dan sedang cari kerja (banyak lowongan
full-stack). Waktu untuk proyek ini: sisa hari **setelah** 75 menit wajib
(lamaran + latihan English). Proyek ini satu-satunya sumber semangat —
jangan dipotong, tapi jangan jadi pelarian dari cari kerja.

Dua tujuan yang harus tercapai bersamaan:

1. **Bukti full-stack di CV dalam ≤4 minggu**: URL live, repo FE + repo BE
   Go dengan README berscreenshot.
2. **Belajar FE sungguhan**: kode yang dirender tetap ditulis founder.

## 1. Lima perubahan (delta) + syarat board

| # | Perubahan | Syarat board |
|---|---|---|
| 1 | **BFF Go dimajukan** dari Fase 6 ke **langsung setelah data v2 jadi** (sesi 5). | Go stdlib `net/http` + `go:embed` JSON, 2 endpoint data + `/healthz`, CORS, tes table-driven, Dockerfile, **≤3 sesi**. Host gratis tanpa cold start berat (Vercel Go preset, lihat S8). Setelah FE baca API, **JSON dihapus dari bundle FE** — BE satu-satunya sumber data. |
| 2 | **Dua mode dikunci 3 bulan**: Sendiri (Hitung Biaya) + Bareng (Patungan). Komunitas/Anak/Khusus ditunda. | Bulat. |
| 3 | **Concierge test = side task, bukan gate.** Kirim 5 rekomendasi manual via WA minggu 1, lanjut ngoding tanpa menunggu balasan. Hasilnya dipakai minggu 3 untuk mengurutkan Patungan vs penyempurnaan Hitung Biaya. | "Bertindak" = menghubungi venue atau datang; ≥2 dari 5 bukan teman dekat; tanya perilaku masa lalu, bukan niat. Catatan jujur: **Patungan dibangun karena alasan belajar**, bukan karena bukti permintaan. |
| 4 | **Hasil alat bantu = URL.** Setiap hasil Hitung Biaya / Patungan terepresentasi penuh di URL; tombol "Bagikan" = teks + URL ke WhatsApp. Share loop utama = hasil Patungan ke grup WA. | Kontrak URL § 5: slug stabil + `v=1`, **harga tidak pernah di URL** (dihitung ulang dari data), fallback kalau param rusak. Setiap penawaran tampilkan tanggal "harga dicek". Patungan dikasih **CTA di halaman tujuan** dan klik penerima diukur. |
| 5 | **Kartu "coming soon"** untuk Komunitas/Anak/Khusus, masing-masing satu aksi terukur ("Kabari saya"). | Dipasang **bulan ke-2**, bukan sekarang. Urutan kartu diacak (bias posisi). Klik "Kabari saya" sinyal lemah — hanya penentu urutan mode ke-3, bukan bukti permintaan. |

**Tambahan board (disetujui):** build log mingguan Bahasa Inggris ≤300
kata di LinkedIn (angka + 1 keputusan arsitektur) — sekaligus latihan
English, bukti ke recruiter, kanal distribusi pertama. Review Jumat 15
menit: lamaran terkirim / klik "hubungi" / 1 pelajaran (tracker:
`~/angling/cv/log.md`).

## 2. TODO — checklist sesi (1 sesi ≈ 90–120 menit)

Legenda: **F** = founder menulis, **A** = AI menulis penuh, **R** = AI
review saja. ★ = wajar 2 sesi. Nomor sesi = urutan, bukan kalender.
Target: 5 sesi/minggu. Tool AI: kerjakan checkbox pertama yang kosong,
centang kalau "selesai kalau" terpenuhi, lalu update § Status di
`plan.md`.

### Minggu 1 — Fondasi data (plan-v2 § 8 malam 1–5, tanpa perubahan)

- [ ] **S0 (hari ini, 30 menit, F, di luar sesi)** Concierge: kirim pesan
  ke 5 orang (≥2 bukan teman dekat; 2 ngegym, 2 futsal/padel, 1 bebas).
  Template di § 6. Catat di `docs/data/concierge.md`: nama, tanggal, apa
  yang direkomendasikan, jawaban "terakhir olahraga bareng siapa bayar
  apa", dan kolom **bertindak? (hubungi/datang/tidak)** diisi belakangan.
  _(ditunda 2026-10-18 oleh founder → dilipat ke S15.)_
- [ ] **S1 — Copy pass + outreach harga** (F, R; A tulis template pesan).
  _(ditunda 2026-10-18 oleh founder — masih utang.)_
  Tagline + sub di header, `lang="id"`, `<title>`, meta description,
  label dimensi chip "Olahraga / Budget / Area", "Belum dikurasi" →
  "Harga belum dicek", empty state copy. Kirim ≤10 pesan tanya harga ke
  venue yang harganya null; catat di `docs/data/outreach.md`.
  *Selesai kalau:* tagline terbaca, lint+build hijau, ≥8 pesan terkirim.
- [x] **S2 — Token CSS** (A tulis file token; F terapkan). ±12 custom
  property di `:root`, hapus `.modal*`, hex literal hanya di `:root`.
  *Selesai kalau:* `grep -rn '#[0-9a-fA-F]\{3,6\}' fe/src --include=*.css`
  hanya kena baris di `:root`.
  _(Selesai 2026-10-18: 14 token di `fe/src/styles/tokens.css`; 0 hex &
  0 keyword warna di luar `:root`, semua token terpakai, lint+build hijau.)_
- [x] **S3 — `src/lib/formatPrice.js`** (F; A tulis 6 kasus uji) signature
  v2: `formatPrice({ priceMin, priceMax, priceUnit, priceVisibility })`
  → "Rp500rb–2,5jt /bulan", "mulai Rp50rb /jam", "Harga via WA", "Harga
  belum dicek". Dipanggil dari `Place` & `PlaceDetail` menggantikan 3
  cabang duplikat. *Selesai kalau:* 6 kasus benar, duplikasi hilang.
  _(Selesai 2026-10-21: 6/6 kasus di `scripts/check-format-price.mjs`;
  `PRICE_UNIT_LABEL` 6/7 unit — `free` tanpa label karena harga 0 →
  "Gratis"; duplikasi cabang hilang.)_
- [x] **S4 — Skema v2** (F; R). `fe/src/data/types.js` (JSDoc typedef
  Venue + Offering, tanpa organizers) + `TIER_RULES` (`tiers.js`).
  Founder jelaskan alasan tiap field. **Catatan v3:** typedef ini nanti
  dipindah jadi struct Go di S6; di FE tetap ada sebagai dokumentasi
  bentuk response API. *Selesai kalau:* typedef disetujui.
  _(Selesai 2026-10-21, `fe@c435385`: `types.js` = `Venue` + `Offering`
  + `ScheduleSlot` + 5 enum union (`Sport`, `Audience`, `OfferingType`,
  `PriceUnit`, `JoinMethod`); `tiers.js` = `TIER_RULES` 5 sport. File
  diketik AI atas permintaan founder (kontrak data = tooling, bukan kode
  render); alasan field kunci dibahas bersama: `sports` array (Sabuga),
  `lat/lng` `number|null` (11/22 venue belum punya koordinat; link Maps
  diturunkan dari koordinat, bukan field baru), `priceVisibility` (publik
  vs ditahan venue vs belum dicek), `venueId`+`organizerId` minimal satu
  terisi, `lastVerified` tanggal bukan boolean. Keputusan: tetap JS +
  JSDoc, TS ditinjau setelah Fase 2.)_
- [x] **S5 — Migrasi + validasi** (A tulis; F jalankan & baca diff).
  `scripts/migrate-v1-to-v2.mjs` (sekali jalan lalu hapus) →
  `venues.json` + `offerings.json`; `scripts/validate-data.mjs`
  (aturan di plan-v2 § 6). Script Node ini **disposable** — di S6
  aturannya diport ke Go test, lalu script Node dihapus di S9. *Selesai
  kalau:* validator hijau; 22 venue + ≥8 offering; lat/lng terisi
  untuk venue yang `sourceUrl`-nya Google Maps.
  _(Selesai 2026-10-22, `fe@bdc0462`: 22 venue + 8 offering; 11 venue
  berkoordinat, 11 null sampai S14; 14 venue tanpa offering = warning
  (TODO kurasi, bukan error). `validate-data.mjs` + `npm run validate`
  jadi pre-hook `build`; script migrasi dijalankan sekali lalu dihapus.
  Keputusan migrasi: `lastVerified` null (tanggal scrape ≠ verifikasi),
  `tier` dibuang (dihitung dari `TIER_RULES`), `places.json` dibiarkan
  sampai S9 supaya list page tetap render, `joinMethod` hanya `["wa"]`
  kalau nomor WA dipublikasikan, `offeringType` diturunkan dari
  (sport, unit).)_

### Minggu 2 — BFF Go (≤3 sesi) + FE baca API

- [x] **S6 — Repo `be/`, model, loader, validasi** (F menulis Go — ini
  keahliannya; A review + tulis `Makefile`/skeleton tes kalau diminta).
  `git init` di `be/`, remote baru `jakskal/bandung-sport-be`.
  `go mod init github.com/jakskal/bandung-sport-be` (Go ≥1.22, pakai
  pattern routing stdlib). Copy `venues.json` + `offerings.json` ke
  `be/data/`. Paket `internal/catalog`: struct `Venue`, `Offering`
  (field = § 6 plan-v2, tag JSON camelCase), baca dari `data.FS`
  (`be/data/embed.go`, lihat § 8), `Load(fsys fs.FS) (*Catalog, error)`, `Validate() []error` (port semua aturan
  validate-data.mjs), `Tier(venue)` dari `TIER_RULES`, `PriceFrom(venue)`
  = penawaran `public` termurah. `catalog_test.go`: tes table-driven
  untuk `Tier`, `PriceFrom`, dan **satu tes `TestDataIsValid`** yang
  memuat data embed dan gagal kalau `Validate()` tidak kosong (ini
  validator permanennya). *Selesai kalau:* `go test ./...` hijau,
  `go vet` bersih.
  _(Selesai 2026-10-22, `be@79f59db`: `model.go` (Venue/Offering/
  ScheduleSlot/Price, pointer untuk field nullable), `load.go`
  (`Load(fs.FS)` + indeks id→venue & venue→offerings), `validate.go`
  (`Validate` + `Warnings`, port aturan validator Node), `tier.go`
  (`TIER_RULES`, `Tier`, `PriceFrom`), `data/embed.go`; `Makefile` +
  README. Tes: `TestTier` 14 kasus, `TestPriceFrom` 5,
  `TestLoadIndexes`, `TestDataIsValid` (22 venue, ≥8 offering);
  `go test ./...` + `go vet` hijau. Paritas dengan validator Node:
  0 error, 16 warning sama. **Catatan:** kode Go sesi ini ditulis AI atas
  permintaan founder (aturan default: founder menulis); review founder
  belum dilakukan. Repo GitHub `jakskal/bandung-sport-be` sudah dibuat
  (initial commit + MIT) dan `be@79f59db` sudah di-push.)_
- [x] **S7 — HTTP server + CORS + Dockerfile** (F; A boleh tulis CORS
  middleware + Dockerfile + README skeleton). Endpoint § 4. `cmd/api/
  main.go`: `PORT` env (default 8080), `ALLOWED_ORIGINS` env (comma
  separated), graceful shutdown, `log/slog`. `internal/httpapi`:
  handler + `httptest` table-driven (200 list, 200 detail, 404 id
  asing, 405 method, CORS preflight). Dockerfile multi-stage
  (`golang:1.2x-alpine` → `gcr.io/distroless/static`), `CGO_ENABLED=0`.
  *Selesai kalau:* `docker build` + `docker run -p 8080:8080` lalu
  `curl localhost:8080/api/venues | jq length` = 22.
  _(Selesai 2026-10-22, `be@c471fa2`: `cmd/api/main.go` = PORT +
  ALLOWED_ORIGINS + slog + timeout server + graceful shutdown;
  `Validate()` dijalankan saat startup (exit 1 kalau data rusak).
  `internal/httpapi` = 3 route + `{id}`, JSON error body, 405 + `Allow`,
  CORS preflight 204 hanya untuk origin terdaftar. `Dockerfile`
  multi-stage (golang:1.23-alpine → distroless static nonroot,
  CGO_ENABLED=0) + `.dockerignore`; `Makefile` `run`/`build`; README
  endpoint + env. Tes: 9 kasus `httptest`. Verifikasi: `go test`/`go vet`
  hijau, `docker build` + `docker run -p 18081:8080` →
  `curl /api/venues` length 22, detail FTL = `Sedang` + `priceFrom`.)_
- [x] **S8 — Deploy BE ke Vercel (Go preset, gratis, tanpa kartu)** (F; A
  jelaskan config). **Ditunda 2026-10-22 (keputusan founder): dikerjakan
  sebelum S13, bukan sekarang.** S9–S12 cukup pakai API lokal
  (`http://localhost:8080`); S13 (`VITE_API_URL`) dan S15 (URL live)
  baru butuh URL publik. Jangan sampai masalah deploy baru ketahuan di
  hari deploy — kerjakan minimal satu sesi sebelum S13. Vercel mendeteksi `go.mod` di root repo + entrypoint
  `cmd/api/main.go`, server wajib listen di env `PORT`. Tambah
  `vercel.json`: `{ "framework": "go" }`. Import repo `bandung-sport-be`
  ke project Vercel baru, env `ALLOWED_ORIGINS=http://localhost:5173`.
  Catat URL di `be/README.md`. Dockerfile dari S7 tetap disimpan (untuk
  lokal + portofolio), bukan untuk deploy. Cold start Go ~0,3–1 dtk
  diterima; FE wajib punya skeleton/loading. **Tanpa database**: data
  `go:embed`, update data = commit + auto-deploy. Cadangan kalau Vercel
  Go bermasalah: Koyeb free (container dari Dockerfile).
  *Selesai kalau:* `curl <url>/healthz` 200 dari HP (data seluler).
  *Dikerjakan & lulus 2026-10-29:* live di **`https://bandung-sport-be.vercel.app`**
  — `/healthz` 200 (`{"offerings":8,"status":"ok","venues":22}`),
  `/api/venues` 200 + header CORS muncul, `vercel.json` (`framework: go`)
  masuk di `be@946f739`, URL tercatat di `be/README.md` § Deploy, founder
  konfirmasi tes HP oke. Baris `ALLOWED_ORIGINS=http://localhost:5173` di
  instruksi atas **sudah usang** — lihat keputusan di S13.
- [x] **S9 ★ — FE baca API (menggantikan plan-v2 malam 6)** (F; A beri 1
  contoh pola `useFetch`). `fe/.env.example` + `VITE_API_URL`;
  `src/api/client.js` (`fetchVenues()`, `fetchVenue(id)`, lempar error
  kalau `!res.ok`); `src/hooks/useFetch.js` (`{ data, error, loading }`,
  `AbortController` di cleanup); `ListPage` pakai `fetchVenues`, tampil
  loading skeleton + error state dengan tombol "Coba lagi"; filter URL
  tetap jalan (param `olahraga`/`tier`/`area`, nilai `sport` kanonik);
  harga card = `priceFrom` via `formatPrice`; tier dari response; route
  `/place/:id` → `/tempat/:venueId`, `parseInt` dibuang. **Hapus**
  `src/data/places.json`, `venues.json`, `offerings.json`,
  `scripts/validate-data.mjs` dari FE. *Selesai kalau:* share URL,
  refresh, back/forward OK; matikan BE → error state tampil, bukan
  layar putih; `grep -rn 'data/.*\.json' fe/src` kosong.
  *Materi FE:* `useEffect` + cleanup, promise/async, state mesin 3 fase
  (loading/error/data), env var Vite, CORS dari sisi browser.
  _(Dikerjakan ulang 2026-10-22 oleh founder: percobaan pertama ditulis
  AI lalu di-reset — FE adalah materi belajar founder, bukan bagian yang
  didelegasikan.)_
- [x] **S10 ★ — Detail page v2 dari API (plan-v2 malam 8)** (F, R).
  `VenueDetail` (`fetchVenue`), "← Semua tempat", nama + meta + badge
  tier, daftar paket `OfferingRow` + `PriceTag` (harga + unit +
  visibility), tiap paket tampil **"Harga dicek <bulan tahun>"** hanya
  jika `lastVerified != null`; tombol WA full-width ("Tanya harga via
  WhatsApp" bila tidak ada harga public), sekunder Maps/IG/Website
  berlabel; venue tanpa paket → empty state jujur; 404 untuk id asing
  dengan link balik. *Selesai kalau:* venue 3 paket = 3 baris; venue
  tanpa harga tampil benar; id asing → 404.

### Minggu 3 — Lolos tes 5 detik + live

- [x] **S11 — Hierarki card (plan-v2 malam 7)** (F; A beri 1 sketsa).
  Nama + badge tier (pill) → **harga hero** (`--fs-price`, bold) →
  "Olahraga · Area" kecil; card putih, border, radius; chip aktif =
  `--brand`. *Selesai kalau:* 1 teman menyebut harga tanpa ditanya.
- [ ] **S12 — Header yang menjawab "ini apa"** (F, R). Tagline besar +
  sub 1 kalimat + CTA tunggal "Lihat semua tempat" (anchor ke list);
  komposisi `Header` jadi komponen sendiri; mobile-first (≤390px dulu).
  Belum ada pemilih mode — mode baru ada setelah S17/S20. *Selesai
  kalau:* 2 dari 3 orang bisa jawab "ini buat apa" dalam 5 detik dari
  screenshot.
  *Catatan 2026-10-29 (dibuat, belum lulus):* `fe@45d2b14`..`3738531` —
  komponen `Header` + `--fs-hero` + CTA anchor + `<title>`/meta/`lang`
  selesai; copy di `docs/copy/header.md`. 1 responden: "oh list doang".
  Tes 5 detik **dipindah ke S15** (keputusan founder) — checkbox ini
  sengaja belum dicentang, jangan dianggap selesai.
- [x] **S13 — Deploy FE (Vercel, preset Vite)** (F; A jelaskan config).
  Import repo `bandung-sport-web` ke project Vercel kedua; build
  `npm run build`, output `dist`, env `VITE_API_URL` = URL BE Vercel.
  SPA fallback: `vercel.json` di `fe/` berisi
  `{ "rewrites": [{ "source": "/(.*)", "destination": "/index.html" }] }`.
  Analytics pageview: Vercel Web Analytics (gratis, 1 paket `@vercel/
  analytics` + 1 komponen) — event klik custom tetap di S20. Update
  `ALLOWED_ORIGINS` di project BE dengan origin FE (production + preview
  `*.vercel.app` kalau mau preview jalan).
  *Selesai kalau:* refresh di `/tempat/v-…` tidak 404 di HP.
  *Dikerjakan & lulus 2026-10-29:* live di
  **`https://bandung-sport-web.vercel.app`** — root 200, deep link
  `/tempat/v-urban-gym` 200 (rewrite SPA di `fe/vercel.json` berfungsi,
  ke-track `fe@7481833`), bundle produksi memanggil
  `bandung-sport-be.vercel.app` dan **nol** `localhost:8080` (berarti
  `VITE_API_URL` ter-set di project Vercel), `<title>` + meta live identik
  dengan build HEAD `fe@3738531`. Founder konfirmasi **tes refresh dari HP
  = oke** → gerbang terpenuhi.
  *Keputusan founder 2026-10-29:* `ALLOWED_ORIGINS` production yang masih
  ikut `http://localhost:5173` **dibiarkan** — bukan utang, jangan
  "diberesin" lagi. API ini data publik tanpa auth, CORS cuma penjaga
  browser. Konsekuensi yang diterima: `be/README.md` § Deploy ("production
  lists the frontend origin only") tidak persis mencerminkan dashboard.
  Belum diverifikasi: Vercel Web Analytics (S13 memintanya; event klik
  custom tetap S20).
- [ ] **S14 — Input data** (F, timebox 60 menit). Balasan outreach →
  `be/data/offerings.json` dengan `lastVerified` hanya kalau beneran
  dikonfirmasi; koordinat manual untuk venue sisa; `go test` hijau;
  redeploy BE. *Selesai kalau:* `go test ./...` hijau, data live.
- [ ] **S15 — Kirim ke orang + README + CV** (F; A beri template pesan &
  README). Tes 5 detik ke 1 orang → perbaiki → kirim ke 5 teman (boleh
  sama dengan peserta concierge) dengan 1 pertanyaan. README kedua repo:
  What/Why/Stack/Architecture (diagram 3 kotak FE → API → JSON embed)/
  Run/Test/Deploy + 2 screenshot HP. Tambah baris proyek di CV & LinkedIn
  ("Projects": Bandung Sport — React 19 SPA + Go API, deployed on Vercel, link).
  Build log #1 di LinkedIn. *Selesai kalau:* 5 terkirim, README live,
  CV terupdate. **← Tujuan 4 minggu tercapai di sini.**

### Minggu 4 — Buffer + mulai Mode Sendiri

- [ ] **S16 — Buffer** untuk slip dari ★ dan perbaikan dari respons
  teman. Kalau tidak ada slip: lanjut S17.
- [ ] **S17 — Vitest + `monthlyCost`** (A setup Vitest; F tulis fungsi +
  tes). `fe/src/lib/cost.js` sesuai tabel plan-v2 § 6 (W = 4,33;
  bulatkan 1000; visibility ≠ public → null). Tes: tiap unit + kasus
  null + pembulatan. *Selesai kalau:* `npm test` hijau ≥10 kasus.
- [ ] **S18 ★ — Route `/sendiri` + form** (F; A 1 contoh form terkontrol).
  `src/modes.js` (data), route `/:mode` divalidasi (slug asing →
  NotFound). Form: olahraga (select dari data), area, **kali/minggu**
  (1–7). Semua input di URL (`?olahraga=fitness&area=dago&kali=3`), form
  = controlled dari `useSearchParams`. *Selesai kalau:* refresh
  mempertahankan input.
- [ ] **S19 ★ — Hasil Hitung Biaya** (F, R). Daftar penawaran yang cocok
  terurut `monthlyCost` naik, tiap baris: nama tempat, paket, **biaya/
  bulan**, biaya/sesi, jarak kalau ada koordinat (haversine di
  `src/lib/geo.js`, area → centroid di `src/data/areas.js`). Harga
  `on-request` masuk kelompok terpisah "Harga via WA". Tombol
  "Bagikan" = `navigator.share` fallback `wa.me/?text=` berisi ringkasan
  + URL halaman. *Selesai kalau:* founder sendiri pakai untuk milih gym;
  URL hasil dibuka di HP lain menampilkan hasil sama.
- [ ] **S20 — Event klik** (A pasang; F baca). Umami cloud / GoatCounter:
  event `contact_click` (ContactLinks), `share_click`, `mode_open`.
  *Selesai kalau:* dashboard menampilkan 1 klik uji dari HP.

### Bulan 2 — Mode Bareng (Patungan) + coming soon

- [ ] **S21–S23 ★ — `/bareng` Patungan.** Form: olahraga (court-rental),
  orang, jam, hari/jam (prime/non-prime kalau data ada). Hasil: per
  lapangan **per orang per sesi**, terurut termurah; tombol "Bagikan ke
  grup". URL § 5 (`v=1`, slug tempat + paket, orang, jam). **Halaman
  hasil yang dibuka penerima** punya CTA: "Hubungi lapangan" (WA) dan
  "Hitung buat grup lain" → ini yang diukur (`contact_click` dengan
  `source=shared`).
- [ ] **S24 — Kartu "coming soon"** (F). Tiga kartu kecil di bawah dua
  mode nyata: Komunitas / Anak / Program khusus, urutan diacak saat
  render, satu tombol "Kabari saya" → `wa.me/<nomor founder>?text=
  Kabari saya kalau mode <X> jadi` (satu klik = satu baris chat, mudah
  dihitung). Event `interest_<mode>`.
- [ ] **S25–S28 — Data pass ke ≥50 penawaran** (1 dari 4 sesi, timebox
  60 menit), tambah badminton (court-rental), form lapor (Google Form)
  di detail page, ritual mingguan plan-v2 § 10.
- [ ] **Keputusan akhir bulan 2:** baca `concierge.md` + klik
  `contact_click` + `interest_*` → tentukan apakah mode ke-3 mulai di
  bulan 3 atau dua mode dipertebal datanya.

### Bulan 3 — Target: ≥50 penawaran nyata, ≥10 orang pernah klik "hubungi"

- [ ] Tier ulang `TIER_RULES` dari distribusi data (ritual mingguan).
- [ ] Hard evaluation: kalau <10 klik hubungi dari orang selain founder
  → stop nambah mode, fokus distribusi (kirim link ke grup WA olahraga
  Bandung, bukan nambah fitur) sesuai plan-v2 § 10.

## 3. Yang TIDAK dibangun sampai ada trigger terukur

Database, ORM, login, panel admin, scraper, generator kartu gambar IG,
GraphQL/gRPC, TypeScript, Tailwind, state library, PWA, ML rekomendasi,
peta interaktif, booking/pembayaran, mode ke-3 sebelum keputusan akhir
bulan 2. Ide baru → `plan.md` § "Nanti", bukan dikerjakan.

**Soal database, eksplisit:** sampai ≥150 penawaran atau form lapor masuk
≥5/minggu (trigger plan-v2 § 5), data = JSON `go:embed` di repo BE; edit
data = PR + auto-deploy, riwayat = git. Kalau trigger tercapai, pilihan
gratis yang sudah dicatat supaya tidak dicari ulang: **Neon** (Postgres
serverless, free tier, tersedia juga lewat Vercel Marketplace) atau
**Supabase** (Postgres + auth). Keduanya Postgres → `database/sql` +
`pgx`, tanpa ORM. Jangan dipasang sekarang.

## 4. Kontrak API (BFF Go)

Base: `https://bandung-sport-be.vercel.app` (nama project Vercel BE). Semua response `Content-Type:
application/json; charset=utf-8`. Error = `{ "error": "not found" }`.
BE **tidak** memfilter — 22–200 venue cukup difilter di FE; filter di BE
baru kalau payload >200 KB.

| Method & path | Response |
|---|---|
| `GET /healthz` | `200 {"status":"ok","venues":22,"offerings":9}` |
| `GET /api/venues` | `200 VenueSummary[]` |
| `GET /api/venues/{id}` | `200 VenueDetail` · `404` |

```jsonc
// VenueSummary — satu kartu
{ "id": "v-ftl-dago", "name": "FTL Gym Dago", "area": "Dago",
  "sports": ["fitness"], "lat": -6.8776, "lng": 107.6155,
  "tier": "Sedang",                 // null = "Harga belum dicek"
  "priceFrom": {                    // penawaran public termurah, atau null
    "priceMin": 500000, "priceMax": 2500000,
    "priceUnit": "monthly", "priceVisibility": "public" },
  "offeringCount": 2, "lastVerified": "2026-10-12" }

// VenueDetail = VenueSummary + kontak + offerings[]
{ ...VenueSummary,
  "address": null, "whatsapp": "+62818687858", "instagram": "…",
  "website": "…", "sourceUrl": "…", "tags": [],
  "offerings": [ /* Offering persis bentuk plan-v2 § 6 */ ] }
```

Aturan: field null dikirim sebagai `null` (bukan dihilangkan) supaya FE
tidak menebak. `tier` dan `priceFrom` dihitung di BE (`internal/catalog`)
— fakta tentang data. `monthlyCost`, patungan, jarak dihitung di FE —
fungsi dari input user. Versi API belum perlu; kalau bentuk berubah,
ubah FE dan BE bersamaan (satu founder, dua repo).

CORS: `Access-Control-Allow-Origin` hanya untuk origin yang ada di
`ALLOWED_ORIGINS`; method `GET, OPTIONS`; preflight 204.

## 5. Kontrak URL (hasil alat bantu)

Prinsip: URL = **input user + identitas data**, tidak pernah hasil
hitungan atau harga. Halaman menghitung ulang dari API, jadi link lama
otomatis menampilkan harga terkini + tanggal "harga dicek".

| Halaman | Contoh | Param |
|---|---|---|
| List | `/?olahraga=futsal&tier=murah&area=kopo` | semua opsional |
| Detail | `/tempat/v-ftl-dago` | slug stabil berprefiks `v-` |
| Sendiri | `/sendiri?v=1&olahraga=fitness&area=dago&kali=3` | `kali` 1–7 |
| Hasil Sendiri (dibagikan) | `/sendiri?v=1&olahraga=fitness&area=dago&kali=3&pilih=of-ftl-member` | `pilih` = paket yang ditandai |
| Bareng | `/bareng?v=1&olahraga=futsal&orang=10&jam=2&hari=sat` | `orang` 2–30, `jam` 1–4 |
| Hasil Bareng (dibagikan) | `/bareng?v=1&olahraga=futsal&orang=10&jam=2&pilih=of-xyz-sewa` | sama + `pilih` |

Aturan: nama param Bahasa Indonesia huruf kecil (URL dibaca orang di
WA); nilai `olahraga` = `sport` kanonik; param rusak/di luar rentang →
pakai default dan tampilkan notice kecil, **jangan** error page; `pilih`
menunjuk id yang tidak ada → abaikan; `v` dinaikkan hanya kalau arti
param berubah. Satu custom hook `useUrlForm(schema)` membaca/menulis
`useSearchParams` dengan validasi — ditulis founder di S18, dipakai
ulang di S21.

Teks share (dibentuk di FE): `"<judul hasil> — <angka utama>. Cek di
<URL>"`, mis. `"Futsal 10 orang 2 jam di Kopo — Rp28rb/orang. Cek di
https://…/bareng?v=1&…"`. Angka di teks boleh, karena teks WA memang
snapshot; URL-nya yang selalu segar.

## 6. Template pesan

**Concierge (S0)** — ke 5 orang, personal, bukan broadcast:
> Bro/sis, gue lagi bikin direktori olahraga Bandung yang ada harganya.
> Boleh tanya 2 hal? (1) Terakhir lo olahraga bareng/ngegym kapan, di
> mana, bayarnya gimana? (2) Lo lagi nyari apa sekarang — gym deket
> rumah, lapangan buat mabar, atau lainnya? Nanti gue kirim 2–3 opsi +
> harga. Kalau salah satunya lo hubungi/datangi, kabarin ya 🙏

Setelah dapat jawaban: kirim 2–3 opsi dari 22 venue (nama, harga, jarak
kira-kira, link WA venue). Catat di `docs/data/concierge.md`. Cek 7 hari
kemudian: bertindak atau tidak.

**Outreach harga ke venue (S1)**:
> Halo kak, saya Angling, lagi bikin direktori olahraga Bandung buat
> warga. Boleh info harga membership / sewa per jam terbaru? Mau saya
> cantumkan beserta kontak WA ini supaya orang langsung hubungi kakak.
> Terima kasih 🙏

**Kirim ke 5 teman (S15)**: 1 link + 1 pertanyaan: "Dari 5 detik pertama,
ini buat apa? Terus tempat mana yang harganya paling masuk akal buat lo?"

## 7. Instruksi untuk tool AI pelaksana (mode latihan) — tempel di prompt

1. Baca `plan.md` § Status, lalu `docs/plan-v3-delta.md` § 2, § 4, § 5,
   § 7, dan `docs/plan-v2.md` § 6. Kerjakan **hanya** checkbox pertama
   yang belum dicentang di § 2. Satu sesi, satu checkbox.
2. Mulai sesi dengan: tujuan sesi (1 kalimat), konsep yang dipakai (≤150
   kata, analogi Go boleh), lalu tugas konkret pertama untuk founder.
3. **Kode yang dirender ditulis founder**: komponen, hook, state, JSX,
   CSS, fungsi pure di `src/lib`. Kamu boleh menulis penuh: script
   migrasi/validasi Node, file token CSS, config deploy (`_redirects`,
   Dockerfile, perintah gcloud), setup Vitest/Umami, CORS middleware Go,
   README skeleton, dan **satu contoh** per pola baru (lalu founder
   meniru untuk kasus berikutnya).
4. Kode Go: founder menulis (dia backend Go engineer); kamu review
   seperti rekan senior: error handling, context, test coverage, nama.
5. Review dengan menunjuk file:baris dan bertanya "kenapa", bukan menulis
   ulang. Founder stuck: tanya dulu apa yang sudah dicoba → arah → nama
   API → potongan kode, bertahap.
6. Jangan menambah fitur, mode, dependency, field data, atau endpoint di
   luar sesi ini. Daftar § 3 adalah larangan keras. Ide → `plan.md`
   § "Nanti".
7. Jangan pernah mengarang harga, jadwal, kontak, koordinat. Tidak tahu
   = `null` + `priceVisibility: "unknown"`.
8. Teks UI Bahasa Indonesia; nama variabel, commit message, komentar,
   README dalam Bahasa Inggris. Diskusi: Indonesia santai.
9. Tutup sesi: minta founder jalankan `npm run lint && npm run build`
   (FE) dan/atau `go vet ./... && go test ./...` (BE), pastikan hijau,
   commit dengan pesan Conventional Commits, **push** ke remote
   masing-masing (ini repo publik untuk portofolio).
10. Update `plan.md` § Status (3–6 baris): checkbox yang dicentang,
    materi FE yang dilewati, utang, sesi berikutnya. Centang checkbox
    di `docs/plan-v3-delta.md` § 2.
11. Kalau founder minta "kerjain aja semuanya", ingatkan tujuan belajar
    sekali, lalu tawarkan: kamu tulis 1 contoh, dia tulis sisanya.
12. Kalau sesi sudah >2 jam atau founder bilang capek: simpan progres
    (commit WIP), tulis status, berhenti. Esok hari ada lamaran kerja
    yang lebih penting.

## 8. Struktur repo (3 tempat)

```
~/angling/proj/bandung-sport-complete/   ← repo privat: docs, plan (root ini)
├── AGENTS.md, plan.md, docs/, research/
├── fe/   ← repo publik jakskal/bandung-sport-web  (Vite + React 19, JS)
│   └── src/{api,hooks,components,pages,lib,styles,data(types.js, modes.js, areas.js saja)}
└── be/   ← repo publik jakskal/bandung-sport-be  (Go stdlib)
    ├── go.mod                       ← wajib di root repo (Vercel membacanya)
    ├── vercel.json                  ← { "framework": "go" }
    ├── cmd/api/main.go              ← entrypoint yang dikenali Vercel; listen env PORT
    ├── internal/catalog/{model.go,load.go,tier.go,catalog_test.go}
    ├── internal/httpapi/{handler.go,cors.go,handler_test.go}
    ├── data/{embed.go,venues.json,offerings.json}   ← satu-satunya sumber data
    │     embed.go: `package data` + `//go:embed *.json` + `var FS embed.FS`
    │     (go:embed hanya bisa file di dalam folder paketnya sendiri)
    ├── Dockerfile, Makefile, README.md
```

Hosting (semua gratis, tanpa kartu): FE = Vercel project `bandung-sport-web`
(preset Vite), BE = Vercel project `bandung-sport-be` (preset Go). Dua
project, satu akun. Env FE: `VITE_API_URL`. Env BE: `ALLOWED_ORIGINS`, `PORT`
(diisi Vercel).

`be/` dan `fe/` adalah gitlink/submodule-less folder di repo root (repo
root tidak melacak isinya; masing-masing punya `.git` sendiri).

## 9. Horizon & ukuran keberhasilan

| Horizon | Selesai kalau | Output vs outcome |
|---|---|---|
| 4 minggu (S15) | URL live FE + BE, list/detail lolos tes 5 detik (2/3 orang), 2 README berscreenshot, baris proyek di CV/LinkedIn, build log #1 | output + 1 outcome: ≥2 dari 5 teman membalas dengan pertanyaan atau nama tempat |
| 3 bulan | `/sendiri` + `/bareng` live, ≥50 penawaran `public`, form lapor, event klik terpasang | **≥10 orang berbeda pernah klik "hubungi"**, ≥1 link Patungan di-forward |
| 12 bulan | end game plan-v2 § 1 | ≥50% update data dari luar founder |

Aturan potong plan-v2 § 10 tetap berlaku: 2 minggu tanpa commit yang
di-deploy → scope fase dipotong separuh; 2 fase tanpa sinyal luar →
berhenti nambah mode, fokus distribusi.
