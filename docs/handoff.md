# Handoff — Bandung Sport

> Dokumen serah-terima untuk AI agent / thread baru. Perbarui setiap sesi
> selesai (bersama `plan.md` § Status dan `AGENTS.md`).
>
> Terakhir diperbarui: **2026-10-29** (S12 masih berjalan — terhambat tes 5
> detik, bukan terhambat kode).

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

## STRUKTUR REPO
/home/user/angling/proj/bandung-sport-complete/   (root = repo privat: docs)
  docs/plan-v3-delta.md   <- scope & urutan sesi
  docs/copy/header.md     <- keputusan copy S12 + hasil tes
  plan.md                 <- status per sesi
  AGENTS.md               <- aturan kerja lengkap (CLAUDE.md -> sini)
  fe/   repo github.com/jakskal/bandung-sport-web (Vite + React 19, JS)
  be/   repo github.com/jakskal/bandung-sport-be  (Go stdlib, go:embed)

## STATUS TERKINI (2026-10-29)
- S2–S7, S9, S10, S11 SELESAI. S8 (deploy BE) ditunda ke sebelum S13, TAPI
  konfigurasi Vercel-nya sudah masuk `be@6cffbda` (README + `.vercel`) dan
  sudah push.
- S12 BERJALAN, kodenya selesai: `fe@45d2b14` (Header + CTA anchor) ..
  `fe@3738531` (title/meta/lang). Lint, build, `go test ./...` hijau.
- **Gerbang S12 belum lulus:** tes 5 detik baru 1 responden, jawabannya
  "oh list doang" -> paham bentuknya (Q1) tapi tidak menangkap pembeda (Q3).
  Ambang: Q1 >= 2/3 DAN Q3 >= 1/3.

## TUGASMU SEKARANG (urutan)
1. JANGAN nyuruh founder menjalankan tes 5 detik screenshot -- keputusan
   2026-10-29: tes DIPINDAH ke S15 (link live ke 5 orang). S12 dibiarkan
   BELUM dicentang; itu disengaja, bukan kelupaan.
2. Kerjakan S8 (deploy BE ke Vercel): config sudah ada di `be@6cffbda`
   (`vercel.json` Go preset + README). Catat URL production, isi
   `ALLOWED_ORIGINS` dengan origin FE, `go test ./...` harus tetap hijau.
3. Lanjut S13 (deploy FE): import `bandung-sport-web` ke project Vercel
   kedua, build `npm run build`, output `dist`, env `VITE_API_URL` = URL BE,
   `vercel.json` rewrites SPA fallback. *Selesai:* refresh di
   `/tempat/v-...` tidak 404 di HP.
4. S14 (input data dari outreach) lalu S15 -- di sinilah tes 5 detik +
   pertanyaan "ini buat apa" dijalankan ke manusia sungguhan. Kalau gagal,
   yang diotak-atik adalah **subline** (tagline sudah dikunci, dan founder
   menolak kata "harga" di header -- lihat docs/copy/header.md § 1).

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
- Belum diverifikasi founder: `fe/.gitignore` punya baris mencurigakan
  `ga n` (belum di-commit, terlihat seperti keystroke nyasar) dan `be`
  commit `6cffbda` isinya cuma `.gitignore` tapi pesannya `fix(html)` --
  pesan commit salah tempel dari fe.

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
| S8 | konfigurasi siap | deploy BE (`be@6cffbda`) — jalankan sebelum S13 |
| S13 | berikutnya | deploy FE (Vercel preset Vite) + analytics |
| S14–S15 | | feedback 5–10 orang, concierge, README + CV |

Detail lengkap: `docs/plan-v3-delta.md` § 2.

## 2. Kondisi teknis per 2026-10-29

- `fe/` HEAD `3738531`, ter-push, working tree bersih **kecuali** `M
  .gitignore` (baris `ga n` — belum diputuskan).
- `be/` HEAD `6cffbda`, ter-push, `go test ./...` hijau.
- Root: `.gitignore` (`.commandcode`, `.qoder`) + `Makefile` di-commit
  2026-10-29, gitlink `be` dibump ke `6cffbda`.
- Endpoint: hanya 3 (list, detail, health). Tidak ada DB, tidak ada auth.
- Data: 22 venue, 8 penawaran berharga publik, `lastVerified` 2026-10-22.

## 3. Utang teknis yang diketahui

Lihat § "UTANG YANG DICATAT" di blok prompt atas — sudah satu tempat, jangan
diduplikasi.
