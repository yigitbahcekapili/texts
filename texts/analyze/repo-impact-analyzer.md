# Repo Impact Analyzer -- Özet Doküman

## 🎯 Problem

Bir feature veya property kaldırılmak istendiğinde:

-   Hangi repolarda kullanıldığı bilinmiyor\
-   Manuel arama gerekiyor (zaman kaybı + hata riski)\
-   Sistem genelinde etki analizi yapılamıyor

------------------------------------------------------------------------

## 💡 Çözüm

Bitbucket üzerinden erişilebilen bir araç ile:

-   Kullanıcı input verir (örneğin kaldırılacak property / feature)
-   Tüm repo'lar otomatik taranır\
-   İlgili kullanımlar bulunur\
-   Analiz edilerek etki raporu ve aksiyon planı oluşturulur

------------------------------------------------------------------------

## ⚙️ Teknik Akış

1.  **Tetikleme**

Kullanıcı Bitbucket üzerinde:

-   Bir buton (örneğin: "Impact Analyze")
-   veya bir input alanı (modal / panel)

aracılığıyla analiz başlatır:

remove LegacyField from Order

------------------------------------------------------------------------

2.  **Webhook / API**

-   Bitbucket → Backend servise istek gönderir

------------------------------------------------------------------------

3.  **Job Oluşturma**

-   İstek queue'ya alınır\
-   Worker tarafından işlenir

------------------------------------------------------------------------

4.  **Repo Tarama**

Repo'lar:

-   clone edilir veya cache'ten alınır

Arama yöntemleri:

a)  ripgrep (MVP)

-   Çok hızlı text search tool'u\
-   Dosyalar içinde string bazlı arama yapar\
-   Örn: LegacyField geçen tüm yerleri bulur

b)  AST bazlı analiz (opsiyonel)

-   Kod parse edilerek gerçek kullanım bulunur\
-   False positive azalır

c)  Multi-language parser (ileri seviye)

-   Tek altyapı ile farklı diller desteklenir

------------------------------------------------------------------------

5.  **Ham Veri Çıkarma**

``` json
[
  {
    "repo": "order-service",
    "file": "Order.cs",
    "line": 42,
    "usageType": "property-read",
    "codeSnippet": "order.LegacyField"
  },
  {
    "repo": "payment-service",
    "file": "PaymentHandler.java",
    "line": 88,
    "usageType": "property-write",
    "codeSnippet": "order.setLegacyField(value)"
  }
]
```

------------------------------------------------------------------------

6.  **Analiz**

Ham veriler:

-   filtrelenir\
-   gruplanır\
-   analiz edilir

Üretilen çıktılar:

-   Etkilenen repo ve servisler\
-   Kritik kullanım noktaları\
-   API / DTO etkileri\
-   Olası riskler

------------------------------------------------------------------------

7.  **Sonuç**

```{=html}
<!-- -->
```
    Impact Analysis:

    - 5 repo etkileniyor
    - 2 kritik servis (order-service, payment-service)
    - 3 endpoint değişecek

    Riskler:
    - API response değişimi
    - Null reference ihtimali

    Önerilen adımlar:
    1. DTO güncelle
    2. Mapping kaldır
    3. DB migration yap
    4. Consumer servisleri update et

Çıktı seçenekleri:

-   Bitbucket UI içinde gösterim (panel / modal)
-   İndirilebilir doküman (txt / md)
-   Log veya storage'a kaydedilmiş rapor

------------------------------------------------------------------------

## 🧱 Mimari Bileşenler

-   Backend API (.NET)
-   Worker / Queue sistemi
-   Repo erişim katmanı (clone / cache)
-   Search engine (ripgrep / parser)
-   Analiz katmanı

------------------------------------------------------------------------

## 🌍 Multi-language Destek

### MVP

-   String-based search (dilden bağımsız)

### Gelişmiş

-   Dil bazlı parser (C#, Java, JS)

### Önerilen

-   Tek altyapı ile multi-language analiz (AST yaklaşımı)

------------------------------------------------------------------------

## 🚀 MVP Scope

-   Repo tarama (ripgrep)
-   Temel analiz (rule-based + basit yorumlama)
-   Bitbucket üzerinden tetikleme
-   Metin tabanlı çıktı üretimi

------------------------------------------------------------------------

## ⚠️ Kritik Noktalar

-   Önce deterministic search yapılmalı\
-   Analiz katmanı kontrollü olmalı\
-   Performans için paralel tarama gerekli\
-   Repo cache mekanizması önemli

------------------------------------------------------------------------

## 🎯 Kısa Özet

Bu sistem, Bitbucket üzerinden tetiklenen bir analiz ile tüm repolarda
yapılan değişikliğin etkisini otomatik olarak tespit eder ve
uygulanabilir aksiyon planı üretir.
