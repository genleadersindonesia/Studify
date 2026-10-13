# Studify – Panduan Deploy

Struktur proyek:

```
index.html                  -> website Studify (frontend)
api/ai.js                   -> fungsi server untuk Vercel
netlify/functions/ai.js     -> fungsi server untuk Netlify
netlify.toml                -> pengalihan /api/* ke fungsi Netlify
.env.example                -> contoh variabel lingkungan
```

Frontend memanggil `POST /api/ai`. Fungsi server menambahkan API key lalu meneruskan ke Claude, jadi key tidak pernah terlihat di browser.

## 1. Ambil API key
Buat akun di Claude Platform (https://platform.claude.com), buat API key, dan isi saldo/kredit.

## 2A. Deploy ke Vercel (disarankan)
1. Unggah folder ini ke repo GitHub, lalu "Add New Project" di vercel.com dan pilih repo tersebut. (Atau jalankan `npx vercel` di folder ini.)
2. Buka Project Settings > Environment Variables, tambahkan `ANTHROPIC_API_KEY` berisi key kamu.
3. Deploy ulang. Selesai: buka URL Vercel-mu dan fitur AI aktif.

## 2B. Deploy ke Netlify
1. Unggah folder ke GitHub lalu "Import from Git" di netlify.com (drag-and-drop folder tidak menjalankan fungsi server).
2. Site configuration > Environment variables > tambahkan `ANTHROPIC_API_KEY`.
3. Deploy ulang.

## 3. Uji di komputer sendiri
```
npm i -g vercel
cp .env.example .env     # isi API key
vercel dev
```
Buka http://localhost:3000. Membuka `index.html` langsung dari file (file://) tidak akan menjalankan fitur AI karena tidak ada server.

## Keamanan dan biaya
- Jangan taruh API key di `index.html`.
- `api/ai.js` sudah membatasi ukuran pesan dan 20 permintaan/menit per IP, tapi pembatas itu sederhana dan bisa terlewati di hosting serverless. Jika situs dibuka untuk umum, tambahkan login atau kode akses, dan atur batas pengeluaran di dashboard Claude Platform.
- Semua pemakaian ditagih ke akun API-mu. Ringkasan dan podcast dari materi panjang memakai lebih banyak token.
- Model bisa diganti lewat variabel `CLAUDE_MODEL` (default `claude-sonnet-5-5`).

## Catatan perilaku
- Jawaban AI muncul setelah selesai (tidak mengalir sedikit demi sedikit seperti di dalam Claude).
- Jika halaman dibuka di dalam Claude, Studify otomatis memakai fasilitas AI bawaan Claude; di hosting sendiri memakai `/api/ai`.
- Data materi, kartu, dan progres tetap tersimpan di browser pengguna (localStorage).
