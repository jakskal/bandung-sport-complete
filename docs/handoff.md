# Handoff — Bandung Sport

> Dokumen serah-terima untuk AI agent / thread baru. Perbarui setiap sesi
> selesai (bersama `plan.md` § Status dan `AGENTS.md`).
>
> Terakhir diperbarui: **2026-10-28** (setelah S10 selesai, S11 belum commit).

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
4. docs/plan-v2.md § 6 (model data), § 7 (arsitektur FE)

## ATURAN KERAS
- Kode FE yang dirender (komponen, halaman, hook, CSS) DITULIS FOUNDER.
  Kamu: beri arah, review, tunjuk baris yang salah, beri SATU contoh per pola
  kalau founder stuck >30 menit. JANGAN menulis seluruh fitur FE untuknya.
- Kode Go (be/) boleh kamu tulis (founder mendelegasikan), tapi review-nya
  belum dilakukan.
- Scope dikunci. Jangan tambah fitur/dependency/endpoint di luar sesi aktif.
  Ide baru dicatat di plan.md § "Nanti".
- Jangan mengarang harga/jadwal/kontak/koordinat. Tidak tahu = null.
- Bahasa diskusi: Indonesia santai. Teks UI: Indonesia. Kode/commit/README:
  Inggris.
- Tutup sesi: lint+build hijau -> commit+push repo terkait -> centang checkbox
  di docs/plan-v3-delta.md -> update plan.md § Status + AGENTS.md.

## STRUKTUR REPO
/home/user/angling/proj/bandung-sport-complete/
  docs/plan-v3-delta.md   <- scope & urutan sesi
  plan.md                 <- status per sesi
  AGENTS.md               <- aturan kerja lengkap
  fe/   repo github.com/jakskal/bandung-sport-web (Vite + React 19, JS)
  be/   repo github.com/jakskal/bandung-sport-be (Go stdlib, go:embed)

## STATUS (2026-10-28)
- S9 SELESAI (fe@d28e599): FE baca API, useFetch, skeleton, error+retry,
  filter URL olahraga/tier/area, route /tempat/:venueId, data JSON FE dihapus.
- S10 SELESAI (fe@51252d5): PriceTag, OfferingRow, "Harga dicek <bulan tahun>",
  CTA WA, empty state. be@fa1e254 set lastVerified=2026-10-22 untuk 8 offering.
- S11 (hierarki card) HAMPIR SELESAI, BELUM COMMIT. Yang tersisa:
  * Place.css:26 `border: 1px solid var(--border),;` -> hapus koma (CSS invalid)
  * .place-card-header belum flex (badge tier harus di kanan nama)
  * .place-card h2 perlu font-size 1rem, margin 0
  * meta " - " -> " · ", TierBadge cukup tampil "Murah" bukan "Tier Murah"
  * PriceTag.css orphan (kelasnya .price-tag, komponen pakai .place-tag)
- Sesi berikutnya: S12 (header "ini apa" + CTA "Lihat semua tempat"),
  lalu S13 (deploy FE Vercel). S8 (deploy BE) sengaja ditunda ke sebelum S13.

## CARA MENJALANKAN
Node: wajib pakai ~/.n/bin (v22); `node` default v16.
  export PATH="$HOME/.n/bin:$PATH"
BE: cd be && ALLOWED_ORIGINS=http://localhost:5173 make run   (port 8080)
FE: cd fe && npm run dev                                      (port 5173)
Cek: npm run lint && npm run build
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

## JEBAKAN YANG SUDAH DITEMUKAN (jangan diulang)
- formatPrice menerima SATU objek, bukan positional args.
- priceFrom adalah objek -> spread `{...venue.priceFrom}`, jangan
  `priceMin={venue.priceFrom}`.
- priceVisibility bisa undefined -> fallback 'unknown'.
- oxlint menolak set-state-in-effect: loading diturunkan saat render
  (pola prevFetcher), bukan setLoading(true) di dalam useEffect.
- Nilai fungsi di useState/setter HARUS dibungkus: useState(() => fn),
  setPrevFetcher(() => fn) — kalau tidak, React memanggilnya sebagai updater.
- AbortController harus dibuat DI DALAM effect (satu controller = satu request).
- Vite: .env.local harus ada sebelum dev server start; nama variabel
  VITE_API_URL (konstanta di client.js saat ini membaca VITE_API_BASE_URL —
  perlu disamakan).

## TUGASMU SEKARANG
1. Selesaikan 5 perbaikan CSS/teks S11 di atas (biarkan founder yang mengetik;
   tunjuk baris + alasan, bukan menulis seluruh file).
2. Minta founder menjalankan tes 5 detik (tanya orang: "berapa harganya?").
3. Commit + push fe, update gitlink root (`git add fe be`), centang S11,
   update plan.md § Status + AGENTS.md.
4. Lanjut S12 sesuai docs/plan-v3-delta.md § 2.
```

---

## 1. Peta sesi (ringkas)

| Sesi | Status | Isi |
|---|---|---|
| S2–S5 | selesai | token CSS, `formatPrice`, skema v2, migrasi + validator |
| S6–S7 | selesai | katalog Go + HTTP API + Docker (`be@c471fa2`) |
| S9 | selesai | FE baca API (`fe@d28e599`) |
| S10 | selesai | detail page v2 (`fe@51252d5`, data `be@fa1e254`) |
| S11 | **belum commit** | hierarki card (harga hero + badge tier) |
| S12 | berikutnya | header "ini apa" + CTA |
| S13 | | deploy FE (S8 deploy BE diselipkan sebelum ini) |
| S14–S15 | | feedback 5–10 orang, concierge |

Detail lengkap: `docs/plan-v3-delta.md` § 2.

## 2. Kondisi teknis per 2026-10-28

- `fe/` HEAD `51252d5` + perubahan S11 belum di-commit.
- `be/` HEAD `fa1e254`, test hijau (`go test ./...`).
- Root repo: gitlink `fe`/`be`, dokumen di-commit terpisah.
- `src/data/` FE tinggal `types.js` (dokumentasi bentuk response API).
- `scripts/` FE tinggal `check-format-price.mjs`.
- Endpoint yang ada: hanya 3 (list, detail, health). Tidak ada DB, tidak ada auth.

## 3. Utang teknis yang diketahui

- `client.js` membaca `VITE_API_BASE_URL`, sementara `.env.example` menulis
  `VITE_API_URL` -> selalu jatuh ke fallback `localhost:8080`. Samakan sebelum
  deploy (S13).
- `navigate(-1)` untuk tombol "Kembali" di detail page; kalau halaman dibuka
  langsung dari share URL, riwayat bisa keluar dari aplikasi.
- `trace` review kode Go S6/S7 oleh founder belum dilakukan.
- Halaman detail masih menampilkan `venue.priceFrom` (harga termurah) sebagai
  harga utama, bukan ringkasan paket.
