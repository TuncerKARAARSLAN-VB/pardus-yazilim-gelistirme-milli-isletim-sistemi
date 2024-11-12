# Docker Üzerine Pardus Server Kurmak

Pardus server'ı Docker üzerinde çalıştırmak için aşağıdaki adımları izleyebilirsiniz. Docker, uygulamaları ve hizmetleri izole bir ortamda çalıştırmayı sağlayan popüler bir platformdur, bu da Pardus gibi bir işletim sistemini container (kapsayıcı) olarak çalıştırmayı mümkün kılar.

## 1. **Docker Kurulumu**

Eğer Docker, sisteminize yüklü değilse, önce Docker'ı kurmanız gerekir. Aşağıdaki adımlarla Docker'ı kurabilirsiniz.

### 1.1: Docker Kurulumu

Pardus üzerinde Docker kurulumunu yapmak için şu komutları kullanabilirsiniz:

```bash
sudo apt update
sudo apt install -y docker.io
```

Kurulum tamamlandıktan sonra Docker servisini başlatın ve otomatik başlatmayı etkinleştirin:

```bash
sudo systemctl start docker
sudo systemctl enable docker
```

Docker'ın doğru şekilde çalıştığını doğrulamak için aşağıdaki komutu kullanarak versiyonunu kontrol edebilirsiniz:

```bash
docker --version
```

## 2. **Pardus için Docker Image'ı Çekme**

Pardus, Debian tabanlı bir dağıtım olduğu için, Docker Hub üzerinde resmi olarak yayınlanmış bir Pardus imajı bulunmamaktadır. Ancak, Debian imajını kullanarak bir Pardus server ortamı oluşturabilirsiniz. Bu, Pardus'un temel paketlerini ve yapılandırmalarını içerecek şekilde özelleştirilebilir.

### 2.1: **Debian İmajı ile Pardus Kurulumu**

1. **Debian imajını çekmek**:

```bash
docker pull debian:latest
```

2. **Debian container'ını çalıştırın**:

```bash
docker run -it --name pardus_server debian:latest bash
```

Bu komut, en son Debian imajını indirir ve bir container içinde başlatır. Şimdi, Pardus ile uyumlu yazılımları ve paketleri Debian üzerinde yükleyerek Pardus server'ı oluşturabilirsiniz.

## 3. **Pardus Spesifik Paketlerini Yükleme**

Pardus, Debian tabanlı olduğu için, Pardus’un spesifik araçlarını ve paketlerini yüklemek için Debian paketlerini kullanabilirsiniz. Ancak, Pardus'ta özel yapılandırmalar veya paketler varsa, onları kendiniz eklemeniz gerekebilir. Örneğin, Pardus deposundan bazı paketleri kurarak sisteminizi Pardus’a benzer hale getirebilirsiniz.

Örnek olarak, Pardus’a özgü bazı yazılımlar veya yapılandırmalar şunlar olabilir:

1. **Pardus Reposunu Ekleyin** (Eğer mevcutsa):
   Eğer özel bir paket deposu varsa, Debian container'ınıza Pardus’un depolarını ekleyebilirsiniz:

```bash
echo "deb http://deb.pardus.org.tr/pardus/ p7 main" >> /etc/apt/sources.list
```

2. **Pardus paketlerini yükleyin**:

```bash
sudo apt update
sudo apt install pardus-package-name
```

## 4. **Docker Container’ı Kalıcı Hale Getirme**

Docker container'ı kapattığınızda, yapılan değişiklikler kaybolur. Ancak, bir Docker volume kullanarak container’ı kalıcı hale getirebilirsiniz:

```bash
docker run -it -v /path/to/local/storage:/path/in/container debian:latest bash
```

Bu komut, container içindeki belirli bir yolu yerel makinenizdeki bir dizine bağlar, böylece veriler kalıcı olur.

## 5. **Pardus Server için Özelleştirilmiş Dockerfile**

Eğer sürekli olarak bu tür bir ortamı kullanmak istiyorsanız, özel bir `Dockerfile` oluşturabilirsiniz. Örneğin:

```dockerfile
# Debian temel imajını kullan
FROM debian:latest

# Pardus reposunu ekleyin ve gerekli paketleri yükleyin
RUN echo "deb http://deb.pardus.org.tr/pardus/ p7 main" >> /etc/apt/sources.list \
    && apt update \
    && apt install -y pardus-package-name

# Container başladığında bash ile başlasın
CMD ["bash"]
```

Bu dosyayı `Dockerfile` olarak kaydedin ve ardından container'ı inşa edin:

```bash
docker build -t pardus_server .
```

Sonrasında container’ı başlatabilirsiniz:

```bash
docker run -it pardus_server
```

## 6. **Sonuç**

Pardus server'ı Docker üzerinde çalıştırmak için Debian imajını kullanarak özelleştirilmiş bir container oluşturabilirsiniz. Bu, Pardus’a özgü yazılımlar ve yapılandırmalar eklenerek tamamen çalışabilir bir ortam haline getirilebilir. Docker'ın sağladığı izolasyon ve taşınabilirlik özellikleri sayesinde, bu ortamı kolayca farklı sistemlere taşıyabilir ve yönetebilirsiniz.
