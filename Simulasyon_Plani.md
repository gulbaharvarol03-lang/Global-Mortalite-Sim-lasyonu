# Global Mortality Dynamics Simulator
## README — Teknik ve Akademik Belgeleme (Final Sürüm)

> **Proje Türü:** Akademik Karar Destek Simülasyonu  
> **Kapsam:** Küresel Mortalite Dinamikleri (1990–2500)  
> **Dosya:** `index.html` (tek dosya, bağımsız çalışır — bağımlılık gerektirmez)  
> **Kullanılan Teknolojiler:** HTML5, Vanilla CSS, Vanilla JavaScript, Chart.js, SVG

---

## 1. Proje Özeti

Bu simülasyon, küresel ölüm oranları üzerindeki sağlık politikalarının etkisini **kohort tabanlı bir epidemiyolojik projeksiyon motoru** aracılığıyla modellemektedir. Araç, basit bir veri görselleştirme uygulaması olmayıp; Dünya Sağlık Örgütü (WHO) ve Küresel Hastalık Yükü (IHME GBD) metodolojilerine dayanan **Population Attributable Fraction (PAF)** ve **Relative Risk (RR)** formülleriyle kalibre edilmiş, senaryo tabanlı bir karar destek sistemidir.

Simülasyon, tek bir politika kararının nüfus düzeyindeki ölüm yükünü nasıl ve ne kadar değiştirdiğini zaman ekseninde izlemeye, bölgeler ve gelir grupları arasında karşılaştırmaya olanak tanır.

---

## 2. Simülasyon Motoru Mimarisi

### 2.1 Zaman Aralığı ve Veri Tabanı

| Aşama | Dönem | Yöntem |
|:---|:---|:---|
| Tarihsel Baseline | 1990–2026 | WHO/UN gerçek demografik verileri ile geri-interpole edilmiş kohort hesapları |
| Gelecek Projeksiyonu | 2027–2500 | Her yıl için politika senaryolarına göre yeniden hesaplanan ileri projeksiyon |

### 2.2 Kohort Yapısı

Popülasyon, WHO metodolojisine uygun **21 yaş kohortu** (0–4, 5–9, …, 95–99, 100+) ve **iki cinsiyet grubu** (erkek/kadın) üzerinden modellenmektedir. Her kohort için yaşa özgü kaba ölüm hızları (Age-Specific Mortality Rate) ayrı hesaplanmakta; bu değerler politika etkilerine göre dinamik olarak güncellenmektedir.

### 2.3 Risk Faktörü ve Ölüm Motoru

Her simülasyon yılında aşağıdaki risk faktörleri hesaplanır ve politika etkileri uygulanır:

```
cardioMultiplier    = pm25Multiplier × smokingMultiplier × ssbMultiplier × sodiumMultiplier × activityMultiplier
cancerMultiplier    = pm25Multiplier × smokingMultiplier × alcoholMultiplier
respMultiplier      = pm25Multiplier × smokingMultiplier
diabetesMultiplier  = ssbMultiplier × activityMultiplier
injuryMultiplier    = alcoholMultiplier (kaza kanalı)
```

Tüm çarpanlar, WHO kılavuzu olan **5 µg/m³ PM2.5 sınırı** ve IHME GBD Relative Risk eğrilerinden türetilmiştir.

---

## 3. Bölgesel Veri Tabanı

Simülasyon, UN Nüfus Projeksiyonları 2024 verileriyle kalibre edilmiş **8 makro-bölge** üzerinden çalışmaktadır:

| Bölge Kodu | Bölge Adı | Nüfus (2026 tahmini) | Gelir Sınıfı |
|:---|:---|---:|:---|
| USA | Kuzey Amerika | 500,000,000 | Yüksek |
| BRA | Latin Amerika | 660,000,000 | Orta |
| RUS | Rusya ve Orta Asya | 290,000,000 | Orta |
| IND | Güney Asya | 1,900,000,000 | Orta |
| CHN | Doğu ve Güneydoğu Asya | 2,100,000,000 | Yüksek |
| AFR | Afrika | 1,450,000,000 | Düşük |
| EUR | Avrupa ve Orta Doğu | 1,150,000,000 | Yüksek |
| AUS | Okyanusya | 50,000,000 | Yüksek |

Her bölgeye ait **PM2.5 başlangıç değerleri**, **sigara prevalansı**, **sodyum alım indeksi** ve **şekerli içecek tüketim indeksi** WHO 2024 bölgesel raporlarından alınmış proxy değerlerle tanımlanmıştır.

---

## 4. Sağlık Politikaları Modülü

Simülasyonda yer alan her politika şu kriterleri karşılamaktadır:

1. WHO veya IHME GBD yayınlarında epidemiyolojik kanıtı mevcuttur.
2. Hangi risk faktörünü değiştirdiği açıkça tanımlanmıştır.
3. Kullanılan katsayı, kaynak türüyle birlikte (ampirik veri / muhafazakâr senaryo varsayımı) etiketlenmiştir.
4. Model içindeki etki fonksiyonu doğrudan risk faktörü değişkenine uygulanmakta; diğer politikaları etkilememektedir.

### 4.1 Politika Detayları

#### Hava Kirliliği Azaltımı (PM2.5)
- **Hedef Risk Faktörü:** PM2.5 Ortam Hava Kirliliği
- **Etkilenen Hastalıklar:** İskemik Kalp Hastalığı, İnme, KOAH, Akciğer Kanseri, Alt Solunum Yolu Enfeksiyonları
- **Katsayı Temeli:** IHME GBD Relative Risk eğrileri (PM2.5 maruziyeti başına)
- **Etki Formülü:** `pm25Level *= (1 - 0.25 × (intensity/100))`
- **Varsayım Statüsü:** *Orta Güçlü Senaryo Varsayımı.* Katı kentsel temiz hava bölgesi uygulamalarıyla PM2.5'in yerel ortalamada %25'e kadar düşürülebileceği varsayılmıştır.
- **Kaynak:** [WHO Ambient Air Quality Guidelines](https://www.who.int/news-room/fact-sheets/detail/ambient-(outdoor)-air-quality-and-health)

#### Tütün Kontrolü
- **Hedef Risk Faktörü:** Sigara Kullanım Prevalansı
- **Etkilenen Hastalıklar:** Akciğer Kanseri, KOAH, İskemik Kalp Hastalığı, İnme
- **Katsayı Temeli:** IHME GBD Relative Risk (Sigara → Akciğer Kanseri RR: 15–30; IHD RR: 2–3)
- **Etki Formülü:** `smokingPrevalence *= (1 - 0.15 × (intensity/100))`
- **Varsayım Statüsü:** *Ampirik Hedef Projeksiyonu.* WHO'nun MPOWER çerçevesinin tam uygulanmasıyla elde edilen %15 bağıl düşüş hedефine dayanmaktadır.
- **Kaynak:** [WHO Global Report on Tobacco Use Trends](https://www.who.int/publications/i/item/9789240039322)

#### Şekerli İçecek Vergisi
- **Hedef Risk Faktörü:** Şekerli İçecek (SSB) Tüketim İndeksi
- **Etkilenen Hastalıklar:** Tip 2 Diyabet, Kardiyovasküler Hastalıklar
- **Katsayı Temeli:** IHME GBD PAF (Yüksek Açlık Plazma Glukozu)
- **Etki Formülü:** `ssbConsumption *= (1 - 0.20 × (intensity/100))`
- **Varsayım Statüsü:** *Muhafazakâr Senaryo Varsayımı.* Gözlemlenen %20 fiyat esnekliğine dayanan tüketim düşüşü varsayımı.
- **Kaynak:** [WHO SSB Tax Policy Reports (2022)](https://www.who.int/news/item/13-12-2022-who-calls-on-countries-to-tax-sugar-sweetened-beverages)

#### Tuz Azaltma Stratejisi
- **Hedef Risk Faktörü:** Diyet Sodyum Alımı
- **Etkilenen Hastalıklar:** İnme, Hipertansif Kalp Hastalığı
- **Katsayı Temeli:** IHME GBD PAF (Yüksek Sistolik Kan Basıncı)
- **Etki Formülü:** `sodiumIntake *= (1 - 0.30 × (intensity/100))`
- **Varsayım Statüsü:** *Ampirik Hedef.* WHO'nun küresel sodyum alımında %30 bağıl düşüş hedefiyle uyumludur.
- **Kaynak:** [WHO SHAKE Technical Package for Salt Reduction](https://www.who.int/publications/i/item/WHO-NMH-NVI-16.4)

#### Alkol Kontrolü (Vergilendirme ve Reklam Kısıtlaması)
- **Hedef Risk Faktörü:** Alkol Tüketim İndeksi
- **Etkilenen Hastalıklar:** Trafik Kazaları, Sindirim Kanserleri, Karaciğer Sirozu
- **Katsayı Temeli:** WHO fiyat esnekliği tahminleri
- **Etki Formülü:** `alcoholConsumption *= (1 - 0.20 × (intensity/100))`
- **Varsayım Statüsü:** *Muhafazakâr Senaryo Varsayımı.* Vergilendirme politikalarının tüketimde %20'ye kadar bağıl düşüş sağlayacağı varsayılmıştır.
- **Kaynak:** [WHO Global Status Report on Alcohol and Health](https://www.who.int/publications/i/item/9789241565639)

#### Fiziksel Aktivite Altyapısı
- **Hedef Risk Faktörü:** Fiziksel Hareketsizlik Prevalansı
- **Etkilenen Hastalıklar:** İskemik Kalp Hastalığı, Tip 2 Diyabet
- **Katsayı Temeli:** IHME GBD Relative Risk (Düşük Fiziksel Aktivite)
- **Etki Formülü:** `physicalInactivity *= (1 - 0.10 × (intensity/100))`
- **Varsayım Statüsü:** *Muhafazakâr Senaryo Varsayımı.* Kentsel yürüyüş ve bisiklet altyapı yatırımlarının hareketsizlik prevalansını %10 oranında azaltabileceği kabul edilmiştir. Bu oran, %25 gibi daha agresif tahminlerin akademik açıdan savunulabilirliğinin güç olması nedeniyle bilinçli olarak düşürülmüştür.
- **Kaynak:** [WHO Global Action Plan on Physical Activity](https://www.who.int/publications/i/item/9789241514187)

> **Önemli Not:** Tüm katsayılar kesin epidemiyolojik sonuçlar bildirmemekte olup; fiyat/prevalans esnekliği (Price/Prevalence Elasticity), IHME GBD PAF ve RR analizlerinden türetilmiş **senaryo tabanlı karar destek projeksiyonlarıdır.** Her politika için varsayım statüsü (ampirik hedef / muhafazakâr senaryo / orta güçlü senaryo) kaynaklar panelinde ayrıca belirtilmektedir.

---

## 5. Harita ve Isı Haritası Modülü

Interaktif SVG dünya haritası, her simülasyon yılı için **Kaba Ölüm Hızı (Crude Death Rate)** formülüyle renklendirilen bir ısı haritası üretir:

```
Crude Death Rate = (yearlyDeaths / population) × 1000
```

Renk skalası sarıdan koyu kırmızıya doğru ölüm hızının artışını temsil eder. Politika devreye alındığında, ilgili bölgenin rengi yıllar içinde daha sağlıklı (sarı) tonlara doğru kayar.

Haritada bir bölgeye tıklandığında tüm grafikler, ölüm istatistikleri ve CSV dışa aktarma yalnızca o bölgenin verisine filtrelenir.

---

## 6. CSV Dışa Aktarma

Simülasyon, `mortality_simulation_data.csv` adında aşağıdaki sütunlara sahip bir çıktı dosyası üretir:

| Sütun | Açıklama |
|:---|:---|
| `Year` | Simülasyon yılı (1990–2500) |
| `Region` | Bölge kodu (USA, IND, AFR vb.) |
| `Population` | O yılki bölge nüfusu (kişi) |
| `TotalDeaths` | Toplam ölüm sayısı |
| `PreventedDeathsComparedToBaseline` | Aktif politikaların önlediği tahmini ölüm sayısı (baseline ile karşılaştırma) |
| `CardioDeaths` | Kardiyovasküler hastalık ölümleri |
| `CancerDeaths` | Kanser ölümleri |
| `RespiratoryDeaths` | Kronik solunum hastalığı ölümleri |
| `DiabetesDeaths` | Diyabet ölümleri |
| `InfectiousDeaths` | Bulaşıcı hastalık ölümleri |
| `InjuryDeaths` | Yaralanma/kaza kaynaklı ölümler |
| `PM25_Level` | O yılki bölge PM2.5 seviyesi (µg/m³) |
| `Smoking_Prevalence_Pct` | Sigara kullanım prevalansı (%) |
| `SSB_Consumption_Index` | Şekerli içecek tüketim indeksi |
| `Sodium_Intake_Index` | Sodyum alım indeksi |
| `Alcohol_Consumption` | Alkol tüketim indeksi |
| `Physical_Inactivity` | Fiziksel hareketsizlik prevalansı (%) |
| `ActivePolicies` | O senaryoda aktif olan politikaların listesi |

`PreventedDeathsComparedToBaseline` sütunu; aktif politika senaryosundaki ölüm sayısı ile aynı kohort ve nüfus için hiçbir politika uygulanmadığı ("BASELINE") durumundaki ölüm sayısı arasındaki fark olarak hesaplanmaktadır.

---

## 7. Kullanıcı Arayüzü

- **Çok Dilli Destek:** Türkçe/İngilizce dinamik dil geçişi
- **Simülasyon Kontrolü:** Play/Pause, yıl slider'ı, sıfırlama
- **Gerçek Zamanlı Log Terminali:** Her politika aktivasyonu, intensity değişikliği ve yıl geçişi kayıt altına alınır
- **Kaynaklar ve Varsayımlar Paneli:** Her politika için kaynak URL, hedef risk faktörü, etkilenen hastalıklar, katsayı temeli ve varsayım statüsünü gösteren şeffaf açıklama ekranı
- **Bölge Filtresi:** Harita üzerinden veya listeden bölge seçimi yapılabilir

---

## 8. Akademik Sınırlılıklar

Bu simülasyon, aşağıdaki sınırlılıkları açıkça kabul etmektedir:

1. **Doğrusal Katsayılar:** Tüm politika etki fonksiyonları doğrusal (linear) formüller kullanmaktadır. Gerçek epidemiyolojik eğriler genellikle doğrusal değildir (azalan marjinal getiri, eşik etkileri vb.).
2. **İkame Etkisi Yok:** Şekerli içecek tüketiminin azalması durumunda bireylerin başka şeker kaynaklarına yönelmesi gibi davranışsal ikame etkileri modele dahil edilmemiştir.
3. **Bölgesel Homojenlik Varsayımı:** Her makro-bölge içindeki alt-ülke varyasyonu yoksayılmıştır; bölge tek bir ortalama değerle temsil edilmektedir.
4. **Politika Etkileşimleri:** Birden fazla politikanın eş zamanlı uygulanması durumunda sinerjik veya antagonist etkileşimler ayrıca modellenememiştir; çarpanlar bağımsız olarak uygulanmaktadır.
5. **Veri Güncelliği:** Başlangıç risk faktörü değerleri WHO 2024 verileri için proxy tahminlerdir; ulusal düzeyde yıllık güncelleme yapılmamaktadır.

---

## 9. Kurulum ve Çalıştırma

Herhangi bir kurulum, sunucu veya bağımlılık gerektirmez.

```
simülasyon/
└── index.html   ← Tek dosya. Tarayıcıda doğrudan açılır.
```

Dosyayı modern bir tarayıcıda (Chrome, Firefox, Edge) açmak yeterlidir. Tüm hesaplama istemci tarafında (client-side) gerçekleşir.

---

*Bu belge, simülasyonun akademik teslim ve raporlama sürecine yönelik hazırlanmıştır.*
