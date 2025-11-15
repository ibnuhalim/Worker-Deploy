☁️ Panduan Deployment Cloudflare Worker

Halo! Dokumen ini adalah panduan lengkap Anda untuk mendeploy Worker Cloudflare menggunakan GitHub Actions. Kami punya dua strategi, pilih yang paling cocok untuk skala proyek Anda!

🎯 Tujuan Utama

Mengotomatisasi pembuatan rute domain dan deployment Worker.

🤯 Tantangan Besar: API Timeout (Kesalahan 504)

Saat Worker memiliki terlalu banyak rute (domain + sub-domain), API Cloudflare sering mengalami Timeout. Kami menawarkan dua solusi untuk masalah ini:

Strategi

Deskripsi

Kapan Menggunakan

A. Single Worker (Legacy)

Semua rute ke SATU Worker 💥.

Jika total rute Anda SANGAT SEDIKIT (kurang dari 50). Risiko timeout tinggi.

B. Multi-Worker Sharding

1 Domain Utama = 1 Worker Unik (Terpisah) ✅.

DIREKOMENDASIKAN! Solusi stabil untuk mencegah timeout dan mengelola banyak domain.

🛠️ Persiapan File (Wajib)

Pastikan file-file ini ada di akar (root) repositori Anda:

File

Deskripsi

Diperlukan untuk

worker.js

Kode inti Worker Anda.

A & B

customdomain.txt

Daftar prefix sub-domain (contoh: api, blog).

A & B

main_domains.txt

Daftar domain utama (hanya untuk Strategi B).

B

deploy_chunked.yml

Workflow untuk Strategi B (Sharding).

B

[Deploy Injektor].yml

Workflow untuk Strategi A (Legacy).

A

⚙️ Detail Strategi A: Deployment Worker Tunggal

(File: [Deploy Injektor].yml)

Ini adalah cara tradisional, cocok untuk proyek kecil yang rutenya tidak akan bertambah.

📝 Input yang Anda Masukkan:

worker_name: Nama Worker.

main_domain: Domain utama tunggal (misalnya, nzr2805.my.id).

cloudflare_account_id / cloudflare_api_token: Kredensial akun.

➡️ Proses:

Workflow mengambil main_domain dari input dan semua prefix dari customdomain.txt.

Semua rute digabung menjadi satu wrangler.toml.

Satu Worker dideploy dengan semua rute.

🚀 Detail Strategi B: Multi-Worker Sharding (The Pro Way) 

(File: deploy_chunked.yml)

Solusi terbaik untuk skalabilitas dan keandalan!

📝 Input (Hanya Kredensial!):

cloudflare_account_id / cloudflare_api_token: Kredensial akun.

Input nama Worker dan domain utama Dihapus karena prosesnya otomatis.

🛠️ Logika Otomatis yang Keren:

Langkah

Deskripsi

Tujuannya

1. Pembagian (Chunking)

Membagi main_domains.txt menjadi 1 domain per bagian.

Setiap panggilan API hanya memproses 1 domain, mengurangi beban.

2. Penamaan Otomatis

Worker diberi nama dari kata pertama domain. Contoh: blueivy.qzz.io $\to$ Worker blueivy.

Worker unik untuk setiap domain.

3. Deployment Serial

Setiap Worker dideploy satu per satu.

Memastikan tidak ada tabrakan API.

4. Jeda Aman

Ada jeda 20 detik (sleep 20) di antara setiap deployment.

Memberi waktu pemulihan API Cloudflare untuk mencegah 504 Timeout.

Dengan Strategi B, Anda bisa mendeploy ratusan domain dengan percaya diri!

🏃 Cara Menjalankan

Buka tab Actions di repo GitHub Anda.

Pilih workflow yang ingin Anda jalankan ([Deploy Injektor] atau Deploy Chunked Multi-Domain Worker).

Klik "Run workflow" dan isi kredensial yang diminta.
