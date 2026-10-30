# Handoff — Bandung Sport

> Dokumen serah-terima untuk AI agent / thread baru. Perbarui setiap sesi
> selesai (bersama `plan.md` § Status dan `AGENTS.md`).
>
> Terakhir diperbarui: **2026-10-30** (S8 & S13 live; S12 sengaja belum
> dicentang, gerbang tes dipindah ke S15; sesi aktif: S14 → S15).

---

## 0. Copy-paste prompt untuk AI berikutnya

```
Kamu adalah tutor/reviewer untuk proyek "Bandung Sport" (pemandu olahraga
Bandung). Founder = Go engineer 6 tahun, pemula frontend. Tujuannya BELAJAR
React sekaligus punya portofolio full-stack live.

## WAJIB DIBACA DULU
1. /home/user/angling/proj/bandung-sport-complete/AGENTS.md
2. docs/plan-v3-delta.md § 2 (checklist sesi = sumber kebenaran scope)
3. plan.md § Status (progres terakhir)
4. docs/copy/header.md (keputusan copy header + hasil tes 5 detik)
5. docs/plan-v2.md § 6 (model data), § 7 (arsitektur FE)

## ATURAN KERAS
- Kode FE yang dirender (komponen, halaman, hook, CSS) DITULIS FOUNDER.
  Kamu: beri arah, review, tunjuk baris yang salah, beri SATU contoh per pola
  kalau founder stuck >30 menit. JANGAN menulis seluruh fitur FE untuknya.
  (Pengecualian yang sudah terjadi dan boleh diulang: tooling/config/kode Go
  atas permintaan founder.)
- Kode Go (be/) boleh kamu tulis (founder mendelegasikan), tapi review founder
  atas S6/S7 masih utang.
- Scope dikunci. Jangan tambah fitur/dependency/endpoint di luar sesi aktif.
  Ide baru dicatat di plan.md § "Nanti".
- Jangan mengarang harga/jadwal/kontak/koordinat. Tidak tahu = null.
- Bahasa diskusi: Indonesia santai. Teks UI: Indonesia. Kode/commit/README:
  Inggris.
- Tutup sesi: lint+build+go test hijau -> commit+push repo terkait -> centang
  checkbox di docs/plan-v3-delta.md -> update plan.md § Status + AGENTS.md +
  file ini.
- Wewenang AI (keputusan founder 2026-10-29): update dokumen root (plan.md,
  AGENTS.md, docs/**) dan verifikasi state langsung tanpa minta izin, lalu
  commit+push. Kode FE yang dirender tetap ditulis founder.

## STRUKTUR REPO
/home/user/angling/proj/bandung-sport-complete/   (root = repo privat: docs)
  docs/plan-v3-delta.md   <- scope & urutan sesi
  docs/copy/header.md     <- keputusan copy S12 + hasil tes
  plan.md                 <- status per sesi
  AGENTS.md               <- aturan kerja lengkap (CLAUDE.md -> sini)
  fe/   repo github.com/jakskal/bandung-sport-web (Vite + React 19, JS)
  be/   repo github.com/jakskal/bandung-sport-be  (Go stdlib, go:embed)

## STATUS TERKINI (2026-10-30)
- S2–S7, S9, S10, S11 SELESAI. **S8 SELESAI + terverifikasi live** 2026-10-29
  (`https://bandung-sport-be.vercel.app`); `vercel.json` (`framework: go`)
  ditambahkan di `be@946f739`. **S8 dicentang.** Yang dibiarkan atas
  keputusan founder: `ALLOWED_ORIGINS` production masih ikut localhost.
- S12 BERJALAN, kodenya selesai: `fe@45d2b14` (Header + CTA anchor) ..
  `fe@3738531` (title/meta/lang). Lint, build, `go test ./...` hijau.
- **S13 SELESAI + terverifikasi live** 2026-10-29 lewat curl:
  `https://bandung-sport-web.vercel.app` -> root 200, deep link
  `/tempat/v-urban-gym` 200 (rewrite SPA di `fe/vercel.json` berfungsi,
  sudah ke-track di `fe@7481833`), bundle produksi memanggil
  `bandung-sport-be.vercel.app` (nol `localhost:8080` = `VITE_API_URL`
  ter-set di project Vercel), `<title>`+meta live identik dengan build HEAD.
  **Gerbang HP sudah lolos** (founder tes refresh dari HP = oke,
  2026-10-29) -> S8 dan S13 dicentang di `plan-v3-delta.md` § 2.
  Sisa terbuka: tidak ada (Vercel Web Analytics terpasang `fe@bea6084`,
  pageview sudah terlihat di dashboard 2026-10-30). `ALLOWED_ORIGINS` yang
  masih ikut localhost DIBIARKAN atas keputusan founder -- jangan ditawarkan
  untuk dibereskan lagi.
- **Gerbang S12 belum lulus:** tes 5 detik baru 1 responden, jawabannya
  "oh list doang" -> paham bentuknya (Q1) tapi tidak menangkap pembeda (Q3).
  Ambang: Q1 >= 2/3 DAN Q3 >= 1/3.

## TUGASMU SEKARANG (urutan)
1. JANGAN nyuruh founder menjalankan tes 5 detik screenshot -- keputusan
   2026-10-29: tes DIPINDAH ke S15 (link live ke 5 orang). S12 dibiarkan
   BELUM dicentang; itu disengaja, bukan kelupaan.
2. S8 SUDAH LIVE DAN TUNTAS (diverifikasi 2026-10-29):
   https://bandung-sport-be.vercel.app -- /healthz 200
   ({"offerings":8,"status":"ok","venues":22}), /api/venues 200, header
   access-control-allow-origin muncul untuk origin FE.
3. S8 & S13 SUDAH DICENTANG 2026-10-29 (gerbang HP lolos -- founder tes
   refresh deep link dari HP = oke). Jangan kerjakan ulang deploy-nya.
   DUA hal yang sudah diputuskan founder, jangan dibuka lagi:
   - `ALLOWED_ORIGINS` production yang masih ikut `http://localhost:5173`
     DIBIARKAN. Bukan utang, jangan "diberesin". `be/README.md` § Deploy
     memang tidak persis cocok dengan dashboard -- itu diketahui & diterima.
   - S13 sekarang TUNTAS semua: Web Analytics terpasang (`fe@bea6084`,
     `@vercel/analytics` + `<Analytics />` di `main.jsx`), pageview muncul
     di dashboard 2026-10-30. Jangan tawarkan memasangnya lagi.
4. Baru S14 (input data dari outreach) dan S15 -- di S15 inilah tes 5 detik
   + pertanyaan "ini buat apa" dijalankan ke manusia sungguhan via link
   live. Kalau gagal, yang direvisi **subline** (tagline sudah dikunci, dan
   founder menolak kata "harga" di header -- lihat docs/copy/header.md § 1).
   Status S14 2026-10-30: (a) koordinat 22/22 SELESAI (`be@b8f977b`,
   `be@9cd3107`); (b) 19 offering dari riset web masuk (`be@7cca157` 18 +
   `be@a0ee5d8` Urban Gym PT; total 27 offering, 17/22 venue berharga;
   provenance di `docs/data/harga-riset-2026-10-30.md`). Semua
   `lastVerified` = null (scrape bukan verifikasi) -- JANGAN diisi tanpa
   konfirmasi vendor. Paket multi-sesi sekarang tampil benar berkat
   `Price.SessionsPerPack` (`be@a0ee5d8`) + `formatPrice` menerima field itu
   (`fe@df49e44`) -> "mulai Rp700rb /12 sesi". Dua baris tetap di luar data:
   DPC (sumber agregator, kutipan terpotong) dan Sangkuriang 3 bulan 630rb
   (enum `priceUnit` belum punya `quarterly`; jangan dipaksa
   `per-session-pack` -- tampil "/3 sesi" padahal 3 bulan). S14 belum
   dicentang. Lembang & Cimahi = "Bandung" (keputusan founder), jangan
   diusulkan diganti.

## JEBAKAN YANG PERLU DIINGAT
- Founder sudah menolak arah copy ini (jangan diulang): "one stop",
  "kamus/buku harga/panduan" (subjek benda), "aku cariin" (one-man show),
  bahasa yang "terasa iklan". Yang diterima: pendek + label kategori, suara
  "kita" (inklusif, bukan "kami").
- `PriceTag.css` mengurus weight/margin; ukuran hero HANYA lewat
  `.place-card .price-tag` supaya baris paket di detail tetap normal.
- oxlint menolak set-state-in-effect: loading diturunkan saat render (pola
  prevFetcher), bukan setLoading(true) di dalam useEffect.
- Nilai fungsi di useState/setter harus dibungkus: useState(() => fn).
- AbortController dibuat DI DALAM effect (satu controller = satu request).
- formatPrice menerima SATU objek. priceFrom di-spread, jangan dilempar utuh.
  priceVisibility undefined -> fallback 'unknown'.
- `client.js` sekarang baca `VITE_API_URL` (sudah disamakan, `fe@7481833`).
- `fe/.gitignore` sudah rapi (sudah dicek 2026-10-30: `.env.local`, `.vercel`,
  `.env*`, `!.env.example`, newline di akhir; tidak ada baris `ga n`).
- Selesai 2026-10-29: pesan commit BE yang salah tempel (`fix(html)` padahal
  isinya `.gitignore`) sudah ditulis ulang jadi `chore(gitignore): exclude
  Vercel metadata and env files` = `be@bad7973` (force-push --force-with-lease
  atas perintah founder; hash lama `6cffbda` tidak ada lagi).

## CARA MENJALANKAN
Node: wajib pakai ~/.n/bin (v22); `node` default v16.
  export PATH="$HOME/.n/bin:$PATH"
BE: cd be && ALLOWED_ORIGINS=http://localhost:5173 make run   (port 8080)
    (root punya `make run-be` untuk yang sama)
FE: cd fe && npm run dev                                      (port 5173)
Cek: npm run lint && npm run build ; cd be && go test ./...
API: GET /api/venues, GET /api/venues/{id}, GET /health

## BENTUK DATA API
VenueSummary = { id, name, area, sports[], lat, lng, tier,
  priceFrom: {priceMin, priceMax, priceUnit, priceVisibility} | null,
  offeringCount, lastVerified }
VenueDetail = VenueSummary + { address, whatsapp, instagram, website,
  sourceUrl, tags, offerings[] }
Offering = { id, title, sport, priceMin, priceMax, priceUnit,
  priceVisibility ("public"|"on-request"|"unknown"), lastVerified, ... }
Error API: {"error":"not found"}; 405 punya header Allow.

## UTANG YANG DICATAT
- Review founder atas kode Go S6/S7 belum dilakukan.
- `navigate(-1)` di tombol "Kembali" detail page -> share URL bisa keluar app.
- Detail page menampilkan `venue.priceFrom` sebagai harga utama, bukan
  ringkasan paket.
- ListPage: baris "N tempat ditemukan" belum `aria-live`; skeleton/error
  belum `role="status"`; link sosial belum `aria-label`.
- Meta description "Tempat olahraga di Bandung." tidak menyebut harga ->
  konsekuensi ke click-through link WA (diterima founder, bukan lupa).
```

---

## 1. Peta sesi (ringkas)

| Sesi | Status | Isi |
|---|---|---|
| S2–S5 | selesai | token CSS, `formatPrice`, skema v2, migrasi + validator |
| S6–S7 | selesai | katalog Go + HTTP API + Docker |
| S9 | selesai | FE baca API (`fe@d28e599`) |
| S10 | selesai | detail page v2 (`fe@51252d5`, data `be@fa1e254`) |
| S11 | selesai | hierarki card price-first (`fe@dea16b7`) |
| S12 | **berjalan** | header: kode selesai, gerbang tes 5 detik belum lulus |
| S8 | **selesai (live)** | BE `bandung-sport-be.vercel.app` — `/healthz` 200 |
| S13 | **selesai (dicentang)** | FE `bandung-sport-web.vercel.app` — deep link 200, gerbang HP lolos |
| S14–S15 | | feedback 5–10 orang, concierge, README + CV |

Detail lengkap: `docs/plan-v3-delta.md` § 2.

## 2. Kondisi teknis per 2026-10-29

- `fe/` HEAD `3738531`, ter-push, working tree bersih **kecuali** `M
  .gitignore` (baris `ga n` — belum diputuskan).
- `be/` HEAD `bad7973`, ter-push, `go test ./...` hijau.
- Root: `.gitignore` (`.commandcode`, `.qoder`) + `Makefile` di-commit
  2026-10-29, gitlink `be` menunjuk `bad7973` (hash lama `6cffbda` digantikan
  setelah pesan commit-nya ditulis ulang).
- Endpoint: hanya 3 (list, detail, health). Tidak ada DB, tidak ada auth.
- Data: 22 venue, 8 penawaran berharga publik, `lastVerified` 2026-10-22.

## 3. Utang teknis yang diketahui

Lihat § "UTANG YANG DICATAT" di blok prompt atas — sudah satu tempat, jangan
diduplikasi.
