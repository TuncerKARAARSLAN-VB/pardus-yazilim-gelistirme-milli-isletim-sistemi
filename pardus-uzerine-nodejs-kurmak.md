# Pardus Üzerine NodeJS Kurmak 

Pardus üzerine Node.js kurmak için aşağıdaki adımları izleyebilirsiniz. Node.js, JavaScript ile sunucu tarafı uygulamalar geliştirebilmenizi sağlayan popüler bir platformdur. Pardus, Debian tabanlı bir dağıtım olduğu için, Debian veya Ubuntu üzerine yapılacak kurulum adımları aynıdır.

## 1. **Paket Depolarını Güncelleme**

İlk olarak, sistemdeki paket listelerini güncellemek için şu komutu çalıştırın:

```bash
sudo apt update
```

## 2. **Node.js'i Kurma**

Node.js'in en güncel sürümünü yüklemek için, öncelikle NodeSource'un resmi deposunu eklemeniz gerekir. Bu şekilde, en son kararlı sürümü elde edebilirsiniz.

### 2.1: **NodeSource Repository Ekleme**

Node.js'in Debian ve Ubuntu için resmi deposunu eklemek için aşağıdaki komutu çalıştırın:

```bash
curl -sL https://deb.nodesource.com/setup_16.x | sudo -E bash -
```

Burada `16.x`, Node.js'in 16.x sürümünü belirtir. Farklı bir sürüm kullanmak isterseniz, `16.x` yerine uygun sürümü yazabilirsiniz (örneğin, `18.x`).

### 2.2: **Node.js Kurulumu**

Depoyu ekledikten sonra, Node.js'i yüklemek için aşağıdaki komutu çalıştırabilirsiniz:

```bash
sudo apt install -y nodejs
```

Bu komut Node.js'i ve npm (Node Package Manager) paket yöneticisini kurar.

## 3. **Kurulumun Doğrulanması**

Kurulumun başarılı olup olmadığını kontrol etmek için şu komutları kullanarak Node.js ve npm sürümlerini kontrol edebilirsiniz:

```bash
node -v
npm -v
```

Bu komutlar, yüklü olan Node.js ve npm sürümlerini gösterir.

## 4. **Ekstra Paketler (Opsiyonel)**

Node.js ile bazı ek araçlar ve paketler yüklemek gerekebilir. Örneğin, geliştirme için `build-essential` paketini yüklemek isteyebilirsiniz:

```bash
sudo apt install -y build-essential
```

Bu paket, Node.js modüllerini derlemek için gereken bazı temel araçları sağlar.

## 5. **Node.js ile Basit Bir Uygulama Çalıştırma**

Node.js'in kurulumunu test etmek için basit bir uygulama yazabilirsiniz. Örneğin:

1. Bir dosya oluşturun (`app.js`):

```javascript
// app.js
console.log("Node.js çalışıyor!");
```

2. Bu dosyayı çalıştırın:

```bash
node app.js
```

Ekranda şu çıktıyı görmelisiniz:

```
Node.js çalışıyor!
```

## Sonuç

Bu adımlarla Pardus üzerine Node.js kurulumunu tamamlayabilirsiniz. Node.js'i kullanarak sunucu tarafı JavaScript uygulamaları geliştirebilir, npm ile çeşitli paketleri yönetebilirsiniz.