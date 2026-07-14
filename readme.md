# DVWA + BunkerWeb WAF — Docker Compose Setup

## Arsitektur

```
Internet / Browser
       │
       ▼
┌──────────────────────────────────────────┐
│  BunkerWeb (WAF + Reverse Proxy)         │  ← port 80 / 443
│  - ModSecurity + OWASP CRS              │
│  - Bad behavior detection               │
│  - Rate limiting                        │
│  - Bot detection                        │
│  - Web UI dashboard                     │
└────────────┬─────────────────────────────┘
             │ bw-services network
             ▼
┌──────────────────────────────────────────┐
│  DVWA                                    │  ← tidak expose ke host
│  (Damn Vulnerable Web Application)       │
└────────────┬─────────────────────────────┘
             │ dvwa-internal network
             ▼
┌──────────────────────────────────────────┐
│  MariaDB (DVWA database)                 │
└──────────────────────────────────────────┘

Komponen BunkerWeb:
  bunkerweb    ← Nginx + ModSec (traffic masuk)
  bw-scheduler ← Config manager
  bw-ui        ← Web dashboard
  bw-db        ← Database BunkerWeb (MariaDB)
```

## Cara Pakai

### 1. Jalankan stack
```bash
docker compose up -d
```

### 2. Buka Web UI BunkerWeb
Buka browser → **http://localhost/changeme**

Ikuti setup wizard:
- Buat akun admin
- Konfigurasi reverse proxy (skip dulu, bisa dari UI nanti)

### 3. Tambahkan DVWA sebagai service di BunkerWeb UI
Setelah wizard selesai, masuk ke menu **Services** → **New Service**:

| Field | Value |
|-------|-------|
| Server name | `localhost` |
| Use reverse proxy | ✅ Yes |
| Reverse proxy URL | `/` |
| Reverse proxy host | `http://dvwa:80` |

Klik **Save** → BunkerWeb otomatis reload config.

### 4. Akses DVWA
Buka browser → **http://localhost**

Login: `admin` / `password` → klik **Create / Reset Database**

---

## Port & Akses

| Service | URL | Keterangan |
|---------|-----|------------|
| DVWA (via WAF) | http://localhost | Traffic diproteksi BunkerWeb |
| BunkerWeb UI | http://localhost/changeme | Dashboard manajemen WAF |

---

## Fitur BunkerWeb yang Aktif

| Fitur | Status |
|-------|--------|
| ModSecurity WAF | ✅ On |
| OWASP CRS Rules | ✅ On |
| Bad Behavior Detection | ✅ On |
| Rate Limiting | ✅ On |
| Bot Protection | ✅ On |
| DNSBL | ❌ Off (lab lokal) |

---

## Tips Lab

**Lihat log BunkerWeb secara live:**
```bash
docker logs -f bunkerweb
docker logs -f bw-scheduler
```

**Cek status semua container:**
```bash
docker ps --format "table {{.Names}}\t{{.Status}}\t{{.Ports}}"
```

**Hentikan & bersihkan:**
```bash
# Hentikan saja
docker compose down

# Hentikan + hapus semua data (reset total)
docker compose down -v
```
