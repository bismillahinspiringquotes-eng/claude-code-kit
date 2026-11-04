---
name: anti-ai-slop
description: Pakai sebelum mengirim teks apa pun yang dibaca manusia — laporan ke founder, dokumen/PDF klien, caption, email, isi Sheet, log yang dibaca orang. Membuang pola tulisan khas AI (bukan sekadar X tapi Y, keterangan gambar bergaya tagline, panah berantai, huruf tebal di mana-mana, istilah teknis) dan menulis ulang jadi bahasa lugas. Use before sending any human-read text to strip AI-writing patterns.
---

# Anti AI slop

Tulisan berbau AI langsung ketahuan, dan yang menanggung malunya orang yang mengirim, bukan modelnya. Skill ini dipakai **sebelum** teks dikirim, bukan sesudah ditegur.

## Kapan dipakai

- Laporan ke founder / atasan / tim
- Dokumen, PDF, proposal, resume untuk klien
- Caption, posting, email, pesan WA
- Isi yang dibaca orang di Sheet, dasbor, log rekap

Tidak berlaku untuk: kode, komentar kode, catatan internal yang cuma dibaca mesin.

## Pola yang wajib dibuang

| Pola | Contoh | Ganti dengan |
|---|---|---|
| Kontras palsu | "bukan sekadar aplikasi, tapi partner" | sebut faktanya: "aplikasi ini mencatat pesanan dan stok" |
| Keterangan gambar bergaya tagline | "Langsung ke pokok", "Satu link, banyak cara bayar" | apa yang tampil di gambar: "Halaman bayar dengan pilihan transfer dan QRIS" |
| Daftar "Tujuan" yang mengambang | "Meningkatkan pengalaman pengguna" | hasil yang bisa dicek: "pesanan masuk tercatat dalam 1 menit" |
| Panah berantai dalam kalimat | "pesan → bot → admin → selesai" | kalimat biasa |
| Huruf tebal di mana-mana | tiap kalimat ada **tebal** | tebal cuma untuk satu hal yang benar-benar tak boleh terlewat, atau tidak sama sekali |
| Markdown di dokumen untuk orang | `**`, backtick, `#` di PDF/Word/Sheet | teks polos, judul bagian polos |
| Pembuka/penutup basa-basi | "Tentu! Berikut adalah…", "Semoga membantu!" | langsung isi |
| Sok akrab di bot | "Pas, 15 kg masuk 👍" | "Pesanan 15 kg tercatat." |
| Istilah teknis ke orang awam | push, commit, deploy, offset, endpoint, JSON, nama berkas | apa yang orang itu lihat dan apa yang harus dia lakukan |
| Paralelisme tiga | "cepat, mudah, dan aman" | satu klaim yang bisa dibuktikan |
| Kata pengisi | "sangat", "berbagai", "secara signifikan", "solusi komprehensif" | angka atau hapus |

## Cara menulis ulang

1. **Satu informasi per kalimat.** Kalimat pendek dan faktual.
2. **Angka dengan satuannya dan tanggal ukurnya.** "52 pesan (diukur 4 Nov)", bukan "banyak pesan".
3. **Istilah milik pembaca.** Klien bilang "pesanan", jangan tulis "order entry". Founder bilang "tamu", jangan tulis "user".
4. **Laporan ke founder = apa yang dia lihat/rasakan + apa yang harus dia lakukan.** Nol nama berkas, kode commit, nama alat. Kalau dia perlu bertanya "maksudnya apa", tulisannya gagal.
5. **Satu ide, satu paragraf.** Kalau paragraf butuh judul kecil supaya terbaca, paragrafnya terlalu panjang.

## Cek terakhir sebelum kirim

Baca ulang sekali, **khusus** mencari pola di tabel atas. Bukan cek tata bahasa, cek bau AI. Lalu baca keras-keras dalam kepala: kalau terdengar seperti slide presentasi, tulis ulang.

## Kenapa skill ini ada

Dari kejadian nyata (disamarkan):
- Dokumen usulan untuk calon klien dibalas founder: "bahasanya tolol, AI slop". Isinya penuh "bukan sekadar X, tapi Y", keterangan gambar bergaya slogan, dan huruf tebal di setiap baris. Dokumen itu mau dibawa ke klien, jadi muka founder yang dipertaruhkan.
- Resume untuk pemilik toko dikirim penuh tanda `**`. Balasan: "lo pikir yang baca robot?"
- Laporan ke founder memakai "offset", "push", "commit", "tab sesi". Founder bukan programmer, jadi laporannya tidak terbaca sama sekali.

## Batasan

- Jangan membuang bukti demi ringkas. Potong basa-basi, bukan angka dan buktinya.
- Jangan mengganti satu gaya AI dengan gaya AI lain ("Mari kita bedah…"). Tujuannya bahasa orang, bukan gaya baru.
- Kalau pembaca menulis dengan register tertentu (gue/lo, saya/Bapak), ikuti register pembaca.
