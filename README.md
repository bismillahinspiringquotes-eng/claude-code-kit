# Claude Code Kit

Aturan, template, dan skill supaya Claude Code kerja rapi: nggak nebak, nggak ngarang, nggak boros, dan nggak nulis kayak robot. Diambil dari pemakaian harian di beberapa usaha yang berjalan, bukan teori.

*English summary at the bottom.*

## Mulai dari sini (pemula)

Belum pernah pakai Claude Code, atau sudah pakai tapi Claude sering ngaco? Ikuti langkah ini. Semua perintah lo jalankan sendiri di Terminal, bukan disuruh ke Claude.

**0. Ambil repo ini.**

```
git clone https://github.com/bismillahinspiringquotes-eng/claude-code-kit.git
cd claude-code-kit
```

Nggak punya git? Klik tombol hijau **Code → Download ZIP** di halaman GitHub, ekstrak, lalu buka Terminal di folder hasil ekstraknya.

**1. Pasang empat skill dasar.** Jalankan dari dalam folder `claude-code-kit`:

```
mkdir -p ~/.claude/skills
cp -r skills/anti-ai-slop skills/pakai-alat-dulu skills/bukti-sebelum-klaim skills/tanya-seperlunya ~/.claude/skills/
```

Perintah ini cuma menyalin empat folder berisi teks. Mau lihat isinya dulu? Buka `skills/<nama>/SKILL.md`, isinya instruksi biasa, tanpa kode yang dijalankan.

Isinya:
- **anti-ai-slop** — Claude membuang pola tulisan khas AI sebelum mengirim teks ke orang.
- **pakai-alat-dulu** — Claude mengecek repo, skrip, dan data yang sudah ada sebelum nebak atau nyuruh lo.
- **bukti-sebelum-klaim** — Claude nggak bilang "selesai" tanpa bukti yang bisa dicek.
- **tanya-seperlunya** — Claude berhenti nanya hal yang sudah lo izinkan atau bisa dia cek sendiri.

**2. Bikin `CLAUDE.md` untuk proyek lo.**
Salin [templates/minimal-claude-md.md](templates/minimal-claude-md.md) ke folder proyek lo, beri nama `CLAUDE.md` (huruf besar semua). Isi tiga bagian wajibnya: proyeknya apa, gaya bicara, dan 1–3 aturan yang nggak boleh dilanggar. Bagian lain hapus dulu.

**3. Buka sesi baru dan coba.**
Masuk ke folder proyek lo, jalankan `claude`, ketik `/skills`. Empat nama tadi harus muncul. Setelah itu kerja seperti biasa.

**Kenapa nggak minta Claude yang pasang?** Claude Code waspada terhadap repo dari internet yang menyuruhnya menulis ke folder pengaturannya sendiri, dan itu sikap yang benar. Jadi pemasangan dilakukan lo sendiri dengan satu perintah salin yang isinya bisa lo baca.

## Kalau Claude masih ngaco

| Gejala | Baca |
|---|---|
| Boros kata, basa-basi, mengulang rencana | [docs/02-fixing-broken-claude-md.md](docs/02-fixing-broken-claude-md.md) §1 |
| Tulisannya berbau AI | skill [anti-ai-slop](skills/anti-ai-slop/SKILL.md) |
| Nyuruh lo menjalankan perintah yang bisa dia jalankan sendiri | [docs/05-default-to-execute.md](docs/05-default-to-execute.md) + skill [pakai-alat-dulu](skills/pakai-alat-dulu/SKILL.md) |
| Bilang "sudah beres" padahal belum | [docs/04-deploy-verification.md](docs/04-deploy-verification.md) + skill [bukti-sebelum-klaim](skills/bukti-sebelum-klaim/SKILL.md) |
| Kebanyakan nanya | skill [tanya-seperlunya](skills/tanya-seperlunya/SKILL.md) |
| Bikin fitur baru padahal sudah ada yang mirip | [docs/06-audit-existing-before-build.md](docs/06-audit-existing-before-build.md) |
| Menjalankan perintah yang merusak berkas | [docs/07-destructive-action-discipline.md](docs/07-destructive-action-discipline.md) |
| Aturan di `CLAUDE.md` diabaikan | [docs/02-fixing-broken-claude-md.md](docs/02-fixing-broken-claude-md.md) |

## Isi repo

```
claude-code-kit/
├── README.md       ← halaman ini
├── CLAUDE.md       ← aturan Claude saat merawat repo ini
├── docs/           ← alasan di balik tiap aturan, dengan kejadian nyatanya
├── skills/         ← skill siap pasang (satu folder = satu skill)
└── templates/      ← template CLAUDE.md (minimal dan lengkap)
```

Daftar lengkap: [docs/INDEX.md](docs/INDEX.md) dan [skills/README.md](skills/README.md). Rencana tulisan berikutnya: [ROADMAP.md](ROADMAP.md).

## Prinsip

- **Tiap aturan lahir dari kejadian nyata.** Kalau aturannya terasa abstrak, cerita kejadiannya hilang. Bilang saja.
- **Alat milik sendiri dulu.** Rekomendasi default bisa dipasang di server sendiri dan tidak mengunci lo ke satu vendor.
- **Bahasa Indonesia apa adanya.** Gaya bicara dengan pemilik proyek (gue/lo) beda dengan gaya bicara ke pengguna (aku/kamu). Dua-duanya dikunci di `CLAUDE.md`.

Kit ini bukan yang pertama untuk tiap bagiannya. Memori lintas repo, misalnya, sudah dikerjakan proyek lain (claude-mem, Virtual Monorepo Pattern dari Owen Zanzal). Yang khas di sini gabungannya: disiplin kerja, kenyataan bisnis di Indonesia, dan suara berbahasa Indonesia.

## Lisensi

MIT. Silakan pakai, ubah, dan bagikan.

## Pembuat

Ruby Perkasa — pendiri beberapa usaha perjalanan dan software di Bandung dan Labuan Bajo. Masukan lewat GitHub repo ini.

---

## English summary

Rules, templates, and ready-to-install Claude Code skills that stop Claude from guessing, inventing, over-talking, and writing like a robot. Extracted from daily use across several running businesses.

**Quickstart:** clone this repo, then from inside it install the four basic skills yourself (plain-text files, no code runs), and copy `templates/minimal-claude-md.md` into your project as `CLAUDE.md`:

```
git clone https://github.com/bismillahinspiringquotes-eng/claude-code-kit.git && cd claude-code-kit
mkdir -p ~/.claude/skills
cp -r skills/anti-ai-slop skills/pakai-alat-dulu skills/bukti-sebelum-klaim skills/tanya-seperlunya ~/.claude/skills/
```

- `anti-ai-slop` — strips AI-writing patterns before sending human-read text
- `pakai-alat-dulu` ("use your tools first") — check existing repo, scripts, data before guessing or delegating to the user
- `bukti-sebelum-klaim` ("evidence before claims") — separate built / merged / live / used; test measuring tools against known cases
- `tanya-seperlunya` ("ask only when needed") — don't ask what's already authorized or measurable; questions need a reason code + evidence

Skill bodies are in Indonesian; Claude reads them fine in any language. Docs in `docs/` are mostly English.
