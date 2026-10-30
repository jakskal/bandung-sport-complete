# Riset harga online — 14 venue (2026-10-30)

Provenance untuk offering yang dipetakan ke `be/data/offerings.json` dari
riset web (agent, 30 Okt 2026). Aturan: tiap harga punya URL + kutipan
persis; konten >1 tahun → confidence low; angka yang cuma muncul di
Story/screenshot/snippet ambigu TIDAK dipetakan. `lastVerified` di JSON
dibiarkan `null` sampai ada konfirmasi nyata dari vendor (S14), karena
tanggal scrape ≠ verifikasi (preseden S5).

## Baris yang dipetakan ke offerings.json

| venue | offering | harga | unit | visibilitas | sumber | kutipan | confidence |
|---|---|---|---|---|---|---|---|
| Urban Gym | PT 12 sesi | 700000 | per-session-pack | public | https://www.instagram.com/urbangymbdg/ (official-ig, snippet) | "Latihan bareng trainer profesional dalam 12 sesi hanya Rp700.000!" | medium |
| The Gym Pod @ Superindo Dago | Pay per visit (mulai) | 30000 | per-visit | public | https://www.instagram.com/p/DFCdt0JyYsb/ (official-ig, snippet) | "Bisa bayar per visit , harga mulai dari 30k. Access serba digital dengan aplikasi The Gym Pod." | medium |
| The Gym Pod @ Superindo Dago | Membership | null | null | on-request | https://www.thegympod.id/ (official-site, difetch) | "Pay only when you use. No Sign-up fees. IDR 0.- Membership. No Cancellation fees." | medium |
| Sangkuriang Fitness Center | Membership 1 bulan (promo) | 230000 (coret 280000) | monthly | public | https://www.instagram.com/p/DXeewv8Eh8g/ (official-ig, snippet) | "Membership 1 Bulan Rp280.000 menjadi Rp230.000 Hemat Rp50" | medium |
| Sangkuriang Fitness Center | Membership 3 bulan (total paket) | 630000 (coret 840000) | per-session-pack (sessions=3 bulan) | public | https://www.instagram.com/p/DXeewv8Eh8g/ (official-ig, snippet) | "Hemat Rp210.000! Membership 3 Bulan Rp840.000 menjadi Rp630.000" | medium |
| Komodoz Gym Bandung | Membership bulanan (+Rp20K daftar) | 150000 | monthly | public | https://www.instagram.com/p/DEZXPkdy0Fj/ (official-ig, snippet; perkiraan awal 2025) | "Pricelist Membership: • Bulanan: Rp150K/bulan (+ Rp20K pendaftaran pertama kali)" | low (2025) |
| Komodoz Gym Bandung | Harian | 20000 | per-visit | public | https://www.instagram.com/p/DEZXPkdy0Fj/ (official-ig, snippet; 2025) | "• Harian/Visit: Rp20K" | low (2025) |
| Komodoz Gym Bandung | Paket couple (2 orang) | 264000 | monthly | public | https://www.instagram.com/p/DEZXPkdy0Fj/ (official-ig, snippet; 2025) | "• Paket Couple: Rp264K/bulan (2…" | low (2025) |
| DIRAYA GYM CIBIRU | Membership bulanan | 150000 | monthly | public | https://www.instagram.com/reel/DVLQgbnkqcu/ (official-ig, snippet; 2026) | "Cibiru, Jatinangor member only 150k visit only 25k @dirayagym" | medium |
| DIRAYA GYM CIBIRU | Harian | 25000 | per-visit | public | https://www.instagram.com/reel/DVLQgbnkqcu/ (official-ig, snippet; 2026) | "PRICE LIST HARIAN RP25.000 PAKET MEMBER PAKET PERSONAL TRAINER DIRAYA GYM" | medium |
| DPC - Dago Padel Club | Sewa court | 250000–300000 | per-hour | public | https://www.lemon8-app.com/@harapanrakyat.com/7668933171396133394?region=id (blog, snippet; teks aslinya terpotong) | "Harga padel court dari 250-300k aja." | medium |
| Dago Mirador Padel | Sewa court (Sabtu) | 200000 | per-hour | public | https://www.instagram.com/reel/DasSi-yyBCW/ (reel pihak ketiga, snippet) | "Untuk sewa court di hari Sabtu, tarifnya cuma sekitar Rp200.000 per jam." | medium |
| Padel View Dago | Sewa court | 175000 | per-hour | public | https://www.instagram.com/p/DYeSxLDEa9u/ (posting daftar pihak ketiga, snippet) | "2. Padel View Dago … Rp175.000/jam" | medium |
| Peak Padel Dago | Sewa court per sesi (mulai) | 100000 | per-session-pack (sessions=1) | public | https://ayo.co.id/v/peak-padel-dago (booking, difetch OK; tarif court JSON-LD 125000) | "Mulai dari / Rp 100,000 / Per Sesi … Harga tertera sudah termasuk dua dus air mineral ukuran 240ml." | high |
| Peak Padel Dago | Membership | null | null | on-request | https://ayo.co.id/v/peak-padel-dago (booking, difetch OK) | "Paket membership hanya dapat dibeli melalui Aplikasi AYO Indonesia" | high |
| Aranya Padel | Sewa court | null | null | on-request | https://courtside.id/clubs/aranya-padel + IG @aranyapadel (WA 0813-1117-8841) | "Info & booking: 0813-1117-8841" | high (status unknown) |
| Futsal Attelo | Sewa lapangan | null | null | unknown | https://www.instagram.com/p/De507W3BqXy/ (official-ig; tarif baru berlaku 25 Okt 2026, angka tidak terekstrak; WA 085111886478) | "Per 25 Oktober 2026 Berlaku" | low |
| Lapang Futsal Tawekal | Sewa lapangan | null | null | unknown | (nol data — 5+ pencarian; keberadaan via https://jabar.kemenag.go.id/daerah/ekstrakurikuler-futsal-mis-al-misbah-tingkatkan-kerjasama-tim-di-lapang-tawekal-cipadung-5fK1xw) | — | low |

Ditambah: Kolam Renang Muslimah Arcamanik — tiket dewasa 15000 / SD 10000 /
pra-sekolah 6000, per-visit, public, source
https://lifehacks.id/kolam-renang-muslimah-arcamanik/ (blog, difetch penuh):
"Harga tiket untuk dewasa ialah Rp 15.000, untuk usia SD Rp 10.000 dan anak
pra sekolah Rp 6.000." — medium (tanggal publikasi tidak terlihat).

## Review 2026-10-30 (sebelum commit) — 3 baris dicoret

Dipetakan 18 dari 21 baris. Yang dibuang:

| Baris | Alasan |
|---|---|
| DPC - Dago Padel Club, sewa court 250–300rb/jam | Sumber tunggal blog agregator (lemon8), kutipan terpotong ("250-300k aja"), tanpa atribusi venue yang jelas; `joinMethod` juga tidak terbukti. Kartu DPC lebih jujur tampil "Harga belum dicek". |
| Sangkuriang, membership 3 bulan 630rb | `formatPrice` FE belum menampilkan `sessionsPerPack`, jadi baris ini tampil sebagai **"Rp630rb /sesi"** padahal 3 bulan — salah 3×. |
| Urban Gym, paket PT 12 sesi 700rb | Sama: akan tampil "Rp700rb /sesi" padahal 12 sesi — salah 12×. |

Keduanya (Sangkuriang 3 bulan & Urban Gym PT) dimasukkan lagi setelah
`formatPrice` menerima `sessionsPerPack`. Dicatat di `plan.md` § Nanti.

Yang dipertahankan meski confidence rendah: Komodoz (pricelist IG awal 2025,
riwayat tutup-buka) — harga bersumber resmi, `lastVerified` tetap `null`.

## Tidak dipetakan (dan alasannya)

- **Eldorado Swimming Pool** — semua angka (65k/45k/35k Lebaran 2025 via
  ulasbandung.com; 25k–45k reguler via snippet suara.com 404; funsplash
  snippet "Weekday 25rb Weekend 35rb") berumur >1 tahun ATAU snippet-only,
  dan profil IG resmi memuat "Informasi PENUTUPAN Kolam Renang". Perlu
  konfirmasi status kolam via (022) 2011666 sebelum input.
- Urban Gym membership — situs resmi timeout; tabel harga
  `urbangymbdg.com/pricing-table/` berisi placeholder tema ("$99/month").
- Dago Mirador promo Bank Mega ("Disc 50rb/100rb") — besaran diskon, bukan
  harga final.
- Peak Padel "Free Padel and Mahjong" (event 30 Agu 2026, snippet TikTok) —
  event sekali waktu.
- DPC angka "175–225rb/jam" — reel bertag #padelcikarang, atribusi tidak jelas.
- The Gym Pod "member mulai 420K", Komodoz PT "Rp500K/12 sesi", Diraya promo
  "100k" (2024) — hanya via URL redirect tak tersitasi / konten >1 tahun.

## Flag status venue

- Komodoz Gym Setiabudi: riwayat "TUTUP SEMENTARA" Ags 2024 diperpanjang,
  anggota sempat dialihkan ke @komodoz_gym.sdt, lalu teaser "kembali lagi".
  Verifikasi sebelum klaim harga.
- Eldorado: kemungkinan kolam tutup (lihat di atas).
- `kolammuslimah.com` mati (DNS); venue sendiri aktif.
- Semua venue padel tanpa website sendiri; booking via Courtside/AYO/WA.
