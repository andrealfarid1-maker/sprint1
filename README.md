# Solace Coffee

Website katalog produk untuk kedai kopi **Solace Coffee**. Dibangun dengan
Next.js 14 (App Router) + Tailwind CSS. Data produk disimpan statis di
`data/products.json` — belum memakai database.

## Menjalankan secara lokal

```bash
npm install
npm run dev
```

Buka http://localhost:3000.

## Build untuk produksi

```bash
npm run build
npm start
```

Sudah diverifikasi `npm run build` berjalan sukses dan menghasilkan halaman
statis untuk semua produk (SSG), siap di-deploy ke Vercel, Netlify, atau
server Node apa pun.

## Struktur halaman

- `/` — Beranda: hero, kategori, produk terlaris, lokasi & jam operasional.
- `/katalog` — Semua produk dengan filter kategori + pencarian.
- `/produk/[id]` — Detail produk: galeri foto, pilihan varian, tambah ke keranjang.
- `/keranjang` — Keranjang belanja (disimpan di localStorage browser).
- `/checkout` — Form data pemesan + pilihan metode pembayaran (QRIS, DANA, OVO, GoPay, Transfer Bank).

## Mengubah data produk

Edit `data/products.json`. Setiap produk punya field:

```json
{
  "id": "kopi-susu-gula-aren",
  "nama": "Kopi Susu Gula Aren",
  "kategori": "kopi",
  "harga": 25000,
  "stok": 40,
  "terlaris": true,
  "deskripsi": "...",
  "varian": [{ "nama": "Ukuran", "opsi": ["Reguler", "Large"] }],
  "gambar": ["/images/products/nama-file-1.svg"]
}
```

`kategori` harus salah satu dari slug yang didaftarkan di `config/site.ts`
(`site.categories`). Tambahkan gambar asli produk ke `public/images/products/`
lalu ganti path di field `gambar` (boleh `.jpg`/`.png`/`.webp`).

> Gambar bawaan di repo ini adalah placeholder SVG bergradasi warna brand,
> silakan ganti dengan foto asli produk kamu.

## Mengubah warna & tampilan

Semua warna brand ada di **satu file**: `config/colors.js`. Ubah nilai hex di
sana, lalu seluruh situs (tombol, badge, teks aksen, footer, dst.) otomatis
mengikuti karena `tailwind.config.js` membaca file ini.

Info umum situs (nama brand, alamat, nomor WhatsApp, jam operasional, rekening
bank, kategori menu, metode pembayaran) ada di `config/site.ts`.

## Metode pembayaran

Checkout saat ini bersifat simulasi (belum terhubung payment gateway
sungguhan): setelah pengguna mengisi data dan memilih metode (QRIS / DANA /
OVO / GoPay / Transfer Bank), sistem menampilkan kode pesanan dan instruksi
pembayaran. Untuk produksi nyata, hubungkan tombol "Buat Pesanan" di
`app/checkout/page.tsx` ke payment gateway (mis. Midtrans, Xendit) atau API
backend kamu.

## Catatan teknis

- Keranjang disimpan di `localStorage` browser lewat React Context
  (`lib/cart-context.tsx`) — belum ada backend/database.
- Halaman produk di-generate statis (`generateStaticParams`) dari
  `data/products.json` saat build.
- Font: `@fontsource/fraunces` (display) dan `@fontsource/inter` (body),
  di-bundle secara lokal lewat npm (tidak perlu fetch ke Google Fonts).
