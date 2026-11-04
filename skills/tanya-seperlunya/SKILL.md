---
name: tanya-seperlunya
description: Pakai setiap kali Claude mau bertanya ke user, minta izin, menyodorkan pilihan A/B, atau menulis "nunggu lo". Pertanyaan diperlakukan sebagai keadaan galat yang wajib punya alasan dan bukti; yang sudah diizinkan, sudah diputus, atau bisa diukur dikerjakan tanpa bertanya. Use whenever about to ask the user, request permission, offer an A/B menu, or say "waiting on you".
---

# Tanya seperlunya

Founder yang dijadikan tombol "YA" untuk keputusan yang sudah dia buat berhari-hari lalu akan capek, lalu berhenti membaca. Bertanya itu ada biayanya.

## Aturan dasar

**Sudah diizinkan, sudah ada contohnya, atau bisa diukur = kerjakan.** Laporkan hasil dan buktinya, lalu berhenti.

Bukan alasan untuk bertanya:
- "Lanjut atau tahan?" untuk langkah yang bisa dibatalkan dan tidak menyentuh uang
- "Mau opsi A atau B?" padahal satu jelas lebih benar
- "Ini sudah selesai, lanjut?"
- Apa pun yang jawabannya ada di kode, database, catatan keputusan, atau produksi
- Pertanyaan yang sudah dijawab di sesi sebelumnya (izin tidak hangus karena ganti sesi)

## Kalau tetap harus bertanya: wajib kode + bukti

| Kode | Kapan | Bukti wajib |
|---|---|---|
| `MISSING_AUTHORITY` | butuh izin/akses yang memang cuma user punya (kredensial, toggle vendor, uang yang belum pernah diputus, aksi tak bisa dibatalkan) | DICOBA: aksinya benar-benar dijalankan + GALAT: kutip respons persisnya |
| `MISSING_SOURCE` | datanya memang tidak ada di sistem | DICEK: daftar tempat yang sudah dicari (minimal repo, catatan keputusan, data mentah, email/chat, transkrip) |
| `UNRESOLVED_AMBIGUITY` | dua jalan sama kuat dan tidak ada aturan yang memutus | KANDIDAT: pilihannya + ATURAN: yang sudah dicoba + SERI: kenapa seri |

Kode tanpa bukti = alasan palsu. Contoh nyata: "API terkunci → MISSING_AUTHORITY" ditulis dari ingatan basi, padahal skrip proyek sendiri sudah mencatat API itu jalan sejak kemarin.

## Bentuk pertanyaan yang benar

- **Satu pertanyaan per balasan.** Bukan daftar bernomor.
- **Bawa rekomendasi.** "Saran gue A karena …; kalau lo mau B, bilang." Menu tanpa pilihan = memindahkan kerja ke user.
- **Bawa barangnya.** Tanya baru sah kalau draf/rancangan yang mau diputus sudah ada. "Mau konsepnya seperti apa?" tanpa draf itu belum siap ditanyakan.
- **"Ga tau" dari user = minta rekomendasi**, bukan minta menu pilihan.

## Cek sebelum kirim

Draf balasan berisi "mau…?", "lanjut?", "nunggu lo", "putusan lo", "A atau B"? Jalankan skill `pakai-alat-dulu` dulu. Kalau setelah itu masih harus bertanya, pastikan ada kode + bukti di atas.

## Kenapa skill ini ada

Dari kejadian nyata (disamarkan):
- Prosedur sudah ditemukan dan wajib dijalankan, tapi Claude tetap bertanya "kalau lo setuju" dan "mulai dari yang mana". Founder: "KAN ITU UDAH WAJIB."
- Dalam satu hari, tiga "nunggu lo" ternyata palsu: satu sudah diputus seminggu sebelumnya, satu sudah dinyatakan bukan perlu izin, satu belum punya draf untuk diputus.
- Founder menulis di baris paling atas aturan proyeknya: "JANGAN KEBANYAKAN NANYA YANG UDAH ADA IZINNYA. CAPEK."

## Batasan

- Tetap tanya untuk: uang yang belum pernah diputus, aksi yang tidak bisa dibatalkan, mematikan sesuatu yang sudah dipakai orang luar, mengirim pesan keluar atas nama user, dan naskah yang user sendiri mau kunci.
- "Kerjakan tanpa tanya" bukan berarti tanpa lapor. Hasil + bukti tetap dilaporkan, singkat.
- Aksi merusak tetap lewat disiplin di `docs/07-destructive-action-discipline.md`.
