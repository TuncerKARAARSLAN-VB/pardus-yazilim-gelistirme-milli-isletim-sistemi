# Pardus Üzerine Nginx Kurmak

Pardus üzerine Nginx kurmak için aşağıdaki adımları izleyebilirsiniz. Nginx, güçlü bir web sunucusu ve ters proxy sunucusu olarak yaygın olarak kullanılmaktadır. Pardus, Debian tabanlı olduğu için, Debian ve Ubuntu üzerindeki kurulum adımları da geçerlidir.

## 1. **Paket Depolarını Güncelleme**

Öncelikle sistemdeki paket listelerinin güncellenmesi gerekir:

```bash
sudo apt update
```

## 2. **Nginx Kurulumu**

Nginx'i yüklemek için aşağıdaki komutu kullanabilirsiniz:

```bash
sudo apt install nginx
```

Bu komut, Nginx ve gerekli bağımlılıkları yükleyecektir.

## 3. **Nginx'in Başlatılması**

Kurulum tamamlandığında, Nginx servisinin başlatılması gerekir:

```bash
sudo systemctl start nginx
```

Ayrıca, Nginx'in sistem açılışında otomatik olarak başlamasını sağlamak için şu komutu kullanabilirsiniz:

```bash
sudo systemctl enable nginx
```

## 4. **Nginx'in Durumunu Kontrol Etme**

Nginx'in düzgün çalışıp çalışmadığını kontrol etmek için aşağıdaki komutu kullanabilirsiniz:

```bash
sudo systemctl status nginx
```

Bu komut, Nginx'in çalışıp çalışmadığını ve durumu hakkında bilgi verecektir.

## 5. **Firewall Ayarlarını Yapılandırma (Opsiyonel)**

Eğer firewall (UFW veya başka bir güvenlik duvarı aracı) kullanıyorsanız, Nginx'in web trafiğine izin vermek için ilgili portları açmanız gerekebilir. HTTP ve HTTPS trafiği için şu komutları kullanabilirsiniz:

```bash
sudo ufw allow 'Nginx HTTP'
sudo ufw allow 'Nginx HTTPS'
```

Firewall'ı kontrol etmek için şu komutu kullanabilirsiniz:

```bash
sudo ufw status
```

## 6. **Nginx'in Test Edilmesi**

Kurulumdan sonra, Nginx'in düzgün çalışıp çalıştığını web tarayıcınızdan kontrol edebilirsiniz. Sunucunuzun IP adresine veya "localhost" adresine gidin:

```
http://localhost
```

Eğer her şey doğru çalışıyorsa, Nginx'in varsayılan hoş geldiniz sayfasını görmelisiniz.

## 7. **Nginx Yapılandırma Dosyaları**

Nginx yapılandırma dosyası genellikle `/etc/nginx/nginx.conf` konumundadır. Ayrıca, her web sitesi için yapılandırma dosyaları `/etc/nginx/sites-available/` dizininde bulunur ve etkinleştirilen dosyalar `/etc/nginx/sites-enabled/` dizininde yer alır.

## 8. **Nginx'i Yeniden Başlatma**

Yapılandırma dosyalarında yapılan değişikliklerin etkili olabilmesi için Nginx servisini yeniden başlatmak gerekmektedir:

```bash
sudo systemctl restart nginx
```

## Sonuç

Pardus üzerinde Nginx kurmak için bu adımları izleyebilirsiniz. Bu kurulum, web sunucunuzu çalıştırmaya ve konfigüre etmeye başlamak için yeterli olacaktır. Eğer ek ayarlar veya özel yapılandırmalar yapmanız gerekirse, `/etc/nginx/nginx.conf` dosyasını ve sanal ana bilgisayar dosyalarını düzenleyebilirsiniz.
