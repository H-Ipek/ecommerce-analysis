# 🛒 E-Ticaret Müşteri Davranışı Analizi

## 👤 Proje Ekibi

| Bilgi | Detay |
|-------|-------|
| **Ad SoyAD** | Hava İpek Karagöz |
| **Öğrenci Numarası** | 1306240041 |
| **Ders** | Veri Bilimi |
| **Öğretim Üyesi** | Doç. Dr. Muhammed Erdem İsenkul |
| **Son Teslim Tarihi** | 3 Temmuz 2026 |

---

## 📌 Proje Hakkında

Bu proje, İngiltere merkezli bir online hediyecilik mağazasının
2010–2011 yıllarına ait işlem verilerini analiz etmektedir.
Veri bilimi iş akışının tüm adımları (yükleme, temizleme, EDA,
modelleme) tek bir Jupyter Notebook içinde uygulanmıştır.

**Tema:** E-ticaret & Müşteri Davranışı  
**Yöntem:** Vibe coding (Claude,Gemini,ChatGPT + Jupyter Notebook)

---

## 📂 Proje Yapısı

```
ecommerce-analysis/
├── notebook.ipynb       # Ana analiz dosyası
├── report.pdf           # Rapor
├── prompt_log.md        # AI prompt günlüğü
├── README.md            # Bu dosya
├── requirements.txt     # Kullanılan kütüphaneler
├── .gitignore           # data/ klasörü hariç tutulmuş
└── data/
    └── data.csv         # Veri seti (Git'e eklenmez)
```

---

## 🗃️ Veri Kaynağı

| | |
|--|--|
| **Veri Seti** | UCI Online Retail Dataset |
| **Kaggle Linki** | https://www.kaggle.com/datasets/carrie1/ecommerce-data |
| **Orijinal Kaynak** | UCI Machine Learning Repository |
| **Hazırlayan** | Dr. Daqing Chen |
| **Lisans** | CC BY 4.0 |
| **Kapsam** | Aralık 2010 – Aralık 2011, 541.909 işlem, 8 sütun |

> Veri seti lisans gereği repoya dahil edilmemiştir.
> Yukarıdaki Kaggle linkinden `data.csv` olarak indirip
> `data/` klasörüne koymanız gerekmektedir.

---

## ⚙️ Kurulum ve Çalıştırma

### 1. Repoyu klonla
```bash
git clone https://github.com/[kullanici-adin]/ecommerce-analysis.git
cd ecommerce-analysis
```

### 2. Kütüphaneleri yükle
```bash
pip install -r requirements.txt
```

### 3. Veri setini hazırla
Kaggle'dan `data.csv` dosyasını indirip `data/` klasörüne koy:
```
ecommerce-analysis/
└── data/
    └── data.csv
```

### 4. Notebook'u çalıştır
```bash
jupyter notebook notebook.ipynb
```

Açıldıktan sonra: **Kernel → Restart & Run All**

---

## 📦 Kullanılan Kütüphaneler

```
pandas
numpy
matplotlib
seaborn
scikit-learn
plotly
jupyter
```

Tam versiyon bilgisi için `requirements.txt` dosyasına bakınız.

---

## 🔬 Araştırma Soruları

1. İadeler satış gelirini ne ölçüde etkiliyor ve hangi ürünler en çok iade ediliyor?
2. Satışlar mevsimsel bir örüntü gösteriyor mu; hafta içi/sonu ve saat bazında farklılıklar var mı?
3. Müşterileri RFM metriklerine göre segmentlere ayırdığımızda anlamlı gruplar ortaya çıkıyor mu?
4. Müşterilerin yüzde kaçı toplam gelirin %80'ini oluşturuyor? (Pareto analizi)
5. Müşterilerin kaçı yalnızca bir kez alışveriş yapmış, kaçı tekrar eden müşteri?

---

## 📊 Temel Bulgular

| Gösterge | Değer |
|----------|-------|
| Temizlenmiş veri | 392.692 işlem |
| Toplam satış geliri | £8.887.208 |
| İade / satış oranı | %10,09 |
| En yüksek aylık gelir | Kasım 2011 — £1.156.205 |
| Gelirin %80'ini oluşturan müşteri | %26,0 (1.130 kişi) |
| Tekrar eden müşteri oranı | %65,6 |
| K-Means optimal K | 4 (Silhouette: 0.6162) |

---

## 📝 Teslim Dosyaları

- [x] `notebook.ipynb` — Çalışır analiz notebook'u
- [x] `report.pdf` — Rapor
- [x] `prompt_log.md` — AI prompt günlüğü 
- [x] `README.md` — Bu dosya
- [x] `requirements.txt` — Kütüphane listesi
