# Prompt Günlüğü — Veri Bilimi Dönem Projesi (Final)
**Proje:** UCI Online Retail Dataset — E-Ticaret & Müşteri Davranışı Analizi  
**Kullanılan Modeller:** Claude (Anthropic) · Gemini (Google) · ChatGPT (OpenAI)  
**Dönem:** 25/26 Bahar

> Her prompt için şu bilgiler verilmiştir:
> `Adım` — projedeki aşama, `Model` — hangi AI ile konuşuldu,
> `Teknik` — kullanılan prompt mühendisliği tekniği, `Amaç` — neden bu prompt yazıldı.

---

## GÜN 1 — Analiz ve Geliştirme

---

### Prompt #1 — Veri Yükleme ve Encoding Sorunu

**Adım:** Veri Okuma  
**Model:** Claude  
**Teknik:** Bağlam + hata mesajı ekleme, spesifik soru

```
Bir Jupyter Notebook üzerinde pandas ile CSV okuyorum.
Aşağıdaki kodu çalıştırdığımda UnicodeDecodeError alıyorum:

    df = pd.read_csv('data/data.csv')

Hata mesajı:
    UnicodeDecodeError: 'utf-8' codec can't decode byte 0xe9 in position 12

Neden bu hatayı alıyorum ve encoding='ISO-8859-1' parametresi bu durumu
nasıl çözüyor? UTF-8 ile ISO-8859-1 arasındaki farkı da kısaca açıkla.
Sadece bu konuyu açıkla, başka öneri verme.
```

**Ne öğrendim:** ISO-8859-1 (Latin-1), Batı Avrupa dillerindeki özel karakterleri (é, ü, ç gibi) UTF-8'den farklı şekilde encode eder. Ürün isimlerinde Fransızca/Almanca karakter içeren satırlar bu yüzden UTF-8 ile açılamıyor.

---

### Prompt #2 — Veri Temizleme Stratejisi

**Adım:** Veri Temizleme  
**Model:** Claude  
**Teknik:** Rol atama + adım adım açıklama talebi + kısıt belirleme

```
Sen deneyimli bir veri mühendisisin ve bana e-ticaret verisini temizlemeyi
öğretiyorsun.

Elimdeki UCI Online Retail veri seti şu sorunları içeriyor:
- CustomerID sütununda %24.93 eksik değer var
- InvoiceNo'nun bazıları 'C' ile başlıyor 
- UnitPrice'ın bazı satırları negatif veya sıfır
- Tekrarlayan (duplicate) satırlar mevcut

Her sorun için:
1. Bu sorunun neden var olduğunu açıkla
2. Doğru temizleme yaklaşımını söyle
3. Pandas kodu yaz

Önemli kısıt: İadeleri silme, ayrı bir DataFrame'e al çünkü bunları
daha sonra analiz edeceğim.
```

**Ne öğrendim:** İadeleri tamamen silmek yerine ayırmak, AS1 (iade analizi) sorusunun cevaplanabilmesini sağladı. 

---

### Prompt #3 — EDA için Görsel Seçimi

**Adım:** Keşifsel Veri Analizi  
**Model:** Gemini  
**Teknik:** Seçenek kısıtlama + gerekçe isteme + çıktı formatı belirleme

```
UCI Online Retail veri setinde aşağıdaki sütunlar var:
InvoiceDate, Quantity, UnitPrice, CustomerID, Country, TotalPrice (türetilmiş)

Bu veri seti için tam olarak 5 farklı grafik türü öner.
Her öneri için:
- Grafik türü
- X ve Y eksenleri
- Neden bu grafik (1 cümle gerekçe)

Cevabını tablo formatında ver. Kod yazma, sadece plan ver.
```

**Ne öğrendim:** Gemini'nin önerdiği Gün × Saat heatmap fikri EDA'nın en güçlü görsellerinden biri oldu, B2B profilini tek bakışta ortaya koydu.

---

### Prompt #4 — Araştırma Soruları Çerçeveleme

**Adım:** Araştırma Soruları  
**Model:** ChatGPT  
**Teknik:** Kısıt + örnek format + alan bilgisi bağlamı

```
E-ticaret veri bilimi projem için araştırma soruları yazıyorum.
En az 3 somut soru isteniyor; ben 5 tane yazmak istiyorum.

Kısıtlar:
- Her soru veriyle doğrudan cevaplanabilir olmalı
- Soyut veya yoruma açık sorular olmamalı (örn. "müşteriler mutlu mu?" gibi sorular olmaz)
- Her soru farklı bir analiz tekniği gerektirmeli
- Sorular birbiriyle mantıksal bir akış oluşturmalı

Önerilerin için şu formatı kullan:
Soru: [soru metni]
Analiz yöntemi: [kullanılacak teknik]
Beklenen çıktı: [sayısal veya görsel sonuç]
```

**Ne öğrendim:** Soruları önceden net çerçevelemek analizi odaklı tuttu. AS4 (Pareto) ve AS5 (sadakat) ChatGPT'nin önerdiği ek sorulardı; ikisi de notebooka dahil edildi.

---

### Prompt #5 — İade Analizi (AS1)

**Adım:** Araştırma Sorusu 1  
**Model:** Claude  
**Teknik:** Chain-of-thought (adım adım düşünme talebi) + iki alternatif karşılaştırma

```
İade analizi yapıyorum. İki yaklaşım arasında karar veremiyorum:

Yaklaşım A: İadeleri iade ADEDİNE göre sırala (hangi ürün en çok iade edildi)
Yaklaşım B: İadeleri iade TUTARINA göre sırala (hangi ürün en fazla gelir kaybı yarattı)

Adım adım düşün:
1. Her yaklaşımın iş açısından ne anlam ifade ettiğini açıkla
2. Bir mağaza yöneticisi için hangisi daha karar verici bilgidir?
3. Sonuç olarak hangi yaklaşımı önereceğini söyle ve tek bir pandas kod bloğu yaz

Veri: returns DataFrame'i, ReturnValue sütunu (abs(Quantity*UnitPrice))
```

**Ne öğrendim:** Tutar bazlı yaklaşımın daha anlamlı olduğunu gerekçesiyle anladım: 500 adet £0.50'lik ürün iadesi ile 1 adet £300'lük ürün iadesi aynı adet ama çok farklı iş sorunu anlamına geliyor.

---

### Prompt #6 — Heatmap Yorumlama

**Adım:** EDA — Gün × Saat Isı Haritası  
**Model:** Claude  
**Teknik:** Gözlem verme + yorum talebi + iş bağlamı ekleme

```
Seaborn heatmap oluşturdum: X ekseni saat (6-20), Y ekseni haftanın günleri
(Pzt-Cmt). Renk yoğunluğu fatura sayısını gösteriyor.

Gözlemlerim:
- Cumartesi ve Pazar neredeyse tamamen boş (açık sarı)
- Hafta içi 10:00-15:00 bloğu koyu kırmızı
- Saat 6'da sadece 1 fatura var

Bu örüntü hakkında şunu söyle:
1. Bu işletmenin B2B mi B2C mi olduğu hakkında ne düşünüyorsun? Neden?
2. Pazarlama açısından bu bulgunun önemi nedir?
3. Saat 12'deki zirve neden önemli?

Yorumunu 3-4 cümleyle sınırla. Teknik terim kullanma, iş dili kullan.
```

**Ne öğrendim:** Görsel oluşturmak yetmiyor, yorumu da önceden düşünmem gerektiğini anladım. Bu prompt'tan çıkan metni notebooka gözlem olarak, rapora da bulgu olarak direkt ekledim.

---

### Prompt #7 — RFM Hesaplama Mantığı

**Adım:** Modelleme — RFM  
**Model:** Claude  
**Teknik:** "Neden" sorusu + yanlış anlaşılabilecek detayı sorgulama

```
RFM hesaplıyorum. Reference date olarak veri setinin son tarihini almayı
düşünüyordum ama şunu fark ettim:

Son günde alışveriş yapan müşterinin Recency değeri 0 oluyor.
Bu bir sorun mudur?

Şu iki seçenek arasında hangisi daha doğru:
A) reference_date = df['InvoiceDate'].max()
B) reference_date = df['InvoiceDate'].max() + pd.Timedelta(days=1)

Neden fark yarattığını sayısal örnekle göster, sonra doğru kodu yaz.
```

**Ne öğrendim:** +1 gün eklemenin sadece estetik değil analitik bir neden taşıdığını anladım: 0 ile 1 günlük Recency istatistiksel olarak ayrıştırılabilir olmalı. Bu detay notebooka açıklama olarak eklendi.

---

### Prompt #8 — StandardScaler Gerekliliği

**Adım:** Modelleme — Ölçeklendirme  
**Model:** Gemini  
**Teknik:** Sezgisel açıklama talebi + sayısal örnek isteme

```
K-Means'i RFM verisine uygulamadan önce StandardScaler kullanmam
gerektiğini biliyorum ama sezgisel olarak neden gerekli olduğunu
tam anlamıyorum.

Şu değerlerle somut bir örnek ver:
- Recency: 0-374 gün aralığı
- Frequency: 1-209 fatura aralığı
- Monetary: £3 - £280.206 aralığı

Ölçeklendirme olmadan K-Means ne hesaplar?
Ölçeklendirme sonrası ne değişir?
Sayısal olarak göster, formül yazmana gerek yok.
```

**Ne öğrendim:** Monetary sütununun £280.000 gibi uç değerleri olmadan Recency ve Frequency tamamen görmezden geliniyordu. Scaler bu üç metriği eşit "oy hakkına" kavuşturdu.

---

### Prompt #9 — Elbow ve Silhouette Kombinasyonu

**Adım:** Modelleme — K Seçimi  
**Model:** Claude  
**Teknik:** İki yöntemi karşılaştırma + karar destek talebi

```
K-Means için optimal K'yı arıyorum. Elbow plot'umu çizdim:
- K=2→3: inertia 9000'den 5400'e düşüyor (büyük sıçrama)
- K=3→4: 5400'den 4300'e düşüyor (orta sıçrama)
- K=4→5: 4300'den 3100'e düşüyor (küçülüyor ama...)
- K=5 sonrası: çok yavaş düşüyor

Silhouette score'larım:
K=3: 0.5942
K=4: 0.6162
K=5: 0.6165

Şu soruyu cevapla:
K=4 mü K=5 mi seçmeliyim? İki metriği birlikte yorumlayarak gerekçeli
bir karar ver. Yorumlanabilirlik faktörünü de hesaba kat.
```

**Ne öğrendim:** K=5'in silhouette skoru sadece 0.0003 daha yüksek — bu istatistiksel olarak anlamlı değil. Ama K=4 bir segmenti K=5'e böldüğünde iş yorumu zorlaşıyor. Yorumlanabilirlik + marginal kazanım analizi bir karar çerçevesi olarak benim için yeniydi.

---

### Prompt #10 — Cluster İsimlendirme

**Adım:** Modelleme — Segment Yorumu  
**Model:** ChatGPT  
**Teknik:** Tablo veri verme + alan uzmanı rolü + çıktı kısıtı

```
Sen bir CRM danışmanısın. K-Means kümeleme sonuçlarım şu:

| Cluster | Recency (ort.) | Frequency (ort.) | Monetary (ort.) | Müşteri Sayısı |
|---------|---------------|-----------------|----------------|----------------|
| 0       | 43.7 gün      | 3.68 fatura     | £1.353         | 3.054          |
| 1       | 248.1 gün     | 1.55 fatura     | £479           | 1.067          |
| 2       | 7.4 gün       | 82.5 fatura     | £127.187       | 13             |
| 3       | 15.5 gün      | 22.3 fatura     | £12.690        | 204            |

Her cluster için:
1. Segment adı (2-3 kelime, iş dünyasında kullanılan bir terim)
2. Bu müşterilere önerilecek tek bir pazarlama aksiyonu

Cevabını tablo formatında ver. Teknik açıklama yapma.
```

**Ne öğrendim:** "VIP / Şampiyon", "Sadık Müşteriler", "Aktif / Potansiyel", "Kaybedilen / Uyuyan" isimlerini ChatGPT önerdi. Bunları doğrularken her cluster'ın RFM ortalamalarına geri döndüm ve isimler gerçekten veriye uyuyordu.

---

## GÜN 2 — Dokümantasyon ve Çapraz Doğrulama

---

### Prompt #11 — Pareto Eğrisi Yorumlama

**Adım:** Araştırma Sorusu 4  
**Model:** Claude  
**Teknik:** Bulgu verme + karşılaştırma talebi + yanıltıcı yorumu sorgulama

```
Pareto analizim şu sonucu verdi:
Müşterilerin %26'sı toplam gelirin %80'ini oluşturuyor.

Klasik 80-20 kuralı %20 müşteri bekliyor ama ben %26 buldum.
Bu şu anlama mı geliyor:
A) Mağazanın müşteri konsantrasyonu düşük, dolayısıyla riski az
B) 80-20 kuralı bu veri seti için geçerli değil
C) Fark istatistiksel açıdan önemsiz, oran yine de yüksek konsantrasyon gösteriyor

Hangisi doğru, neden? Bu mağazanın B2B profiliyle nasıl ilişkilendirilir?
```

**Ne öğrendim:** %20 ile %26 arasındaki fark, B2B mağazaların genellikle daha az ama büyük müşteriyle çalışmasından kaynaklanıyor. 80-20 kuralı bir kılavuz, mutlak bir yasa değil o yüzden bu nüansı raporda belirtmek önemliydi.

---

### Prompt #12 — Çapraz Model Doğrulama

**Adım:** Doğrulama  
**Model:** Gemini (Claude çıktısını doğrulatmak için)  
**Teknik:** Bağımsız değerlendirme talebi + spesifik iddia sorgulama

```
Bir veri bilimi projesinde K-Means ile RFM segmentasyonu yaptım.
Aşağıdaki iki iddiayı bağımsız olarak değerlendir:

İddia 1: "Silhouette score 0.6162 gerçek dünya verisi için iyi bir
ayrışmayı gösteriyor."

İddia 2: "13 müşteriden oluşan VIP segment (£127.187 ort. harcama)
toptan alıcı profilini yansıtıyor."

Her iddia için:
- Onaylıyor musun? (Evet/Kısmen/Hayır)
- Gerekçen nedir? (1-2 cümle)
- Varsa alternatif yorum?

Başka hiçbir konuya girme.
```

**Ne öğrendim:** Gemini her iki iddiayı da onayladı ama İddia 1 için "0.6 sınır değeri veri boyutuna ve alana göre değişir" uyarısını ekledi. Bu uyarıyı sınırlamalar bölümüne not olarak ekledim.

---

### Prompt #13 — README Yazımı

**Adım:** Dokümantasyon  
**Model:** Claude  
**Teknik:** Şablon isteme + doldurulacak alanları belirleme

```
GitHub repository'im için README.md yazıyorum.
Veri bilimi projesi, Python/Jupyter kullanıldı.

Şu bölümleri içeren bir şablon oluştur:
1. Proje Başlığı ve Tek Cümle Açıklama
2. Kurulum (pip install komutları)
3. Çalıştırma Talimatları (adım adım)
4. Kullanılan Kütüphaneler (versiyon numarasıyla)
5. Veri Seti (kaynak, lisans, indirme linki)
6. Proje Yapısı (dosya ağacı)

Kullandığım kütüphaneler: pandas, numpy, matplotlib, seaborn,
scikit-learn, plotly
Veri: UCI Online Retail Dataset, CC BY 4.0

Türkçe yaz. Yorum satırlarını placeholder olarak bırak,
ben dolduracağım.
```

**Ne öğrendim:** İyi bir README'nin sadece "ne kullandım" değil "nasıl çalıştırırım" sorusunu da cevapladığını gördüm. Dosya ağacı bölümü özellikle projenin yapısını netleştirdi.

---

### Prompt #14 — Notebook Yorum Satırları Kalite Kontrolü

**Adım:** Son Kontrol  
**Model:** Claude  
**Teknik:** Kod inceleme + spesifik kriter listesi

```
Notebookumun bir bölümünü paylaşıyorum. Yorum satırlarını değerlendir:

[KOD BLOĞU]
rfm = df_clean.groupby('CustomerID').agg(
    Recency =('InvoiceDate', lambda x: (reference_date - x.max()).days),
    Frequency=('InvoiceNo', 'nunique'),
    Monetary =('TotalPrice', 'sum')
).reset_index()

Şu kriterlere göre değerlendir:
1. Her satırın ne yaptığı açık mı?
2. Neden bu yaklaşım seçildi (alternatifler neden reddedildi)?
3. Başlangıç seviyesindeki bir okuyucu anlayabilir mi?

Eksik olan yorumları yaz, mevcut olanlara dokunma.
Yorum satırları # ile başlamalı, Türkçe olmalı.
```

**Ne öğrendim:** lambda x: (reference_date - x.max()).days satırının ".max() neden?" ve "neden days?" sorularına cevap vermediğini fark ettim. İyi yorum satırı "ne yapıyor" değil "neden böyle" sorusunu cevaplıyor.

---

### Prompt #15 — Sınırlamalar Bölümü

**Adım:** Rapor Yazımı  
**Model:** ChatGPT  
**Teknik:** Kendi analizini eleştirme talebi + akademik ton

```
Veri bilimi projemin sınırlamalarını yazıyorum.
Projemle ilgili şu gerçekleri biliyorum:
- CustomerID %25 eksik
- Veri tek bir yıl
- K-Means küre şeklinde kümeler varsayıyor
- Ürün kategorisi bilgisi yok

Bu gerçekleri akademik bir rapor için sınırlama maddeleri haline getir.
Her madde için:
- Sınırlamanın ne olduğunu açıkla
- Analizi nasıl etkilediğini belirt
- Varsa alternatif yaklaşımı (tek cümle) söyle

Savunmacı değil, dürüst ve nesnel bir ton kullan.
Türkçe, 4-5 madde yeterli.
```

**Ne öğrendim:** Sınırlamaları "proje başarısız" değil "analitik dürüstlük" çerçevesinde sunmak gerekiyor. Bu prompt'tan çıkan metin rapora neredeyse direkt girdi.

---

## Kullanılan Prompt Mühendisliği Teknikleri — Özet

| Teknik | Kullanıldığı Prompt Sayısı | Örnek |
|--------|---------------------------|-------|
| Rol atama | #2, #10, #15 | "Sen deneyimli bir veri mühendisisin..." |
| Kısıt belirleme | #3, #4, #13 | "Sadece tablo formatında ver" |
| Chain-of-thought | #5, #7, #9 | "Adım adım düşün:" |
| Bağlam + hata verme | #1, #6 | Hata mesajını kopyalayıp yapıştırma |
| Çıktı formatı | #4, #10, #12 | "Tablo formatında ver, teknik terim kullanma" |
| Karşılaştırmalı karar | #8, #9, #11 | "A mı B mi, neden?" |
| Bağımsız doğrulama | #12 | Aynı iddiayı farklı modele sormak |
| Kalite kontrol | #14 | Spesifik kriter listesiyle kod inceleme |

---

*Buraya eklenen toplam prompt: 15 | Kullanılan model: 3 (Claude, Gemini, ChatGPT)*
