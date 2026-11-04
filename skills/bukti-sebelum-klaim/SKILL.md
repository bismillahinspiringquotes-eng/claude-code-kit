---
name: bukti-sebelum-klaim
description: Pakai sebelum menulis "selesai", "sudah jalan", "sudah aman", "nol sisa", atau meneruskan laporan dari sesi/agen/orang lain. Memisahkan tingkat bukti (dibuat ≠ masuk main ≠ tayang ≠ dipakai), mengecek pemanggil, dan menguji alat ukur ke keadaan yang sudah diketahui sebelum angkanya dipercaya. Use before claiming done/working/safe or relaying someone else's report.
---

# Bukti sebelum klaim

Catatan yang salah lebih berbahaya daripada tidak ada catatan: butir yang sudah tertulis "selesai" tidak dikejar siapa pun lagi.

## Empat tingkat — tulis tingkat yang benar-benar terbukti

| Tingkat | Bukti minimum |
|---|---|
| Dibuat | berkas/commit/PR ada |
| Masuk cabang utama | `git merge-base --is-ancestor <commit> origin/main` berhasil, atau status PR = merged |
| Tayang | URL yang dibuka orang mengembalikan isinya (`curl` + cocokkan teks), migrasi terlihat di DB produksi |
| Dipakai | ada perilaku nyata: baris baru tercatat, pesan terkirim, pengguna menyelesaikan alurnya |

"PR dibikin" bukan "masuk main". "Masuk main" bukan "tayang". Judul commit yang muncul di `git log --all` juga belum bukti, karena cabang yang belum digabung ikut terbaca.

## Cek sebelum klaim

1. **Laporan orang lain bukan bukti.** Laporan sesi lain, agen lain, atau rekan kerja diperiksa sendiri dulu. Verifikasi sendiri hampir selalu menemukan sesuatu yang lebih.
2. **Cari pemanggilnya.** Fitur, kolom, atau fungsi yang lengkap tapi nol pemanggil/penulis itu belum ada. Contoh: kolom "terakhir dipakai" yang tak pernah diisi, sehingga dasbor menampilkan "Never" untuk semua token yang aktif.
3. **Uji alat ukurnya dulu.** Sebelum percaya angka dari skrip/penyapu/pengukur baru, adu ke satu kasus yang jawabannya sudah diketahui:
   - kontrol positif: kasus yang *pasti* bermasalah harus terdeteksi
   - kontrol negatif: kasus yang *pasti* bersih harus lolos
   Alat yang tidak bisa menunjukkan kegagalan tidak bisa membuktikan keberhasilan.
4. **Baca kasusnya satu per satu, minimal sampelnya.** Hitungan otomatis "24 gagal" bisa berisi 19 salah ukur. Buka beberapa kasus mentah sebelum angkanya dilaporkan.
5. **Stempel tanggal ukur.** Angka tanpa tanggal ukur akan dibaca sebagai angka hari ini. Tulis "diukur 4 Nov 19.30".

## Format laporan

```
<apa> — <tingkat yang terbukti> — bukti: <perintah/URL + hasil singkat> — diukur <tanggal>
```

Contoh: "Halaman harga — tayang — `curl https://contoh.id/harga` memuat 'Rp450.000' — diukur 4 Nov 20.10."

## Kenapa skill ini ada

Dari kejadian nyata (disamarkan):
- Tiga perbaikan dicatat "selesai" berdasarkan laporan sesi lain. Ketiganya masih di PR terbuka, termasuk kebocoran data tamu. Kebocorannya hidup dua jam lebih lama dari yang tercatat, dan tidak ada yang mengejar karena catatannya bilang beres.
- Pengukur lebar halaman bilang "aman" karena membandingkan dua angka yang ikut melebar bersama. Ketahuan cuma karena ada satu angka pembanding dari jalur lain.
- Penyapu klaim harga melahirkan 14 tuduhan palsu dari 15. Ketahuan karena kalimatnya dibaca satu per satu.
- Penggaris otomatis menghitung 24 dari 52 kasus gagal. Setelah dibaca manual, yang benar-benar gagal cuma 2; sisanya pasangan dari subjek berbeda dan nilai lama yang terbaca sebagai nilai baru.

## Batasan

- Jangan berhenti di "200 OK". Isi halamannya yang dicocokkan.
- Kalau buktinya belum bisa didapat (butuh login, butuh akses), tulis tingkat yang terbukti dan apa yang belum. Jangan naikkan tingkat karena "seharusnya sudah".
- Skill ini melengkapi `pre-deploy-3gate` (khusus deploy), bukan menggantikannya.
