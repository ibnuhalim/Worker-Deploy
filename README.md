🚀 Cloudflare Worker Auto-Deployment Guide
GitHub Actions · Zero-Timeout · Multi-Domain Ready
Panduan Deploy Worker Otomatis · Anti Timeout · Multi Domain
🌐 ENGLISH VERSION
⭐ Overview

This guide explains two automated deployment strategies for Cloudflare Worker using GitHub Actions:

Fully automated Worker deployment

Automatic domain & subdomain routing

Timeout-safe deployment up to hundreds of domains

⚠️ Common Problem: Cloudflare 504 Timeout

Deploying many routes in one Worker often triggers:

504 Timeout — Cloudflare API took too long to respond


To avoid this, two deployment approaches are provided.

🔍 Strategy Comparison
Strategy	Description	Best For
1. Legacy – Single Worker	All routes inside one Worker	Small projects (<50 routes)
2. Sharded – Multi Worker	One Worker per domain	Large projects, 100+ domains
📦 Required Repository Files
File	Description	Used By
worker.js	Worker script	Both
customdomain.txt	Subdomain prefixes	Both
main_domains.txt	Domain list	Sharded
[Deploy Injektor].yml	Legacy deployment	Legacy
deploy_chunked.yml	Multi-worker sharding	Sharded
🧰 Strategy 1 — Legacy (Single Worker)

Workflow: [Deploy Injektor].yml

Required Inputs

worker_name

main_domain

Cloudflare API Token + Account ID

How It Works

Loads main domain

Reads prefixes from customdomain.txt

Generates a single large route list

Deploys one Worker

Not recommended for large-scale projects due to timeout risk.

🧩 Strategy 2 — Multi-Worker Sharding (Recommended)

Workflow: deploy_chunked.yml

Required Inputs

cloudflare_account_id

cloudflare_api_token

How It Works
Step	Description	Purpose
Loader	Reads domain list	Determine workers
Generator	Creates one Worker per domain	Avoid overload
Route Builder	Combines prefixes from customdomain.txt	Full automation
Serial Deploy	Deploys workers one-by-one	Prevent conflicts
Cooldown	Waits 20s between deploys	Prevent API 504

This method can handle 100–500+ domains without issues.

▶️ Running the Deployment

Open Actions tab

Select workflow:

Legacy → [Deploy Injektor]

Sharded → Deploy Chunked Multi-Domain

Click Run workflow

Enter Cloudflare credentials

Done 🎉

🇮🇩 VERSI BAHASA INDONESIA
⭐ Ringkasan

Panduan ini menjelaskan dua metode deployment Worker Cloudflare secara otomatis menggunakan GitHub Actions:

Deployment otomatis Worker

Pembuatan rute domain/subdomain otomatis

Deploy stabil tanpa risiko 504 Timeout

⚠️ Masalah Umum: Timeout 504 Cloudflare

Jika terlalu banyak rute digabung dalam satu Worker, API Cloudflare gagal memproses dan muncul:

504 Timeout — Permintaan API terlalu lama diproses


Solusinya: gunakan metode sesuai kebutuhan proyek.

🔍 Perbandingan Strategi
Strategi	Deskripsi	Cocok Untuk
1. Legacy – Single Worker	Semua rute dijadikan satu Worker	Proyek kecil (<50 rute)
2. Sharded – Multi Worker	Satu domain = satu Worker	Proyek besar (100+ domain)
📦 File Wajib di Repository
File	Fungsi	Digunakan Oleh
worker.js	Script Worker	Semua
customdomain.txt	Daftar prefix subdomain	Semua
main_domains.txt	Daftar domain utama	Sharded
[Deploy Injektor].yml	Workflow metode Legacy	Legacy
deploy_chunked.yml	Workflow multi-worker	Sharded
🧰 Strategi 1 — Legacy (Single Worker)

Workflow: [Deploy Injektor].yml

Input yang Dibutuhkan

worker_name

main_domain

API Token & Account ID Cloudflare

Cara Kerja

Membaca domain utama

Mengambil prefix dari customdomain.txt

Menggabungkan semua rute

Deploy 1 Worker untuk semua rute

Tidak cocok untuk jumlah domain besar (rawan timeout).

🧩 Strategi 2 — Multi-Worker Sharding (Direkomendasikan)

Workflow: deploy_chunked.yml

Input yang Dibutuhkan

cloudflare_account_id

cloudflare_api_token

Cara Kerja
Tahap	Fungsi	Tujuan
Loader	Membaca semua domain	Menentukan Worker
Generator	Membuat Worker per domain	Menghindari beban berlebih
Route Builder	Menggabungkan prefix subdomain	Otomatis
Deploy Serial	Deploy satu per satu	Anti bentrok API
Cooldown	Tunggu 20 detik	Cegah Timeout 504

Aman digunakan untuk ratusan hingga ribuan rute.

▶️ Cara Menjalankan Workflow

Buka tab Actions

Pilih workflow:

Legacy → [Deploy Injektor]

Sharded → Deploy Chunked Multi-Domain

Klik Run workflow

Masukkan kredensial Cloudflare

Deployment berjalan otomatis 🎉
