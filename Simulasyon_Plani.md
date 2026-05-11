# Proje: Küresel Mortalite ve Risk Yönetimi Simülasyonu

## 1. Projenin Amacı ve Kapsamı
Bu simülasyon, "Küresel İnsan Ölüm Oranları ve Epidemiyolojik Risk Faktörleri" üzerine inşa edilmiş makro ölçekli, veri odaklı bir sebep-sonuç ağı projesidir. Temel amaç; politika yapıcıların (devletler, Dünya Sağlık Örgütü vb.) alacağı sosyo-ekonomik ve sağlık kararlarının, toplumun günlük yaşam tarzına, maruz kaldığı çevresel toksisiteye ve nihayetinde spesifik hastalıkların (Kardiyovasküler, Onkolojik, Solunum vb.) ölüm oranlarına olan çok boyutlu etkisini modellemektir. Sistem, alınan kararların toplum sağlığı üzerindeki uzun vadeli iz düşümünü ve ortalama yaşam beklentisindeki (Life Expectancy) dalgalanmaları interaktif olarak analiz etme imkanı sunar.

## 2. Sistemin Çalışma Prensibi ve Simülasyon Mantığı
Simülasyon motoru, Dünya Sağlık Örgütü (WHO) ve Küresel Hastalık Yükü (IHME Global Burden of Disease) verilerinin sunduğu gerçek dünya baz oranlarını (baseline) başlangıç noktası (2026 yılı) olarak kabul eder. Zaman ekseni ivmelendirilmiş şekilde ilerlerken, "Risk Çarpanları Modeli" (Risk Multiplier Model) devreye girer. Kullanıcı, sisteme dışarıdan "Sağlık Politikaları" adı altında müdahalelerde bulunduğunda, algoritma bu müdahalelerin risk faktörleri üzerindeki matematiksel etkisini saniyelik bazda yeniden hesaplayarak nihai mortalite oranlarını (ölüm istatistiklerini) dinamik olarak günceller.

## 3. Sistemdeki Veri Üreticileri (Ajanlar ve Metrikler)
Sistem mimarisi, birbiriyle entegre çalışan temel dinamikler üzerine kurulmuştur:
*   **Demografik ve Biyolojik Veriler:** Toplam küresel nüfus, ortalama yaşam beklentisi, yaşlanma ve kalıtımsal yatkınlıklar gibi doğal süreçleri simüle eder.
*   **Davranışsal ve Çevresel Risk Faktörleri:** Hareketsiz yaşam (Sedanter), rafine şeker tüketimi, uyku yoksunluğu, havadaki partikül madde oranı (PM 2.5), kalsiyum eksikliği ve kronik stres gibi dinamik değişkenler.
*   **Hastalık Sınıflamaları:** Kalp ve Damar Hastalıkları, Onkolojik vakalar, KOAH/Solunum Yolu Hastalıkları, Tip-2 Diyabet & Böbrek Yetmezliği, Kazalar ve Enfeksiyonlar.

## 4. Etki-Tepki Mekanizması: Değişkenler ve Domino Etkisi
Simülasyon ekosisteminde her değişken doğrusal olmayan bir ağa (Network) bağlıdır. Uygulanan bir politika, genellikle bir faktörü iyileştirirken bir başkasında yan etki yaratabilir:
*   **Kalsiyum Destek Programları (Süt Tüketimi):** Erken yaşta uygulandığında, geriatrik (yaşlılık) dönemdeki osteoporoz kaynaklı kalça kırığı komplikasyonlarını ve yatağa bağımlı mortaliteyi ciddi oranda düşürür.
*   **Beslenme ve Obezite Regülasyonları:** Örneğin "Şeker Vergisi" veya "Fast-Food Yasağı", insülin direncini ve obeziteyi kırarak Tip-2 Diyabet ve böbrek yetmezliği kaynaklı ölümleri domine eder. Ancak kısa vadeli psikolojik stresi minör oranda artırabilir.
*   **Çevresel ve Çalışma Politikaları:** Şehir içi dizel yasağı veya bisiklet altyapısına yatırım (Aktif Şehir) gibi kararlar hem PM 2.5 (hava kirliliği) oranını düşürerek KOAH ölümlerini engeller, hem de kardiyovasküler dayanıklılığı artırır.
*   **Modern Yaşam Regülasyonları:** "Haftada 4 Gün Mesai" veya "Ücretsiz Psikolojik Destek", toplumdaki otonom sinir sisteminin aşırı uyarılmasını (kronik kortizol/stres) dengeleyerek ani kalp krizlerini ve dikkat eksikliğine bağlı ölümcül kazaları dramatik biçimde azaltır.

## 5. Analitik Çıktılar ve Veri Görselleştirme
Simülasyon esnasında oluşan veri seti, kullanıcı arayüzüne (UI) canlı olarak şu formatlarda aktarılır:
*   **Makro Trend Analizi:** Zaman serisi (Time-series) çizgisel grafiklerle toplumun ortalama yaşama süresinin yıllara göre evrimi.
*   **Epidemiyolojik Dağılım:** Hastalık sınıflarının genel ölüm içerisindeki payını anlık olarak yansıtan dinamik "Doughnut (Halka)" grafikler.
*   **Risk Monitörü:** Risk faktörlerinin anlık durumunu (güvenli, riskli, kritik seviye) renklendirilmiş skalalarla (Bar chart) gösteren telemetri paneli.
*   **Sistem Logları:** Uygulanan politikaların etkilerini ve rastgele oluşan sosyo-ekonomik şokların (örn: Küresel Krizler) veri tablosuna yansımalarını kaydeden anlık bildirim ekranı.
