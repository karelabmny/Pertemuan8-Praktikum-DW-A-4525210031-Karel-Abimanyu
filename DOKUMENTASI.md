# Dokumentasi Tugas Individu: Form Registrasi Event Techno IT

**Nama:** Karel Abimanyu Ahpandi | **NIM:** 4525210031 | **Kelas:** A
**Mata Kuliah:** Desain Web | **Halaman:** `index.html`, `css/style.css`, `js/validasi.js`

## 1. Kontrol Form dan Aturan Validasi

| Kontrol | Field | Aturan Validasi |
|---|---|---|
| Teks | `nama` | wajib, 3-60 karakter, huruf dan spasi saja |
| Email | `email` | wajib, format email valid |
| Teks | `telepon` | wajib, diawali 08, 10-13 digit angka |
| Select | `sesi` | wajib dipilih |
| Radio | `tiket` | wajib pilih satu |
| Checkbox | `topik[]` | minimal satu dicentang |
| Textarea | `harapan` | wajib, 30-300 karakter |
| Upload | `bukti` | wajib, PDF/JPG/PNG, maks. 2 MB |
| Checkbox | `persetujuan` | wajib dicentang |

## 2. Test Case Validasi

Hasil aktual diperoleh dengan menjalankan `index.html` dan `validasi.js` di jsdom (simulasi DOM di Node.js), bukan di browser sungguhan. Pengujian di browser tetap perlu dilakukan untuk screenshot.

| No | Field | Input Uji | Hasil yang Diharapkan | Hasil Aktual | Status |
|---|---|---|---|---|---|
| 1 | Semua | Data valid lengkap | Tidak ada error, banner sukses | Tidak ada error, banner sukses | Lolos |
| 2 | nama | (kosong) | "Nama wajib diisi." | "Nama wajib diisi." | Lolos |
| 3 | nama | `Budi123` | Ditolak, hanya huruf | "Nama 3-60 karakter, hanya huruf dan spasi." | Lolos |
| 4 | email | `saipul@` | Ditolak, format salah | "Format email tidak valid (contoh: nama@domain.com)." | Lolos |
| 5 | telepon | `0812abc` | Ditolak, ada huruf | "Nomor harus diawali 08 dan berisi 10-13 digit angka." | Lolos |
| 6 | telepon | `12345` | Ditolak, tidak diawali 08 | "Nomor harus diawali 08 dan berisi 10-13 digit angka." | Lolos |
| 7 | sesi | Tidak dipilih | Ditolak | "Pilih salah satu sesi event." | Lolos |
| 8 | tiket | Tidak dipilih | Ditolak | "Pilih jenis tiket." | Lolos |
| 9 | topik | Tidak ada yang dicentang | Ditolak | "Pilih minimal satu topik." | Lolos |
| 10 | harapan | `Mau belajar` (11 karakter) | Ditolak, kurang dari 30 | "Harapan minimal 30 karakter (saat ini 11)." | Lolos |
| 11 | harapan | 301 karakter | Ditolak, lebih dari 300 | "Harapan maksimal 300 karakter (saat ini 301)." | Lolos |
| 12 | bukti | `virus.exe` | Ditolak, format salah | "Format file harus PDF, JPG, atau PNG." | Lolos |
| 13 | bukti | `ktm.pdf` 3 MB | Ditolak, melebihi 2 MB | "Ukuran file maksimal 2 MB." | Lolos |
| 14 | bukti | Tidak diunggah | Ditolak | "Bukti identitas wajib diunggah." | Lolos |
| 15 | persetujuan | Tidak dicentang | Ditolak | "Anda harus menyetujui penggunaan data." | Lolos |

Skenario input salah: 14 (nomor 2-15), melebihi syarat minimal lima.

## 3. Validasi yang Tetap Wajib di Server

Validasi di browser hanya membantu pengguna. Penyerang dapat mengirim request langsung (curl, Postman, DevTools) tanpa melewati JavaScript, sehingga semua aturan harus diulang di server:

1. **Semua aturan tabel 1** (wajib, panjang, pola, format email, nomor telepon), karena atribut HTML dan JavaScript bisa dihapus atau dimodifikasi.
2. **Nilai select dan radio harus berada dalam daftar yang diizinkan** (`pagi/siang/online`, `mahasiswa/umum/vip`). Nilai lain bisa dikirim manual.
3. **File upload**: ekstensi dan MIME type dicek dari isi file (bukan dari nama), ukuran dibatasi di server (`upload_max_filesize` atau setara), nama file diganti dengan nama acak, dan file disimpan di luar folder yang bisa dieksekusi. Pengecekan `accept` dan `size` di browser mudah dilewati.
4. **Escape dan parameterized query** untuk mencegah XSS dan SQL injection pada nama dan harapan.
5. **Aturan yang hanya bisa dicek server**: email atau nomor telepon sudah terdaftar, kuota sesi masih tersedia, perlindungan CSRF, dan pembatasan jumlah request.

## 4. Catatan Pembagian Peran

Tugas ini individu, sehingga seluruh pekerjaan dikerjakan sendiri dan tidak ada pembagian peran.

## 5. Ringkasan (150-250 kata)

Halaman awal berupa form lamaran magang saya ubah menjadi form registrasi event Techno IT. Perubahan utama: field posisi dan sistem kerja diganti menjadi sesi event (select) dan jenis tiket (radio), kemampuan diganti topik yang diminati (checkbox), motivasi diganti harapan (textarea), serta upload CV dan portofolio diganti satu upload bukti identitas. Saya menambahkan js/validasi.js dan gaya pesan error di style.css. Tampilan hanya diubah pada warna aksen ke ungu bertema Omen (Valorant), latar tetap putih.

Masalah pertama, pesan bawaan browser berbeda-beda dan tidak bisa diatur, sehingga form diberi atribut novalidate dan semua pesan ditampilkan oleh JavaScript di bawah tiap field. Masalah kedua, atribut HTML5 tidak bisa memeriksa ukuran dan ekstensi file, jadi dua hal itu dicek lewat JavaScript. Masalah ketiga, saat pengujian otomatis ditemukan bug: akses form.harapan gagal, sehingga saya memakai form.elements.

Solusinya ada sembilan aturan validasi: wajib diisi, panjang minimal dan maksimal, pola huruf pada nama, format email, pola nomor telepon, pilihan wajib pada radio, minimal satu checkbox, format dan ukuran file, serta persetujuan. Lima belas skenario diuji, dan semuanya menghasilkan pesan yang sesuai harapan. Validasi di browser mudah dilewati, sehingga server tetap wajib memeriksa ulang semua aturan.
