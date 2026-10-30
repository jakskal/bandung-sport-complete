# Copy header (S12) — KEPUTUSAN FINAL

> Kriteria sesi: `docs/plan-v3-delta.md` § 2 S12 — tagline besar + sub 1
> kalimat + CTA "Lihat semua tempat". *Selesai kalau:* 2 dari 3 orang bisa
> jawab "ini buat apa" dalam 5 detik.
> **Status: subline direvisi 2026-10-30** (opsi A, setelah tes 2 orang:
> "ga ngerti setelah kita pilih itu lalu di apakan"). Tagline & `<title>`
> tetap seperti yang dikunci 2026-10-29. Implementasi revisi `fe@8474a79`
> (dikerjakan AI atas persetujuan founder).

## 1. Pasangan yang dipakai

| Slot | Teks | Karakter | Batas |
|---|---|---|---|
| Tagline | **Bandung Sport — Direktori olahraga Bandung** | 42 | ≤60 ✅ |
| Subline | **Kita bantu cari tempat olahraga — pilih satu, lalu chat pengelolanya** | 69 | ≤110 ✅ |
| CTA | **Lihat semua tempat** | — | anchor ke list |

Keputusan 2026-10-29 (founder), tiga revisi dari versi 2026-10-28:

1. **Tagline** diganti dari "Bandung. Ayo olahraga." (22 kar.) ke nama + label
   di atas (42 kar.). Alasan: orang langsung tahu ini direktori, tidak perlu
   menyimpulkan dari subline. Konsekuensi diterima: ~3 baris di 390px.
2. **Subline** pakai versi pendek tanpa kata "harga".
3. **`<meta name="description">`** = "Tempat olahraga di Bandung." — tanpa
   daftar cabang (gym/futsal/padel/…) karena cabang akan bertambah, dan tanpa
   klaim harga. Kalimat panjang yang menyebut cabang ditolak founder (terasa
   tidak humanis + mengunci cakupan).

## 2. Fakta data yang membatasi klaim (dicek 2026-10-28)

- 22 venue, **8 penawaran berharga publik** → haram: "lengkap", "termurah",
  "semua ada harganya".
- Rentang nyata: Rp0 (lintasan lari) – Rp2.500.000/bln (membership fitness).
- Satuan di data: `monthly`, `per-hour`, `per-visit`. Area: Dago, Cibiru,
  Arcamanik, Setiabudi, Lembang.
- Padel: 0 penawaran berharga → jangan dipakai sebagai contoh harga.

## 3. Aturan yang keluar dari 7 putaran diskusi (simpan, jangan diulang)

| Putaran | Yang kamu tolak | Pelajaran buat copy berikutnya |
|---|---|---|
| 1 | "Olahraga di Bandung, harganya kelihatan" | Bahasa terlalu rapi, nggak ada identitas. |
| 2 | "Olga di Bandung mah kudu jelas ongkosna" | **Jangan maksa kata lokal.** Identitas datang dari ritme, bukan tempelan. |
| 3 | "Solusi cerdas…" / "Platform direktori…" | Klise iklan = langsung kalah. |
| 4 | "One stop olahraga di Bandung" | Klaim payung bukan positioning: nggak membedakan dari Google Maps, dan janji transaksional yang belum ada. |
| 5 | "Buku harga…", "Kamus…", "Panduan…" | Subjek **benda** = nameplate. Headline benda nggak akan pernah terasa manusia. |
| 6 | "Aku cariin tempat olahraga Bandung…" | Subjek **"aku"** = one-man show. Arah produk ini partisipatif. "Kami" juga belum (kolektifnya belum ada). |
| 7 | "Harganya pasti ada", "Tinggal pilih" | Masih **terasa iklan** karena menawarkan. → keputusan: pendek & besar, nol nada jualan. |

**Aturan yang tersisa:** headline tidak menjelaskan produk dan tidak merayu;
subline yang bekerja menjelaskan. Brand-nya orang-orangan, bukan
mengidentifikasi.

## 4. Kenapa F4 menang (dan risiko yang kamu terima)

Yang bikin aman: "Bandung" sudah di nama brand + logo, dan subline langsung
menyebut "biaya", "harga", "kontak" — jadi informasi "ini apa" tetap ada,
hanya saja tidak dipikul tagline.

**Risiko yang kamu terima sadar:** tagline 22 karakter tanpa kata "olahraga"
dan tanpa harga = tugas menjawab "ini buat apa" 100% pindah ke subline. Kalau
tes 5 detik gagal, yang harus diperbaiki **bukan** taglinenya — tapi panjang
atau urutan subline (pakai cadangan § 1), atau memastikan logo/nama brand
cukup besar di atasnya.

## 5. Loop kontribusi (fase berikutnya, bukan sekarang)

Headline tidak menyebut pembuatnya, jadi baris kontribusi bisa naik tanpa
rebranding:

- Sekarang (opsional, kecil, di bawah list): "Datanya dibantu warga Bandung."
- Saat fitur kontribusi / mode Bareng (Bulan 2): "Ada yang salah atau belum
  update? Sampaikan — kami catat sumber dan tanggalnya."

## 6. Tes 5 detik (3 orang, warga Bandung 18–35, bukan developer)

Screenshot header saja, lebar 390px, tampil **5 detik**, lalu disembunyikan.

| # | Pertanyaan | Lulus kalau |
|---|---|---|
| Q1 | "Kamu lihat apa tadi? Buat apa?" | menyebut cari tempat olahraga di Bandung |
| Q2 | "Kalau mau olahraga nanti sore, apa yang kamu dapat dari situ?" | jawaban konkret: nama tempat, angka/satuan harga, atau "ada yang gratis" |
| Q3 | "Bedanya apa sama cari `gym di Bandung` di Google?" | menyebut harga tertulis / nggak perlu nanya DM |
| Q4 | "Kalau ada harga yang salah, apa yang kamu lakuin?" | menyebut ada tempat buat ngabarin |

Ambang lulus: **Q1 ≥ 2 dari 3** (gerbang S12) **dan Q3 ≥ 1 dari 3**. Q4
dicatat saja, belum menggagalkan — fitur kontribusinya memang belum ada.
Kalau Q1 ≤ 1: jangan sentuh tagline, perbaiki subline dulu (§ 4).

### Hasil tes (dikerjakan 2026-10-29, 2 responden)

| Responden | Jawaban mentah | Dibaca sebagai |
|---|---|---|
| #1 | "oh list doang" | Q1 **lulus** (paham ini daftar tempat), Q3 **gagal** (tidak ada sinyal "ini beda dari Google — harganya ditulis") |
| #2 | "Awalnya ga ngerti itu apaan dan harus di apain tapi setelah baca itu kaya rate harga kaya dari murah-biasa-mahal gitu gaksi. Cuma aku ga ngerti setelah kita pilih itu lalu di apakan" | Q1 **setengah** (butuh baca dulu, tidak tertangkap seketika), Q2 **lulus** (menyebut angka/tier harga), Q3 **lulus** (tier murah–mahal disebut spontan = pembeda dari Google), **Q5 baru: gagal** (tidak tahu langkah setelah memilih) |
| #3 | belum | — |

Status gerbang: **belum lulus** — Q1 baru 1/2 jelas (ambang 2/3), Q3 1/2
sudah aman. Dua temuan:

1. **Tier harga bekerja.** #2 menyebut "rate harga murah-biasa-mahal" tanpa
   ditanya → sinyal pembeda dari Google ada di kartu, bukan di header.
2. **"Lalu di apakan" tidak terjawab di layar pertama.** Subline sekarang
   berakhir di "kamu tinggal pilih", jadi memilih terbaca sebagai langkah
   terakhir. Padahal setelah kartu diklik ada detail + tombol "Tanya harga
   via WhatsApp" (S10). Kegagalan ini **copy**, bukan komponen: janji di
   subline berhenti tepat sebelum bagian yang berguna.

Catatan metode: #2 menjawab setelah **membaca** halaman (bukan 5 detik lalu
ditutup). Artinya masalahnya bukan cuma kecepatan tangkap — bahkan setelah
sempat membaca, tujuan akhirnya tetap tidak kelihatan. Ini lebih berat dari
kegagalan 5 detik biasa dan cukup untuk merevisi subline sekarang.

**Keputusan founder 2026-10-29:** tes screenshot **ditunda**, tidak
dilanjutkan ke responden #2/#3 sekarang. Validasi dipindah ke S15 (kirim
link live ke 5 orang + tes 5 detik) — reaksi pada link asli lebih jujur
daripada reaksi pada screenshot. Konsekuensi yang diterima: S12 tetap
**belum dicentang**; kalau S15 menunjukkan orang tidak menangkap pembeda,
yang direvisi adalah subline, bukan komponen atau tagline.

### Revisi subline (2026-10-30) — **opsi A dipakai**

Sasaran: menjawab "lalu di apakan" tanpa kata "harga". Batas ≤110 karakter,
nada tetap datar (aturan § 3).

| Opsi | Teks | Karakter | Risiko |
|---|---|---|---|
| A | Kita bantu cari tempat olahraga — pilih satu, lalu chat pengelolanya. | 69 | "chat" cuma akurat untuk 19/22 venue (3 sisanya IG/website) |
| B | Kita bantu cari tempat olahraga — pilih satu, lalu lihat kontaknya. | 70 | Aman 22/22, tapi "lihat kontak" kurang mendorong aksi |
| C | Kita bantu cari tempat olahraga — pilih satu, lalu tanya langsung. | 66 | "tanya" tanpa objek bisa dibaca "tanya-tanya doang" |
| D | (subline tetap) + kartu diberi penanda bisa dibuka | — | Bukan copy: menyentuh komponen, dan #2 sudah baca sampai list tanpa paham |

Catatan: memperpanjang subline jadi 2 baris di 390px kemungkinan besar
malah memperlambat tangkapan pertama (#1 "list doang"). Kalau ragu antara A
dan B, pilih **A** — "chat pengelolanya" adalah bagian yang selama ini tidak
terlihat, sedangkan 3 venue tanpa WA tetap punya tombol kontak di detail.

**Keputusan founder 2026-10-30: opsi A.** Diimplementasikan `fe@8474a79`
(dikerjakan AI atas permintaan founder): teks subline diganti, `max-width`
subline 34ch → 42ch supaya 69 karakter tetap 2 baris di layar 390px, dan
baris "Tier: Belum dicek" di detail page dihapus (tier null = tidak ada
band, bukan harga belum dicek).

**Yang harus dicek setelah deploy:** tes ulang ke **1 orang baru** (jangan
responden #1/#2 — mereka sudah tahu jawabannya), satu pertanyaan:
"terus abis pilih, ngapain?" Lulus kalau jawabannya menyebut chat/WA ke
tempatnya. Kalau lulus → S12 dicentang, lanjut kirim link ke 5 orang.

## 7. Catatan implementasi (buat founder)

- `<title>` rute `/`: **"Bandung Sport — Direktori olahraga Bandung"**
  (keputusan founder 2026-10-29; usulan awal "tahu biayanya sebelum
  berangkat" tidak dipakai). Ditaruh di `fe/index.html` (bukan hanya
  `document.title`) karena preview link WA membaca HTML statis.
- `<meta name="description">`: **"Tempat olahraga di Bandung."** Tanpa daftar
  cabang olahraga (akan bertambah, dan menyebut 5 nama terasa mengunci
  cakupan) dan tanpa klaim harga.
- `<html lang="id">` — pembaca layar + mesin pencari pakai ini.
- Jangan ulang kalimat harga di tagline **dan** di CTA/section yang sama —
  satu kesempatan per ide di layar pertama.
- Tombol detail page sudah bilang "Tanya harga via WhatsApp" (S10) — jangan
  pakai "Nanya harga kok lewat DM" di header, ide yang sama dua kali.
- CTA "Lihat semua tempat" = anchor ke list. Belum ada pemilih mode di header
  (mode baru ada setelah S17/S20).
- Font: tagline 22 karakter = ~1 baris di 390px dengan tipografi hero. Kalau
  dipaksa 2–3 baris cuma buat kelihatan besar, itu kalah di tes 5 detik.
