# KÜRESEL EPİDEMİYOLOJİ VE DEMOGRAFİ SİMÜLASYONU 
## Kapsamlı Mimari ve Sistem Tasarım Belgesi (V2.0 - Final)

Bu belge, küresel çapta insan ölüm oranlarını, epidemiyolojik eğilimleri ve demografik değişimleri modelleyen gelişmiş simülasyon motorunun tüm teknik, kavramsal ve işlevsel altyapısını detaylandırmaktadır. Model, klasik bir veri gösterim aracından ziyade, **"Kohort Tabanlı (Cohort-based)"** ve **"Gecikmeli Etkileri Hesaplayabilen (Lagged Impact)"** kompleks bir karar destek sistemi olarak kurgulanmıştır.

---

## 1. SİMÜLASYON MOTORUNUN ÇEKİRDEK MİMARİSİ (CORE ENGINE)

Simülasyon, deterministik algoritmalar ile rastgele (stochastic) olayları harmanlayan hibrit bir zaman motoru (Time-Engine) kullanmaktadır.

* **Tarihsel Baseline (1990-2026):** Gerçek dünya verileri referans alınarak geçmiş 36 yıllık demografik değişimler modelin doğrulanması (validation) amacıyla veri tabanında tutulmaktadır.
* **Gelecek Projeksiyonu (2026-2500):** Sisteme entegre edilen yapay zeka parametreleri ve sağlık politikaları doğrultusunda, insanlığın önümüzdeki yaklaşık 500 yıllık serüveni modifiye edilebilir şekilde simüle edilmektedir.
* **Saniyede İşlenen Veri Hacmi:** Motor, çalıştırıldığı her döngüde (her simülasyon yılında) 9 farklı küresel bölge, 5 farklı yaş grubu ve 3 farklı gelir düzeyi için eşzamanlı karmaşık ölüm/doğum denklemleri çözmektedir.

---

## 2. EPİDEMİYOLOJİK MATRİS VE ÖLÜM SEBEPLERİ (MORTALITY MATRIX)

Sistemdeki insan ölümleri tek bir istatistik olmaktan çıkarılarak, Dünya Sağlık Örgütü (WHO) standartlarına uygun **4 Temel Kategori** altında detaylı alt kırılımlara ayrılmıştır:

1. **Bulaşıcı Olmayan Hastalıklar (NCDs - %70+):** 
   * Kardiyovasküler (Kalp-Damar) Hastalıklar, Kanser Tipleri, Kronik Solunum Yolu Hastalıkları, Diyabet ve Böbrek Yetmezliği. (Özellikle Yüksek ve Orta gelir grupları ile yaşlı popülasyonu hedefler).
2. **Bulaşıcı ve Bulaşıcı Olmayan (Communicable - %20):**
   * Solunum Yolu Enfeksiyonları, Su Kaynaklı Bulaşıcılar (Kolera vb.), İhmal Edilen Tropikal Hastalıklar (NTDs). (Özellikle Afrika ve Düşük gelirli bölgelerde simüle edilir).
3. **Yaralanmalar (Injuries - %8-10):**
   * İş Kazaları, Trafik Kazaları ve Şiddet Olayları. (Genç/Yetişkin popülasyonda yoğunlaşır).
4. **Anne ve Yenidoğan Ölümleri (Maternal/Neonatal):**
   * Sağlık altyapısı gelişmemiş bölgelerdeki doğum komplikasyonları.

---

## 3. SOSYOEKONOMİK VE MEKANSAL KATMANLANDIRMA

Simülasyon dünyası homojen değildir. Her bir coğrafya kendi ekonomik gerçekliğine göre tepki verir:
* **9 Küresel Makro-Bölge:** Kuzey Amerika, Latin Amerika, Avrupa & Orta Doğu, Afrika, Güney Asya, Doğu Asya vb.
* **Gelir Düzeyi Parametreleri:** (Düşük, Orta, Yüksek). Yüksek gelirli bölgelerde ortalama yaşam süresi (Life Expectancy) otomatik olarak daha yüksek başlar, ancak kanser ve obezite katsayıları fazladır.
* **Yaş Kohortları:** Popülasyon Bebek (0-4), Çocuk (5-14), Genç (15-29), Yetişkin (30-69) ve Yaşlı (70+) olmak üzere 5 segmente ayrılmıştır.

---

## 4. DİNAMİK POLİTİKALAR VE İNTERJENERASYONEL (NESİLLER ARASI) ETKİLER

Bu projenin en profesyonel yetkinliği, politikaların **Gecikmeli Etki (Lagged Effect)** prensibiyle çalışmasıdır. Sisteme dahil edilen başlıca politikalar ve senaryolar şunlardır:

### 4.1. Aktif Halk Sağlığı Politikaları
* **Çocuklara Süt Programı (School Milk Program):** Bu politika, devreye alındığı an ölümleri aniden düşürmez. Çocuk kohortunda kalsiyum eksikliğini giderir. O çocuklar simülasyonda yaşlanıp 70+ yaşına geldiklerinde, küresel çaptaki "Kemik Erimesi (Osteoporoz)" kaynaklı ölümcül kırık ve düşme vakalarında dramatik bir kırılma yaratır.
* **Evrensel Sağlık Sigortası:** Düşük gelirli grupların tedaviye erişimini artırarak anne/bebek ve enfeksiyon ölümlerini baskılar.
* **Haftada 4 Gün Mesai Sistemi:** Stres seviyelerini ve uyku yoksunluğunu düşürerek Yetişkin kohortunda kalp krizlerini ve iş kazalarını ciddi oranda engeller.
* **Dizel Yasağı & Obezite Reklam Yasağı:** Kronik solunum yolu hastalıklarını ve kalp-damar tıkanıklıklarını azaltan hedefe yönelik regülasyonlar.

### 4.2. Yapay Zeka Tarafından Yönetilen Makro Senaryolar
* Küresel İklim Krizi (Warming), Biyoteknoloji Devrimi (Nanobots), İçilebilir Su Altyapısı ve Laboratuvar Üretimi Sentetik Et tüketimine geçiş gibi makro değişkenler; bölgesel ölüm oranlarını saniyeler içinde baştan aşağı yeniden yapılandırır.

---

## 5. KULLANICI ARAYÜZÜ (UI/UX) VE VERİ GÖRSELLEŞTİRME

Tasarım dili, veri yoğunluklu bir komuta merkezi (Cyberpunk / Dashboard) estetiğine sahiptir:
* **İnteraktif Vektörel Harita:** Anlık ölüm hızlarını (Crude Mortality) ve Sağlık İndekslerini "Isı Haritası (Heatmap)" formatında renklendiren interaktif SVG tabanlı dünya haritası.
* **Real-time Veri Ticker'ları:** Saniyede güncellenen Nüfus, Günlük Ortalama Ölüm ve Lider Ölüm Sebebi panoları.
* **Gelişmiş Filtreleme:** Yıl, Bölge, Yaş Grubu ve Gelir Düzeyine göre eşzamanlı veri filtreleme yapabilen akıllı sol kontrol paneli.
* **Simülasyon Motor Günlükleri (Terminal Logs):** Simülasyonun arka planda aldığı kararları, uygulanan senaryoları ve istatistiksel uyarıları yazılımcı/araştırmacı gözünden gösteren canlı log terminali.
* **Zaman Çizelgesi Kontrolü:** İstendiği an durdurulabilen, devam ettirilebilen ve 1990'a sıfırlanabilen esnek kronoloji yöneticisi.

---

## 6. VERİ ÇIKTISI VE BİLİMSEL RAPORLAMA (CSV EXPORT ENGINE)

Geliştirilen bu yazılım sadece görsel bir şov değil, aynı zamanda araştırmacılar için devasa bir veri üretim (Synthetic Data Generation) laboratuvarıdır.
* **Veri İndirme Altyapısı:** Kullanıcı, uyguladığı politikaların dünyayı nasıl değiştirdiğini görmek için "İndir" fonksiyonunu kullanarak 1990-2500 yılları arasındaki tüm kohort, bölge ve ölüm nedeni istatistiklerini CSV formatında çekebilir.
* **Raporlanabilirlik:** Bu çıktılar Excel, SPSS veya Python Pandas gibi ortamlara aktarılarak karmaşık istatistiksel regresyon analizlerine ve bilimsel makale grafiklerine dönüştürülebilir. Modeli akademik standartlarda eşsiz kılan temel özellik budur.
