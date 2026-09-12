# Belajar DevOps: Web App dengan CI/CD Pipeline

Proyek ini adalah implementasi pipeline deployment otomatis dari nol, dibangun sepenuhnya dari perangkat mobile (Android) sebagai latihan fundamental Cloud & DevOps.

## Live Demo
🔗 https://zpyrous.my.id

## Arsitektur

```
Developer (Mobile) → GitHub → GitHub Actions → VPS (Docker) → Cloudflare Tunnel → Public
```

1. Kode diedit dan di-push ke GitHub dari perangkat mobile
2. GitHub Actions otomatis trigger saat push ke branch `main`
3. Actions melakukan SSH ke VPS, pull kode terbaru, build ulang Docker image
4. Container berjalan di VPS (port 8080), tidak diekspos langsung ke publik
5. Cloudflare Tunnel menghubungkan domain custom ke container, tanpa membuka port VPS ke internet — IP asli server tersembunyi
6. Tunnel berjalan sebagai systemd service, aktif permanen meski server di-restart

## Tech Stack

- **Infrastructure**: VPS (Ubuntu 22.04 LTS)
- **Containerization**: Docker
- **CI/CD**: GitHub Actions
- **Networking & Security**: UFW (firewall), Cloudflare Tunnel (zero exposed ports)
- **Web Server**: Nginx (dalam container)

## Keamanan

- Server tidak memiliki port yang terekspos langsung ke publik (semua trafik lewat Cloudflare Tunnel)
- Firewall (UFW) membatasi akses hanya ke port yang diperlukan
- Kredensial deployment (SSH key) disimpan sebagai GitHub Secrets, tidak pernah muncul di kode

## Cara Kerja CI/CD

File konfigurasi: [`.github/workflows/deploy.yml`](.github/workflows/deploy.yml)

Setiap push ke branch `main` akan otomatis:
1. Connect ke VPS via SSH
2. Pull perubahan kode terbaru
3. Rebuild Docker image
4. Restart container dengan versi terbaru

Tidak ada langkah manual — dari commit sampai live, seluruhnya otomatis.

## Yang Dipelajari

- Provisioning dan pengelolaan VPS dari command line
- Containerization dengan Docker
- Membangun pipeline CI/CD dari nol
- Konsep zero-trust networking dengan Cloudflare Tunnel
- Dasar hardening server (firewall, service management)
