🌙 Cloudflare Worker Auto-Deployment Guide
<p align="center"> <b>Dark Theme · Dual Language · 2 Columns</b> </p>
<style> td { vertical-align: top; padding: 10px; } table { width: 100%; } </style> <table> <tr> <td width="50%">
🇺🇸 English Version
🚀 Overview

Automated deployment system for Cloudflare Worker using GitHub Actions.
Supports:

Auto route creation

Multi-domain worker sharding

Timeout-safe deployment

⚠️ Issue: Cloudflare 504 Timeout

Large route lists in a single Worker may trigger:

504 Timeout – Cloudflare API took too long to respond


We use two deployment strategies to avoid this.

🔍 Strategy Comparison
Strategy	Description	Best for
Legacy (Single Worker)	All routes in one Worker	Small projects (<50 routes)
Sharded (Multi-Worker)	1 Worker per domain	Large projects (100+ domains)
📦 Required Files
File	Purpose
worker.js	Worker script
customdomain.txt	Subdomain prefixes
main_domains.txt	Domain list
[Deploy Injektor].yml	Legacy workflow
deploy_chunked.yml	Sharded workflow
🧰 Legacy Mode (Single Worker)

Workflow: [Deploy Injektor].yml

Inputs:

worker_name

main_domain

Cloudflare API token & account ID

Flow:

Load main domain

Load subdomain prefixes

Build one large route list

Deploy single Worker

🧩 Sharded Mode (Multi Worker)

Workflow: deploy_chunked.yml

Inputs:

Cloudflare API token

Cloudflare account ID

Flow:

Each domain → handled individually

Auto name Worker for each domain

Build routes using prefixes

Deploy sequentially

20s cooldown to avoid 504 errors

▶️ Running Deployment

Open GitHub Actions

Select workflow

Fill Cloudflare credentials

Deploy 🎉

📌 Done!

Need English-only version? Diagram? Flowchart? Ask anytime.

</td> <td width="50%" style="border-left:1px solid #333">
🇮🇩 Versi Indonesia
🚀 Ringkasan

Sistem deployment otomatis untuk Cloudflare Worker menggunakan GitHub Actions.
Mendukung:

Pembuatan rute otomatis

Multi-domain worker sharding

Deploy aman tanpa timeout

⚠️ Masalah: Cloudflare 504 Timeout

Terlalu banyak rute dalam satu Worker dapat menyebabkan:

504 Timeout – API Cloudflare terlalu lama merespons


Ada dua strategi deployment sebagai solusi.

🔍 Perbandingan Strategi
Strategi	Penjelasan	Cocok Untuk
Legacy (Single Worker)	Semua rute digabung	Proyek kecil (<50 rute)
Sharded (Multi-Worker)	1 domain = 1 Worker	Proyek besar (100+ domain)
📦 File Yang Dibutuhkan
File	Fungsi
worker.js	Script Worker
customdomain.txt	Prefix subdomain
main_domains.txt	Daftar domain
[Deploy Injektor].yml	Workflow Legacy
deploy_chunked.yml	Workflow Sharded
🧰 Mode Legacy (Single Worker)

Workflow: [Deploy Injektor].yml

Input:

worker_name

main_domain

API token & account ID

Alur:

Membaca domain utama

Memuat prefix subdomain

Menyatukan semua rute

Deploy 1 Worker

🧩 Mode Sharded (Multi Worker)

Workflow: deploy_chunked.yml

Input:

API token

Cloudflare account ID

Alur:

Setiap domain → Worker terpisah

Nama Worker dibuat otomatis

Rute dibuat dari prefix

Deploy satu per satu

Jeda 20 detik untuk menghindari 504

▶️ Menjalankan Deployment

Buka GitHub Actions

Pilih workflow

Isi kredensial Cloudflare

Deploy otomatis 🎉

📌 Selesai!

Ingin versi lebih aesthetic, bergaya card, atau full dark-mode HTML/CSS? Cukup bilang!

</td> </tr> </table>
