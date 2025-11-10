# SWOT Analizi — OCPP Güvenliği (TLS Downgrade Senaryosu)

Bu çalışma, elektrikli araç şarj istasyonları ile merkezi sistem (CSMS) arasındaki iletişimde
oluşabilecek güvenlik açıklarını incelemek için hazırlanmıştır.  
Senaryo konusu: **TLS Downgrade (zayıflatma) saldırısı**.

---

##  Güçlü Yönler
- **OCPP standart bir protokoldür.**  
  Farklı markalara ait sistemlerin birbiriyle uyumlu çalışmasını sağlar.  

- **TLS kullanımı:**  
  Veri gizliliği ve bütünlüğü korunur.  

- **Kimlik doğrulama özelliği:**  
  İstasyon ve merkez arasında güvenli bağlantı kurulabilir.  

---

##  Zayıf Yönler
- **TLS sürüm farkı:**  
  Eski sürümler kullanılırsa güvenlik seviyesi düşebilir.  

- **Sertifika yönetimi zayıf olabilir.**  
  Süresi dolan veya yanlış yapılandırılmış sertifikalar sistem güvenliğini azaltır.  

- **Bazı istasyonlar güncellenmez.**  
  Bu da açıkların devam etmesine neden olur.  

---

##  Fırsatlar
- **Elektrikli araç kullanımı artıyor.**  
  Güvenli şarj altyapısına olan ihtiyaç da artmakta.  

- **Yeni güvenlik standartları geliyor.**  
  OCPP 2.0.1 ve TLS 1.3 gibi güncel çözümler daha güvenli iletişim sağlar.  

- **Siber güvenlik bilinci yükseliyor.**  
  Bu alanda yapılan çalışmalar destekleniyor.  

---

## ⚠️ Tehditler
- **TLS Downgrade saldırıları:**  
  Saldırgan bağlantıyı zayıf bir şifreleme seviyesine düşürebilir.  

- **Yetkisiz komut ekleme:**  
  Saldırgan OCPP mesajlarını değiştirip yeni komutlar ekleyebilir.  

- **Veri sızıntısı:**  
  Kimlik bilgileri veya token’lar çalınabilir.  

- **Ağ ortasında saldırı (MitM):**  
  Saldırgan veri trafiğini izleyip değiştirebilir.  

---

##  Sonuç
Bu analiz, **TLS Downgrade saldırılarının OCPP sistemleri için ciddi bir tehdit** olduğunu göstermektedir.  
Güvenliği artırmak için TLS 1.3 kullanımı, otomatik sertifika yenileme ve düzenli güncellemeler önerilir.

---

> 📌 **Not:** Bu analiz grup çalışması kapsamında hazırlanmıştır.
> Görseller ve diğer belgeler GitHub reposunda yer almaktadır.
