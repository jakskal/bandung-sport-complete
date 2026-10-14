# Layout: Detail Page

> Status: SKETSA — dibuat 2026-10-04 (malam), acara polish PR berjalan.
> Update kalau realita implementasi beda.

## Sekarang (kondisi current, sebelum polish)

```
┌─────────────────────────────┐
│ Kembali                     │  ← link polos nyamping
│ FTL Gym Dago Bandung        │  ← h1
│ Fitness - Dago              │
│ Harga: Rp 500.000 - ...     │  ← semua baris sama bobotnya
│ Tier: Sedang                │
│ Whatsapp: +62...            │  ← kontak numpuk tanpa grup
│ Instagram: https://...      │  ← URL mentah panjang
│ Website: https://ftlgym.com │
└─────────────────────────────┘
```

## Target (setelah polish)

```
┌─────────────────────────────┐
│ ← Kembali                   │  ← kecil, redup
│                             │
│ FTL Gym Dago Bandung        │  ← h1 besar
│ Fitness — Dago              │  ← meta, warna redup
│                             │
│ Rp 500.000 – Rp 2.500.000   │  ← DITONJOLKAN (besar + bold)
│ /monthly                    │  ← unit nyambung
│ [Sedang]                    │  ← tier badge (pill berwarna)
│                             │
│ Kontak                      │  ← heading kecil
│ ┌─────────────────────────┐ │
│ │ 💬 +62 818-6878-58      │ │  ← WA rapi
│ │ 📷 @ftlgym.id           │ │  ← IG (handle, bukan URL)
│ │ 🌐 ftlgym.com           │ │  ← label "Website", bukan URL
│ └─────────────────────────┘ │
└─────────────────────────────┘
```

Emoji di sketsa = placeholder icon, boleh diganti teks polos dulu
("WA:", "IG:", "Web:") — icon library bukan scope sekarang.

## State khusus

1. **Venue nggak ketemu** (`/place/999`): pesan "Venue tidak ditemukan"
   + link `← Kembali ke list` — JANGAN polos tanpa jalan keluar.
2. **Harga null**: jangan tampil "Harga: N/A" — ganti teks
   "Hubungi untuk harga" (meta-info: kontaknya ada di bawah).
3. **Semua kontak null**: sembunyikan seluruh blok Kontak
   (section kosong itu jelek).

## Urutan PR implementasi

1. Fix not-found fallback (tambah link balik).
2. Label website: teks "Website", href tetap URL.
3. Restrukturisasi JSX sesuai target di atas (harga null →
   "Hubungi untuk harga"; kontak null → hide blok).
4. Styling: spacing, harga besar/bold, badge tier, blok kontak.
