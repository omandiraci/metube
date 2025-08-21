## MeTube Docker Guide

Bu rehber, `metube-docker-compose.yml` dosyasıyla MeTube’u kurma, yapılandırma ve yönetme adımlarını içerir.

### 1) Önkoşullar
- Docker ve Docker Compose kurulu olmalı
- Port 8081 kullanılabilir olmalı (ya da alternatif bir port belirlenmeli)

### 2) Dosya İçeriği
`metube-docker-compose.yml` içeriği:
```yaml
services:
  metube:
    image: ghcr.io/alexta69/metube
    container_name: metube
    restart: unless-stopped
    ports:
      - "8081:8081"
    volumes:
      - "./downloads:/downloads"
    environment:
      - PUID=1000
      - PGID=1000
      - TZ=Europe/Istanbul
```

Alanların açıklaması:
- `ports`: Host 8081 → Container 8081. Çakışma varsa host tarafını örn. `9090:8081` yapabilirsiniz.
- `volumes`: İndirilenler hosttaki `./downloads` klasöründe kalıcı saklanır.
- `PUID/PGID`: Konteyner içi süreçlerin hosttaki dosya izinleriyle uyumlu çalışması için kullanıcı/grup kimliği. macOS’ta genellikle 501:20, Linux’ta çoğunlukla 1000:1000.
- `TZ`: Zaman dilimi.

### 3) Çalıştırma
```bash
cd /Users/ozcan/Desktop/DockerVM/metube
docker compose -f metube-docker-compose.yml up -d
```

Tarayıcıdan erişim:
```
http://localhost:8081
```
Uzak sunucuda ise `http://<sunucu-ip>:8081`.

### 4) Güncelleme
```bash
docker compose -f metube-docker-compose.yml pull
docker compose -f metube-docker-compose.yml up -d
```

### 5) Durdurma / Kaldırma
```bash
docker compose -f metube-docker-compose.yml down
```

### 6) Loglar ve Sorun Giderme
- Logları izle:
```bash
docker logs -f metube
```
- Port çakışması: 8081 başka bir servis tarafından kullanılıyorsa portu değiştirin.
- İzinler (özellikle Linux):
```bash
sudo chown -R 1000:1000 ./downloads
```
macOS’ta PUID/PGID farklı olabilir; kendi kullanıcı kimliğinizi öğrenmek için:
```bash
id -u; id -g
```
ve `metube-docker-compose.yml` içinde `PUID/PGID` değerlerini buna göre güncelleyin.

### 7) Özelleştirme İpuçları
- Ters proxy (Nginx/Traefik) ile HTTPS sağlanabilir.
- Farklı bir indirilenler klasörü istiyorsanız `./downloads` yolunu değiştirin.
- Konteyner adını `container_name` ile özelleştirebilirsiniz.

### 8) Yararlı Komutlar
```bash
# Durum
docker ps

# Konteyner içine gir
docker exec -it metube sh

# Disk kullanımını kontrol et (downloads)
du -sh ./downloads
```


