# AGENTS.md — Bandung Sport

> Untuk SEMUA AI agent (Amp, Claude Code, Codex, Cursor, Copilot, dsb): ini
> adalah proyek pribadi pemilik repo. Baca file ini sebelum bekerja.
>
> **SUMBER KEBENARAN SEKARANG (2026-10-17): `docs/plan-v3-delta.md`**
> (urutan kerja + checklist sesi + kontrak API/URL + instruksi tool AI § 7),
> di atas `docs/plan-v2.md` (visi, model data § 6, arsitektur FE § 7, ritual
> § 10). `plan.md` tinggal § Status (progres per sesi) dan catatan sejarah.
> Kalau bertentangan: plan-v3-delta > plan-v2 > plan.md > bagian "Progres"
> lama di file ini.
>
> **Mulai sesi:** baca `plan.md` § Status → kerjakan checkbox pertama yang
> kosong di `docs/plan-v3-delta.md` § 2 → ikuti instruksi § 7 di sana.

## Apa ini

**Bandung Sport** — pemandu olahraga Bandung: "olahraga apa, di mana, bareng
siapa, berapa duit". Data Tempat + Penawaran (paket/sewa dengan harga nyata),
dilihat lewat mode: Sendiri (Hitung Biaya) dan Bareng (Patungan) dulu; mode
lain menyusul kalau ada sinyal (lihat `docs/plan-v2.md` § 4).

PENTING — ini BUKAN proyek kantor. Dua tujuan sekaligus: (1) **kendaraan
belajar frontend** pemilik repo (backend Go engineer 6 th, pemula FE) — kode
yang dirender ditulis founder; (2) **bukti full-stack untuk cari kerja**
(founder di-PHK 15 Okt 2026): FE React + BE Go live dalam ≤4 minggu. Waktu
proyek = sisa hari setelah 75 menit wajib lamaran + latihan English.

## Arah v3 (sesi 2026-10-17) — BACA `docs/plan-v3-delta.md`

- Board review final (5 peran) → 5 delta disepakati pemilik repo: BFF Go
  dimajukan ke setelah data v2 (sesi 6–8, Go stdlib + go:embed, Vercel Go preset,
  BE jadi satu-satunya sumber data); dua mode dikunci 3 bulan (Sendiri,
  Bareng); concierge = side task bukan gate; hasil alat bantu = URL (slug +
  `v=1`, tanpa harga) dengan CTA di halaman tujuan; kartu coming-soon di
  bulan ke-2 (urutan diacak).
- Struktur jadi 3 repo: root privat (docs), `fe/` publik
  `jakskal/bandung-sport-web`, `be/` publik `jakskal/bandung-sport-be`
  (dibuat di sesi S6).
- **Tugas berikutnya: S0 (concierge, 30 menit) lalu S1 (copy pass +
  outreach harga)** — checklist di `docs/plan-v3-delta.md` § 2.
  **Update 2026-10-26:** S0 & S1 ditunda (S0 dilipat ke S15); S2 (Token
  CSS), S3 (`formatPrice`), S4 (skema v2), S5 (migrasi + validator), S6
  (katalog Go), S7 (HTTP API + Docker), **S9 (FE baca API) SELESAI**
  (`fe@d28e599`: `api/client.js`, `hooks/useFetch.js`, `ListPage` +
  skeleton + error, `Place.jsx` v2, route `/tempat/:venueId`,
  `PlaceDetail` basic dari API; bersih-bersih selesai) — sesi aktif
  sekarang **S12 (header "ini apa")**. **S8 (deploy BE) ditunda ke sebelum
  S13** (keputusan 2026-10-22). Catatan: kode Go S6/S7 ditulis AI atas
  permintaan founder (review founder belum); FE ditulis founder.
  **S10 SELESAI 2026-10-28** (`fe@51252d5`): `PriceTag`, `OfferingRow`
  (judul + harga + "Harga dicek <bulan tahun>" hanya bila `lastVerified`
  != null), CTA WA full-width, empty state jujur, `lastVerified` data BE
  diisi untuk semua penawaran berharga (`be@fa1e254`).
  **S11 SELESAI 2026-10-28** (`fe@dea16b7`): `TierBadge` pill, kartu
  price-first (nama+badge → harga hero → meta kecil), chip aktif `--brand`,
  token `--fs-price`/`--radius-card`/`--brand`.
  **S12 BERJALAN (2026-10-29)**: komponen `Header/` + CTA anchor + `<title>`
  + meta description + `lang="id"` sudah commit (`fe@45d2b14`..`3738531`);
  copy final di `docs/copy/header.md`. **Belum lulus gerbang**: 1 orang
  menjawab "oh list doang" di tes 5 detik → Q3 (pembeda dari Google) gagal.
  Keputusan 2026-10-29: tes screenshot **ditunda ke S15** (link live), S12
  sengaja belum dicentang. **S8 terverifikasi sudah LIVE**
  (`https://bandung-sport-be.vercel.app`, 2026-10-29) — sesi aktif sekarang
  **S13 (deploy FE)**.
- Root repo: `.gitignore` (`.commandcode`, `.qoder`) + `Makefile` di-commit
  2026-10-29. `fe/` + `be/` sudah ter-push.

## Status lama (per 2026-09-24 s/d 2026-10-13) — sejarah, bukan instruksi

- Rencana 5 bulan sudah final, ada di `plan.md`. Baca itu dulu.
- Struktur folder: `fe/` (frontend) + `be/` (backend, menyusul di Bulan 2).
- `fe/` sudah di-setup: Vite + React (template react), oxlint, Prettier,
  `npm run dev / build / lint / format` semuanya jalan.
- Langkah berikutnya (Bulan 1): halaman list statis dengan data hardcode —
  dikerjakan sendiri oleh pemilik repo, AI sebagai tutor.

### Progres terakhir (sesi 2026-09-24 malam)

- Pemilik repo sudah: paham alur `index.html` → `main.jsx` → `App.jsx`,
  render header sendiri, dan bikin list statis dari array `places` hardcode
  pakai `.map()` di `App.jsx` (data: 3 gym, nama + area).
- PR kecil yang sudah dibahas: unused imports (solusi: oxlint `--fix` /
  organize-imports VS Code) dan konsep `key` (kenapa `key={index}` kurang,
  arah: `key={place.id}`).
- **PR berikutnya (belum dikerjakan)**: (1) tambah `id` per place dan pakai
  sebagai key, (2) ekstrak komponen `PlaceCard({ place })` — materi props.
- Setelah itu: pindahkan data hardcode ke file JSON terpisah (scope v1).
- Keputusan 2026-09-27 (pemilik repo): kualitas data seed dibiarkan dulu —
  nama/harga boleh dummy, typo & casing & tier yang kontradiksi boleh sementara.
  **Utang tercatat**: wajib beresin konsistensi data (tier vs harga per kategori,
  casing area/kategori) SEBELUM fitur filter dikerjakan (Bulan 2), karena filter
  langsung kena bug ini. Data kurasi massal (20–30 tempat) baru dikumpulkan
  setelah list page matang; scraping bukan scope v1.
- Keputusan desain data: tier murah/mahal itu relatif per kategori olahraga
  (gym 450rb gila, padel 450rb wajar) — konsistensi tier hanya wajib dalam
  satu kategori. Nanti di Bulan 2 bisa dihitung otomatis dari data (BFF).

### Progres (sesi 2026-09-27)

- **Bulan 1 selesai**: list statis jalan — data di `src/data/places.json`
  (10 tempat, field: id/name/area/category/priceMin/priceMax/tier),
  komponen `Place` (folder `src/components/Place/`), card styling (CSS class,
  tier badge via CSS variables), grid responsif `auto-fill minmax`, container,
  hover state. Lint + build hijau.
- Materi yang sudah dilewati pemilik repo: komponen + props, `.map()` + key,
  default vs named export, import JSON, JSX conditional, className dinamis,
  CSS variables, grid responsif, toLocaleString.
- Git: repo di dalam `fe/` (keputusan pemilik repo: multi-repo per deployable,
  biar profil GitHub bersih dari agent docs), remote GitHub
  `jakskal/bandung-sport-web` (SSH), branch `main`. Docs (plan/AGENTS)
  direncanakan masuk repo private terpisah — BELUM dibuat.
- **PR berikutnya (Bulan 2 dimulai)**: (1) beresin konsistensi data seed
  (wajib sebelum filter), (2) fitur filter dengan React state.
- Catatan tutor: `transition` di `:hover` perlu dipindah ke base state
  `.place-card` (belum dilakukan terakhir dicek).

### Update data (sesi 2026-09-27 sore)

- Data seed lama diganti hasil scraping agent + kurasi pemilik repo:
  22 venue real (Fitness/Futsal/Padel/Swimming Pool/Running Track),
  field baru: `priceUnit`, `whatsapp`, `instagram`, `website`, `sourceUrl`.
- Sistem tier jadi 4: Gratis/Murah/Sedang/Mahal (Title Case, konsisten),
  CSS `.tier-sedang` + `--place-tier-medium` sudah ada.
- Aturan data yang disepakati: harga null → tier null (tier turunan harga);
  null = info belum terverifikasi, tidak boleh diganti nilai karangan.
- Tempel pelajaran FE: asumsi 2 bentuk harga (tunggal/range) didobrak data
  baru (min tanpa max) → branching 3 cabang + guard `== null`;
  regresi formatting saat refactor (toLocaleString/unit sempat hilang).
- Readme repo FE udah ditulis ulang (What/Why/Stack/Run/Roadmap, English).
- `docs/` dibuat di root (akan ikut repo private) — sketsa layout list page
  + filter ada di `docs/layout/list-page.md` (wireframe mobile, empty state,
  urutan PR filter). Update sketsa kalau realita implementasi beda.
- **Filter dimulai (belum commit)**: `useState` + `visiblePlaces` (filter
  kategori) + result count udah jalan di `App.jsx` — state masih hardcoded
  `'Futsal'` buat bukti konsep.
- **PR berikutnya (lanjutan filter)**: (1) ganti hardcode jadi chip row —
  derive `categories` pakai `new Set`, render `<button>` (bukan div),
  onClick `setActiveCategory`, chip aktif via className dinamis + CSS chip;
  (2) filter tier (pola sama), (3) filter area pakai `<select>`,
  (4) empty state + reset + count udah sebagian ada. UX: single-select
  per dimensi, chip "Semua" buat reset.

### Progres (sesi 2026-10-03) — FILTER v1 SELESAI

- **Checklist v1 pertama di `plan.md` resmi dicentang**: halaman list +
  filter 3 dimensi (kategori chip, tier chip, area select) + count +
  empty state + reset kondisional (`hasActiveFilters`).
- Komponen `FilterChips` terekstrak (props: items/activeItem/onItemClick) —
  DRY, siap dipakai ulang. Filter area: controlled `<select>`.
- Materi baru yang dilewati: `useState`, derived state (`visiblePlaces`,
  `hasActiveFilters`), event handler, controlled component, callback props,
  DRY/refactoring, konvensi naming CSS (kebab-case vs BEM, horizon CSS
  Modules).
- Data: "Belum dikurasi" jadi chip tier (fitur curation-aware, keputusan
  pemilik repo).
- **Sisa scope v1**: detail page per tempat (harga, fasilitas, kontak/IG) —
  masuk Bulan 3 sesuai plan (routing). Deployment Bulan 4.
- Catatan tutor: `.no-places` udah distyling (PR kosmetik landed:
  center + hover reset button + transition 0.3s).

### Keputusan produk & revisi peta (2026-10-03 malam)

- **Personalized suggestion resmi jadi FLAGSHIP v2** (sebelumnya catatan
  kaki v2/v3): input param → rekomendasi olahraga+venue+biaya. Engine
  v1-nya = wizard yang memanfaatkan filter; guardrail framing kesehatan.
- **`tags` per venue masuk rencana kurasi** (cocok-buat-siapa,
  ada-kelas, intensitas) — tapi mulai SETELAH detail page jadi (keputusan
  pemilik repo: feature-first, schema data mengikuti kebutuhan fitur;
  pass kurasi besar = prasyarat bar v1.0 "worth showing", bukan task
  berikutnya).
- **plan.md direvisi jadi milestone-based** (bukan bulan): v1.0 "worth
  showing" (detail page + 40–60 venue + deploy statis + 5–10 user terdekat),
  v1.5 "worth sharing" (feedback diterapkan → publik). Evidence: koperasi-
  react (3 minggu sampai beta deployed; mati karena tanpa user). Deploy
  ngikutin bar "worth it", bukan tanggal. Guardrail fitur baru tetap.

### Progres (sesi 2026-10-04) — ROUTING + DETAIL PAGE SELESAI

- **Checklist v1 kedua tercentang**: detail page jalan via React Router
  (`react-router` v7, library mode). Route: `/` = ListPage,
  `/place/:id` = PlaceDetail; card = `<Link>` (bukan onClick).
- Arsitektur baru: folder `src/pages/` (ListPage, PlaceDetail);
  App.jsx tinggal route config; `main.jsx` bungkus `BrowserRouter`.
- Komponen `Place` jadi kartu murni (kontak dipindah ke detail page —
  alasan: a-in-a invalid HTML + kontak memang konten detail).
- Materi baru: useParams, dynamic segment `:id`, konversi tipe param
  (`parseInt`/`Number` — params selalu string), not-found fallback,
  Link vs useNavigate, provider pattern (BrowserRouter), global
  `box-sizing: border-box` reset, box model (content-box vs border-box).
- Catatan tutor: fallback not-found belum punya link balik ke `/`;
  label website masih URL mentah (cosmetic); detail page belum ada
  styling khusus (polos) — PR kosmetik nanti.
- **PR berikutnya**: polish kosmetik detail page, lalu checkpoint
  review v1.0 (sesuai revisi peta) — sisanya data pass 40–60 venue +
  deploy statis + 5–10 user terdekat.

### Progres (sesi 2026-10-11) — POLISH EXISTING FEATURES SELESAI

- Batch polish fitur existing tuntas (B,C,D,A): 404 catch-all route,
  scroll-to-top (PUSH only, POP restore via `useNavigationType`),
  document title per halaman, dan **filter tersinkron ke URL search
  params** (`useSearchParams` — state filter dihapus, URL = sumber
  kebenaran; hasil filter bisa di-share via URL).
- Lint bersih, semua alur terverifikasi (share URL terfilter, refresh,
  back/forward).
- Keputusan 2026-10-10 (pemilik repo): sempat mempertimbangkan mode
  full-AI untuk program startup, diputuskan **lanjut belajar saja**.
- **Keputusan pending**: deploy statis dulu atau data pass 40–60 venue
  + tags dulu (rekomendasi tutor: deploy dulu). Checkpoint review
  v1.0: kecepatan aktual mengkonfirmasi revisi peta milestone-based.

### Hasil board review wizard (sesi 2026-10-11) — SPEC RATIFIED

- Wizard spec dilewati board review 5 role (2 investor, CEO, Product
  Lead, CTO — subagent diskusi via board): **GO-WITH-CONDITIONS,
  konsensus**. Spec final ratifikasi pemilik repo:
  `docs/spec/wizard-rekomendasi.md` v0.3 + **Logic Spec v1** (pure
  matching module + test plan, siap build saat gate lolos).
- Keputusan ratifikasi: **general first** — usia/kondisi TIDAK jadi
  input MVP (masuk sebagai tags venue belakangan, band names bukan
  angka); input wizard = area tinggal + budget (toggle bulan/minggu);
  matching = **proximity rank** (haversine, koordinat di-parse dari
  sourceUrl Google Maps yang sudah ada di data!) + budget ternormalisasi
  (floor 1000 IDR, aturan konversi terkunci dulu); alasan rekomendasi
  hanya dari nilai yang beneran ngeranking; empty state jujur; hasil
  shareable via URL.
- **Urutan kerja disepakati dewan (serial)**: (1) data pass 40–60 venue
  (dgn koordinat + tags + durasi_sesi + aturan satuan) + deploy + intro
  5–10 teman + 5 wawancara + concierge test WA — (2) demand gate ≥3/5 —
  (3) baru wizard dibangun dari Logic Spec v1. Density beats breadth.
- WHO/guidelines = konten dikutip versioned, tidak pernah logic;
  "describe venues, never assess a person".
- **PR berikutnya**: data pass fase 1 — parse koordinat dari 22 venue
  existing + validation script (launch blocker).

### Arah baru v2 (sesi 2026-10-13) — BACA `docs/plan-v2.md`

- Scope v1 + wizard v0.3 **digantikan** oleh `docs/plan-v2.md`: pemandu
  olahraga Bandung multi-mode (sendiri/bareng/komunitas/anak/khusus),
  data v2 Tempat+Penyelenggara+Penawaran, roadmap per fase, Fase 0 dirinci
  malam-per-malam (§ 8), instruksi untuk tool AI pelaksana (§ 9).
- Guardrail lama direvisi (lihat `docs/plan-v2.md` § 4); aturan kerja di
  bawah tetap berlaku.
- **Tugas berikutnya: Fase 0 malam 1** (copy pass). Belum ada commit.

## Aturan kerja (WAJIB diikuti)

Versi lengkap dan terbaru: `docs/plan-v3-delta.md` § 7. Ringkasnya:

1. **Jangan implementasikan semuanya untuk user.** AI = TUTOR: jelaskan
   konsep, beri latihan, review kode yang user tulis. Boleh ditulis penuh
   oleh AI: tooling (script migrasi/validasi, token CSS, config deploy,
   Dockerfile, setup test, CORS middleware) dan **satu contoh** per pola.
   Kode Go ditulis founder (keahliannya), AI review.
2. **Scope dikunci** di `docs/plan-v3-delta.md` § 2 (sesi aktif) dan § 3
   (daftar jangan-dibangun). Tolak penambahan fitur/mode/dependency/
   endpoint di luar sesi yang ditunjuk; ide dicatat di `plan.md` § "Nanti".
3. Kalau user stuck >30 menit, jangan langsung kasih solusi — tanya dulu apa
   yang sudah dicoba, baru beri petunjuk bertahap.
4. Bahasa diskusi: Indonesia santai. Teks UI: Indonesia. Nama variabel,
   komentar, commit, README: Inggris.
5. Jangan mengarang harga/jadwal/kontak/koordinat; tidak tahu = null.
6. Setiap sesi selesai: lint/build/test hijau → commit + push repo terkait →
   centang checkbox di `docs/plan-v3-delta.md` § 2 → update `plan.md`
   § Status (3–6 baris), supaya handoff antar-thread/agent selalu akurat.

## Struktur folder

- `docs/plan-v3-delta.md` — **SUMBER KEBENARAN**: urutan sesi, kontrak API &
  URL, template pesan, instruksi tool AI.
- `docs/plan-v2.md` — visi, peta segmen, model data v2 (§ 6), arsitektur FE
  (§ 7), ritual mingguan (§ 10). Urutan fase § 5/§ 8 **digantikan** v3.
- `plan.md` — § Status (progres per sesi, wajib diupdate) + sejarah rencana.
- `AGENTS.md` / `CLAUDE.md` — file ini (CLAUDE.md menunjuk ke sini).
- `docs/handoff.md` — prompt serah-terima untuk AI/thread baru (status,
  jebakan, tugas berikutnya). Update setiap sesi selesai.
- `docs/` lainnya — sketsa layout, review board, spec wizard lama (arsip),
  `docs/data/` (outreach.md, concierge.md — dibuat saat S0/S1).
- `fe/` — repo publik `jakskal/bandung-sport-web`: Vite + React 19 (JS),
  react-router v7, oxlint, Prettier.
- `be/` — repo publik `jakskal/bandung-sport-be` (dibuat di S6): Go stdlib,
  `go:embed` data JSON, deploy Vercel (preset Go). Satu-satunya sumber data setelah S9. FE juga di Vercel (preset Vite). Tanpa DB sampai trigger plan-v2 § 5.
