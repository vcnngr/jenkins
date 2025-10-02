# VCNNGR Minideb

Minimal Debian-based container image ottimizzata per i container VCNNGR.

## 🎯 Caratteristiche

- **Base**: Debian 12 (Bookworm) Slim
- **Size**: ~100MB (vs ~120MB Bitnami Minideb)
- **Utilities**: Preinstallati tool essenziali
- **Security**: Aggiornamenti automatici di sicurezza
- **Locale**: UTF-8 configurato
- **User**: Supporto gosu per non-root users

## 📦 Pacchetti Preinstallati

### Essenziali
- `ca-certificates` - Certificati SSL/TLS
- `curl`, `wget` - Download utilities
- `gnupg`, `dirmngr` - GPG keys management
- `openssl` - SSL/TLS library

### System Utilities
- `procps` - Process utilities (ps, top, etc.)
- `gosu` - Execution as different user
- `less`, `vim-tiny` - Text utilities

### Compression
- `bzip2`, `gzip`, `xz-utils`, `unzip`

### Network
- `netcat-openbsd` - Network debugging

### Build Tools (Minimal)
- `gcc`, `g++`, `make` - Compilation tools

## 🚀 Utilizzo

### Come Base Image

```dockerfile
FROM vcnngr/minideb:bookworm

# Installa pacchetti con utility preinstallata
RUN install_packages \
    apache2 \
    php8.2 \
    mariadb-server

# Il resto della configurazione...
```

### Build Locale

```bash
cd docker/minideb
docker build -t vcnngr/minideb:bookworm .
```

### Tag e Push

```bash
# Tag con versione
docker tag vcnngr/minideb:bookworm vcnngr/minideb:12
docker tag vcnngr/minideb:bookworm vcnngr/minideb:latest

# Push su Docker Hub
docker push vcnngr/minideb:bookworm
docker push vcnngr/minideb:12
docker push vcnngr/minideb:latest
```

## 🔧 install_packages Utility

Script intelligente per installazione pacchetti:

```bash
# Installa uno o più pacchetti
install_packages nginx php-fpm mysql-client

# Caratteristiche:
# - Update automatico cache se necessario
# - Opzioni ottimizzate (no-install-recommends)
# - Pulizia automatica post-installazione
# - Gestione errori
```

## 📊 Confronto con Bitnami Minideb

| Caratteristica | VCNNGR Minideb | Bitnami Minideb |
|----------------|----------------|-----------------|
| Base | Debian 12 Slim | Debian 12 Slim |
| Size | ~100MB | ~120MB |
| gosu | ✅ | ✅ |
| install_packages | ✅ | ✅ |
| Build tools | ✅ Minimal | ❌ |
| Customizzabile | ✅ Full | ⚠️ Limited |
| Branding | VCNNGR | Bitnami |

## 🔒 Sicurezza

- Updates automatici durante build
- Minimal attack surface (pochi pacchetti)
- Non-root user support via gosu
- CA certificates aggiornati
- Locale sicuro (UTF-8)

## 📝 Versioning

```
vcnngr/minideb:bookworm    # Debian 12
vcnngr/minideb:12          # Alias Debian 12
vcnngr/minideb:latest      # Latest stable
```

## 🔄 Updates

Per aggiornare la base image:

```bash
# Build con no-cache per forzare apt update
docker build --no-cache -t vcnngr/minideb:bookworm .
```

## 🤝 Utilizzo in Altri Container VCNNGR

Tutti i container VCNNGR dovrebbero usare questa base:

```dockerfile
FROM vcnngr/minideb:bookworm AS builder
# ... build stage ...

FROM vcnngr/minideb:bookworm AS final
# ... final stage ...
```

## 📚 Best Practices

1. **Usa sempre install_packages** invece di apt-get diretto
2. **Multi-stage builds** per ridurre dimensioni finali
3. **Pulisci sempre** dopo installazioni (già fatto da install_packages)
4. **User 1001:0** per compatibilità OpenShift
5. **Verifica gosu** prima di usarlo: `gosu nobody true`

## 🐛 Troubleshooting

### Pacchetto non trovato
```bash
# Forza update cache
apt-get update
install_packages tuo-pacchetto
```

### Permessi negati
```bash
# Assicurati di eseguire come root durante build
USER root
RUN install_packages ...
```

### Size troppo grande
```bash
# Verifica cosa occupa spazio
docker history vcnngr/minideb:bookworm
```

---

**Maintainer**: vcnngr  
**License**: MIT  
**Source**: https://github.com/vcnngr/wordpress-mariadb-stack
