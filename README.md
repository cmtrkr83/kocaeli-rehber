# Kurum ve Yönetici Rehberi

Okul/kurum yöneticilerine tek dokunuşla ulaşmak için mobil öncelikli bir rehber. Excel'den veri yükler, cihazda saklar ve kurulabilir bir PWA olarak çalışır.

## Veri gizliliği

Bu depoda **örnek kayıt bulunmaz**. Rehber verisi yalnızca kullanıcının cihazında (`localStorage`) tutulur ve sunucuya gönderilmez. Veri yüklemek için uygulamadaki **Ayarlar → Excel yükle** akışı kullanılır.

## Özellikler

- Excel (`.xlsx`, `.xls`, `.csv`) yükleme, ilçe filtresi, arama
- Sabit/cep telefonu için `tel:` bağlantıları, WhatsApp kısayolu
- PWA: ana ekrana ekleme, tam ekran (`standalone`), çevrimdışı açılış
- "Gömülü HTML indir" ile veriyi içeren tek dosyalık kopya

## Docker / Portainer

Depoyu sunucuda `/opt/kocaeli-rehber` altına klonlayın:

```bash
sudo mkdir -p /opt/kocaeli-rehber
sudo chown "$USER":"$USER" /opt/kocaeli-rehber
git clone https://github.com/cmtrkr83/kocaeli-rehber.git /opt/kocaeli-rehber
```

Portainer'da **Stacks → Add stack** ile `deploy/docker-compose.yml` içeriğini kullanın veya doğrudan:

```bash
docker compose -f /opt/kocaeli-rehber/deploy/docker-compose.yml up -d
```

Uygulama `http://SUNUCU_IP:8088` adresinde açılır. Nginx statik dosyaları bind mount ile okur; güncelleme için `git pull` yeterlidir, container'ı yeniden başlatmak gerekmez.

> Not: `deploy/docker-compose.yml` içindeki `/opt/kocaeli-rehber` yolu, depoyu farklı bir dizine klonlarsanız güncellenmelidir.

## PWA kurulumu

- **iPhone:** Safari ile açın → Paylaş → Ana Ekrana Ekle. Yalnızca Safari'nin ana ekran kısayolu tam ekran (`standalone`) açılır.
- **Android:** HTTPS adresi üzerinden Chrome'un kurulum istemini kullanın.
- Service worker HTTPS (veya localhost) gerektirir; düz HTTP LAN adresinde çevrimdışı önbellek çalışmaz.

## zrok

HTTPS yayını için sunucudaki zrok kurulumu kullanılabilir:

```bash
zrok reserve public --unique-name rehber41 http://127.0.0.1:8088
zrok share reserved rehber41 --headless
```

## Statik dosyalar

- `index.html` — uygulama (gömülü veri boş)
- `manifest.webmanifest` — PWA tanımı
- `sw.js` — çevrimdışı önbellek (statik dosyalar değişince `CACHE` sürümünü artırın)
- `icons/` — uygulama ikonları
- `deploy/` — nginx ve compose yapılandırmaları
