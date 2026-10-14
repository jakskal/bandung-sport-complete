# Plan v2 — Bandung Sport: Pemandu Olahraga Bandung

> Status: DISEPAKATI SECARA UMUM (2026-10-13) — menggantikan "Definisi v1"
> di `plan.md` dan spec wizard v0.3 (`docs/spec/wizard-rekomendasi.md`,
> diarsipkan; isinya dilebur jadi alat bantu per mode, lihat § 4).
> Dokumen ini dipakai sebagai kontrak kerja untuk tool AI pelaksana
> dalam **mode latihan** (§ 9). Angka-angka ambang (≥15 offering, dsb)
> adalah angka awal, boleh disetel di ritual mingguan.
>
> Cara pakai untuk tool AI: baca § 0, § 5, § 8, § 9. Kerjakan HANYA
> tugas malam yang ditunjuk di `plan.md` § Status.

## 0. Satu paragraf

Bandung Sport adalah **pemandu olahraga Bandung untuk semua orang**:
menjawab "olahraga apa, di mana, bareng siapa, berapa duit" lewat
beberapa **mode** sesuai siapa yang nyari dan untuk apa — rutin sendiri,
main bareng, komunitas terbuka, anak, program khusus. Datanya bukan
"tempat dengan satu harga", tapi **Tempat + Penyelenggara + Penawaran**
(paket/program/sesi dengan harga, satuan, audiens, jadwal). Mode hanyalah
cara melihat penawaran. Dibangun satu mode per fase, tiap fase berakhir
dengan URL live yang dikirim ke orang nyata. Proyek ini sekaligus
kendaraan belajar frontend pemilik repo: kode yang dirender ditulis
founder, AI menulis tooling dan satu contoh per pola.

## 1. End game

Kalau berhasil, Bandung Sport jadi **jawaban default di grup WA Bandung**
untuk pertanyaan olahraga: "gym murah deket Buahbatu?", "fun football
yang nerima orang baru Sabtu pagi?", "les renang anak umur 7 di
Antapani?". Moat-nya **kesegaran data lokal** yang akhirnya dirawat
bersama penyelenggara dan pengguna (lapor harga/komunitas), bukan fitur.

**Satu metrik pembukti:** jumlah orang per minggu yang klik
"hubungi/gabung" di suatu penawaran (*weekly qualified contacts*). Klik
ini bukti orang beneran berangkat, bukan sekadar lihat-lihat.

Target end game (bukan tenggat): ≥200 tempat, ≥500 penawaran, ≥50% update
data datang dari luar founder, pengunjung organik mingguan.

## 2. Peta segmen (eksplorasi, bukan scope)

Siapa saja yang mungkin dilayani, dan penawaran apa yang mereka butuhkan.
Skor = kemudahan mengumpulkan datanya (5 = gampang).

| Segmen | Pertanyaan nyata | Penawaran yang dibutuhkan | Data |
|---|---|---|---|
| Pekerja 20–30an | "Gym buka jam 5 pagi di Setiabudi?" / "Run club Dago yang ramah pemula?" | membership, kelas, komunitas lari | 4 |
| 30–40an sibuk | "Bisa sejam pas istirahat siang deket Asia Afrika?" | per kunjungan, kelas pendek, jam buka | 4 |
| Mahasiswa | "Gym paling murah deket Dipatiukur?" / "Mabar futsal anak rantau?" | per kunjungan, harga pelajar, mabar | 4 |
| Dewasa tanpa grup | "Fun football terbuka umum, bayar per datang, Sabtu pagi?" | **komunitas terbuka** (iuran per datang, link WA, level) | 2 |
| Grup kantor/teman | "Sewa futsal 10 orang di Kopo, per orang jadi berapa?" | sewa per jam, kapasitas | 4 |
| Ortu anak 5–12 | "SSB di Buahbatu nerima umur 7?" / "Les renang anak berapa sebulan?" | akademi/klub: rentang usia, jadwal, iuran + pendaftaran | 3 |
| Remaja/pelajar | "PB bulutangkis Arcamanik yang ada seleksi?" | klub berlevel | 2 |
| Pemula / kurang PD | "Gym yang nggak intimidatif? Kelas khusus pemula?" | kelas pemula, PT, gym khusus perempuan | 2 |
| Perempuan | "Gym khusus cewek?" / "Pilates reformer per sesi berapa?" | studio/kelas khusus perempuan | 3 |
| Ibu hamil / pasca melahirkan | "Yoga hamil di mana, mulai trimester berapa?" | **program khusus** dengan syarat penyelenggara | 2 |
| Lansia | "Senam lansia pagi di Taman Lansia / Tegalega?" | komunitas gratis, kolam dangkal | 2 |
| Rehab cedera | "Fisioterapi olahraga yang ngerti ACL?" | di luar scope — cukup dirujuk | 1 |
| Disabilitas | "Kolam yang aksesibel kursi roda?" | atribut aksesibilitas tempat | 1 |
| Atlet amatir | "Turnamen padel open bulan ini?" | event/turnamen | 2 |
| Pendatang baru | "Baru pindah ke Bandung, komunitas olahraga apa aja?" | semua, dirangkum per area | 3 |
| Turis / akhir pekan | "Jogging pagi enak di mana? Hiking Tahura?" | fasilitas gratis, rute | 5 |

**Insight penting:** data yang paling gampang (gym, sewa lapangan,
fasilitas gratis) justru yang paling sudah dilayani Google Maps. Nilai
pembeda ada di data yang **susah**: komunitas terbuka, akademi anak,
program khusus. Maka urutan fase (§ 5) mulai dari yang gampang (buat
belajar + fondasi), tapi tidak berhenti di situ.

## 3. Jenis penawaran yang harus muat di data

| Jenis (`offeringType`) | Field khas | Bentuk harga | Basi seberapa cepat |
|---|---|---|---|
| `membership` | durasi, jam akses | per bulan/tahun; sering "hubungi kami" | bulanan (promo) |
| `visit-pass` | jam buka | per kunjungan | stabil |
| `personal-training` | sesi per paket | per paket (8/12 sesi) | stabil |
| `class` (yoga, zumba, muay thai, pilates) | jadwal rekuren, level, kuota | per kelas / paket | musiman |
| `court-rental` (futsal, padel, badminton, basket) | kapasitas, jam prime/non-prime | per jam | sedang |
| `program` (akademi anak, prenatal, lansia, pemula) | `ageMin/ageMax`, syarat, jadwal | pendaftaran + iuran bulanan | per semester |
| `community-session` (fun football, run club, mabar) | penyelenggara, cara gabung, rekurensi, "terima umum?" | iuran per datang / gratis / patungan | **mingguan — paling cepat basi** |
| `free-access` (Saraga, Gasibu, Tegalega, jogging track) | jam akses, tiket | gratis / tiket | stabil |
| event/turnamen | tanggal tunggal | registrasi | sekali jalan — **ditunda**, nanti cukup `dates[]` di schedule |

## 4. Mode produk & alat bantunya

Mode = sudut pandang user saat masuk app. Saling eksklusif, 1 route
per mode, konfigurasi di satu file `src/modes.js` (data, bukan logika).

| Mode | Route | Pertanyaan pembuka | Jenis penawaran | Alat bantu khas | Frekuensi pakai |
|---|---|---|---|---|---|
| **Rutin sendiri** | `/sendiri` | Olahraga apa? Area? Berapa kali seminggu? | membership, visit-pass, PT, free-access | **Hitung biaya**: frekuensi/minggu → biaya per bulan & per sesi, terurut | sedang (saat cari/ganti) |
| **Main bareng** | `/bareng` | Olahraga? Berapa orang? Hari apa? | court-rental | **Patungan**: harga/jam × jam ÷ orang; jam mana lebih murah | **tinggi** (mingguan) |
| **Komunitas** | `/komunitas` | Olahraga? Hari? Terima orang baru? | community-session | **Mabar minggu ini** + "masih jalan?/sudah bubar" | **tinggi** (mingguan, sosial) |
| **Untuk anak** | `/anak` | Usia anak? Olahraga? Area? | program (anak/remaja) | biaya setahun (pendaftaran + iuran×12) | sekali pakai, WTP tinggi |
| **Program khusus** | `/khusus` | Program untuk siapa? (hamil / lansia / pemula / perempuan) | program, class | checklist "tanyakan ke penyelenggara" + disclaimer | rendah |
| **Jalan-jalan aktif** | (dilebur ke `/sendiri` via free-access; dipisah kalau datanya ≥15) | Pagi/sore? Gratis? | free-access | — | musiman |

**Wizard "Rekomendasi buat saya" (spec v0.3) dibubarkan jadi ini.** Yang
bikin wizard susah dibayangkan: dia mencoba jadi satu fitur untuk semua
orang. Dipecah per mode, tiap alat bantu kecil, jujur, dan hitungannya
murni dari data (`monthlyCost`, haversine). Lapisan "rekomendasi lintas
mode" boleh dibangun di end game, di atas penawaran yang sudah tebal.

**Guardrail yang direvisi (2026-10-13):**
- Usia BOLEH ada di data — sebagai `ageMin/ageMax` **penawaran** yang
  disebutkan penyelenggara sendiri (klub anak 6–12 th), bukan input
  tentang tubuh user. Audiens pakai band (`umum, anak, remaja, lansia,
  prenatal, pemula, perempuan`).
- Mode program khusus (hamil/lansia): framing **"program yang tersedia
  untuk ibu hamil"**, bukan "olahraga yang cocok untukmu". Syarat
  penyelenggara ditampilkan apa adanya ("wajib izin dokter") + disclaimer
  statis. Rehab cukup dirujuk ke klinik, tidak dikurasi.
- Prinsip tetap: **describe offerings, never assess a person.**
- Tanpa login/booking/pembayaran sampai ada trigger terukur (§ 7).
- Harga tidak pernah dikarang: `priceVisibility` membedakan "venue tidak
  membuka harga" (`on-request`) dari "belum dicek" (`unknown`).

## 5. Roadmap fase

Prinsip: waktu dikunci, scope yang dikorbankan (Shape Up). Satu mode per
fase. **Mode tidak dibangun sebelum ada ≥15 penawaran nyata untuk mode
itu** — data dulu, UI belakangan. Fase selesai = URL live dikirim ke ≥3
orang yang relevan dengan mode itu + ada ≥1 sinyal balik (tanya balik,
buka lagi, kirim info tempat).

| Fase | Tujuan | Mode aktif | Data minimum | Appetite | Selesai kalau | Sinyal murah | Materi FE |
|---|---|---|---|---|---|---|---|
| **0 Fondasi** | Lolos tes 5 detik, data v2, live | list umum | 22 tempat, ≥15 penawaran berharga (target, bukan blocker) | 12–16 malam | URL live dikirim ke 5 teman | 2/5 membalas dengan pertanyaan atau nama tempat | token CSS, pure function, bentuk data, deploy SPA |
| **1 Sendiri** | `/sendiri` + Hitung biaya; fasilitas gratis ikut | sendiri | ≥25 penawaran gym/renang, ≥8 free-access | 8 malam | founder sendiri pakai untuk milih gym | 3 teman ngegym coba kalkulator; 1 tanya "kok X nggak ada?" | form terkontrol, angka di URL, Vitest untuk `monthlyCost` |
| **2 Bareng** | `/bareng` + Patungan; badminton ditambah | sendiri, bareng | ≥20 penawaran sewa lapangan | 8 malam | link patungan dikirim ke 1 grup WA nyata | ada yang forward link | komponen reusable, share URL, Web Share API |
| **3 Komunitas** | `/komunitas` + Mabar minggu ini + form lapor (Google Form) | +komunitas | ≥10 komunitas aktif terima umum | 8 malam | 1 laporan dari orang selain founder | ≥2 laporan/minggu | jadwal & tanggal (`Intl`), filter hari, data cepat basi |
| **4 Anak + Kelas** | `/anak` (usia, jadwal) + `class` tampil di sendiri | +anak | ≥15 program anak, ≥10 kelas | 10 malam | 3 ortu terima link, 1 tanya detail | "ada yang buat umur 7?" | rentang usia, tampilan jadwal mingguan, a11y |
| **5 Program khusus** | `/khusus` (prenatal/lansia/pemula/perempuan) | +khusus | ≥10 program terverifikasi dengan syarat | 6 malam | 1 bidan/instruktur mengonfirmasi datanya | — | conditional UI, konten statis |
| **6 BFF Go** ⚑ | JSON → API; form lapor masuk antrean review | semua | ±150+ penawaran | 10–12 malam | FE baca dari API; laporan ke DB | proses laporan <10 mnt/minggu | `fetch`, loading/error state, env var |
| **End game** | semua mode + lapisan rekomendasi lintas mode | semua | ≥200 tempat, ≥500 penawaran | — | ≥50% update dari luar founder | pengunjung organik mingguan | — |

**Keputusan fasilitator yang menyimpang dari agen:** Product menyarankan
komunitas lebih dulu daripada bareng (frekuensi tinggi, pembeda). CEO
menaruh bareng dulu (data gampang). Saya pilih **bareng dulu** karena
datanya sudah ada (futsal/padel) dan alat "Patungan" mudah — tapi
**pengumpulan data komunitas dimulai sejak Fase 1** (1 malam per minggu),
supaya saat Fase 3 tiba ambang ≥10 sudah tercapai.

⚑ **Trigger BFF Go** (salah satu): JSON >150 penawaran; laporan masuk ≥5/
minggu 2 minggu berturut; update jadwal manual >1 jam/minggu; butuh
secret (API key). Kalau belum ada, tetap JSON — bisa berarti Fase 6
datang sebelum Fase 4/5, itu normal.

**Form lapor** (Google Form, tanpa backend) dipasang di detail page sejak
Fase 1: "Harga salah / sudah tutup / tambah tempat".

## 6. Model data v2

Tiga file ternormalisasi (bentuk tabel, siap dipetakan ke BFF Go):
`src/data/venues.json`, `src/data/organizers.json`,
`src/data/offerings.json`. Join ditulis sekali di `src/data/index.js`
(`getVenue`, `offeringsByVenue`, `venueWithOfferings`). Kontrak ditulis
sebagai `@typedef` JSDoc di `src/data/types.js` (tetap JS).

**Fase 0 hanya dua file: `venues.json` + `offerings.json`.**
`organizers.json` beserta join, validasi, dan typedef-nya baru dibuat di
Fase 3 (komunitas) — sampai saat itu `organizerId` selalu `null` dan
cukup disebut di typedef sebagai `string|null`. Data melayani kebutuhan
sekarang, bukan kebutuhan tiga fase ke depan.

```json
// venues.json — tempat fisik, netral, tidak tahu harga
{ "id": "v-ftl-dago", "name": "FTL Gym Dago", "area": "Dago",
  "sports": ["fitness"], "lat": -6.8776, "lng": 107.6155,
  "address": null, "whatsapp": "+62818687858", "instagram": "…",
  "website": "…", "sourceUrl": "…", "tags": [], "lastVerified": "2026-10-12" }

// organizers.json — klub/komunitas/akademi, boleh tanpa tempat tetap
{ "id": "o-fun-football-btr", "name": "Fun Football Buah Batu",
  "kind": "community", "instagram": "…", "whatsapp": "+62…",
  "lastVerified": "2026-10-10" }

// offerings.json — sesuatu yang bisa diikuti/dibayar
{ "id": "of-ftl-member", "venueId": "v-ftl-dago", "organizerId": null,
  "sport": "fitness", "offeringType": "membership",
  "title": "Membership bulanan", "audience": ["umum"],
  "ageMin": null, "ageMax": null,
  "priceVisibility": "public", "priceMin": 500000, "priceMax": 2500000,
  "priceUnit": "monthly", "sessionsPerPack": null, "groupSize": null,
  "durationMin": null, "joinMethod": ["walk-in", "wa"],
  "schedule": [], "locationNote": null, "lastVerified": "2026-10-12" }

{ "id": "of-ffb-sabtu", "venueId": null, "organizerId": "o-fun-football-btr",
  "sport": "football", "offeringType": "community-session",
  "title": "Fun football Sabtu pagi", "audience": ["umum"],
  "priceVisibility": "public", "priceMin": 35000, "priceMax": 35000,
  "priceUnit": "per-visit", "joinMethod": ["wa"],
  "schedule": [{ "days": ["sat"], "start": "06:00", "end": "08:00" }],
  "locationNote": "Rotasi lapangan sekitar Buah Batu, cek IG",
  "lastVerified": "2026-10-10" }

{ "id": "of-hiu-anak", "venueId": "v-kolam-tirta", "organizerId": "o-hiu-swim",
  "sport": "swimming", "offeringType": "program", "title": "Latihan anak",
  "audience": ["anak"], "ageMin": 5, "ageMax": 12,
  "priceVisibility": "public", "priceMin": 400000, "priceMax": null,
  "priceUnit": "monthly", "joinMethod": ["daftar-online"],
  "schedule": [{ "days": ["sat", "sun"], "start": "07:00", "end": "09:00" }],
  "lastVerified": "2026-10-11" }
```

**Enum terkunci** (tambah lewat PR, bukan ketik bebas):
- `offeringType`: `membership | visit-pass | personal-training | class | court-rental | program | community-session | free-access`
- `priceUnit`: `monthly | weekly | per-visit | per-hour | per-session-pack | per-year | free`
- `priceVisibility`: `public | on-request | unknown`
- `audience[]`: `umum | anak | remaja | lansia | prenatal | pemula | perempuan`
- `sport`: huruf kecil kanonik (`fitness, futsal, padel, badminton, basketball, swimming, running, football, yoga, pilates, zumba, muay-thai, hiking`); label Indonesia di `src/data/sports.js`
- `joinMethod[]`: `walk-in | wa | daftar-online`
- `organizer.kind`: `club | community | academy | coach`
- `schedule[]`: `{ days: ["mon".."sun"], start: "HH:MM", end: "HH:MM" }`

**Aturan:**
- `venueId` dan `organizerId` boleh null, **minimal satu terisi**.
- `public` ⇒ `priceMin` angka ≥ 0; `on-request`/`unknown` ⇒ harga null.
- `null` = belum terverifikasi, **bukan** nilai karangan. Field yang tidak
  berlaku boleh dihilangkan (= null).
- ID string berprefiks (`v-`, `o-`, `of-`) — `parseInt` di detail page dibuang.
- **`tier` dihapus dari data**, dihitung dari **harga mentah dalam satuan
  alami tiap sport** — bukan lewat `monthlyCost` (yang butuh asumsi
  frekuensi dan jumlah orang; untuk `per-hour` tanpa `groupSize` hasilnya
  null dan semua venue futsal kehilangan tier). Ambang di
  `src/data/tiers.js`, dikunci per `(sport, priceUnit)`:
  ```js
  export const TIER_RULES = {
    fitness:  { unit: 'monthly',   murah: 200000, sedang: 600000 },
    swimming: { unit: 'per-visit', murah: 25000,  sedang: 60000 },
    futsal:   { unit: 'per-hour',  murah: 120000, sedang: 250000 },
    padel:    { unit: 'per-hour',  murah: 250000, sedang: 450000 },
    running:  { unit: 'per-visit', murah: 10000,  sedang: 30000 },
  }
  ```
  `tier(venue)` = bucket dari `min(priceMin)` atas penawaran `umum` +
  `public` yang `priceUnit`-nya cocok dengan rule sport itu; `free` atau
  0 → Gratis; tidak ada yang cocok → null → "Harga belum dicek". Sport
  tanpa rule → null (tambah rule lewat PR). Angka ambang di atas tebakan
  awal; setel di ritual mingguan setelah lihat distribusi datanya.
- `lastVerified` = tanggal founder/penyelenggara mengonfirmasi dari sumber
  primer. **Tanggal scrape bukan verifikasi.**

**`src/lib/cost.js` — pure function, ditulis founder, diuji Vitest:**

```js
/** @returns {{min:number, max:number|null} | null} */
export function monthlyCost(offering, timesPerWeek, { groupSize } = {})
```

W = 4.33 minggu/bulan; rumus sama untuk min & max; bulatkan ke 1000.

| unit | rumus |
|---|---|
| `monthly` | p |
| `weekly` | p × W |
| `per-visit` | p × n × W |
| `per-hour` | p × (durationMin/60 ∥ 1) × n × W ÷ (groupSize ∥ offering.groupSize); orang tidak diketahui → null |
| `per-session-pack` | (p ÷ sessionsPerPack) × n × W |
| `per-year` | p ÷ 12 |
| `free` | 0 |
| visibility ≠ public | null |

**Tooling (boleh ditulis AI, Node polos, tanpa dependency):**
- `scripts/migrate-v1-to-v2.mjs` — sekali jalan lalu dihapus: 1 place → 1
  venue + 1 offering (kalau ada harga/unit); lat/lng dari regex
  `!3d(-?\d+\.\d+)!4d(-?\d+\.\d+)`; category → sport; harga null →
  `priceVisibility: "unknown"`.
- `scripts/validate-data.mjs` — `npm run validate`, dipanggil sebelum
  `build`, exit ≠ 0 kalau error. Cek: id unik + prefiks; referensi
  venueId ada (organizerId: Fase 3); enum valid; aturan visibility ↔
  harga; priceMax ≥ priceMin; per-session-pack ⇒ sessionsPerPack; lat/lng
  dalam bbox Bandung Raya (≈ -7.2…-6.7, 107.4…107.9); jam `HH:MM` &
  start < end; lastVerified valid & tidak di masa depan; ageMin ≤ ageMax
  hanya kalau audience anak/remaja; WA berformat `+62`; venue tanpa
  offering = warning; lastVerified > 45 hari = warning (ini jadi to-do
  ritual freshness, dicetak terurut + link `wa.me/…?text=` template).

## 7. Arsitektur FE

- **Route:** `/` (pemilih mode + list umum sementara), `/:mode` (divalidasi
  ke `modes.js`, slug asing → NotFound), `/tempat/:venueId`,
  `/komunitas/:organizerId`. `/place/:id` dibuang tanpa redirect (belum
  ada user).
- **`src/modes.js`** = data: `{ slug, label, intro, match: { offeringType:
  [...] }, facets: ['sport','area','day'], tools: ['monthlyCost'] }`.
- **State di URL** (`useSearchParams`, dibungkus custom hook
  `useFilters()`), semua hasil derived:
  `filterOfferings(all, mode.match, params)` → dikelompokkan per
  tempat/penyelenggara.
- **Komponen inti (±9):** `ModePicker`, `ResultsPage` (generalisasi
  ListPage), `FilterBar` (pakai ulang `FilterChips` + select area/hari),
  `VenueCard` (turunan `Place`), `OfferingRow`, `PriceTag` (harga + unit +
  visibility, "Harga via WA"), `ScheduleText`, `VenueDetail` /
  `OrganizerDetail` (berbagi `ContactLinks`).
- **Styling:** CSS custom properties, ±12 token di `:root`, hex literal
  hanya di `:root`, file CSS per folder komponen. Tanpa framework.
- **Tetap JS + JSDoc.** TS dipertimbangkan setelah Fase 2.
- **Jangan sekarang:** TypeScript (satu dimensi belajar lagi; validator +
  JSDoc sudah menutup risiko data), Tailwind (menyembunyikan CSS yang
  sedang dipelajari), state library (URL + derived cukup), backend/DB/auth
  (belum ada trigger § 5), CMS (60 record kalah praktis dari JSON +
  validator), ML rekomendasi (belum ada data perilaku; hitungan
  deterministik bisa dijelaskan), PWA (belum ada pemakaian berulang).

## 8. Fase 0 malam-per-malam (1 malam ≈ 45–60 menit)

F = founder menulis; A = AI menulis penuh; R = AI review saja.

**Prinsip urutan:** bentuk data dulu, polish belakangan. Card dan detail
page bergantung pada bentuk harga — kalau dipoles sebelum migrasi ke
offering, ditulis dua kali. Yang tidak bergantung bentuk data (copy,
token CSS) boleh duluan.

**Pengumpulan harga berjalan paralel, bukan satu malam.** Fakta: hanya
8/22 venue punya harga (Padel 0/5). Harga gym tidak ada di web — dapatnya
dari balasan DM IG/WA yang datang 1–3 hari kemudian. Maka outreach
dimulai malam 1 dan balasan diinput kapan pun masuk (≤10 menit, di luar
jatah malam). Kalau sampai malam deploy tetap <15 penawaran public,
**tetap deploy** dengan "Harga via WA" — jangan tunda.

Nomor malam = urutan, bukan kalender. Tugas bertanda ★ wajar makan 2
malam untuk pemula; itu bukan keterlambatan.

| Malam | Tugas | Siapa | Selesai kalau | Materi FE |
|---|---|---|---|---|
| 1 | **(a)** Copy pass: tagline + sub di header, `lang="id"`, `<title>`, meta description; count jadi teks kecil; label dimensi chip ("Olahraga / Budget / Area"); "Belum dikurasi" → "Harga belum dicek"; empty state copy. **(b)** Kirim pesan template ke ≤10 venue yang harganya null (WA/DM IG): "Halo, boleh info harga membership/sewa per jam? Mau saya cantumkan di direktori olahraga Bandung". Catat di `docs/data/outreach.md` | F, R; A tulis template pesan | tagline terbaca; lint+build hijau; ≥8 pesan terkirim | `index.html`, SEO dasar, props baru di `FilterChips` |
| 2 | Token CSS ±12 di `:root` (`--font`, `--text`, `--muted`, `--border`, `--brand`, `--space`, `--radius`, `--fs-price`, `--tier-*` ×4 non-merah) + `body` font; hapus `.modal*`; pindahkan `.place-list li` | A tulis file token; F terapkan | `grep '#[0-9a-f]' src` di luar `:root` kosong | custom properties, cascade |
| 3 | `src/lib/formatPrice.js` dengan signature **v2 sejak awal**: `formatPrice({ priceMin, priceMax, priceUnit, priceVisibility })` → "Rp500rb–2,5jt /bulan", "mulai Rp50rb /jam", "Harga via WA", "Harga belum dicek"; `PRICE_UNIT_LABEL`. Sementara dipanggil dari `Place` & `PlaceDetail` dengan objek yang dibentuk dari field lama (hapus 3 cabang duplikat) | F; A tulis 6 kasus uji | semua kasus benar di console; duplikasi hilang | pure function, `Intl.NumberFormat`, DRY |
| 4 | Tulis skema v2 (§ 6, **tanpa organizers**) jadi `src/data/types.js` (JSDoc) + putuskan enum + `TIER_RULES` di `tiers.js`; founder jelaskan alasan tiap field | F; R | typedef disetujui | bentuk data FE, normalisasi |
| 5 | `scripts/migrate-v1-to-v2.mjs` + `scripts/validate-data.mjs` + `npm run validate`; jalankan, baca diff, hapus script migrasi | A tulis; F jalankan & baca | validator hijau; 22 venue + ≥8 offering; lat/lng terisi untuk 11 | Node script, data integrity |
| 6 ★ | ListPage baca v2 lewat `src/data/index.js` (harga card = penawaran termurah "mulai Rp…" via `formatPrice`); tier dihitung dari `TIER_RULES`; filter URL (`kategori`→`sport`, `tier`, `area`) tetap jalan; `/place/:id` → `/tempat/:venueId`, `parseInt` dibuang | F; A beri petunjuk bertahap | share URL, refresh, back/forward OK; 3 venue futsal masih punya tier | `filter/reduce`, derived data, join |
| 7 | Hierarki card: nama + badge tier (pill) → **harga hero** (`--fs-price`, bold) → "Olahraga · Area" kecil; card putih, border, radius; chip aktif = `--brand` | F; A beri 1 sketsa | teman sebut harga tanpa ditanya | visual hierarchy, skala font |
| 8 ★ | Detail page v2: "← Semua tempat" (Link), nama + meta + badge, **daftar paket** (`OfferingRow` + `PriceTag`), **tombol WA full-width** ("Tanya harga via WhatsApp" kalau tidak ada harga public), sekunder Maps/IG/Website berlabel; venue tanpa paket → empty state jujur; badge "Dicek Okt 2026" hanya kalau `lastVerified != null` | F, R | venue 3 paket = 3 baris; venue tanpa harga tampil benar | list bersarang, composition, conditional render |
| 9 | Input balasan outreach yang sudah masuk + koordinat manual untuk 11 venue sisa; `lastVerified` hanya untuk yang beneran dikonfirmasi; timebox 60 menit, berapa pun hasilnya | F | validator hijau | — |
| 10 | Deploy statis (Cloudflare Pages / Netlify) + SPA rewrite `/* /index.html 200` + **Cloudflare Web Analytics** (1 script tag, pageview saja — lihat catatan metrik di bawah) | F; A jelaskan config | refresh di `/tempat/v-…` tidak 404 | build output, SPA fallback |
| 11 | Tes 5 detik ke 1 orang → kirim ke 5 teman (2 ngegym, 2 futsal, 1 umum) dengan 1 pertanyaan; catat respons; buffer perbaikan | F; A beri template pesan | 5 terkirim, respons tercatat, 1 perbaikan di-deploy | observasi user |
| 12–16 | Buffer: slip dari ★, perbaikan dari respons teman, input balasan outreach susulan | F | — | — |

**Catatan metrik (jujur):** metrik produk (§ 1) adalah klik
"hubungi/gabung", tapi di Fase 0 itu **belum terukur** — pageview
Cloudflare tidak menghitung klik, dan belum ada backend. Fase 0 metriknya
manual: balasan dari 5 teman. Event klik dipasang di **Fase 1** (Umami
cloud gratis atau GoatCounter; satu `onClick` di `ContactLinks`), bukan
sekarang. Tool AI pelaksana: jangan habiskan satu malam membanding-
bandingkan analytics.

Link "Lapor harga salah" (Google Form) masuk Fase 1.

## 9. Instruksi untuk tool AI pelaksana (mode latihan)

Tempel ini di prompt tool AI yang menemani founder:

1. Baca `plan.md` § Status dan `docs/plan-v2.md` § 0, 6, 8. Kerjakan
   **hanya tugas malam yang ditunjuk** — satu tugas, satu malam.
2. Jelaskan konsepnya dulu (≤150 kata; analogi Go boleh kalau membantu),
   baru beri tugas. Founder menulis komponen, state, JSX, CSS.
3. Review dengan menunjuk baris dan bertanya "kenapa", bukan menulis ulang.
4. Yang boleh kamu tulis penuh: script migrasi/validasi, file token CSS,
   konfigurasi deploy, setup Vitest, dan **1 contoh** per pola baru.
5. Founder stuck: tanya dulu apa yang sudah dicoba, lalu petunjuk
   bertahap (arah → nama API → potongan kode).
6. Jangan menambah fitur, mode, dependency, atau field data di luar tugas
   malam ini. Ide baru dicatat di `plan.md` § "Nanti".
7. Jangan pernah mengarang harga, jadwal, kontak, koordinat. Tidak tahu
   = null.
8. Teks UI Bahasa Indonesia; nama variabel & komentar Inggris.
9. Tutup sesi dengan: "Jalankan `npm run lint && npm run build` (dan
   `npm run validate` setelah malam 5), pastikan hijau, lalu commit."
10. Update `plan.md` § Status: tugas selesai, materi yang dilewati, utang,
    tugas malam berikutnya.

## 10. Ritual mingguan (≤15 menit, Minggu malam) & aturan potong

Catat: penawaran `public` (total / +minggu ini); venue tanpa harga; link
dikirim ke siapa & siapa yang balas; laporan dari luar; 1 hal yang
dipelajari; malam terpakai vs appetite fase.

- 2 minggu tanpa commit yang di-deploy → scope fase dipotong separuh,
  waktu tidak ditambah.
- 2 fase berturut tanpa sinyal dari luar → mode berikutnya ditunda, fokus
  ke distribusi (kirim link, bukan nambah fitur).
- Pendataan maksimal 1 dari 4 malam, timebox 60 menit.
- Polish berhenti kalau tes 5 detik lolos 2 dari 3 orang, atau sudah
  makan >2 malam dalam satu fase.

## 11. Risiko & mitigasi

| Risiko | Mitigasi |
|---|---|
| Pendataan menelan semua waktu | kuota 1 dari 4 malam; harga tak jelas tetap null + link lapor; validator mencetak to-do verifikasi terurut |
| Terlalu banyak mode terlalu cepat | satu fase satu mode; ambang ≥15 penawaran nyata sebelum route mode dibuat |
| Komunitas cepat basi, 1 link mati merusak trust | `lastVerified` tampil sebagai umur data; auto-hide >N minggu; tombol "masih jalan?/sudah bubar"; jangka panjang penyelenggara update sendiri |
| Mode medis (hamil/lansia) | "program yang tersedia" + syarat penyelenggara + disclaimer; rehab dirujuk saja |
| Belajar FE vs ngejar fitur | kode yang dirender = founder; tugas >2 malam → AI kasih 1 contoh, founder tiru polanya |
| Pola koperasi-react (jadi, mati) | tiap fase wajib berakhir dengan link ke orang nyata; metrik = klik hubungi/gabung |
