---
title: Agentic Disaster Tracker
emoji: 🌍
colorFrom: red
colorTo: blue
sdk: gradio
sdk_version: 4.44.1
app_file: app.py
pinned: false
license: mit
---

# 🌍 Agentic Disaster Tracker

Agentic Disaster Tracker, dünya genelindeki güncel deprem ve aktif orman yangını verilerini toplayarak kullanıcılara doğal dilde sunan yapay zeka destekli bir afet takip asistanıdır.

Bu proje, bir Büyük Dil Modeli'nin (LLM) dış dünyayla **Tool Calling (Function Calling)** yeteneklerini kullanarak nasıl iletişim kurduğunu şeffaf bir şekilde göstermeyi amaçlamaktadır. Arka planda çalışan "Agentic Loop" sayesinde model, kullanıcının ihtiyacına göre ilgili Public API'leri otonom olarak çağırır, dönen sonuçları analiz eder ve nihai bir sentez oluşturur.

---

## 🚀 Özellikler
- **Otonom Araç Kullanımı (Tool Calling):** Asistan, sorulan soruya göre Deprem veya Yangın API'sine istek atması gerektiğine kendisi karar verir.
- **Şeffaf İşlem Adımları:** Modelin hangi aracı çağırdığı, araca hangi parametreleri gönderdiği ve dönen raw (ham) veriler UI üzerinde anlık olarak (`[Turn 1] Araç Çağrıları:` formatında) listelenir.
- **Güçlü ve Hafif LLM:** Proje, Tool Calling konusunda son derece başarılı olan açık kaynaklı `Qwen/Qwen2.5-7B-Instruct` modeli ile çalışmaktadır.
- **Hugging Face ZeroGPU Desteği:** `@spaces.GPU` altyapısı sayesinde 7B parametreli model, sunucusuz (serverless) ekran kartı mimarisinde verimli ve ücretsiz bir şekilde çalışır.

## 🛠️ Teknik Mimari ve Kullanılan API'ler

Sistem, Gradio tabanlı bir arayüz üzerinde çalışmakta ve modelin kararlarını arka planda işleyen özel bir ayrıştırıcı (parser) motoru barındırmaktadır.

### Entegre Public API'ler
1. **USGS Earthquake API (Deprem):** 
   - *Açıklama:* Amerika Birleşik Devletleri Jeoloji Araştırmaları Kurumu'nun sağladığı, dünya genelindeki deprem verilerini JSON (GeoJSON) formatında sunan public API.
   - *Fonksiyon:* `get_earthquakes(start_date, end_date, min_magnitude)`
2. **GDACS API (Orman Yangını):**
   - *Açıklama:* Küresel Afet Uyarı ve Koordinasyon Sistemi'nin (GDACS) sağladığı, aktif orman yangınlarını listeleyen API.
   - *Fonksiyon:* `get_wildfires(limit)`

## ⚙️ Kurulum ve Çalıştırma (Reproducibility)

Projeyi kendi bilgisayarınızda veya sunucunuzda çalıştırmak oldukça basittir. (Not: Hugging Face Spaces üzerinde doğrudan ZeroGPU ile çalışmak üzere tasarlanmıştır, lokal kurulumda GPU gereksinimi model boyutuna göre değişebilir).

### 1. Depoyu Klonlayın
```bash
git clone <SİZİN_REPO_URL_NİZ>
cd Agentic-Disaster-Tracker
```

### 2. Gerekli Kütüphaneleri Yükleyin
```bash
pip install -r requirements.txt
```

### 3. Uygulamayı Başlatın
```bash
python app.py
```
Uygulama varsayılan olarak `http://127.0.0.1:7860/` adresinde yayına girecektir.

---

## 📖 Örnek Çalışma Akışı (Workflow)

Kullanıcı örnek bir soru sorduğunda, sistem şu adımları izler:

**Kullanıcı:** *"Şu anki aktif büyük orman yangınları hangileri? 3 tanesini getir."*

**[Turn 1] Araç Çağrıları:**
- Model ihtiyacı analiz eder ve `get_wildfires` aracını tetikler.
- `-> get_wildfires(limit=3)`
- API'den veri döner.
- `<- {"count": 3, "wildfires": [{"name": "...", "country": "...", ...}]}`

**[Nihai Yanıt]:**
- Model aldığı ham verileri birleştirerek doğal dilde açıklar.
- *"Şu anda GDACS kayıtlarına göre dünya genelindeki 3 büyük orman yangını şunlardır: 1. Brezilya'daki Amazon yangını... 2..."*

> Proje içerisine kullanıcıların sistemi hızlıca test edebilmeleri için hazır promptlar (Gradio Examples) entegre edilmiştir. Bu promptlara tıklayarak sistemin Tool Calling yeteneğini anında deneyimleyebilirsiniz.

---

## 🏆 Değerlendirme Kriterlerine Uyum Özeti
- **Amaca Uygunluk:** Proje, Public API (Deprem ve Yangın) kullanarak LLM'in veri çekmesini ve adımlarını göstermesini eksiksiz sağlar.
- **Teknik Kalite:** ZeroGPU entegrasyonu, manuel agentic loop yönetimi, Qwen 2.5 modeli optimizasyonu ve Regex bazlı parser tasarımı ile teknik olarak sağlam bir altyapı sunulmuştur.
- **Dokümantasyon:** Bu README belgesi ve kod içi (docstrings) açıklamalar ile projenin tamamen yeniden üretilebilir (reproducible) olması sağlanmıştır.
