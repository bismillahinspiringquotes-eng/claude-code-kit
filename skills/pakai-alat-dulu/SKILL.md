---
name: pakai-alat-dulu
description: Pakai sebelum menjawab dari ingatan, sebelum bilang "gue nggak tahu", sebelum menyuruh user mencari/menjalankan sesuatu, dan sebelum memakai layanan berbayar. Memaksa Claude memeriksa alat dan data yang sudah ada (repo, skrip di tools/, log, transkrip sesi lama, DB, email) dan memakai alat milik sendiri dulu. Use before answering from memory, saying "I don't know", delegating to the user, or reaching for a paid service.
---

# Pakai alat dulu

Kegagalan yang paling sering bukan salah hitung, tapi **lupa bahwa alatnya ada**. Claude menjawab dari ingatan, menebak, atau menyuruh user mencari sendiri, padahal jawabannya ada di satu `grep`.

## Pemicu (berhenti dan cek dulu kalau draf jawaban berisi ini)

- "gue nggak tahu…", "sepertinya…", "biasanya…"
- "coba lo cek…", "jalankan ini lalu tempel hasilnya", "kirim datanya ke gue"
- "mau A atau B?" untuk hal yang bisa diukur
- "nunggu lo" / "nunggu konfirmasi" untuk data yang mungkin sudah tercatat
- Mau memanggil layanan/API berbayar untuk riset, scraping, atau data

## Urutan pemeriksaan

1. **Peta alat proyek.** Baca `README`, `CLAUDE.md`, isi `tools/` atau `scripts/`. Proyek yang sudah berjalan hampir selalu punya skrip untuk tugas yang berulang. Kalau ada skripnya, pakai skripnya, jangan bikin ulang dari nol.
2. **Catatan keputusan.** `grep -rn "<kata kunci>" decisions/ SESSION.md docs/`. Banyak "pertanyaan baru" sudah pernah diputus.
3. **Data mentah.** Database (kueri baca saja), log, berkas ekspor, kotak masuk email (baca saja), riwayat chat yang tersimpan.
4. **Transkrip sesi lama.** Ringkasan akhir sesi itu ringkasan; yang tidak masuk ringkasan hilang dari ingatan tapi masih ada di transkrip. Untuk "data hilang", cek dulu apakah sesi sebelumnya sendiri yang mengubahnya.
5. **Riwayat git.** `git log -S "<teks>"`, `git log --all -- <berkas>`.
6. **Baru** sesudah lima itu kosong: tanya user, atau pakai layanan luar.

## Aturan pakai alat

- **Alat milik sendiri dulu, berbayar terakhir.** Kalau proyek punya skrip riset/scraper sendiri, itu jalur pertama. Layanan berbayar cuma kalau alat sendiri benar-benar tidak bisa, dan sebut biayanya sebelum jalan.
- **Jalankan sendiri, jangan menyuruh.** Kalau Claude punya terminal, "jalankan ini lalu tempel hasilnya" itu salah. User bukan terminal.
- **Alat gagal ≠ hasil kosong.** Scraper yang mengembalikan nol baris bisa berarti diblokir, bukan "tidak ada data". Cek galatnya sebelum menyimpulkan.
- **Jangan ganti barang yang diminta dengan yang lebih gampang.** Diminta X dan X sulit: bilang batasnya dalam satu kalimat, lalu jalur tercepat menuju X. Bukan diam-diam bikin Y.
- **Cari pemanggilnya sebelum bilang "sudah ada".** Fungsi/kolom/fitur yang lengkap tapi tidak pernah dipanggil itu belum ada.

## Kenapa skill ini ada

Dari kejadian nyata (disamarkan):
- Claude meminta founder "kirim dokumen itinerary kapal", padahal datanya ada di listing, PDF operator, Sheet, dan sistem booking. Founder: "ada semua datanya, cek di mana kek."
- Dalam satu hari, dua kali Claude menulis "gue nggak tahu sudah dibalas atau belum" dan "putusan lo" ke founder. Ternyata kotak email bisa dicek sendiri, dan keputusannya sudah tercatat dua minggu sebelumnya.
- Data uji pembayaran "hilang". Claude menyuruh sesi lain menelusuri empat hipotesis, padahal sesi Claude sebelumnya sendiri yang menghapus barisnya. Jawabannya ada di transkrip.
- Riset sosial media berulang kali lewat layanan berbayar, padahal ada skrip riset milik sendiri. Dilanggar empat kali sebelum dijadikan aturan.

## Batasan

- Membaca boleh; mengubah tetap ikut disiplin aksi merusak (lihat `docs/07-destructive-action-discipline.md`). Kueri DB = `SELECT` saja, email = baca saja.
- Rahasia (`.env`, kunci API) jangan dicetak ke layar walau sedang mencari. Cek bentuknya, pakai nilainya hanya di dalam skrip.
- Kalau semua tempat sudah dicek dan memang kosong, sebutkan tempat apa saja yang sudah dicek. "Nggak ketemu" tanpa daftar itu sama dengan tidak mencari.
