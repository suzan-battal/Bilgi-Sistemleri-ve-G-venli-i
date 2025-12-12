# 🔐 TLS Downgrade Senaryosu – OCPP Güvenlik

## Rapor: Senaryo — TLS Zayıflatma (Downgrade) ile Yetkisiz OCPP Komut Enjeksiyonu  
**Senaryo Başlığı:** TLS Downgrade → Kimlik Bilgisi Sızması → Yetkisiz OCPP Komut Enjeksiyonu

---

## 🧾 Özet

Bu senaryoda saldırgan, şarj istasyonu (CS) ile merkezi sistem (CSMS) arasındaki  
**TLS bağlantısını zayıf bir şifreleme algoritmasına veya eski TLS sürümüne zorlayarak**  
(**downgrade**) ya da **Man-in-the-Middle (MitM)** saldırısı gerçekleştirerek iletişimi  
dinler veya değiştirir.

Bu durum sonucunda;

- kimlik bilgileri  
- tokenlar  
- OCPP mesajları  

ele geçirilebilir veya manipüle edilebilir.

**Sonuçlar:**
- Yetkisiz `RemoteStartTransaction / RemoteStopTransaction`
- Sahte veya bozulmuş `MeterValues`
- Hatalı faturalandırma
- Akıllı şarj (Smart Charging) kararlarının bozulması

---

## 1️⃣ Başlangıç Durumu

- Şarj istasyonu (CS) ile merkezi sistem (CSMS) arasında **TLS üzerinden OCPP trafiği** kuruludur.
- Normal koşullarda:
  - En az **TLS 1.2**
  - Sertifika doğrulaması
  - Tercihen **Mutual TLS (mTLS)** beklenir.

---

## 2️⃣ Anomali Oluşumu

- Saldırgan ağ üzerinde **MitM** konumuna geçer:
  - ARP Spoofing
  - Sahte Wi-Fi erişim noktası
  - DNS zehirleme (DNS poisoning)

- TLS el sıkışması (handshake) sırasında:
  - Zayıf cipher suite
  - Eski TLS sürümü (TLS 1.0 / 1.1)

  kullanılmaya zorlanır.

- Böylece saldırgan:
  - Trafiği okuyabilir
  - Kimlik bilgilerini elde edebilir
  - OCPP mesajlarını değiştirebilir

---

## 3️⃣ Saldırı Akışı / Sömürü

- Ele geçirilen bağlantı üzerinden saldırgan:
  - Yetkisiz `RemoteStartTransaction`
  - Yetkisiz `RemoteStopTransaction`
  - Sahte `MeterValues` mesajları
  gönderebilir.

- Alternatif olarak:
  - Firmware / OTA saldırıları için anahtar bilgileri çalınabilir.

**Etkiler:**
- Ücretsiz şarj
- Çift veya yanlış faturalandırma
- Yanlış raporlama
- Hizmet kesintisi (DoS)

---

## 4️⃣ Algılama Mantığı (Detection Logic)

Aşağıdaki göstergeler anomali sinyali olarak değerlendirilir:

- Kullanılan TLS sürümü minimum gereksinimin altında mı?
  - (Örn: TLS 1.0 / TLS 1.1)
- Negotiate edilen cipher zayıf mı?
- Karşı taraf sertifika parmak izi (fingerprint) değişmiş mi?
- OCPP mesaj imzaları / nonce değerleri tutarsız mı?
- `MeterValues` ile smart meter verileri arasında fark var mı?

---

## 5️⃣ Karar ve Tepki Mekanizması

Zayıf TLS veya sertifika anomalisi tespit edildiğinde:

1. Şüpheli TLS oturumu sonlandırılır.
2. Bu oturum üzerinden gelen tüm OCPP komutları reddedilir.
3. Güvenlik ekibine alarm ve detaylı log gönderilir.
4. Etkilenen şarj oturumları için:
   - Manuel doğrulama başlatılır
   - Faturalama / settlement geçici olarak bloke edilir

---

## 6️⃣ Log Örneği

```text
2025-11-02T09:18:42Z | StationID: ST-271 | event=TLS_NEGOTIATION
negotiated_tls=TLS1.0-RSA | expected_min=TLS1.2
peer_cert_fp=AB:1C:...
action=TERMINATE_SESSION
2025-11-02T09:18:45Z | StationID: ST-271
event=OCPP_CMD_REJECTED
cmd=RemoteStartTransaction
reason=TLS_NOT_TRUSTED
anomaly=OCPP_CMD_DROPPED

