# Spec: Wizard "Rekomendasi buat saya" — v0.3 RATIFIED (2026-10-11)

> Status: RATIFIED oleh pemilik repo — jadi kontrak build untuk nanti
> (eksekusi tetap GATED oleh Demand Gate: concierge test ≥3/5).
> Keputusan kunci ratifikasi: **general first** — segmentasi berbasis
> orang (usia/kondisi) ditunda sampai (1) tags terkurasi DAN (2) bukti
> concierge menunjukkan pengguna ingin saran person-shaped.
> Terakhir diubah: 2026-10-11.

## Prinsip (dari pemilik repo)

1. Simple dulu, general. Kondisi spesifik (ibu hamil, lansia) tidak masuk
   logic matching — cukup disentuh di disclaimer.
2. Output di-fase: **A dulu** (rekomendasi utama + alternatif) → lalu C
   (program seminggu) → lalu B (daftar ranked).

## Interpretasi "area" (DIKOREKSI 2026-10-11 — penting)

Area = **area tinggal user**, bukan sekadar filter exact-match.
Matching berbasis **kedekatan (proximity ranking)**:

1. Saring venue yang harganya masuk budget user.
2. Urutkan hasil by jarak dari area tinggal user (terdekat dulu).
3. **Rekomendasi utama** = budget match + paling dekat (ideal: satu area
   dengan user). **2 alternatif** = budget match lainnya, boleh area
   lebih jauh.
4. Transparansi: tiap hasil menampilkan jarak ("± 2 km dari Dago").

## Disclaimer (WAJIB, tampil paling atas halaman wizard)

> "Rekomendasi ini hanya berdasarkan lokasi, waktu yang bersedia
> disediakan, dan budget. Kondisi ibu hamil atau orang tua mohon aware
> dengan kondisi masing-masing dan konsultasikan dengan profesional."

## Input (2 parameter MVP — durasi DIPOTOTONG per amendemen dewan)

1. **Area tinggal user** — dropdown nama area (dari data venue).
2. **Budget** — angka + toggle satuan user: **per bulan** atau
   **per minggu** (keputusan pemilik repo 2026-10-11).

## Logic MVP

1. Filter: venue dengan harga masuk budget (sadar satuan — aturan
   normalisasi WAJIB terkunci + teruji sebelum logic ditulis, syarat CTO).
2. Rank by jarak: haversine(centroid area user, koordinat venue).
3. Output: 1 utama + maksimal 2 alternatif, dengan alasan singkat
   ("± X km dari {area}, sesuai budget {satuan} kamu").
4. Empty state: hasil kosong → pesan + saran longgarkan budget/area.
   (Launch blocker — Lihat Demand Gate.)

## Data requirement (BARU — dan hampir gratis)

- Tiap venue butuh `lat`/`lng`. **Sumber: parse dari sourceUrl Google
  Maps** — pattern `!3d{lat}!4d{lng}` sudah ada di mayoritas baris
  hasil scrape.
- Venue yang source-nya bukan Maps: koordinat dicari sekalian saat
  kurasi (data pass).
- **Centroid per area** = rata-rata koordinat venue di area itu
  (dihitung, tidak dikurasi manual).
- Matching logic = pure module + unit test (syarat CTO), portabel ke
  BFF saat milestone Go tiba.

## Letak & akses (keputusan dewan, expert-grounded)

- **List page: CTA card besar di atas daftar** — satu-satunya primary
  action. Teks: "Rekomendasi buat saya — olahraga sesuai waktu & budget
  kamu".
- **Halaman `/rekomendasi`: SATU layar** — disclaimer + 2 input + hasil
  dirender di bawah form yang sama (NN/g: input + hasil satu layar
  untuk eksplorasi what-if).
- Hasil wizard dapat di-share via URL params (perluasan search-params).

## Demand Gate (syarat dewan — sebelum wizard dibangun)

1. **5 wawancara** Torres-style: "ceritakan terakhir kali kamu memilih
   tempat olahraga" — apa yang sebenarnya menentukan keputusan?
2. **Concierge test**: kirim rekomendasi MANUAL ke 5 orang via WhatsApp.
3. **Gate**: ≥3 dari 5 bertindak atas rekomendasi manual → wizard dibangun.
   Gagal → wizard dibatalkan, persevere di directory (hemat 3 minggu).

## Launch blockers (syarat dewan — peluncuran tanpa ini = dilarang)

1. Aturan normalisasi harga terkunci + tested (weekly→monthly, dsb).
2. Data validation script: tier↔harga konsisten, field wajib ada,
   `lastVerified` terisi — output-nya menggerbangkan data pass.
3. Empty-state wizard terdesain.
4. Event logging minimal client-side (wizard_used, rec_clicked,
   return_visit) — tanpa infrastruktur tambahan.

## Sumber keputusan (board review 2026-10-11)

Dewan: Investor #1, CEO, Investor #2, Product Lead, CTO — semua
GO-WITH-CONDITIONS. Keputusan kunci: decision-layer wedge (harga
ternormalisasi + kesesuaian = data yang tidak diindeks Maps),
concierge gate, serial sequencing untuk founder solo, potong durasi,
density beats breadth. Detail lengkap: board + task results.

## Logic Spec v1 (hasil board review session 2, 2026-10-11 — konsensus 4 role + Investor #1)

> Status: HYPOTHESIS DOCUMENT — invariants terkunci, dibangun HANYA setelah
> Demand Gate lolos (concierge test ≥3/5). Bukti concierge yang menentukan
> parameter, bukan spec ini.

### Module & signature

`src/lib/recommend.js` — pure, deterministic, no React imports, no I/O,
unit-tested; portable ke Go BFF tanpa perubahan.

```
recommendVenues({ homeArea, budgetAmount, budgetUnit }, venues, areas)
→ { main: Rec|null, alternatives: Rec[0..2], empty: EmptyResult|null }
```

Helpers terpisah, masing-masing independently tested: `normalizeBudget`,
`normalizeVenuePrice`, `haversineKm`, `areaCentroid`, `rank`,
`buildReasonParts`, `serializeState`/`parseParams`.

### Langkah (ordered)

1. Validasi input; throw jika area tidak dikenal / budget ≤ 0.
2. Budget → monthly (minggu × `WEEKS_PER_MONTH = 4`).
3. Harga venue → monthly: bulan as-is; minggu ×4; kunjungan ×4
   (`VISITS_PER_MONTH`, asumsi DITAMPILKAN di alasan); **jam → null =
   unmatchable** (dikecualikan, ditawarkan di empty state dengan
   satuan aslinya); matching memakai `priceMin`; `priceMax` display-only;
   `priceMin` null → unmatchable (aturan null). Normalisasi **floor**
   ke 1000 IDR — tidak pernah membulatkan venue masuk budget.
4. Kandidat = monthly ≤ budget DAN punya lat/lng (tanpa koordinat →
   dikecualikan + di-log validator).
5. `distanceKm = haversineKm(areaCentroid(homeArea), venue)` (1 desimal);
   centroid = rata-rata koordinat venue per area (dihitung, bukan
   dikurasi manual; outlier sensitivity di-log validator).
6. Sort asc: jarak → harga ternormalisasi → nama (deterministik).
7. `main = [0]`; `alternatives = [1..2]`.
8. `reasonParts` HANYA dari nilai yang mempengaruhi ranking
   ("±2,1 km dari {area}", "Rp450rb/bln — masuk budget Rp500rb/bln",
   "≈ asumsi 4 kunjungan/bln"); **tags/kategori dilarang** — mereka
   tidak ikut ranking.
9. Empty → 3 venue terdekat TANPA peduli budget, dilabeli
   "di luar budget" + hint longgarkan; BUKAN framing rekomendasi.

### Usia: TIDAK di MVP (konsensus)

Melanggar 3 gerbang sekaligus (matcher tidak memakainya, data band belum
ada, nol bukti demand — preseden durasi). Bentuk eventual kalau lolos
gerbang: **static curated rules table sebagai CONTENT** —
`{band, tags[], weeklyMinutes{min,max}, citation{source,year,url},
version}` — dipakai pure function, tampil sebagai **suitability flag /
reason part yang visible, TIDAK PERNAH numeric multiplier** (multiplier
mengubah urutan diam-diam dan membunuh explainability). Usia ditanya di
wawancara discovery, bukan di form.

### Tags contract

Flat strings, controlled vocabulary di SATU file constants
(`src/data/tagVocabulary.js`); validator menolak tag di luar vocabulary;
tag boleh ship hanya kalau ada consumer yang membacanya. **Nama band**
("pemula", "lansia", "ibu-hamil"), bukan min/max range. Fase A:
display-only ("biasanya cocok untuk: …"); tag-driven matching menunggu
data pass 40–60 + bukti concierge.

### WHO guidelines = CONTENT, tidak pernah logic

`docs/content/guidelines.json` — `{version, bands[...], citation{...}}`,
schema-checked validator; pure `volumeTargetFor(band, guidelines)`
membacanya. **Structurally enforced: type `Rec` tidak punya field untuk
guidelines** — result cards hanya membawa provenance (pagar screenshot
Investor #2, di-enforce compiler). Fase A merender nol; fase C boleh
merender satu baris sitasi generik sebagai content component terpisah.

### Fase C preview (hypothesis, gated bukti fase A)

`buildWeeklyProgram({...}, venues, guidelines) → slots + totals` —
deterministic editable template + "konfirmasi jadwal via WA".
Missing contracts: openHours venue, jadwal kelas, `durasi_sesi`, harga
per-kunjungan lengkap. Tidak pernah menjanjikan: booking, jam kelas,
adherence, outcomes, "meets WHO for you", travel-aware plans (v3).

### Tests wajib

Golden normalization table (termasuk flooring + asumsi ×4), tie-breaks,
empty state, reason ⊆ computation inputs, `serializeState`/`parseParams`
round-trip, determinism.

## Fase berikutnya (jangan dibangun sekarang)

- Fase C: program seminggu (multi-venue, jadwal, total biaya).
- Fase B: daftar ranked lengkap.
- Matching kondisi khusus (ibu hamil dll) = menunggu tags kurasi.
- Travel time (data rute/peta) = v3.
