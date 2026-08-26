# Zırhsız Kurtarıcı Araç — Bilgi Paketi Metodolojisi (Taslak v1)

## 1. Neden bu yapı?

Kara araçları için hazır, tek bir "bilgi seti şablonu" yok — ancak ABD Kara Kuvvetleri'nin
**MIL-STD-40051** standardı, tam bizim aradığımız yapıyı (Operatör El Kitabı / Birlik-Fabrika
Bakım El Kitabı / Yedek Parça Kataloğu) yıllardır kullanıyor. Bunu Lahika-1 (S1000D Genel
Bilgi Seti → şema) ve S1000D Chapter 8.4.1 (infoCode kütüphanesi) ile birleştirerek projeye
özel bir metodoloji kuruyoruz.

| MIL-STD-40051 Bölümü | Karşılığı | S1000D infoCode aralığı | Şema |
|---|---|---|---|
| Appendix B — General Info, Equipment Description, Theory of Operation | Genel Tanıtım / Teknik Özellikler | 040-046 | descript |
| Appendix C — Operator Instructions | Kullanıcı (Operatör) El Kitabı içeriği | 100-199 | crew, descript, proced |
| Appendix D — Troubleshooting Procedures | Arıza Tespit/İzolasyon | 400-449 | fault |
| Appendix E — Maintenance Instructions | Bakım/Onarım El Kitabı (Servis, Söküm-Takma, Onarım) | 200-299 (servis), 500-599 (söküm), 600-699 (onarım), 700-762 (montaj) | descript, proced |
| Appendix F — RPSTL (Repair Parts and Special Tools List) | Resimli Parça Kataloğu | 941, 942 | ipd |
| Appendix I — BDAR (Battle Damage Assessment and Repair) | Muharebe Hasar Değerlendirme/Onarım | 680-693, 278-279 | proced |
| Appendix J — PMC (Preventive Maintenance Checklist) | Bakım Planlama | 0B0-0B5, 280-289 | schedul |
| Appendix K — Lubrication Orders | Yağlama Talimatı | 240-243 | proced |

Bu tablo, üç ana yayın türünün (Kullanıcı El Kitabı / Bakım El Kitabı / IPC) hangi
infoCode'ları kapsayacağını netleştiriyor. Şimdi bunu **ürün ağacı seviyesine** bağlıyoruz.

## 2. Ürün ağacı seviyesi × davranış kategorisi mantığı

Bir bakım teknisyeninin sorduğu sorular üzerinden 5 kategori:

| Kategori | Teknisyen sorusu | infoCode paketi | Şema |
|---|---|---|---|
| **A — Üst Montaj** (Sistem/Alt Sistem/Alt-alt Sistem bütünü, örn. "Motor") | Bu nedir, nasıl çalışır, genel arızası nasıl teşhis edilir, bakım planı nedir? | 040, 042, 0B0(schedul), 410/420, 941(patlatma) | descript, schedul, fault, ipd |
| **B — Fonksiyonel/Arızalanabilir Bileşen (LRU)** | Bu parça arızalanır mı, test edilir mi, söküp takılır mı? | 040, 200, 300, 410/420, 520, 710/720, 941 | descript, proced, fault, ipd |
| **C — Sarf/Periyodik Değişim Parçası (SRU)** | Arızası yok, sadece ömrü doluyor/kirleniyor, değiştiriliyor | 040, 520, 710, 941 | descript, proced, ipd |
| **D — Yapısal/Kapak/Muhafaza** | Arızalanmaz ama kirlenir/hasar görür, söküp takılır | 040, 250(clean), 520, 710, 941 | descript, proced, ipd |
| **E — Elektrik/Kablo/Tesisat eklentisi** | Yukarıdaki kategorilere ek olarak kablo/bağlantı bilgisi var mı? | + 051-058 | + wrngdata |

**Motor örneğiniz üzerinden doğrulama:**
- Motor (bütün, Alt-alt Sistem) → **Kategori A**
- Motor kapağı (kirlenir, arızalanmaz) → **Kategori D** ✓ (sizin verdiğiniz örnekle birebir)
- Yakıt pompası (arızalanabilir, test edilir) → **Kategori B**
- Yakıt filtresi elemanı (arızası izlenmez, periyodik değişir) → **Kategori C**
- Motor kablo demeti → **Kategori B/D + E**

## 3. Yayın Modülü (PM) ile ilişki

Her kategori, aynı zamanda hangi **el kitabında** (PM) yer alacağını da belirliyor:

- Kategori A, B, D'nin `descript` (040) kısmı → **Kullanıcı El Kitabı**'na da girebilir (genel tanıma düzeyinde), **proced** (500-700) kısmı → **Bakım El Kitabı**'na girer.
- Kategori B, C, D'nin `520/710` (söküm-takma) prosedürleri → seviyeye göre **Birlik (Unit)** veya **Fabrika (Depot)** bakım el kitabına ayrılabilir — bu ayrım MSB İş Kuralları'nda henüz netleşmemiş bir konu, birlikte kararlaştırmamız gerekecek.
- Tüm kategorilerin `941` (IPD) kısmı → **Resimli Parça Kataloğu**'na girer.
- Kategori A'nın `410/420` (arıza) kısmı → **Arıza Tespit El Kitabı / Troubleshooting** bölümüne girer.

## 4. Sonraki adım

Bu 5 kategoriyi (A-E) onaylarsanız:
1. SNS listesindeki 707 bileşenin başlıklarını + varsa mühendislik bilgisini kullanarak
   ön-sınıflandırma yapacağım (öneri niteliğinde, sizin onayınızla kesinleşecek)
2. Sistem/Alt Sistem/Alt-alt Sistem seviyelerinin tamamına Kategori A uygulanacak
3. Her kategori için infoCode + şema ana DMC tablosuna işlenecek
4. Kalem Konum Kodu (A/B/C/D/T) kategoriye göre türetilecek (örn. Kategori B/C/D bileşenler
   genelde "B" — üst montajdan sökülen ana bileşen üzerine kurulu obje)

**Netleştirilmesi gereken açık nokta:** Birlik (Unit) / Fabrika (Depot) bakım seviyesi ayrımı
henüz kurallara bağlanmadı — bu ikisi aynı proced DM'ini mi paylaşacak yoksa ayrı mı
üretilecek? Bu, MPG/MSB İş Kuralları'nda netleşince kategorilere işlenecek.
