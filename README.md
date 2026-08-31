# Excel Kütük Analiz Web Uygulaması

Tek dosyalı (`index.html`) bir web uygulamasıdır.
Excel kütük verilerini tarayıp okul/ilçe bazlı analiz, arama, raporlama, etiket ve tutanak oluşturma işlemlerini tarayıcı üzerinde yapar.

## Özellikler

### Veri Yükleme ve Analiz
- `.xlsx` ve `.xls` formatında Excel dosyası yükleme (sürükle-bırak veya tıkla)
- Dosya boyutuna göre ilerleme çubuğu ile yükleme durumu
- Büyük veri setleri için SqlDB (IndexedDB) ve JSON yedekleme

### İstatistik Kartları
- Toplam Okul Sayısı
- Toplam Öğrenci Sayısı
- Toplam İlçe Sayısı
- Resmi Kurum Öğrenci Sayısı
- Resmi Kurum Şube Sayısı
- Özel Eğitim Öğrenci Sayısı (filtre açıkken dışlanan öğrenci sayısını kırmızı ile gösterir)
- Özel Eğitim Kurumu Sayısı (filtre açıkken dışlanan kurum sayısını kırmızı ile gösterir)
- Özel Okul Sayısı
- Özel Okul Öğrenci Sayısı
- Özel Okul Şube Sayısı

### Filtreleme
- KADEME, otistik ve zihinsel engelli sınıflarını dışlayan filtre
- Filtre açık/kapalı anahtarı ile anında geçiş
- Dışlanan kayıt sayısı kartlarda kırmızı ile gösterilir

### Grafikler
- Öğrenci Türü Dağılımı (halka/grafik): Normal, Engelli, Özel Kurum
- İlçelere Göre Öğrenci Dağılımı (yığılmış çubuk grafik): Normal ve özel eğitim öğrencileri ayrıştırılmış

### Arama
- Tüm sütunlarda veya belirli sütunda arama
- Sayfalı sonuç listesi (50'şer satır)
- Arama sonuçlarını Excel'e aktarma

### Okul Raporu
- İlçe ve okul bazlı filtreleme
- Okul listesi tablosu (ilçe, kurum kodu, okul adı, öğrenci sayısı, şube sayısı, özel eğitim)
- Okul tıklama ile şube detayı görüntüleme
- Şube bazlı sınıf listesi yazdırma (A4)
- Tüm okulları Excel'e aktarma
- İlçe bazlı toplu yazdırma

### İlçe Raporu
- İlçe bazlı filtreleme
- İlçe özet raporu (okul, şube, öğrenci, özel eğitim sayıları)
- İlçe raporu yazdırma ve Excel'e aktarma
- İlçe özet modal penceresi

### Etiket Oluştur
- Okul etiketi veya şube etiketi oluşturma
- Kapsam seçimi: Tüm okullar, seçili ilçe, seçili okul
- Sayfa düzeni: Sütun × Satır sayısı ayarı
- 10 farklı arkaplan rengi
- A4 boyutunda önizleme ve yazdırma

### Tutanak Oluştur
- Merkez → İlçe teslim tutanağı
- İlçe → Okul teslim tutanağı
- Tarih, başlık, düzenleyen bilgisi girişi
- A4 yatay formatta yazdırma

### Yedekleme
- Tüm veriyi JSON olarak indirme

## Dosyalar

| Dosya | Açıklama |
|-------|----------|
| `index.html` | Uygulamanın tüm HTML/CSS/JS kodu (tek dosya) |
| `favicon.png` | Tarayıcı sekme ikonu |

## Kurulum

Kurulum gerektirmez. `index.html` dosyasını tarayıcıda açmanız yeterlidir.

## Kullanım

1. `index.html` dosyasını tarayıcıda açın
2. Excel dosyanızı sürükleyip bırakın veya "Dosya Se" butonuyla seçin
3. Sol menüden ilgili modülü seçerek işlemlerinizi yapın:
   - **Ana Sayfa** — İstatistik kartları ve grafikler
   - **Arama Yap** — Öğrenci ve veri arama
   - **Okul Raporu** — Okul listesi, şube detayları, sınıf listesi
   - **İlçe Raporu** — İlçe bazlı analiz ve raporlar
   - **Etiket Oluştur** — Okul ve şube etiketleri
   - **Tutanak Oluştur** — Teslim tutanakları
   - **Yedekleme** — JSON yedek indirme

## Teknik Detaylar

- **Tek dosya mimarisi**: Tüm uygulama tek bir HTML dosyasında (HTML + CSS + JS)
- **Kütüphaneler**: XLSX.js (Excel okuma), Chart.js (grafikler), sql.js (IndexedDB tabanlı SQL)
- **Veri saklama**: IndexedDB + SqlDB (büyük veri için), localStorage (başlıklar için)
- **Güvenlik**: XSS koruması için `escapeHtml()` fonksiyonu kullanımı
- **Tarihçe**: 3.000+ satır JavaScript

## Tarayıcı Desteği

En iyi deneyim için Chrome, Edge veya Firefox tarayıcısı önerilir.
