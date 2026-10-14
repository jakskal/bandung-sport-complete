# Board Review: UI gagal tes 5 detik — 2026-10-13

> Status: HASIL DEWAN, MENUNGGU ARBITRASE pemilik repo (lihat § "Yang
> dewan TIDAK sepakat"). Bukan sumber kebenaran; setelah diarbitrase,
> keputusan final dicatat di `plan.md` § Status.
> Format: 5 peran (2 investor, CEO, Product Lead, CTO) dijalankan sebagai
> subagent paralel, 2 ronde, lalu sintesis fasilitator.

## Fakta data yang diverifikasi fasilitator (bukan klaim peran)

Dicek langsung dari `fe/src/data/places.json` saat review:

- 22 venue, **hanya 8 yang punya harga** (`priceMin != null`).
  Per kategori (total/berharga): Fitness 7/2, Futsal 5/3, **Padel 5/0**,
  Swimming Pool 3/1, Running Track 2/2.
- Area: Dago 7, Cibiru 4, Arcamanik 3, Lembang 3, Setiabudi 2,
  Sumur Bandung 1, Bandung Wetan 1, Citarum 1.
- Koordinat bisa di-parse dari `sourceUrl` (pola `!3d…!4d…`): **11/22**.
- `priceUnit` yang terpakai: `monthly`, `per-hour`, `per-visit`
  (`weekly` belum ada di data, tapi dibutuhkan wizard).
- Field `lastVerified`: **belum ada sama sekali**.
- Koreksi: di Round 2, CTO menghitung 9 venue berharga — yang benar 8
  (hitungan Inv#2).

---

## ROUND 1 — Analisis pembuka

### INVESTOR #1 — Operator-angel

**Kekuatan**

1. **Wedge-nya beneran ada.** Harga gym/padel di Bandung biasanya ngumpet di DM IG dan chat WA. Kamu punya 22 venue dengan harga riil plus unit (monthly/per-hour), dan itu data yang nggak bisa diindeks Google Maps. Ini persis maksud PG di "Make Something People Want": masalahnya nyata, bukan karangan.
2. **Disiplin data yang jujur.** Aturan "harga null → tier null" dan chip "Belum dikurasi" itu sikap yang jarang ada di produk direktori. Kepercayaan user ke harga adalah produknya.
3. **Fondasi share sudah jalan.** Filter tersinkron ke URL params, jadi "nih, futsal murah di Buahbatu" bisa dikirim via WA dalam satu link. Itu mekanisme distribusi gratis yang belum kamu pakai.
4. **Kamu sudah belajar dari koperasi-react.** Demand gate ≥3/5 sebelum wizard dibangun adalah naluri yang benar.

**3 kekhawatiran utama**

1. **Nol user setelah 3 minggu, dan pola koperasi-react mulai terulang.** List, filter 3 dimensi, routing, 404, scroll restoration, tab title, semuanya sudah ada. Tapi belum ada satu pun manusia yang membuka link-nya. Di "Startup = Growth", PG menyebut metrik yang penting adalah user, bukan fitur. Sejak 2026-10-11 deploy statis masih berstatus "pending", padahal tutor sudah merekomendasikan deploy duluan.
2. **Redesain visual: build trap atau prasyarat? Dua-duanya, tergantung skop.** Jujur saja: `<title>fe</title>`, h1 tanpa tagline, dan "Tier: Sedang" sebagai teks berwarna memang bikin tes 5 detik gagal. Kalau teman bingung "ini apaan", tesnya nggak mengukur demand, cuma mengukur kebingungan. Jadi **komunikasi** itu prasyarat. Tapi "warna ad-hoc 4 abu-abu", design system, font custom, dan spotlight CTA wizard itu polish sebagai prokrastinasi. Teman nggak akan pergi gara-gara #f0f0f0 vs #f7f7f7. Mereka pergi kalau nggak paham gunanya. Rework bilang "Good enough is fine". Bedakan tegas: *clarity* wajib, *beauty* nanti.
3. **Wizard ikut menyedot fokus walau belum di-gate.** Mendesain spotlight CTA untuk fitur yang belum lolos concierge test artinya membangun pintu ke ruangan yang belum ada. "Do Things That Don't Scale": rekomendasinya dikirim manual lewat WA dulu, UI-nya belakangan.

**Rekomendasi konkret**

*Minimum presentable* (target ≤5 sesi × 60 menit, sesudah itu langsung deploy):

1. **Tagline + subjudul di header.** Ini 80% solusi tes 5 detik.
2. **`<title>` dan `lang="id"`.** Judul tab = tagline, karena preview link di WA juga ambil dari sini.
3. **Card dibikin bisa di-scan dalam 2 detik.** Urutannya: nama (tebal) → harga besar dengan unit Indonesia ("Rp450rb/bln", bukan "/ monthly") → kategori · area dalam satu baris kecil → tier sebagai badge berwarna (CSS variable-nya sudah ada, tinggal dipakai). Buang label "Harga:" dan "Tier:".
4. **Label dimensi di atas chip** ("Olahraga", "Budget", "Area").
5. **Satu font sistem yang rapi + satu skala spacing.** Ganti #b2dfae dan #007bff dengan satu warna aksen. Selesai.
6. **Detail page:** tombol WA dibikin jadi tombol hijau besar "Tanya via WhatsApp". Itu satu-satunya aksi yang penting di halaman itu.

**Kandidat tagline** (konkret, sebut angka/harga):

- **"Cari tempat olahraga di Bandung, lengkap dengan harganya."**
- **"Gym, futsal, renang, padel di Bandung. Harga jelas, tanpa DM dulu."**
- **"Olahraga di Bandung sesuai budget, dari gratis sampai premium."**

Favoritku yang kedua, karena langsung menyebut sakitnya ("tanpa DM dulu"), sesuai wedge-mu.

**DILARANG sekarang:**

- Desain atau CTA wizard sebelum gate ≥3/5 lolos.
- Dark mode, animasi, ilustrasi, logo, font custom, CSS Modules/Tailwind migration.
- Menambah venue di luar 22 sebelum 5 teman pakai. Density yes, tapi validasi dulu. Data pass 40–60 jalan paralel *setelah* link pertama terkirim.
- Refactor dead code `.modal`. Hapus saja dalam 1 menit, jangan dijadikan PR.
- Mengukur "siap" pakai perasaan. Definisinya: seorang teman bisa menjawab "ini buat apa?" dalam 5 detik tanpa dijelaskan.

**Urutan minggu ini:** polish komunikasi (≤5 sesi) → deploy statis (Netlify/Vercel, 1 sesi) → kirim link + 1 pertanyaan ke 5–10 teman via WA → sekaligus jalankan concierge test wizard secara manual. Getting Real: "Ship it, then iterate on real feedback."

Posisi saya: redesain sebatas *clarity* (tagline, harga terbaca, badge tier, tombol WA) itu prasyarat sah untuk tes teman, maksimal 5 sesi, dan semua yang lewat dari itu sebelum link pertama terkirim adalah prokrastinasi berkedok polish.

### INVESTOR #2 — Consumer/local-services VC

**Kekuatan (jujur, tanpa dilebihkan)**

1. **Data harga riil itu satu-satunya aset yang benar-benar langka.** priceMin/priceMax plus unit (monthly/weekly/per-visit/per-hour) adalah data yang tidak terindeks Google Maps, karena harganya ngumpet di DM IG dan chat WA. Ini cocok dengan tesis Thiel soal *secret*: ada kebenaran yang belum dikumpulkan orang lain. Tapi 22 venue itu belum moat, baru benih moat.
2. **Disiplin "null = belum terverifikasi" adalah fondasi kepercayaan.** Founder menolak mengisi nilai karangan, jadi sinyal kepercayaan terbangun lewat data, bukan copywriting. Di local services, satu harga salah di WA bisa mematikan trust untuk selamanya.
3. **URL-sync filter dan koordinat GPS dari Google Maps** membuat hasil bisa dibagikan dan wizard berbasis jarak bisa dibangun tanpa backend. Biayanya murah, *optionality*-nya tinggi.
4. **Guardrail anti-scope dan demand gate ≥3/5 sebelum build.** Ini jarang saya lihat di founder solo. Pelajaran dari koperasi-react benar-benar dipakai.

**3 kekhawatiran utama**

1. **UI saat ini menyembunyikan wedge-nya sendiri (Porter: positioning yang tidak terbaca sama dengan tidak ada positioning).** Coba lihat card-nya: `<p>{area}</p><p>{category}</p><p>Harga: Rp 500.000 - Rp 2.500.000 / monthly</p>`. Harga, yang merupakan satu-satunya alasan orang tidak cukup pakai Google Maps, tampil sebagai paragraf yang bobotnya sama dengan nama area. Unit "monthly" tampil mentah dalam bahasa Inggris, padahal `lang="en"` dan `<title>fe</title>`. Di mata user, ini terlihat seperti daftar Google Maps versi lebih miskin, padahal sebenarnya ini satu-satunya tempat yang menjawab "berapa duitnya". Substitute terkuat di sini bukan Maps, melainkan **"tanya di grup WA / DM IG"**. Kalau UI tidak langsung menunjukkan bahwa jawabannya sudah ada di sini, user akan balik ke kebiasaan lama.
2. **Staleness tidak terlihat, jadi trust tidak bisa diverifikasi.** Detail page tidak punya "terakhir diverifikasi". Harga gym di Indonesia bergerak tiap promo. Ritual freshness bulanan itu moat di belakang layar, tapi user tidak bisa melihatnya. Moat yang tidak terlihat tidak menaikkan konversi dan tidak membuat user kembali. Ditambah "Harga: N/A" dan tier "N/A", yang dibaca user sebagai produk setengah jadi, padahal sebenarnya itu tanda kejujuran.
3. **Cold-start: ini belum marketplace, dan itu bagus. Tapi supply-nya rapuh.** Menurut kerangka a16z, fase ini adalah "come for the tool". Masalahnya, supply data bergantung pada founder 45–60 menit/hari. Kalau target 40–60 venue dengan refresh bulanan, hitung kasar ±50 venue × beberapa menit per cek WA. Itu bisa memakan seluruh jatah waktu harian. Gurley akan bertanya: berapa *cost per verified price*, dan apakah biaya itu turun seiring skala? Saat ini belum ada mekanisme supaya biayanya turun. Risiko disintermediation (user cukup simpan nomor WA venue lalu tidak kembali) juga nyata. Retensi hanya datang dari *perubahan* harga dan dari keputusan berikutnya, bukan dari kontak.

**Rekomendasi konkret (positioning & brand)**

- **SATU hal yang harus diteriakkan dalam 5 detik adalah harga asli, bukan "hubungi via DM".** Tagline di header menggantikan `<p>{count} tempat ditemukan</p>` sebagai elemen utama, misalnya: *"Harga asli tempat olahraga di Bandung, tanpa DM."* Jumlah tempat cukup jadi teks kecil. Ganti juga `<title>` dan `lang="id"`. Ini sepenuhnya copy dan CSS, jadi tidak melanggar guardrail.
- **Harga dijadikan hero di card.** Hierarki visualnya: harga angka besar dan tebal ("Rp450rb**/bln**", dengan unit yang dilokalkan: /bln, /minggu, /datang, /jam), lalu nama, lalu area dan kategori kecil abu-abu. Tier jadi badge pill, bukan teks "Tier: Sedang". Ini adalah tes 10x Thiel dalam bentuk visual: informasi yang di Maps butuh 3 chat, di sini terbaca dalam 1 lirikan.
- **"Terakhir diverifikasi" adalah badge kepercayaan, bukan footnote.** Taruh tepat di bawah harga, misalnya "✓ Dicek Okt 2026", di card dan detail page. Untuk harga null, jangan tulis "N/A". Tulis dengan jujur: *"Harga belum terverifikasi. Tanya via WA"* dan jadikan tombol WA sebagai CTA utama. Kejujuran diubah jadi fitur. Ritual bulanan sekarang punya wajah.
- **Brand: buang palet Ant Design pinjaman, biru bootstrap #007bff, dan hijau misterius #b2dfae.** Pilih 1 warna brand dan 1 skala abu-abu (bukan 4). Pakai warna tier dengan semantik yang konsisten. Visual seadanya mengirim sinyal "data seadanya", dan itu fatal untuk produk yang menjual *trust on price*.
- **CTA wizard diposisikan sebagai jawaban, bukan filter.** Buat banner spotlight di atas chip: *"Budget Rp500rb/bln, tinggal di Dago? Kami pilihkan 1 tempat terdekat yang masuk budget."* Copy-nya menjanjikan **keputusan** (1 rekomendasi + alasan), bukan daftar. Inilah decision layer: Maps memberi pilihan, Bandung Sport memberi jawaban. Sebelum wizard dibangun, CTA ini bisa langsung mengarah ke WA concierge, sehingga sekaligus menjadi instrumen demand gate 3/5 tanpa melanggar guardrail.

**Posisi saya:** belum layak dapat cek hari ini (nol user, 22 venue, biaya verifikasi belum terukur). Tapi wedge harga ternormalisasi itu nyata dan tidak bisa ditiru Maps. Kalau dalam 4–6 minggu UI sudah meneriakkan harga dan tanggal verifikasi, dan concierge test lolos ≥3/5, saya mau bicara serius.

### CEO — Eksekusi

**1. Kekuatan**
- **Kecepatan eksekusi sudah terbukti.** Dalam 3 minggu, dengan 45–60 menit per hari, list page, filter 3 dimensi, detail page, routing, 404, dan scroll restoration sudah jadi. Artinya mesin build-nya jalan. Yang belum pernah dites adalah apakah ada yang mau pakai.
- **Filter tersinkron ke URL params.** Ini aset distribusi yang belum dipakai. Link seperti `?category=Fitness&tier=Murah` bisa langsung dikirim di WA, dan begitulah cara app ini akan menyebar di tahap awal.
- **Data jujur.** Ada aturan null → tier null, dan ada `sourceUrl` yang memuat koordinat. Wedge "harga ternormalisasi" punya dasar nyata, bukan sekadar klaim.
- **Founder bisa mengkritik produknya sendiri.** Bilang "saya nggak tahu ini buat apa" itu tanda task-relevant maturity (istilah Grove) di sisi produk. Banyak founder tidak sampai ke titik ini.

**2. Tiga kekhawatiran utama**

**a. Redesign tanpa batas, gaya wartime tapi untuk masalah yang salah.** Horowitz bilang, di masa wartime CEO memotong apa pun yang tidak menyelamatkan perusahaan. Ancaman eksistensial di sini adalah nol user dan belum deploy, bukan warna abu-abu. Kalau "sistem desain" dibuka tanpa batas waktu, founder part-time bisa habis 2–3 minggu memilih font dan spacing scale. Itu pola koperasi-react yang terulang: produknya makin cantik, tapi tetap tidak ada yang pakai.

**b. Tes 5 detik itu sebagian besar soal copy, bukan visual.** Lihat fakta kodenya: `<title>fe</title>` dan `lang="en"`; h1 tanpa tagline; `/ monthly` dibiarkan mentah dalam bahasa Inggris; "N/A" muncul di data; `<select>` tanpa label. Semua ini bisa dibereskan dalam 1 malam. Hierarki visual card juga cuma butuh 1–2 malam: harga dibesarkan, tier jadi badge, dan 4 abu-abu hardcode diringkas jadi 2. Sistem desain penuh adalah optimasi untuk masa peacetime, dan kita belum di sana.

**c. Distribusi masih fantasi.** Hoffman memang bilang distribusi itu eksistensial. Tapi dia juga bilang jangan scale sebelum PMF. Di tahap ini "distribusi" artinya 5–10 orang yang benar-benar membuka link dan diwawancara. Itu tidak mungkin terjadi sebelum deploy. Jadi deploy harus didahulukan sebelum data pass 40–60 venue. Soal kepadatan data: 22 venue yang tersebar di 5 kategori terlalu tipis per kategori. Data pass sebaiknya fokus memadatkan **Fitness**, karena itu wedge-nya, bukan menambah kategori.

**3. Rekomendasi: urutan serial ber-timebox** (direvisi di Round 2 — lihat tabel di sana)

Versi Round 1: copy pass 1 mlm → hierarki minimum 2 mlm → tes 5 detik 1 → validator 2 → deploy 1 → fake-door wizard di header 1 → intro teman + 5 wawancara 5 → data pass 6–8 (Fitness ±25–30). Total ±20 malam. Dipotong: sistem desain penuh, spotlight CTA, ikon/animasi/dark mode, custom domain, kategori baru sebelum wawancara, target 60 venue.

**Posisi saya:** perbaiki copy dan hierarki dengan timebox ketat 3 malam, validasi dengan tes 5 detik, deploy di malam ke-7, dan jadikan CTA wizard fake-door ke WA. Redesign yang lebih besar baru boleh dibuka lagi kalau ada data dari user nyata yang membenarkannya.

### PRODUCT LEAD

**1. Kekuatan**
- **Data harga asli itu value prop yang paling mahal, dan ini sudah dipegang.** Moat-nya sudah ada, cuma belum kelihatan.
- **Filter tersinkron ke URL params.** Modal penting buat concierge test via WA.
- **Filter 3 dimensi sudah lengkap** dengan empty state dan reset kondisional.
- **Disiplin guardrail.** Kebalikan dari build trap ala Perri.

**2. Tiga kekhawatiran utama**

**(a) Yang gagal di tes 5 detik adalah value risk, bukan sekadar usability.** Header cuma `<h1>Bandung Sport</h1>` + count, tab browser `fe`. Akar masalahnya **hierarki informasi**, bukan warna. Di card, lima `<p>` punya bobot sama, jadi mata tidak punya titik masuk (F-pattern gagal di baris pertama). Label literal "Harga:"/"Tier:" dan unit mentah melanggar heuristik NN/g #2 (*match between system and the real world*).

**(b) Polish tanpa metrik = output, bukan outcome (Perri/Cagan).** Protokol 5 teman, ≤15 menit/orang: (1) *5-second test* — screenshot mobile 5 detik, tanya "Situs ini buat apa?" dan "Ingat harga salah satu tempat?"; **lulus ≥4/5** sebut "cari tempat olahraga sesuai budget", ≥3/5 ingat angka harga. (2) *First-click test* — "Cari kolam renang yang murah", target <20 detik, 4/5. Jalankan **sebelum** redesign (baseline) dan **sesudah**.

**(c) Sinyal kepercayaan rusak.** "Harga: N/A", "Tier: N/A", chip "Belum dikurasi" terbaca bug. Merah #ff4d4f untuk "Mahal" bermakna *error* (heuristik #4). Detail page tak punya alamat, link Maps, lastVerified.

**3. Rekomendasi konkret**

**(a) Hierarki card: nama → harga → konteks → tier.**
```
FTL Gym Dago                    [Sedang]   ← nama 18px bold; badge pill kanan atas
Rp500rb–2,5jt /bulan                       ← harga 20px bold, elemen terberat
Fitness · Dago                             ← meta 14px abu, satu baris
```
Jarak JANGAN di card list (eksklusif wizard). Harga null → "Harga belum dicek · tanya via WA", badge tier tidak tampil.

**(b) Copy: sekarang → usulan**

| Sekarang | Usulan |
|---|---|
| (tidak ada tagline) | **"Cari tempat olahraga di Bandung, sesuai budget-mu."** |
| (tidak ada) | Sub: "Gym, futsal, renang, padel & jogging, dengan harga asli, dari gratis sampai premium." |
| `22 tempat ditemukan` | "22 tempat" (pindah ke atas grid) |
| baris chip tanpa label | Label: **"Olahraga"**, **"Budget"**, **"Area"** (inline kiri agar ≤130px) |
| chip "Semua" | "Semua olahraga" / "Semua budget" |
| "Belum dikurasi" | "Harga belum dicek" |
| `monthly / weekly / per-visit / per-hour` | `/bulan`, `/minggu`, `/kunjungan`, `/jam` |
| `Rp 500.000 - Rp 2.500.000` | `Rp500rb–2,5jt` |
| `<title>fe</title>`, `lang="en"` | "Bandung Sport — Tempat olahraga sesuai budget", `lang="id"` |
| "Tidak ada tempat yang ditemukan." | "Belum ada tempat yang cocok. Coba longgarkan budget atau area." + Reset |

**(c) CTA wizard:** jangan CTA ke halaman yang belum ada. Pra-gate: card *sekunder* ringan **setelah card ke-6 / akhir list** — "Bingung pilih? Kirim area + budget-mu, kami pilihkan 3 tempat terdekat." → `wa.me`. Pasca-gate naik ke primary sesuai spec v0.3; kata "waktu" dihapus dari copy spec (input wizard tidak menanyakan waktu). Di /rekomendasi: disclaimer satu baris di bawah tombol.

**(d) Detail page:** (1) "← Semua tempat" link teks; (2) nama + "Fitness · Dago" + badge; (3) blok harga besar + "Dicek: Okt 2026" (butuh lastVerified); (4) tombol primary "Chat via WhatsApp" full-width; (5) sekunder "Buka di Google Maps" (link keluar dari sourceUrl), Instagram, Website (label bukan URL mentah); (6) info tambahan.

**Posisi saya:** jalankan 5-second test baseline dulu, perbaiki hierarki dan copy sebelum menyentuh palet warna, dan ganti CTA wizard dengan card concierge WhatsApp yang jujur sampai demand gate lolos.

### CTO

**1. Kekuatan teknis**
- **URL sebagai sumber kebenaran filter** (`useSearchParams`): shareable, tahan refresh, back/forward gratis. Wizard harus pakai pola yang sama.
- **Derived state disiplin**: `visiblePlaces`, `hasActiveFilters` dihitung, tidak disimpan dobel.
- **Aturan data jujur** (null = belum terverifikasi, tier turunan harga) = cikal bakal data contract.
- **Stack boring dan gampang dibuang.**

**2. Tiga kekhawatiran utama**

**(a) Sistem desain jangan over-engineered.** 3 halaman, ±6 file CSS. Masalahnya "belum ada keputusan" (4 abu-abu, hijau tanpa alasan, biru bootstrap), bukan kurang framework. Tailwind menyembunyikan cascade/specificity/box model yang justru sedang dipelajari. **CSS custom properties cukup.**

**(b) Broken windows dulu**: (1) logika harga 3 cabang duplikat di `Place.jsx` & `PlaceDetail.jsx` (sudah pernah regresi); (2) unit Inggris; (3) dead `.modal*`; (4) leak `.place-list li` di `Place.css`.

**(c) Redesign tanpa kontrak data.** Badge "Terverifikasi", alamat, jarak — field-nya belum ada. UI duluan → data diisi asal → melanggar "null ≠ karangan". Vocab `priceUnit` harus dikunci sebelum map label.

**3. Rekomendasi**

**(a) Tokens** di `:root` `src/styles/index.css` (versi ±20; dipangkas jadi 12 di Round 2): font system-ui; space 4/8/12/16/24/32; text sm/base/lg/xl; 1 brand; text/muted/border/surface; tier ×4; radius 8. `body { font-family: var(--font-sans) }`. Aturan: **hex literal hanya di `:root`** (`grep '#[0-9a-f]' src -r`).

**(b) Refactor wajib**: `src/utils/formatPrice.js` (`formatPrice(place)` + `PRICE_UNIT_LABEL`), hapus `.modal*`, pindahkan `.place-list li`, `index.html` lang/title/meta description.

**(c) Kontrak data + validator**: `lat`/`lng`, `lastVerified` (ISO), `address` nullable. `scripts/validate-places.js` Node polos: id unik, field wajib, unit ∈ vocab, min ≤ max, null → tier null, tier ∈ enum, bbox Bandung Raya (≈ -7.2…-6.7, 107.4…107.9), lastVerified ≤45 hari (warning), area/category kanonik. Exit ≠ 0 → blocker sebelum build.

**(d) AI-tooling**: AI tulis token + 1 card contoh; founder migrasi sisanya; AI review screenshot 375px; AI tulis test case, founder implementasi; tidak ada PR "redesign semua".

**Jangan diadopsi**: Tailwind, shadcn/MUI/Chakra, TypeScript (setelah v1.0 deploy), CSS Modules (belum ada bentrok), font kustom.

Posisi saya: bereskan broken windows + kontrak data + validator, lalu redesign pakai CSS variable dengan founder yang mengerjakan migrasinya.

---

## ROUND 2 — Cross-reply

### INVESTOR #1

**1. CTA pre-gate → WA concierge: posisi saya berubah, tapi sedikit.** Yang saya larang di Round 1 adalah *desain wizard*: banner, form, atau janji produk yang belum ada. Link wa.me ke founder bukan fitur. Itu concierge, persis "do things that don't scale" (PG). Jadi saya izinkan **satu link teks**, maks 30 menit. Ke Inv#2: spotlight di atas chip saya tolak — ruang paling berharga dipakai menjual hal yang belum ada, dan tes 5 detik jadi kabur. Penempatan: versi PL (setelah card ke-6). Ke CEO: concierge test yang sebenarnya bukan tombol, tapi founder yang nge-WA 5 teman langsung.

**2. CTO vs CEO: jalur CEO lebih cepat, dengan satu koreksi.** `formatPrice` + label unit otomatis masuk copy pass (unit "monthly" tak bisa diperbaiki tanpa menyentuh dua file duplikat). Dead `.modal` dan leak CSS belum perlu — "It's a problem when it's a problem" (Rework). Validator sebelum deploy, bukan sebelum redesign. `lastVerified` per venue: jangan dulu — satu baris global "Data harga dicek Oktober 2026" di footer cukup. Tokens ±8 cukup; 20 itu "half-assed, not half" terbalik.

**3. PL: baseline formal itu overhead, skip.** Peserta tes = sumber daya langka; tiap teman hanya bisa jadi "first eyes" sekali. Simpan untuk tes sesudah redesign. Cukup screenshot UI lama disimpan di `docs/`.

**4. CEO: densify Fitness, setuju** — tapi *setelah* wawancara, dan kepadatan **per area** (8 gym di Dago lebih berguna daripada 30 tersebar — Facebook mulai dari Harvard).

**Posisi akhir:** copy pass + `formatPrice` + hierarki dengan ±8 tokens → validator minimal → deploy → WA 5 teman personal. ±6 malam sampai link pertama terkirim. Satu link concierge di bawah list boleh; spotlight, baseline formal, data pass sebelum wawancara tidak.

Setuju: CEO (copy dulu, Fitness, ±40), CTO (`formatPrice`, no Tailwind/TS), PL (hierarki card, CTA bawah, "Harga belum dicek"), Inv#2 (harga hero). Tidak setuju: Inv#2 (spotlight, lastVerified per venue sekarang), CTO (20 tokens, bersih-bersih tak terlihat), PL (baseline formal), CEO (fake-door di header).

### INVESTOR #2

**Fakta yang mengubah posisi saya:** dari 22 venue, harga terisi cuma **8**. Fitness 2/7, Padel 0/5, Renang 1/3, dan 7/22 di Dago. Wedge "harga asli" baru ada sepertiganya.

**1. CTA: posisi saya geser.** Banner di atas chip = directory bersaing dengan dirinya sendiri. Core action (filter → detail) harus dibuktikan dulu (Andrew Chen, cold-start). WA concierge di atas → user malas langsung chat founder, tes directory kotor. Kompromi: penempatan PL, setelah card ke-6 / akhir list. Header CEO sama kanibalnya. Framing PL "kirim area + budget, kami pilihkan" jujur karena ruangannya ada (founder). Decision-layer cukup dijual lewat tagline.

**2. `lastVerified`: terima urutan CTO, tolak estimasi waktunya.** Validator 22 baris ±1 malam, isi field 1 malam. `lastVerified` hanya untuk 8 venue yang harganya dicek, sisanya null. Badge mundur maks 2 sesi.

**3. Densitas: CEO dan Inv#1 dua-duanya meleset.** Masalahnya bukan jumlah venue, tapi harga terverifikasi. Menambah venue saat 14 lama masih kosong = menambah utang. Isi 14 harga existing itu bukan "tambah venue" — lakukan sekarang. Densify boleh, **satu** kategori. Batas "tidak memalukan" wizard: per kategori ≥3 venue berharga terverifikasi di tiap 3–4 cluster area utama, ±12–15 venue. Padel 0 harga → sembunyikan dari wizard.

**4. Merah untuk Mahal: setuju PL.** Terbaca menghakimi venue, melanggar "describe venues, never assess". Pushback ke `#dc2626` CTO.

**Posisi akhir:** wedge valid setelah 22 harga terisi + tanggal cek. Isi harga dulu, CTA concierge akhir list, deploy. Tambah venue hanya Fitness sampai 12–15 harga terverifikasi.

Setuju: PL (CTA akhir list, buang merah), CTO (kontrak data sebelum badge), CEO (deploy, densify satu kategori), Inv#1 (clarity dulu). Tidak setuju: Inv#1 (larangan total CTA), CEO (header; ±40 saat 64% harga kosong), CTO (merah).

### CEO

**Ke CTO (urutan): saya ubah.** Copy pass mengganti unit → tanpa `formatPrice` harus ditulis 2× di 3 cabang yang sudah pernah regresi. 30–45 menit, masuk malam pertama. Validator tetap, posisinya **sebelum badge "Dicek" tampil dan sebelum deploy**, bukan sebelum redesign. `lat`/`lng` baru dibutuhkan saat data pass.

**Ke CTO (tokens):** ±20 variable = isi "hierarki minimum 2 malam" saya. Yang saya potong: skala penuh, dark mode, dokumentasi token. Sepakat, beda istilah.

**Ke PL (baseline): terima, versi asinkron** — screenshot 375px via WA ke 5 orang, ±0,5 malam, paralel. Ries: baseline = langkah pertama innovation accounting. Founder juga tahu apakah masalahnya copy atau hierarki — bisa pangkas 2 malam hierarki.

**Ke semua (CTA): batalkan fake door di header.** Inv#2 di atas chip mengkanibal tes direktori. Ke Inv#1: kalau founder benar-benar membalas, itu concierge MVP (Ries), dan itu tes gate-nya sendiri. **Posisi akhir: card sekunder setelah card ke-6 (akhir list kalau <6), aktif hanya setelah deploy.** Copy: "Bingung pilih? Kirim area + budget-mu, aku pilihkan 3 tempat terdekat." → wa.me founder. Klik = event analytics tersendiri.

**Ke Inv#1 (venue):** target "Fitness 25–30" saya tetapkan sebelum ada data. Revisi: kategori/area yang dipadatkan ditentukan hasil 5 wawancara.

| # | Item | Malam | Done |
|---|---|---|---|
| 1 | `formatPrice` + `PRICE_UNIT_LABEL`, hapus `.modal`, perbaiki leak `li` | 1 | lint hijau, card & detail pakai 1 fungsi |
| 2 | Baseline 5-detik (async, paralel) | 0,5 | 5 jawaban tercatat |
| 3 | Copy pass (tagline, label chip, unit, "Harga belum dicek") | 1 | tabel copy PL terpasang |
| 4 | Hierarki + token | 2 | harga elemen terberat, hex hanya di `:root` |
| 5 | Tes 5-detik sesudah | 0,5 | ≥4/5; gagal → 1 iterasi, lalu tetap ship |
| 6 | Validator + `lastVerified` + badge "Dicek" | 1 | script exit 0 untuk 22 venue |
| 7 | Deploy statis + analytics | 1 | URL live, klik WA tercatat |
| 8 | Card concierge setelah card ke-6 | 0,5 | klik tercatat terpisah |
| 9 | Intro teman + 5 wawancara | 5 | 5 catatan, hitung gate ≥3/5 |
| 10 | Data pass sesuai temuan wawancara | 6–8 | ±40 venue, validator hijau |

±19–21 malam. Horowitz: langkah kecil yang dikirim tiap malam > langkah sempurna yang tidak pernah dikirim.

Setuju: CTO (helper dulu, token, validator sebelum badge), PL (baseline, CTA dalam list, no merah), Inv#2 (badge Dicek), Inv#1 (data pass ikut pemakaian). Tidak setuju: Inv#1 (larangan semua CTA), Inv#2 (banner atas), CTO (validator sebelum redesign).

### PRODUCT LEAD

**1. CTA: geser, tapi bukan ke spotlight.** Koreksi diri: ini **bukan fake door** — layanannya sungguhan, dikerjakan manual; Torres: assumption test sah. Argumen "merusak trust" saya cabut. Tapi banner atas tetap ditolak: halaman punya dua pesan utama sekaligus (Krug: satu halaman, satu pesan). Decision layer tak perlu terbaca dalam 5 detik; job utama yang harus terbaca. Traffic ke CTA bawah dari 5–10 teman hampir nol — tapi gate ≥3/5 memang tak boleh bergantung klik organik; diukur dari wawancara + tawaran concierge langsung. Kompromi: satu baris teks kecil di bawah tagline "Bingung pilih? Kirim area + budget-mu →" + card yang sama di akhir list; pesan WA prefilled dengan tag sumber (`[web-top]`/`[web-list]`).

**2. CTO soal badge: setuju penuh.** Sementara di detail: harga ada → harga + unit + link kecil "Sumber: Google Maps/IG" (dari `sourceUrl`); harga null → "Harga belum tersedia" + WA primary "Tanya harga via WhatsApp"; WA null → IG; keduanya null → sembunyikan blok kontak.

**3. Merah untuk Mahal: tetap ditentang.** Tier relatif per kategori — Padel Mahal normal, bukan bahaya. Merah bentrok dengan error validasi nanti. WCAG 1.4.1: label teks wajib apa pun warnanya. Usulan (teks putih, ≥4.5:1, cek ulang di contrast checker): Gratis `#15803d`, Murah `#0f766e`, Sedang `#b45309`, Mahal `#6d28d9` (violet = premium), Belum dicek `#6b7280`.

**4. Baseline: tetap, tapi dikecilkan** — ±20 menit async, UI lama ke 3 orang, UI baru ke 5 orang **berbeda**. Tanpa baseline: tidak tahu apakah copy saja cukup; n=5 noisy, tanpa titik nol = anekdot.

**5. Budget ≤5 sesi realistis kalau dipangkas:** S1 tabel copy; S2–3 hierarki card + warna tier; S4 detail page minimal; S5 baseline + tes ulang + link concierge. Dipotong dari usulan sendiri: badge "Dicek", desain card wizard (jadi link teks), tokens lengkap, alamat.

**Posisi akhir:** clarity dulu (copy + hierarki, 5 sesi), diukur terhadap baseline mini. Concierge WA = layanan nyata, link sekunder ber-tag; gate diukur dari wawancara.

Setuju: CTO (kontrak data, `formatPrice`), Inv#1 (5 sesi lalu deploy), Inv#2 (harga hero, concierge bukan fake door), CEO (copy pengungkit terbesar). Tidak setuju: Inv#2 (banner atas), CEO (header, skip baseline), CTO (merah), Inv#1 (nol link concierge).

### CTO

*(Catatan fasilitator: CTO menghitung 9 venue berharga; angka yang benar 8.)*

**1. CEO: tokens vs 2 malam.** Terima sebagian. **12 token minimum**: `--font`, `--text`, `--muted`, `--border`, `--brand`, `--space` (8px, kelipatan via `calc`), `--radius`, `--fs-price` (20px), `--tier-gratis/murah/sedang/mahal`. Ditunda: `--surface`, skala spacing penuh, 4 tingkat teks, shadow, line-height. `--place-tier-*` sudah ada — tinggal rename + pindah ke `:root`. `formatPrice` + `PRICE_UNIT_LABEL` **harus masuk copy pass** (tanpa helper = duplikasi ketiga).

| Malam | Kerjaan | Menit | Siapa |
|---|---|---|---|
| 0 | Baseline tes 5 detik, async ke 3 orang | 15 | F |
| 1 | `formatPrice` + copy Indonesia | 45 | F |
| 1 | Hapus `.modal`, pindahkan `.place-list li` | 5 | F |
| 2 | 12 token + 1 card contoh | 20 | A |
| 2 | Migrasi list page | 40 | F |
| 3 | Migrasi detail + chips | 50 | F |
| 3 | Review screenshot 375px | – | A |
| 4–5 | Validator + parse koordinat | 2×45 | A tulis, F review |
| 6 | Badge, link Maps, deploy | – | F |

Mengalah soal **redesign sebelum validator**, syaratnya: tidak ada field baru dirender sebelum validator ada (Kleppmann: data hidup lebih lama dari kode).

**2. Badge "Dicek".** **Tanggal scrape tidak sah jadi `lastVerified`** — scrape = observasi mesin, bukan verifikasi ("Don't assume it — prove it"). Definisi: `lastVerified` ISO date, diisi saat founder sendiri mengonfirmasi dari sumber primer; `priceSource` ∈ `wa | official-site | onsite`. Biaya jujur: venue berharga × ~3 menit buka situs resmi ≈ 30 menit; WA async. Render: `lastVerified == null` → badge tidak dirender (venue tanpa harga sudah tampil "Harga belum dicek"). Konsekuensi: badge hanya muncul di 8 card — bukan "wajah" produk. Parse koordinat: regex kena **11/22**; FTL bisa dari `gmaps_urls.json` → 12; 10 gagal (IG, linktr.ee, situs venue) → manual ~2 menit/venue; total ≈ 40 menit.

**3. Merah untuk Mahal: terima.** `#16a34a` + teks putih ≈3.3:1, gagal AA. Usulan (teks putih, cek ulang): Gratis `#166534`, Murah `#3f6212`, Sedang `#92400e`, Mahal `#7c2d12` (coklat hangat).

**4. `.modal`: setuju, masuk commit malam 1.** Aturan: **AI** tulis tooling sekali pakai yang bukan materi FE (validator, parse koordinat, file token + 1 contoh card — pengecualian tooling di AGENTS.md). **Founder** tulis semua JSX/CSS yang dirender selain contoh pertama. AI hanya review.

**5. Cost per verified price, tanpa infra:** validator mencetak to-do (venue `lastVerified` >45 hari, terurut) + link `wa.me/…?text=` berisi template pesan; cek bergilir 5 venue/minggu; draft pesan dibantu LLM, manusia yang kirim & verifikasi (Karpathy "autonomy slider"); Node + JSON, nol innovation token.

**Posisi akhir:** redesign 3 malam boleh duluan dengan 12 token dan `formatPrice` di copy pass; field verifikasi tidak tampil sebelum ada validator + definisi verifikasi jujur.

Setuju: urutan CEO (dengan syarat), merah diganti (PL), `.modal` bukan PR (Inv#1), CTA ke WA (PL/Inv#2). Tidak setuju: tanggal scrape sebagai "Dicek" / badge jadi wajah card (Inv#2); memotong semua token (CEO).

---

## PENUTUP — Sintesis fasilitator

Lihat ringkasan di `plan.md` setelah arbitrase. Versi lengkap verdict, daftar aksi berprioritas, dan daftar ketidaksepakatan ada di balasan thread Amp
(T-01a135db-f745-71ae-934f-ded33ec0b573) dan disalin di bawah.

### Verdict konsensus (5/5 kecuali disebut)

1. **Tes 5 detik gagal karena copy + hierarki informasi, bukan warna.** Clarity wajib sebelum link pertama ke teman; beauty ditunda. Timebox redesign **3–5 malam**, lalu deploy.
2. **Harga adalah hero card.** Nama → harga besar dengan unit Indonesia → "Kategori · Area" kecil → tier sebagai badge pill. Buang label literal "Harga:"/"Tier:". Jarak TIDAK di card list.
3. **Tagline + sub + `<title>` + `lang="id"`** — pengungkit terbesar, 1 malam.
4. **`formatPrice(place)` + `PRICE_UNIT_LABEL` masuk copy pass** (bukan refactor terpisah) — tanpa helper, ganti unit = duplikasi ketiga di kode yang sudah pernah regresi.
5. **Merah untuk Mahal dibuang** (PL, Inv#2, CEO, CTO; Inv#1 tidak keberatan). Tier relatif per kategori; merah = error; "describe venues, never assess".
6. **CTA wizard pre-gate = concierge WA nyata, BUKAN spotlight atas, BUKAN header.** Posisi: card sekunder setelah card ke-6 / akhir list. Ini bukan fake door (layanan ada: founder). Gate ≥3/5 tetap diukur dari wawancara + tawaran langsung, bukan klik organik.
7. **Badge "Dicek" menunggu `lastVerified` + validator.** Tanggal scrape ≠ verifikasi. `null` → badge tidak dirender. Harga null → "Harga belum dicek" + WA sebagai CTA utama.
8. **Validator sebelum deploy** (bukan sebelum redesign), dengan syarat tidak ada field baru dirender sebelum validator ada.
9. **Tanpa Tailwind/shadcn/TypeScript/CSS Modules/Google Fonts.** Design token = CSS variable di `:root`, hex literal hanya di sana.
10. **Data pass ditentukan hasil wawancara**, bukan tebakan. Prioritas pertama: **isi 14 harga venue existing yang null** (Padel 0/5!) sebelum menambah venue baru.
