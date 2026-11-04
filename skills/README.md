# Skills

Skill = berkas instruksi yang dipakai Claude Code otomatis begitu tugasnya cocok. Tiap skill di sini satu folder berisi `SKILL.md`, format yang dibaca Claude Code langsung.

## Cara pasang (2 menit)

Salin folder skill yang lo mau ke salah satu tempat ini:

- **Semua proyek di laptop lo:** `~/.claude/skills/`
- **Satu proyek saja:** `<folder-proyek>/.claude/skills/`

Contoh, pasang empat skill dasar untuk semua proyek:

```
mkdir -p ~/.claude/skills
cp -r skills/anti-ai-slop skills/pakai-alat-dulu skills/bukti-sebelum-klaim skills/tanya-seperlunya ~/.claude/skills/
```

Buka sesi Claude Code baru. Cek dengan mengetik `/skills`; nama skill yang lo salin harus muncul. Kalau tidak muncul, pastikan strukturnya `~/.claude/skills/<nama>/SKILL.md` (berkasnya di dalam folder, bukan langsung di `skills/`).

## Skill dasar (pasang ini dulu)

Empat skill ini menambal kebiasaan buruk Claude yang paling sering muncul, di proyek apa pun:

- **[anti-ai-slop](anti-ai-slop/SKILL.md)** — sebelum mengirim teks yang dibaca manusia: buang pola tulisan khas AI dan istilah teknis, tulis ulang jadi bahasa lugas.
- **[pakai-alat-dulu](pakai-alat-dulu/SKILL.md)** — sebelum menjawab dari ingatan, bilang "nggak tahu", atau menyuruh lo mencari: cek repo, skrip, data, dan catatan yang sudah ada. Alat sendiri dulu, layanan berbayar terakhir.
- **[bukti-sebelum-klaim](bukti-sebelum-klaim/SKILL.md)** — sebelum bilang "selesai": pisahkan dibuat / masuk main / tayang / dipakai, cari pemanggilnya, uji alat ukur ke kasus yang sudah diketahui.
- **[tanya-seperlunya](tanya-seperlunya/SKILL.md)** — sebelum bertanya ke lo: yang sudah diizinkan atau bisa diukur dikerjakan saja; kalau tetap tanya, wajib alasan + bukti.

## Skill khusus

### Rilis
- **[pre-deploy-3gate](pre-deploy-3gate/SKILL.md)** — tiga gerbang sebelum klaim "sudah tayang": type check, build, lalu `curl` URL dan cocokkan isinya.
- **[secret-rotation-walkthrough](secret-rotation-walkthrough/SKILL.md)** — memutar kunci/rahasia yang bocor tanpa ada sistem yang terlewat.

### Keamanan
- **[security-audit-database](security-audit-database/SKILL.md)** — 7 cek database: data antar-pelanggan bocor, kunci admin terbawa ke browser, rute tanpa cek login, data pribadi tanpa enkripsi, endpoint publik membocorkan kolom, webhook tanpa cek tanda tangan, migrasi yang menghapus policy.
- **[security-audit-web-app](security-audit-web-app/SKILL.md)** — 6 cek sisi web: env bocor ke browser, kode server masuk bundle, header keamanan, HTML tak aman, redirect terbuka, middleware bisa dilewati.

### SEO
- **[seo-content-audit](seo-content-audit/SKILL.md)** — title, meta description, heading, alt gambar, tautan internal, data terstruktur, sitemap + robots.
- **[seo-audit-multilocale](seo-audit-multilocale/SKILL.md)** — situs multi-bahasa: hreflang timbal balik, sitemap per bahasa, canonical, JSON-LD per bahasa, redirect.

### Suara / copy
- **[voice-register-check](voice-register-check/SKILL.md)** — memindai teks yang dilihat pengguna: istilah teknis ("error", "null") dan istilah internal ("operator", "slug") yang bocor.

Contoh di skill keamanan/SEO memakai Next.js + Supabase di server sendiri, tapi ceknya berlaku untuk framework lain.

## Menambah skill

Lihat [../CONTRIBUTING.md](../CONTRIBUTING.md). Syarat skill baru:
- Lahir dari masalah nyata yang berulang, bukan "mungkin berguna"
- Satu folder, satu `SKILL.md`, frontmatter `name` sama dengan nama folder + `description` yang menyebut kapan dipakai
- Bahasa polos (pembacanya bisa bukan programmer)
- Diakhiri hasil yang jelas (laporan, daftar cek lulus, langkah berikut)
- Ada bagian batasan: apa yang tidak boleh dilakukan
