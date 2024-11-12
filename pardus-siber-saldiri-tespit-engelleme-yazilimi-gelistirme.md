# Pardus üzerinde siber güvenlik sağlayan, ip yasaklayan yazılım geliştirme

Pardus üzerinde ağ saldırılarını tespit etmek ve iptables üzerinden yasaklama yapmak için bir servis yazılımı geliştirebilirsiniz. Bu yazılım, ağ trafiğini izleyerek anormal davranışlar (örneğin, çok sayıda başarısız oturum açma denemesi, belirli IP'lerden gelen anormal trafik vb.) tespit edebilir ve tespit edilen IP adreslerini `iptables` ile engelleyebilir. Aşağıda bu süreci adım adım açıklayan bir çözüm önerisi bulunmaktadır.

## 1. **Gerekli Araçlar ve Kütüphaneler**

İlk olarak, gerekli ağ izleme ve güvenlik araçlarını yüklemeniz gerekir:

- **iptables**: Linux firewall'ını yönetmek için kullanılır.
- **Python**: Bu servisi yazacağımız dil.
- **Subprocess**: Python'dan komut satırına erişim sağlamak için.

```bash
sudo apt update
sudo apt install iptables python3 python3-pip
sudo apt install tcpdump
```

## 2. **Ağ Trafiğini İzleme ve Saldırı Tespiti**

Ağ trafiğini izlemek için **tcpdump** kullanabiliriz. `tcpdump`, ağda meydana gelen trafiği izler ve paketler hakkında bilgi verir. Anormal bir durum veya saldırı tespit ettiğimizde, bu IP'yi yasaklayacağız.

Bir örnek olarak, TCP SYN Flood veya çok fazla başarısız SSH girişimi gibi saldırıları tespit edebiliriz.

### 2.1: **TCP Saldırılarını Tespit Etme**

Örneğin, SYN flood saldırılarını izlemek için `tcpdump` kullanarak ağ trafiğini izleyebiliriz:

```bash
sudo tcpdump -i eth0 'tcp[13] & 2 != 0'
```

Bu komut, gelen TCP SYN paketlerini izler. Eğer çok fazla SYN paketi bir kaynaktan gelirse, bu potansiyel bir SYN flood saldırısını işaret edebilir.

### 2.2: **SSH Başarısız Giriş Denemeleri**

SSH giriş denemelerini izlemek için `auth.log` dosyasını kontrol edebiliriz. Eğer bir IP çok sayıda başarısız giriş denemesi yaparsa, bu IP'yi yasaklayabiliriz.

```bash
sudo tail -f /var/log/auth.log | grep "Failed password"
```

Bu komut, başarısız SSH oturum açma girişimlerini izler. Çok sayıda başarısız deneme yapan IP'yi yakalayacağız.

## 3. **Yasaklı IP'yi iptables ile Engelleme**

Tespit ettiğimiz saldırgan IP'yi engellemek için `iptables` komutlarını kullanabiliriz.

### 3.1: **Python ile iptables Komutlarını Çalıştırma**

Aşağıda, Python ile tespit edilen IP'yi yasaklayan bir script örneği verilmiştir:

```python
import subprocess

def block_ip(ip):
    # iptables ile IP engelleme
    subprocess.run(['sudo', 'iptables', '-A', 'INPUT', '-s', ip, '-j', 'DROP'])
    print(f"{ip} adresi engellendi.")

def detect_failed_ssh():
    # auth.log dosyasındaki başarısız SSH girişlerini kontrol et
    with open('/var/log/auth.log', 'r') as file:
        lines = file.readlines()
        failed_ips = {}
        
        for line in lines:
            if "Failed password" in line:
                ip = line.split()[-4]  # IP adresi genellikle 4. sıradadır
                if ip in failed_ips:
                    failed_ips[ip] += 1
                else:
                    failed_ips[ip] = 1

        # 5'ten fazla başarısız giriş yapan IP'leri engelle
        for ip, count in failed_ips.items():
            if count > 5:
                block_ip(ip)

if __name__ == "__main__":
    detect_failed_ssh()
```

Bu betik, `/var/log/auth.log` dosyasını okuyarak başarısız SSH giriş denemelerini tespit eder ve 5'ten fazla başarısız denemesi olan IP'leri `iptables` kullanarak yasaklar.

## 4. **Servis Yazılımı Olarak Çalıştırma**

Python betiğini sürekli olarak çalıştırmak için, bu betiği bir systemd servisine dönüştürebiliriz. Böylece servis her sistem açıldığında otomatik olarak çalışacaktır.

### 4.1: **Systemd Servis Dosyası Oluşturma**

Örneğin, `ssh_attack_detector.service` adında bir servis dosyası oluşturabilirsiniz.

```bash
sudo nano /etc/systemd/system/ssh_attack_detector.service
```

Servis dosyasının içeriği şu şekilde olabilir:

```ini
[Unit]
Description=SSH Attack Detector Service
After=network.target

[Service]
ExecStart=/usr/bin/python3 /path/to/your/script.py
Restart=always
User=root

[Install]
WantedBy=multi-user.target
```

Burada, `/path/to/your/script.py` kısmını betiğinizin gerçek yoluyla değiştirin. Bu servis dosyasını kaydedin ve ardından servisi etkinleştirin.

### 4.2: **Servisi Başlatma ve Etkinleştirme**

Servisi başlatmak ve sistem başladığında otomatik çalışması için etkinleştirmek için şu komutları kullanabilirsiniz:

```bash
sudo systemctl daemon-reload
sudo systemctl start ssh_attack_detector.service
sudo systemctl enable ssh_attack_detector.service
```

## 5. **Loglama ve İzleme**

Servisiniz çalışırken, tespit edilen saldırgan IP'leri ve yapılan yasaklamaları log dosyasına kaydedebilirsiniz.

### 5.1: **Python ile Log Kaydetme**

```python
def log_attack(ip):
    with open('/var/log/ssh_attack_log.txt', 'a') as log_file:
        log_file.write(f"{ip} adresi yasaklandı.\n")

# Block IP fonksiyonuna ekle
def block_ip(ip):
    subprocess.run(['sudo', 'iptables', '-A', 'INPUT', '-s', ip, '-j', 'DROP'])
    print(f"{ip} adresi engellendi.")
    log_attack(ip)
```

Bu şekilde tespit edilen IP'lerin loglarını `/var/log/ssh_attack_log.txt` dosyasına kaydedebilirsiniz.

## Sonuç

Bu adımları takip ederek, Pardus üzerinde ağ saldırılarını tespit eden ve iptables üzerinden yasaklayan bir servis yazılımı geliştirmiş olursunuz. Bu yazılım, SSH saldırıları gibi yaygın saldırıları tespit eder ve otomatik olarak firewall kuralları ekleyerek ağınızı korur.
