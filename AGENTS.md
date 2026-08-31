# AGENTS.md

Bu dosya, yapay zeka asistanlarının bu projede çalışırken uyması gereken kuralları ve kullanması gereken komutları tanımlar.

## Proje Yapısı

```
/Users/cem/Desktop/excel/
├── index.html          # Ana uygulama (tek dosya: HTML + CSS + JS)
├── favicon.png         # Tarayıcı sekme ikonu
├── AGENTS.md           # Bu dosya
└── README.md           # Proje açıklaması
```

## Çalışma Kuralları

### Dosya Düzenleme
- Tüm uygulama tek bir `index.html` dosyasında bulunur
- CSS: `<style>` bloğu içinde
- HTML: `<body>` içinde
- JavaScript: `<script>` bloğu içinde
- Yeni özellik eklerken mevcut kod stilini ve yapıyı koru

### Kod Stili
- Değişken isimleri: camelCase (`schoolCount`, `districtData`)
- Fonksiyon isimleri: camelCase (`renderSchoolTable`, `applyDistrictFilter`)
- DOM element ID'leri: camelCase (`schoolTableWrap`, `reportDistrictFilter`)
- Global değişkenler: `window.` prefix kullanma, doğrudan `let/const` ile tanımla
- Event listener'lar: `addEventListener` ile bağla, inline `onclick` kullanmaktan kaçın (pagination hariç)
- Türkçe karakter normalizasyonu: `normalizeHeaderText()` fonksiyonunu kullan

### Test
- Uygulama istemci taraflıdır, sunucu gerektirmez
- Test için: `open /Users/cem/Desktop/excel/index.html`
- Tarayıcı konsolunda hata kontrolü yapılabilir

## Mevcut Özellikler

### Filtreleme Mantığı
- `shouldExcludeBySchoolOrBranch(schoolName, branchName)` → true/false döner
- **Okul hariç tutma** (normalize edilmiş):
  - `"kademe"` → tüm satırlar dışlanır
  - `"engelli"` → tüm satırlar dışlanır
  - `"engelliler"` → tüm satırlar dışlanır
  - `"ozel egitim uygulama"` → tüm satırlar dışlanır
- **Şube hariç tutma** (normalize edilmiş):
  - `"hafif"` → dışlanır
  - `"zihinsel"` → dışlanır
  - `"engelli"` → dışlanır
  - `"engelliler"` → dışlanır
  - `"isitme"` → dışlanır
  - `"otistik"` → dışlanır
- `getExclusionReason()` → dışlanma nedenini string olarak döner
- Normalize: Türkçe karakterler ASCII'ye dönüştürülür (İ→i, ğ→g, ü→u, ş→s, ı→i, ö→o, ç→c)
- web-system-core projesiyle aynı filtre kelimeleri kullanılır

### Etiket Sistemi
- `buildLabelSheetHtml(items, cols, rows, colorObj)` → A4 sayfa HTML'i üretir
- `LABEL_COLORS` dizisi: 10 renk kutucuğu, her biri tam renk seti içerir (`{primary, border, bg, text, muted}`)
- Renk seçimi: kutucuğa tıklama ile yapılır, tıklanınca otomatik `previewLabels()` çağrılır
- Varsayılan satır sayısı: 8
- Varsayılan sütun sayısı: 3
- Sol sidebar: district adı dikey yazılı, renkli arka plan
- İçerik: okul kodu + adı, istatistik kutuları (Şube Sayısı / Öğrenci Sayısı)
- Dinamik font: hücre boyutuna göre otomatik ölçekleme

### Sütun Eşleştirme
- `detectColumns(headers)` → ilçe, okul, kurum kodu, şube sütunlarını bulur
- `computeSearchDisplayCols(headers)` → arama sonucu gösterilecek sütunları belirler
- Pattern matching: `normalizeHeaderText()` ile normalize edilmiş başlık aranır

### Grafikler
- Chart.js kullanılır
- Öğrenci türü: doughnut grafik
- İlçe dağılımı: yığılmış bar grafik (normal + özel eğitim)

### Veri Saklama
- `window.rawExcelData`: Ham Excel verisi (filtre öncesi)
- `window.fullExcelData`: Filtrelenmiş veri (İçerik: başlıklar + satırlar)
- `window.DataService.saveToIndexedDB()`: IndexedDB'ye kaydetme
- `window.SqlDB`: sql.js tabanlı SQL veritabanı

### Sidebar
- `.twitter-card` kutusu: `opacity: 0.2`, `text-align: center`
- `.nav-item` font boyutu: `15px` (kodda `index.html:52`)

## Değişiklik Yaparken Dikkat

1. **XSS koruması**: Kullanıcıdan gelen verileri HTML'e dökerken her zaman `escapeHtml()` kullan
2. **Sayfalama**: Tüm tablolar 50 satır sayfalama ile gösterilir
3. **Responsive**: `@media` kurallarına dikkat et, mobil uyumlu ol
4. **Print**: `@media print` kuralları var, yazdırma stilini bozma
5. **Tablo satır etkileşimi**: Satırlar render sonrası `querySelectorAll` + `forEach` ile doğrudan bağlanır; pagination butonları için inline `onclick` kullanılır ( `index.html:1714` civarı )
6. **Global değişken**: Yeni global değişken eklerken `let/const` ile tanımla, `window.` prefix kullanma
7. **Etiket renkleri**: Yeni renk eklerken `LABEL_COLORS` dizisine `{name, value, border, colors}` objesi ekle
8. **Dinamik görünürlük**: `labelDistrictRow` / `labelSchoolRow` / `labelBranchRow` / `receiptDistrictRow` gibi koşullu alanlarda `style.display` kullanılır

## Yaygın Hatalar

- `escapeHtml()` kullanılmadan HTML'e veri eklemek → XSS açığı
- Sütun indexlerini karıştırmak → yanlış veri gösterimi
- `normalizeHeaderText()` kullanmadan büyük/küçük harf karşılaştırması → eşleşmeme
- Etiket fonksiyonlarına `colorScheme` stringi yerine `colorObj` göndermek → renk çalışmaz
