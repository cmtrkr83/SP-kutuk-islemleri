# AGENTS.md

Bu dosya, yapay zeka asistanlarının bu projede çalışırken uyması gereken kuralları ve kullanması gereken komutları tanımlar.

## Proje Yapısı

```
/Users/cem/Desktop/excel/
├── index.html          # Ana uygulama (tek dosya: HTML + CSS + JS)
├── favicon.png         # Tarayıcı sekme ikonu
└── README.md           # Proje açıklaması
```

## Çalışma Kuralları

### Dosya Düzenleme
- Tüm uygulama tek bir `index.html` dosyasında bulunur
- CSS: `<style>` bloğu içinde (satır ~13-460)
- HTML: `<body>` içinde (satır ~464-870)
- JavaScript: `<script>` bloğu içinde (satır ~873-son)
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
- Dışlanma koşulları:
  - Okul adı "kademe" içeriyor mu → tüm satırlar dışlanır
  - Şube adı "otistik" içeriyor mu → dışlanır
  - Şube adı "zihinsel" içeriyor mu → dışlanır
- `getExclusionReason()` → dışlanma nedenini string olarak döner
- Normalize: Türkçe karakterler ASCII'ye dönüştürülür (İ→i, ğ→g, ü→u, ş→s, ı→i, ö→o, ç→c)

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

## Değişiklik Yaparken Dikkat

1. **XSS koruması**: Kullanıcıdan gelen verileri HTML'e dökerken her zaman `escapeHtml()` kullan
2. **Sayfalama**: Tüm tablolar 50 satır sayfalama ile gösterilir
3. **Responsive**: `@media` kurallarına dikkat et, mobil uyumlu ol
4. **Print**: `@media print` kuralları var, yazdırma stilini bozma
5. **Event delegation**: Tablo satırları için event delegation kullan (dynmaik HTML)
6. **Global değişken**: Yeni global değişken eklerken `let/const` ile tanımla, `window.` prefix kullanma

## Yaygın Hatalar

- `escapeHtml()` kullanılmadan HTML'e veri eklemek → XSS açığı
- Sütun indexlerini karıştırmak → yanlış veri gösterimi
- `normalizeHeaderText()` kullanmadan büyük/küçük harf karşılaştırması → eşleşmeme
- `classList` yerine `style.display` kullanmak → CSS uyumsuzluğu
