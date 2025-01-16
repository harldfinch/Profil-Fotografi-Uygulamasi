# Profil Fotoğrafı Yükleme Uygulamasındaki Güvenlik Açığı

Bu proje, basit bir profil fotoğrafı yükleme uygulamasındaki bir güvenlik açığını ve bu açığın nasıl manipüle edilebileceğini göstermek amacıyla oluşturulmuştur. Projede, bir PHP uygulaması üzerinden dosya uzantılarına dayalı güvenlik zafiyetinin nasıl kullanılabileceği açıklanmıştır.

---

## 📜 Proje Hakkında
### Sorunun Tanımı
Uygulama, yüklenen dosyanın türünü yalnızca dosya adının uzantısına bakarak doğrulamakta ve gerçek dosya türünü kontrol etmemektedir. Bu durum, kötü amaçlı dosyaların yanlış uzantılarla sisteme yüklenmesine olanak tanımaktadır.

Örnek olarak:
- `malicious.jpeg` adlı bir dosya içinde zararlı bir **PHP kodu** barındırılarak uygulamaya yüklenmiştir.
- Bu dosya gerçek bir **PHP dosyası** olmasına rağmen, `.jpeg` uzantısı ile uygulama tarafından kabul edilmiştir.
- Yükleme tamamlandıktan sonra, dosyanın içerdiği kötü amaçlı kod çalıştırılarak sistem manipüle edilmiştir.

---

## 🔍 Güvenlik Açığının Manipüle Edilmesi
Videoda gösterildiği gibi:
1. `malicious.jpeg` adında bir dosya oluşturulmuştur.
   - Bu dosya içerisine aşağıdaki gibi bir PHP kodu yerleştirilmiştir:
     ```php
     <?php
     echo "<script>alert('Sistem manipüle edildi!');</script>";
     ?>
     ```
2. Uygulama, uzantıya dayalı doğrulama yaptığı için dosya kabul edilmiştir.
3. Yüklenen dosya çalıştırıldığında zararlı kod yürütülmüş ve bir **JavaScript alert** penceresi gösterilmiştir.

---

## 📂 Proje Yapısı
Bu repository aşağıdaki içerikleri barındırmaktadır:

```
/profil-fotograf-yukleme-acigi
├── src/               # Kaynak kodlar
│   ├── Uygulama.py     # Dosya yükleme işlemini yöneten ana python dosyası
├── video/             # Güvenlik açığını gösteren video
│   └── Guvensiz_Uygulama.mp4
└── README.md          # Proje açıklama dosyası
```

---

## 💡 Çözüm Önerileri
Bu tür güvenlik açıklarını önlemek için aşağıdaki yöntemler önerilmektedir:

1. **Dosya Türü Kontrolü:**
   - Sadece dosya uzantısına güvenmek yerine, dosyanın **MIME türü** veya **içeriği** doğrulanmalıdır.
   - Örneğin, Python’da `mimetypes` veya `magic` modülleri kullanılabilir.

2. **Sunucu Taraflı Doğrulama:**
   - Yüklenen dosyaların sadece belirli türlerde (örneğin JPEG veya PNG) olduğunu doğrulayın.
   - İzin verilen dosya türlerini bir whitelist ile tanımlayın.

3. **Güvenli Depolama:**
   - Yüklenen dosyaları web erişimine açık bir dizinde saklamayın.
   - Dosyaları çalıştırılamayacak şekilde depolayın.

4. **İçerik Tarama ve Filtreleme:**
   - Yüklenen dosyaları tarayarak potansiyel tehlikeli kodlar veya içeriklerden arındırın.
