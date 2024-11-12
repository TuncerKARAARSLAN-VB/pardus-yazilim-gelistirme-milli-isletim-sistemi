# Pardus Otomatik Güncelleme

Pardus üzerinde tüm yazılımları belirli periyotlarla otomatik olarak güncellemek için bir shell scripti yazabiliriz. Bu script, sistemdeki paketlerin güncellenmesi, yeni paketlerin yüklenmesi veya eski paketlerin kaldırılması gibi işlemleri gerçekleştirecek ve belirli bir periyotla çalışacak. Bu işlemleri cron ile zamanlayarak belirli aralıklarla çalışmasını sağlayabiliriz.

## 1. **Güncelleme Yapacak Shell Scripti**

Aşağıdaki shell scripti, Pardus üzerinde tüm yazılımları güncelleyecek şekilde yazılabilir.

### 1.1: **Shell Scripti (update_system.sh)**

```bash
#!/bin/bash

# Güncel paket listelerini indir
echo "Paket listeleri güncelleniyor..."
sudo apt update

# Güncel olmayan paketleri güncelle
echo "Yazılımlar güncelleniyor..."
sudo apt upgrade -y

# Kullanılmayan paketleri temizle
echo "Kullanılmayan paketler temizleniyor..."
sudo apt autoremove -y

# Paketler için gerekli temizlik işlemi
echo "Paket önbelleği temizleniyor..."
sudo apt clean

echo "Güncelleme tamamlandı."
```

Bu script, şu işlemleri sırasıyla yapacaktır:

1. Paket listelerini günceller (`sudo apt update`).
2. Güncel olmayan paketleri günceller (`sudo apt upgrade -y`).
3. Kullanılmayan paketleri kaldırır (`sudo apt autoremove -y`).
4. Paket önbelleğini temizler (`sudo apt clean`).

## 2. **Scripti Zamanlamak için Cron Kullanma**

Scripti belirli bir periyotla çalıştırmak için **cron** kullanılabilir. Örneğin, her gün saat 3:00'te çalışmasını sağlamak için aşağıdaki adımları izleyebilirsiniz.

### 2.1: **Cron Job Oluşturma**

1. Terminali açın ve crontab düzenleyicisini açın:

   ```bash
   crontab -e
   ```

2. Ardından, scriptin her gün saat 3:00'te çalışması için aşağıdaki satırı ekleyin:

   ```bash
   0 3 * * * /bin/bash /path/to/update_system.sh >> /var/log/update_system.log 2>&1
   ```

   - Bu satır, her gün saat 3:00'te `/path/to/update_system.sh` scriptini çalıştıracak ve çıktıyı `/var/log/update_system.log` dosyasına kaydedecektir.
   - `>>` işareti çıktı ekleme işlemi yaparken, `2>&1` tüm hataları da aynı log dosyasına yönlendirir.

### 2.2: **Cron Job ile Hata Loglama**

Cron joblarının düzgün çalışıp çalışmadığını kontrol etmek için scriptin çıktılarını ve hatalarını log dosyasına yazdırabilirsiniz. Bu, sistemdeki güncellemelerle ilgili bir hata oluştuğunda size bilgi verir.

## 3. **Scripti Çalıştırılabilir Yapma**

Scriptin çalışabilmesi için önce çalıştırılabilir olması gerekiyor. Bunun için script dosyasına uygun izinler verilmelidir:

```bash
chmod +x /path/to/update_system.sh
```

Bu adımla script çalıştırılabilir hale gelir.

## 4. **Sonuç**

Bu shell scripti ve cron job ile Pardus üzerindeki yazılımlarınızı otomatik olarak güncelleyebilir, kullanılmayan paketleri temizleyebilir ve sisteminizin güncel kalmasını sağlayabilirsiniz.