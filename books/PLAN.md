# Rencana Pembuatan Buku: Sistem Operasi (IF25-12007)

Status: **SELESAI — dieksekusi 2026-07-01.** Seluruh langkah di §5 telah dijalankan; output: `books/Sistem-Operasi/main.pdf` (167 hlm).

---

## 1. Keputusan yang Sudah Disepakati

| Aspek | Keputusan |
|---|---|
| Cakupan | Seluruh 12 `modul_XX.tex` + `handson_04/05/06.tex` (Multi-threading, Sinkronisasi, Penjadwalan) |
| Struktur | Bab terintegrasi: teori Modul + sesi lab, diikuti bagian **Latihan** dari Hands-On yang relevan (jika ada) |
| `handson_01/02/03.tex` (stub kosong) | **Dilewati** — tidak ada bagian Latihan untuk topik VirtualBox/Bash/Makefile dari file-file ini |
| Latihan untuk topik 2/4/6 (VirtualBox, Bash, Makefile) | Diambil dari `\section{Hands-On}` yang **sudah tertanam** di `modul_02/04/06.tex` (teks nyata, bukan placeholder) |
| Latihan untuk topik 8-12 (Threading, Race Condition, Mutex, Scheduling) | Dari `handson_04/05/06.tex`, dengan **placeholder dihapus**: baris `\textbf{Jawaban:}` + `\textit{[Isi jawaban di sini]}` dan kotak `\fbox{...[Tempel screenshot...]...}` dibuang; soal, tabel definisi, potongan kode, dan pertanyaan analisis tetap dipertahankan verbatim sebagai daftar soal latihan |
| Bahasa | Bahasa Indonesia, dipertahankan apa adanya |
| Judul & identitas sampul | Diusulkan oleh saya di bawah (§4) — perlu persetujuan |

## 2. Temuan Riset Kunci

Sebelum menyusun struktur, berikut fakta dari korpus yang membentuk keputusan di atas:

- **`handson_01/02/03.tex` kosong total** — 38 baris identik berisi `[Topik Pertanyaan 1]` / `TODO: ganti dengan pertanyaan sesungguhnya`. Tidak ada teks asli untuk diekstrak.
- **`handson_04/05/06.tex` adalah lembar kerja kosong**, bukan bacaan: soal, tabel, dan kutipan kode sudah lengkap, tetapi setiap sel jawaban berisi `[Isi jawaban di sini]` dan setiap slot gambar berisi kotak placeholder `[Tempel screenshot layar di sini]` (total: 4 tabel definisi di h04, ~7 tabel + 6 slot gambar di h05, ~5 tabel + 5 slot gambar di h06). Setelah placeholder dihapus, ini jadi bagian "Latihan" yang bersih.
- **Modul 2/4/6 sudah punya `\section{Hands-On}` mandiri** (baris nyata, tanpa TODO) — dipakai langsung sebagai Latihan topik tersebut. Modul 1/3/5/7/9/10/11/12 tidak punya bagian setara (11/12 justru dipetakan ke handson_04-06 secara terpisah).
- **Referensi/kutipan**: 14 entri `Referensi.bib`, seluruhnya dipakai, tidak ada yang menggantung — aman disalin utuh.
- **Label LaTeX**: 130 label unik lintas seluruh modul+handson. Hanya satu "duplikat" (`fig:h05-4-compare`), dan itu karena satu kemunculan ada di dalam blok comment (`%`) — bukan konflik nyata. Namespace aman digabung dalam satu dokumen.
- **Semua path gambar (`\includegraphics`) resolve ke file yang ada di disk** — termasuk `Figure/11/1.png` dan `Figure/12/1.png` yang di source-nya diberi komentar `% TODO: tambahkan screenshot` tapi file gambarnya **sudah ada** (komentar basi, bukan masalah nyata).
- **68 kemunculan teks hardcoded "Modul N" / "Minggu N"** dalam prosa (bukan `\ref`), terkonsentrasi di modul_11/12 (18+18) dan handson_05/06 (6+6). Karena urutan bab tetap 1→12 dan tidak diberi ulang nomor, referensi tekstual ini tetap valid tanpa perlu diedit.
- **Dua `config.sty` (Modul vs Hands-On) nyaris identik** — beda: 1 baris kosong, dan `commentstyle` (italic di Hands-On, tidak di Modul). Buku akan punya `config.sty` baru sendiri (turunan `Modul Praktikum/config.sty`), bukan pilih salah satu.
- **`\sloppy`** dipakai lokal di awal modul_11 dan modul_12 (mengatasi overfull hbox pada baris tertentu) — akan dipertahankan di bab-bab tersebut.
- Ukuran render saat ini: Modul Praktikum 33 hlm, Hands-On 11 hlm (dokumen terpisah, tidak representatif untuk total buku karena banyak modul/`\input` sedang di-comment-out).

## 3. Struktur Dokumen Buku

```
books/
└── Sistem-Operasi/
    ├── main.tex
    ├── config.sty                  # turunan Modul Praktikum/config.sty, identitas diubah
    ├── Referensi.bib               # salinan penuh dari Modul Praktikum/Referensi.bib
    ├── chapters/
    │   ├── bab_01_pengenalan-os-vm.tex
    │   ├── bab_02_instalasi-ubuntu-virtualbox.tex      (+ Latihan dari modul_02 §Hands-On)
    │   ├── bab_03_navigasi-izin-linux.tex
    │   ├── bab_04_bash-scripting.tex                    (+ Latihan dari modul_04 §Hands-On)
    │   ├── bab_05_build-gcc.tex
    │   ├── bab_06_makefile.tex                          (+ Latihan dari modul_06 §Hands-On)
    │   ├── bab_07_booting-framework-os.tex
    │   ├── bab_08_multithreading.tex                     (+ Latihan dari handson_04, placeholder dibersihkan)
    │   ├── bab_09_race-condition.tex
    │   ├── bab_10_mutex-locks.tex                         (+ Latihan dari handson_05, placeholder dibersihkan)
    │   ├── bab_11_scheduling-round-robin.tex
    │   └── bab_12_scheduling-priority.tex                 (+ Latihan dari handson_06, placeholder dibersihkan)
    └── Figure/
        ├── 01/ … 12/               # disalin dari Modul Praktikum/Figure/
        └── ifitera-header.png      # atau logo pengganti, lihat §4
```

**Catatan pemetaan Latihan → Bab:** `handson_04` (Multi-threading) berisi materi yang membahas Modul 8 secara eksplisit → digabung ke Bab 8, **bukan** dipecah ke bab tersendiri. Sama untuk `handson_05` (bahas Modul 9+10, digabung ke Bab 10 karena itu titik penutup sesi sinkronisasi) dan `handson_06` (bahas Modul 11+12, digabung ke Bab 12).

**Isi tiap bab** (pola konsisten, mengikuti urutan asli tiap `modul_XX.tex`):
1. Tujuan dan Output Praktikum
2. Dasar Teori (dengan tabel ringkasan, jika ada)
3. Alat dan Bahan
4. Langkah-Langkah Praktikum (termasuk sub-sesi, kode, gambar, tabel apa adanya)
5. **Latihan** (section baru) — hanya untuk Bab 2, 4, 6, 8, 10, 12; berisi soal dari sumber terkait dengan placeholder jawaban/screenshot dihapus

Bab 1, 3, 5, 7, 9, 11 berhenti di langkah 4 (tidak ada sumber Latihan yang valid untuk topik tersebut).

## 4. Identitas & Sampul Buku (usul, minta konfirmasi)

Karena ini karya turunan baru (bukan modul praktikum resmi ITERA), sampul dibedakan dari kedua dokumen sumber:

- **Judul**: *Sistem Operasi: Dari Teori ke Implementasi Kernel* — merefleksikan progres materi (konsep OS dasar → CLI/build tooling → implementasi kernel bare-metal: threading, sinkronisasi, scheduling).
- **Subjudul**: Disusun dari Modul Praktikum & Hands-On IF25-12007, Institut Teknologi Sumatera
- **Label sampul**: "Modul Praktikum" → diganti "Buku Ajar" (atau "Diktat"); label "Tim Pengajar & Asisten" dipertahankan (kredit asli tetap tercantum)
- **Header/footer**: `\fancyfoot` diubah dari `Modul Praktikum --- \courseName{}` → `\courseName{} --- Buku Ajar` (atau teks lain sesuai judul final)
- Warna/font/`\lstset` tetap mewarisi `config.sty` asli (tidak ada alasan mengubah tema visual)

*Jika Anda punya judul/subjudul sendiri, sebutkan — poin ini satu-satunya yang murni preferensi, bukan temuan teknis.*

## 5. Langkah Eksekusi (setelah rencana ini disetujui)

1. **Scaffold**: buat `books/Sistem-Operasi/{main.tex, config.sty, Referensi.bib, Figure/}`; salin seluruh `Figure/01`…`Figure/12` dari Modul Praktikum (~12 MB).
2. **Salin 12 bab modul** ke `chapters/bab_XX_*.tex`, ganti nomor label internal bila perlu (`chap:modul-XX` → `chap:bab-XX`) — cross-check ulang tidak ada collision baru.
3. **Bangun 6 bagian Latihan**:
   - Bab 2/4/6: potong `\section{Hands-On}` dari `modul_02/04/06.tex`, tempel sebagai `\section{Latihan}` di akhir bab yang sesuai (ganti judul section, isi soal utuh).
   - Bab 8/10/12: ambil isi `handson_04/05/06.tex`, strip baris `\textbf{Jawaban:}` + `\textit{[Isi jawaban di sini]}` dan blok `\fbox{\begin{minipage}...[Tempel screenshot...]...\end{minipage}}` (termasuk `\begin{figure}...\end{figure}` pembungkusnya jika kosong total), pertahankan soal/tabel/kode, retitle jadi `\section{Latihan}`.
4. **`main.tex`**: struktur sampul mengikuti pola asli (tikz header, tim pengajar, daftar isi), `\input` 12 bab berurutan, lalu `\bibliography{Referensi}` di akhir.
5. **Compile** dengan `latexmk -pdf -interaction=nonstopmode -halt-on-error -file-line-error` dari dalam `books/Sistem-Operasi/`.
6. **Verifikasi**: `pdfinfo` (page count wajar, estimasi ~140-160 hlm gabungan), `pdftotext` sampling tiap bab (cek caption Indonesia, tidak ada macro bocor), grep log untuk `Undefined` / `Overfull` baru yang bukan warisan dari sumber asli.
7. **Cleanup**: pastikan `.gitignore` root sudah menutup artefak build di folder baru ini (polanya global `*.aux` dll., otomatis berlaku); tidak perlu `.gitignore` baru.

## 6. Risiko / Hal yang Perlu Diperhatikan Saat Eksekusi

- Menyalin 12 bab berarti compile time jauh lebih lama dari `main.tex` asli (yang saat ini hanya meng-`\input` 2 modul aktif) — ekspektasi wajar, bukan bug.
- `\sloppy` di bab 11/12 hanya berlaku sampai akhir file (tidak ada `\fussy` penutup) — saat digabung berurutan dalam satu dokumen, efeknya akan "bocor" ke bab 12 dan berlanjut ke bibliography. Perlu ditambahkan `\fussy` di akhir bab 12 agar tidak memengaruhi halaman referensi.
- Nomor gambar/tabel akan berubah (mis. "Gambar 1.1" di Modul Praktikum standalone jadi "Gambar 11.1" di buku) — ini normal untuk `report` class per-chapter numbering, tidak perlu tindakan khusus.
