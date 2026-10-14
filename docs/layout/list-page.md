# Layout: List Page + Filter

> Status: SKETSA AWAL — acara kasar biar kebayang, bukan spec final.
> Terakhir disentuh: 2026-09-27 (saat mulai PR filter Bulan 2).

## Mobile (default, ~375px)

```
┌─────────────────────────────┐
│  Bandung Sport              │  ← header (h1)
│  12 tempat                  │  ← result count (selalu ada)
├─────────────────────────────┤
│ (Semua)(Fitness)(Futsal)…  →│  ← chip kategori, scroll horizontal
│ (Semua)(Gratis)(Murah)…    →│  ← chip tier, scroll horizontal
│ [ Semua area          ▼ ]   │  ← dropdown area (native select)
├─────────────────────────────┤
│ ┌─────────┐  ┌─────────┐   │
│ │FTL Gym  │  │FIT HUB  │   │  ← card grid, auto-fill minmax
│ │Dago     │  │Setiabudi│   │     (sudah jalan hari ini)
│ │Rp 500rb │  │…        │   │
│ │[Sedang] │  │[Murah]  │   │  ← tier badge
│ └─────────┘  └─────────┘   │
│   … lebih banyak card …     │
└─────────────────────────────┘
```

Anotasi perilaku:

- Chip aktif = styling beda (background terisi). Single-select per baris.
  Chip "Semua" = reset dimensi itu.
- Count ikut berubah pas filter aktif: "3 tempat" bukan "12 tempat".
- `→` = scroll horizontal; jangan wrap jadi 2 baris.
- 3 kontrol filter total tingginya jangan lebih dari ~130px — layar HP
  mahal, list-nya si bintang utama.

## Empty state (hasil filter = 0) — WAJIB didesain

```
┌─────────────────────────────┐
│  Bandung Sport              │
│  0 tempat                   │
├─────────────────────────────┤
│ (Futsal)*  (Murah)*         │  ← filter aktif keliatan jelas
│                             │
│   Nggak ada yang cocok 😢   │
│   Coba longgarkan filtermu  │
│   [ Reset semua filter ]    │  ← tombol reset 1-tap
└─────────────────────────────┘
```

## Desktop (≥ ~960px)

Sama aja, cuma:

- container max-width 960px center (sudah ada).
- chip baris kategori & tier bisa jadi nggak perlu scroll.
- grid otomatis nambah kolom (auto-fill) — nggak perlu kode tambahan.

## Yang SENGAJA belum ada (jangan kegoda)

- Bottom-sheet filter ala Airbnb — overkill buat 22 venue.
- Multi-select tier — single-select dulu.
- URL param buat share hasil filter — polish, belakangan (sesuai plan).
- Search bar teks — bukan scope v1.

## Urutan PR implementasi

1. Filter kategori (chip) — state pertama, satu dimensi.
2. Filter tier (chip) — ikut pola yang sama.
3. Filter area (select) — pola sama, kontrol beda.
4. Count + empty state + reset.
