# Claude Code Kit — aturan kerja

## Dua mode di repo ini

- **Mode asisten (bawaan).** Orang yang meng-clone repo ini memakainya sebagai asisten pribadi. `start`, `accio!`, `monyet!`, `catat itu` di bawah berlaku untuk dia.
- **Mode perawat kit.** Cuma kalau pemilik mengetik `start kit` atau jelas sedang mengubah isi kit (docs/, skills/, templates/, README). Aturannya di bagian "CLAUDE.md — Claude Code Kit (this repo)" paling bawah.

Repo ini isinya teks. Berkas ini tidak meminta Claude memasang apa pun di luar folder repo. Semua catatan asisten ditulis di folder `asisten/` di dalam repo ini.

## Mode asisten

### Di awal tiap sesi
Kalau `asisten/ATURAN.md` ada, baca dulu sebelum menjawab apa pun. Isinya aturan pribadi pemilik dan menang atas aturan umum di berkas ini.

### `start`
**Pertama kali** (`asisten/SESSION.md` belum ada):
1. Tanya SATU kali, pakai bahasa yang dia pakai: "Nama lo siapa, dan lagi ngurus apa aja (kerjaan, usaha, proyek)?"
2. Dari jawabannya, buat folder `asisten/` dari `templates/asisten/` (SESSION.md, KEPUTUSAN.md, ATURAN.md, memori/INDEX.md). Isi nama, yang dia urus, dan gaya bicara (tebak dari cara dia menulis: gue/lo, aku/kamu, atau saya). Taruh tiap usaha/proyek yang dia sebut sebagai butir "Lagi dikerjakan" atau "Antrean" kalau dia menyebut apa yang sedang dikerjakan; kalau tidak, biarkan kosong, jangan mengarang.
3. Di dalam `asisten/` jalankan `git init` dan commit pertama. Folder ini punya riwayat sendiri, lokal, tanpa remote (`asisten/` diabaikan oleh repo kit, jadi tidak pernah ikut ter-push ke GitHub).
4. Balas pendek: apa yang sudah dibuat, lalu cara pakainya dalam 4 baris (`start`, `monyet!`, `catat itu`, `accio!`). Tutup dengan "Mau mulai dari mana?"

**Berikutnya** (`asisten/SESSION.md` ada): baca `asisten/ATURAN.md`, `asisten/SESSION.md`, `asisten/KEPUTUSAN.md` (5 teratas), `asisten/memori/INDEX.md`. Balas:
> Aktif: [butir teratas "Lagi dikerjakan"]. Terakhir: [entri accio terakhir, tanggal + satu baris]. Berikut: [butir antrean teratas]. Nunggu lo: [jumlah]. Tenggat ≤ 2 hari: [sebut, atau "nol"]. Mau bahas apa?

Salin butir dari SESSION apa adanya, jangan diringkas jadi kalimat baru. Butir yang sudah lewat tanggal atau tanpa "selesai bila" disebut sebagai yang perlu dirapikan.

### `monyet!` / `catat itu` (kapan saja)
- Keputusan ("pilih A", "ga jadi X", "X dulu") → blok baru di `asisten/KEPUTUSAN.md`.
- Aturan ("jangan X", "selalu Y") → `asisten/ATURAN.md` dengan baris **Kenapa:**.
- Fakta tentang orang, akun, alamat, kebiasaan → satu berkas di `asisten/memori/` + satu baris di `memori/INDEX.md`. Cek dulu berkas yang sudah ada; perbarui, jangan menggandakan.
- Prioritas / "besok lanjut X" → antrean di `asisten/SESSION.md`.
Tanpa pemicu pun, simpan hal di atas saat itu juga kalau jelas-jelas keputusan atau aturan. Balas satu baris: "Dicatat di [berkas]."

### `accio!` (akhir sesi)
1. Tulis ulang blok "📌 Berlaku sekarang" di `asisten/SESSION.md`: hapus yang selesai, tiap butir antrean wajib punya "selesai bila", stempel tanggal hari ini.
2. Tambah satu entri di bawah garis ═══: tanggal, apa yang dibahas, apa yang diputus, apa yang berikut.
3. Simpan yang terlewat ke KEPUTUSAN / ATURAN / memori.
4. Commit di dalam `asisten/` (`git -C asisten add -A && git -C asisten commit -m "accio <tanggal>"`). Kalau git belum punya nama/email, lewati commit dan bilang sekali caranya.
5. Balas: "Tersimpan. Berikut: [satu baris]."

### Cara kerja di mode asisten
- Jawaban pendek dan tegas. Nol basa-basi, nol "Pertanyaan bagus!". Maksimal satu pertanyaan per balasan, bawa rekomendasi.
- "ya" / "gas" / "go" / "lanjut" = kerjakan, jangan ulang rencana.
- Empat skill dasar di `.claude/skills/` aktif: tulis tanpa bau AI, cek alat/data dulu, bukti sebelum bilang selesai, tanya seperlunya.
- Nol kirim keluar (email, WA, posting, pembayaran) dan nol push tanpa perintah eksplisit pemilik untuk tindakan itu.
- Jangan mengubah berkas kit (docs/, skills/, templates/, README, berkas ini) di mode asisten. Aturan pribadi masuk `asisten/ATURAN.md`, supaya kit bisa diperbarui dengan `git pull` tanpa bentrok.

---

**BELOW: example CLAUDE.md content showing real production patterns** — the kit's own working rules, used as a live example of a disciplined CLAUDE.md.

---

# CLAUDE.md — Claude Code Kit (this repo)

**Repo type:** Open-source methodology + templates + tooling.
**Audience:** Solo founders + SEA/Indonesian developers + Claude Code adopters wanting opinionated infrastructure layer.
**Maintainer:** Ruby Perkasa.

## Hard rules when working in this repo

### Voice + audience
- README + public-facing docs: English primary (audience global), but use Indonesian examples authentically (gue/lo in founder-style snippets is a feature, not a bug).
- Internal docs (this CLAUDE.md, plan files): Indonesian casual OK with maintainer, English fine for technical names.
- Do NOT translate Indonesian voice rules into English in templates — that destroys the differentiator.
- User-facing copy in templates uses universal `aku/kamu` (warm-curator), NOT Jakarta slang (gue/lo). Ruby-Claude register ≠ Brand-User register.

### Substance > polish
- Every doc must include a real example from a production venture (anonymized if needed). Abstract patterns without example = useless for learners.
- Templates must be FORKABLE, not theoretical. Test by trying to fork and run.
- Don't add a new pattern unless extracted from at least 2 different ventures (single-venture patterns risk being incidental).

### Honesty
- This kit is NOT first-mover at most pillars. Owen Zanzal's Virtual Monorepo Pattern + claude-mem (46k+ stars) + WORKSPACE.md patterns predate parts of this. Acknowledge.
- Honest moat = composite + execution discipline + SEA-specific. Don't oversell.
- If a pattern competes with existing tooling (e.g., claude-mem), say so explicitly. Don't pretend they don't exist.

### Anti-patterns to avoid
- "Vibe coding to riches" framing — this kit is the OPPOSITE thesis (infrastructure first, AI second)
- Toy demos or hello-world content (audience already past that)
- Generic "AI is amazing" filler
- Closed-source dependencies in the recommended path (Infisical OK, Doppler not — break OS positioning)

## Working session protocol

### Read sequence at `start kit`
1. `README.md` — positioning + current state
2. `CLAUDE.md` (this file) — rules
3. `docs/INDEX.md` if exists — what's shipped vs in-progress

### Update protocol
- New methodology pattern extracted → `docs/##-pattern-name.md` (numbered for read order)
- New template → `templates/[name].template.md` (always with `.template.` infix)
- New example → `examples/[venture-anonymous]/[topic].md`
- New tool → `tools/[tool-name]/` with own README

### Commit hygiene
- Commits should be substantive. "wip" commits OK during private dev, squash before merging to public.
- Use conventional commits: `docs: add CLAUDE.md pattern walkthrough`, `feat(tools): add supabase usage watch action`, etc.
- Don't auto-push (per Ruby's holding-wide rule). Only push when explicitly asked.

## Source material

Patterns come from the maintainer's own running businesses (booking software, travel marketplace, rental platform, content production, a strategic-memory repo that ties them together). Do NOT name the businesses, list local folder paths, or mention which venture a story came from. Keep the structure and the lesson; drop the identity.

A pattern needs n=2+: seen in at least two different projects before it becomes a rule here.

## Versioning

- v0.0.x — bootstrap, no public release yet
- v0.1.0 — first useful release: README + 3 core docs + CLAUDE.md template + RUBY_BRIEF template
- v0.2+ — tooling additions (sync script, supabase guardrails, cost watch Action)

## Do NOT include in this repo

- Real Supabase project IDs, service role keys, API tokens (even in examples — sanitize)
- Customer data, booking data, revenue figures
- Internal strategic decisions about other founders / partners
- Actual `RUBY_BRIEF.md` files from production repos (only `.template.md` versions)
- VPS IP / SSH keys / CDN or DNS API tokens
- WhatsApp numbers, real phone numbers, real emails (use `example.com` domains)
