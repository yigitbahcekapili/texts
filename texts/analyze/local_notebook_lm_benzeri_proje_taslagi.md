# 📘 Local NotebookLM Benzeri Proje Taslağı

## 🎯 Amaç
Teams üzerinden alınan video kayıtlarını analiz ederek:
- Metne dönüştürmek
- Özet çıkarmak
- Aksiyon maddeleri belirlemek
- Kararları listelemek

Tüm bu işlemleri **local (offline) çalışan bir sistem** ile gerçekleştirmek.

---

## 🧠 Genel Mimari

Sistem basit bir veri akışı üzerine kuruludur:

```
Video Kaydı
   ↓
Ses çıkarma
   ↓
Metne dönüştürme
   ↓
Metin işleme (parçalama)
   ↓
LLM analizi
   ↓
Özet / Aksiyon / Karar çıktısı
```

---

## 🔄 Adım Adım Süreç

### 1. Video → Ses
Video dosyasından ses extract edilir.

**Önerilen tool:**
- ffmpeg

---

### 2. Ses → Metin (Speech-to-Text)
Ses dosyası yazıya çevrilir.

**Önerilen tool:**
- faster-whisper

✔ Lokal çalışır
✔ Türkçe desteği iyi
✔ Performanslı

---

### 3. Metin Temizleme ve Parçalama
Elde edilen metin:
- Gereksiz kelimelerden temizlenir
- Küçük parçalara bölünür

**Amaç:**
LLM'e daha doğru ve verimli veri vermek

---

### 4. LLM ile Analiz
Metin parçaları local LLM'e gönderilir.

**Önerilen tool:**
- Ollama

**Model önerisi:**
- llama3

---

### 5. Analiz Çıktısı
LLM'den aşağıdaki formatta çıktı alınır:

- Genel Özet
- Alınan Kararlar
- Aksiyon Maddeleri
- Riskler

---

## 🏗️ Önerilen Teknik Yapı

Basit ve sürdürülebilir bir yapı:

```
.NET Backend (API)
   ↓
Python Worker (AI işlemleri)
   ↓
Ollama (LLM)
```

### Roller:
- **.NET API:** Dosya upload, süreç yönetimi
- **Python:** Ses işleme ve metin üretimi
- **Ollama:** Analiz ve özetleme

---

## ⚙️ Kullanılacak Araçlar (Özet)

| Amaç | Tool |
|------|------|
| Video → Ses | ffmpeg |
| Speech-to-Text | faster-whisper |
| LLM | Ollama (llama3) |
| Backend | .NET |
| AI işlemleri | Python |

---

## 🚀 Ekstra Feature’lar (İleriki Faz)

- 🎤 Konuşmacı ayrımı (kim ne söyledi)
- 🔍 Toplantı içinde arama (semantic search)
- ❓ Soru-cevap sistemi
- 📊 UI ile görselleştirme (timeline, highlight)
- 🧠 Geçmiş toplantılarla bağlantı kurma

---

## 🧪 MVP Önerisi

İlk versiyon için minimum hedef:

1. Video yükleme
2. Metne çevirme
3. Özet çıkarma

Daha sonra kademeli geliştirme yapılabilir.

---

## 💡 Sonuç

Bu proje ile:
- Ücretli araçlara bağımlılık azalır
- Tüm veriler localde kalır
- Toplantı analizleri otomatik hale gelir

Basit bir MVP ile başlayıp zamanla gelişmiş bir AI analiz platformuna dönüşebilir.

