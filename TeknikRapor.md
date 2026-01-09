## 1. Kullanılan Yöntemler ve Seçim Nedenleri 

Bu projede amaç, tek bir insan fotoğrafı üzerinden kilo alma ve kilo verme
durumlarını görsel olarak simüle eden, temel seviyede fakat çalışan bir prototip
geliştirmektir.

### Vücut Segmentasyonu

Vücut segmentasyonu için "rembg" kütüphanesi kullanılmıştır.
Bu kütüphane, U²-Net tabanlı önceden eğitilmiş bir model kullanarak
insanı arka plandan ayırmaktadır.

U²-Net tercih edilmesinin temel nedeni, daha önce medikal görüntüleme alanında
karaciğer segmentasyonu projelerinde **U-Net ve U-Net++ mimarileriyle çalışmış olmamdır**.
Bu mimarilerin encoder–decoder yapısına ve çok ölçekli özellik çıkarımına aşina
olmam, U²-Net tabanlı bir yaklaşımı hızlı ve bilinçli şekilde kullanmamı sağlamıştır.
Hala devam ediyor bu çalışmam. Bu nedenle, prototip geliştirme süresi kısıtlı olduğu için
model eğitimi yerine, önceden eğitilmiş ve güvenilir bir mimariyi
kullanan bir çözüm tercih edilmiştir. Segmentasyon sonucu elde edilen alpha kanalı,
vücut maskesi olarak kullanılmıştır.

---

### Kilo Alma / Verme Simülasyonu

Segmentasyon ile elde edilen vücut bölgesine
geometrik deformasyon uygulanmıştır.

Bu amaçla:
- Görüntünün bel–kalça bölgesini temsil eden sabit bir yatay bant seçilmiştir.
- Bu bant üzerinde x ekseninde ölçekleme (affine scaling) uygulanmıştır.
  - Genişletme → kilo almış görünüm
  - Daraltma → zayıflamış görünüm

Bu yaklaşım, karmaşık vücut modelleme veya 3D yapı gerektirmeden
kilo değişimini görsel olarak ifade edebilmek için tercih edilmiştir.

---

### Görüntü Birleştirme

Deforme edilen vücut bölgesi, orijinal fotoğraf ile
alpha blending yöntemi kullanılarak birleştirilmiştir.
Kenar geçişlerini yumuşatmak ve daha doğal bir görünüm elde etmek için
Gaussian blur uygulanmıştır.

---

## 2. Kullanılan Modeller, Kütüphaneler ve Veri

### Modeller
- U²-Net (rembg kütüphanesi içerisinde önceden eğitilmiş olarak)

### Kütüphaneler
- Python
- OpenCV
- NumPy
- Pillow
- rembg

### Veri
- Projede özel bir veri seti kullanılmamıştır.
- Testler, farklı açılardan çekilmiş bireysel insan fotoğrafları
  kullanılarak yapılmıştır.

---

## 3. Karşılaşılan Sorunlar ve Çözümleri

### Kütüphane ve Sürüm Uyumsuzlukları

Google Colab ortamında, özellikle aşağıdaki kütüphaneler arasında
sürüm uyumsuzlukları yaşanmıştır:
- NumPy
- OpenCV
- Pillow
- TensorFlow (Colab üzerinde ön yüklü olması nedeniyle)

Bu sorunlar:
- Belirli sürümlerin manuel olarak sabitlenmesi,
- Gereksiz veya çakışan kütüphanelerin kaldırılması,
- Runtime’ın yeniden başlatılması
yöntemleriyle çözülmüştür.

Bu süreç, farklı kütüphanelerin aynı ortamda birlikte çalıştırılmasının
pratikte karşılaşılabilecek sorunlarını anlamak açısından faydalı olmuştur.

---

### Segmentasyon Kenar Problemleri

Segmentasyon sonrası, vücut kenarlarında sert geçişler oluşmuştur.

Bu problem:
- Alpha maskesine Gaussian blur uygulanarak
geçişlerin yumuşatılması ile azaltılmıştır.

---

### Deformasyon Gerçekçiliği

Kilo alma / verme simülasyonu için basit affine ölçekleme kullanıldığı için
tam fotogerçekçi sonuçlar elde edilmemiştir.
Ancak bu çalışma, bir prototip olduğu için
amaçlanan görsel etkiyi yeterli seviyede sağlamaktadır.

---

## 4. Kullanılan Altyapı ve Kurulum Süreci

Proje, **Google Colab** üzerinde **CPU** kullanılarak geliştirilmiştir.
GPU kullanımı gerekmemektedir.

Kurulum sürecinde:
- Gerekli Python kütüphaneleri Colab ortamına kurulmuştur,
- Sürüm çakışmaları giderilmiştir,
- Çıktılar Colab üzerinde görselleştirilmiş ve GitHub’a aktarılmıştır.

Google Colab kullanımı, hızlı prototipleme ve
kolay hata ayıklama imkânı sunduğu için tercih edilmiştir.

---

## 5. Sonuç 

Bu çalışma, temel seviyede çalışan bir vücut kilo dönüşüm prototipi sunmaktadır.
Bu prototip, araştırma ve problem çözme becerilerini
göstermeyi amaçlayan bir çalışma olarak tasarlanmıştır.
