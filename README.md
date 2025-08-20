## MeTube (Docker) – Hızlı Başlangıç

YouTube, Vimeo vb. platformlardan video/ses indirmek için web arayüzü sağlayan MeTube’u bu klasördeki Docker Compose tanımıyla kolayca çalıştırabilirsiniz.

### Özellikler
- Web arayüz: 8081 portu
- İndirme klasörü: `./downloads` (host makinenizde kalıcı)
- Kullanıcı/Grup kimliği: `PUID=1000`, `PGID=1000`
- Zaman dilimi: `Europe/Istanbul`

### Gereksinimler
- Docker
- Docker Compose

### Kurulum ve Çalıştırma
1. Dizine gidin:
   ```bash
   cd /Users/user1/Desktop/DockerVM/metube
    # Burada sizin kullanıcı yolunuzu belirtin.
   ```
2. Konteyneri başlatın:
   ```bash
   docker compose -f metube-docker-compose.yml up -d
   ```
3. Tarayıcıdan erişin:
   ```
   http://localhost:8081
   ```
   Uzak bir sunucu kullanıyorsanız `localhost` yerine sunucunun IP’sini kullanın.

### Klasör Yapısı
- `metube-docker-compose.yml`: MeTube servis tanımı
- `downloads/`: İndirilen dosyaların saklandığı klasör (host tarafında)

### Güncelleme
```bash
docker compose -f metube-docker-compose.yml pull
docker compose -f metube-docker-compose.yml up -d
```

### Sorun Giderme
- Logları görüntüleyin:
  ```bash
  docker logs metube
  ```
- İzin hataları (Linux): `downloads` klasörünün MeTube tarafından yazılabilir olduğundan emin olun. Gerekirse:
  ```bash
  sudo chown -R 1000:1000 ./downloads
  ```

### Güvenlik/Notlar
- İsteğe bağlı olarak bir ters proxy (Nginx/Traefik) arkasında çalıştırabilirsiniz.
- 8081 portu başka bir servis tarafından kullanılıyorsa `metube-docker-compose.yml` içindeki port eşlemesini değiştirin.

Daha ayrıntılı adımlar ve özelleştirme için `guide.md` dosyasına bakın.
