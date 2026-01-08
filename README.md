# Vücut Kilo Dönüşüm Prototipi

Bu proje, tek bir insan fotoğrafı üzerinden **kilo alma** ve **kilo verme** durumlarını
görsel olarak simüle eden **basit bir prototip** çalışmasıdır.

---

## 🎯 Amaç

Amaç, bir kişinin fotoğrafını alarak:

1. Kişiyi arka plandan ayırmak (vücut segmentasyonu)
2. Vücut bölgesine geometrik deformasyon uygulayarak
   - kilo almış
   - zayıflamış
   görünümler oluşturmak
3. Sonucu tekrar orijinal görsel ile birleştirmektir.


---

## 🧠 Kullanılan Yöntemler

### 1️⃣ Vücut Segmentasyonu
- `rembg` kütüphanesi kullanılmıştır.
- U²-Net tabanlı, önceden eğitilmiş bir model ile kişi arka plandan ayrılmıştır.
- Segmentasyon sonucu alpha maskesi olarak elde edilmiştir.

### 2️⃣ Kilo Alma / Verme Simülasyonu
- Segmentasyon sonucu elde edilen vücut bölgesi üzerinde
  **x ekseninde ölçekleme (affine deformation)** uygulanmıştır.
- Sabit bir bel-kalça bandı seçilerek:
y1 = int(h * 0.30)
y2 = int(h * 0.85)
  - genişletme → kilo almış görünüm
  - daraltma → zayıflamış görünüm
  elde edilmiştir.

### 3️⃣ Birleştirme (Blending)
- Deforme edilen vücut bölgesi,
  orijinal fotoğraf ile **alpha blending** kullanılarak birleştirilmiştir.
- Kenar geçişlerini yumuşatmak için **Gaussian blur** uygulanmıştır.

---

## 🛠️ Kullanılan Teknolojiler

- Python
- OpenCV
- NumPy
- rembg (U²-Net tabanlı segmentasyon)
- Google Colab

---

## ▶️ Nasıl Çalıştırılır?

1. `vucut.ipynb` dosyasını Google Colab üzerinde açın.
2. Bir insan fotoğrafı yükleyin.
3. Hücreleri sırayla çalıştırın.
4. Aşağıdaki çıktılar otomatik olarak üretilir:
   - `segmentation.png` → Segmentasyon sonucu
   - `final_fat.png` → Kalça bölgesindeki kilo almış görünüm
   - `final_thin.png` → Yine aynı bölgedeki zayıflamış görünüm

---

## 📌 Notlar

- 1U-Net ve U-Net++ ile Karaciğer Segmentasyonu" projesi yapmakta olduğum için U²-Net mimarisi isim olarak ilgimi çekti. Bundan dolayı bunu araştırıp U²-Net tabanlı bir kütüphane kullandım.
---


