# Forecast-WeatherML

**Forecast Weather** isimli hava durumu sitesinden alınan **San Francisco Downtown**'a ait geçen 5 yılın sıcaklık verileri ve bu sıcaklığa etki eden faktörler elde edilmiştir. Bu proje, hava durumu tahminlerinde kullanılan **Machine Learning (ML)** yöntemlerini uygulamak ve analiz etmek için geliştirilmiştir. Proje kapsamında veriler üzerinde **veri hazırlama, temizleme, görselleştirme ve modelleme** adımları gerçekleştirilmiştir. Ayrıca, 3 farklı regresyon modeli ve 1 kümeleme algoritması ile sıcaklık tahminleri yapılmış ve modellerin performansları karşılaştırılmıştır.

---

## İçindekiler
1. [Proje Amacı](#proje-amacı)
2. [Veri Seti ve Kaynağı](#veri-seti-ve-kaynağı)
3. [Kullanılan Modeller](#kullanılan-modeller)
4. [Veri Temizleme ve İşleme](#veri-temizleme-ve-işleme)
5. [Model Performansı](#model-performansı)
6. [Görselleştirme](#görselleştirme)
7. [Sonuçlar](#sonuçlar)
8. [Gelecekteki Çalışmalar](#gelecekteki-çalışmalar)
9. [Youtube Videosu](#youtube-videosu)

---

## 1. Proje Amacı

Bu projenin amacı, geçmiş hava durumu verilerini analiz ederek sıcaklık tahmini yapabilen bir makine öğrenmesi modeli geliştirmektir. Aşağıdaki hedeflere ulaşmak amaçlanmıştır:
- **Veri temizleme ve işleme:** Eksik, hatalı veya uyumsuz verilerin temizlenmesi ve modellenmeye hazır hale getirilmesi.
- **Modelleme ve karşılaştırma:** Farklı makine öğrenmesi modellerini uygulayarak sıcaklık tahminlerinde doğruluk oranlarını karşılaştırmak.
- **Analiz ve yorumlama:** Elde edilen tahmin sonuçlarını görselleştirerek, modellerin etkinliğini analiz etmek.

---

## 2. Veri Seti ve Kaynağı
Yaklaşık 20.000 satır içeren bir veri seti elde edilmiştir. Bu veri setine ait hava durumu verilerine aşağıdaki linkten ulaşılabilir:
- [San Francisco Downtown Hava Durumu Tahmin Verisi](https://forecast.weather.gov/MapClick.php?lat=37.7749&lon=-122.4194)

### Kullanılan Veri Setleri:
- **gecmis_hava_durumu.csv:** Kazıma yöntemiyle Forecast Weather sitesinden elde edilen ham hava durumu verileri.
- **güncellenmis_hava_durumu.csv:** Model eğitiminde kullanılan temizlenmiş ve işlenmiş veri seti.

### Veri Setindeki Özellikler

| **Sütun Adı**        | **Veri Tipi** | **Açıklama**                                                                 |
|-----------------------|---------------|-------------------------------------------------------------------------------|
| Tarih                | Object        | Hava durumu verilerinin kaydedildiği tarih                                    |
| Zaman Dilimi         | Object        | Gün içerisindeki zaman dilimi (ör. "This Afternoon")                          |
| Hava Durumu Özeti    | Object        | Hava durumuna ilişkin genel özet (ör. "Mostly sunny")                         |
| Sıcaklık             | Integer       | O gün kaydedilen sıcaklık değeri                                              |
| Rüzgar Hızı          | Float         | O gün kaydedilen rüzgar hızı                                                  |
| Sunny                | Integer       | Günün güneşli olup olmadığını ifade eden kategorik bir değişken (0 veya 1)    |
| Partly sunny         | Integer       | Günün kısmen güneşli olup olmadığını ifade eden kategorik bir değişken (0/1)  |
| Partly cloudy        | Integer       | Günün kısmen bulutlu olup olmadığını ifade eden kategorik bir değişken (0/1)  |
| Clear                | Integer       | Günün açık olup olmadığını ifade eden kategorik bir değişken (0 veya 1)       |
| Mostly clear         | Integer       | Günün çoğunlukla açık olup olmadığını ifade eden kategorik bir değişken (0/1) |
| Night                | Integer       | Gece olup olmadığını ifade eden kategorik bir değişken (0 veya 1)             |
| Non-Night            | Integer       | Gece olmayan zamanları ifade eden kategorik bir değişken (0 veya 1)           |
| Yaz                  | Integer       | Yaz mevsimine ait bir gün olup olmadığını ifade eden kategorik bir değişken   |
| Serin                | Integer       | Günün serin olup olmadığını ifade eden kategorik bir değişken (0 veya 1)      |
| Kış                  | Integer       | Kış mevsimine ait bir gün olup olmadığını ifade eden kategorik bir değişken   |

---


## 3. Kullanılan Modeller

Projede hava durumu tahmini için aşağıdaki regresyon modelleri kullanılmıştır:
1. **Linear Regression**
2. **Random Forest Regressor**
3. **Passive Aggressive Regressor**
4. **Kmeans Algoritması**

---

## 4. Veri Temizleme ve İşleme

Veri seti üzerinde yapılan veri işleme adımları:

### 4.1. Eksik Değerlerin Doldurulması
- Rüzgar hızı değerlerinin bir kısmı eksik (`null`) olarak kazınmıştır. Bu eksik değerler, aynı gün ölçülen diğer rüzgar hızı değerlerinin ortalaması alınarak tamamlanmıştır.
- Rüzgar hızı değerlerinin birim bilgisi olarak yazılan "mph" ifadeleri temizlenmiş ve değerler sayısal forma dönüştürülmüştür.

### 4.2. Mevsimsel Kategorilerin Oluşturulması
- Mevsimsel dönemler tanımlanarak yeni sütunlar eklenmiştir:
  - **Yaz:** 13 Haziran ile 24 Ekim tarihleri arasındaki dönem.
  - **Serin:** 4 Şubat ile 12 Nisan tarihleri ve 4 Aralık ile 4 Şubat tarihleri arasındaki dönem.
  - **Kış:** Yukarıdaki iki dönem dışında kalan tarihler.  
Her mevsim için veri setine `Yaz`, `Serin` ve `Kış` adında üç yeni sütun eklenmiş ve ilgili tarihlere göre işaretlenmiştir.

### 4.3. Kategorik Verilerin Sayısal Hale Getirilmesi
- **Gece ve Gündüz Durumu:**  
  `Zaman Dilimi` sütununa göre gece ve gündüz durumları `Night` ve `Non-Night` olarak iki yeni sütun eklenmiştir.
- **Hava Durumu Özetleri:**  
  `Hava Durumu Özeti` sütunundaki kategorik değerler (örneğin, "Güneşli", "Sisli", vb.) sayısal hale getirilmiştir. Bu işlem için her kategoriye ait sütun eklenmiş ve `One-Hot Encoding` yöntemi uygulanmıştır.


---

## 5. Model Performansı

| **Model**                 | **Doğruluk Oranı** |
|---------------------------|--------------------|
| **Random Forest**         | %97.97            |
| **Linear Regression**     | %96.98            |
| **Passive Aggressive**    | %94.92            |
| **Kmeans Algoritması**    | %79.16            |

Model, 5 yıllık veriler üzerinde eğitilmiştir. Eğitim verisi, **2020-12-01** ile **2025-01-20** tarihleri arasındaki verileri kapsamaktadır. Bu tarih aralığı, **forecast.ipynb** dosyasındaki aşağıdaki satırlarda belirlenebilir:

### Tarih aralığı
 - start_date = datetime(2020, 12, 1)
 -  end_date = datetime(2025, 1, 20) 
--- 
## 6. Görselleştirme

Projede yapılan analiz ve modelleme sonuçlarını destekleyen görselleştirmeler:

### 6.1. Silhouette Skoruna Göre Küme Sayısı
Aşağıdaki grafik, kümeleme algoritmaları için kullanılan Silhouette skoruna göre farklı küme sayılarını ve en uygun küme sayısını göstermektedir:

![image](https://github.com/user-attachments/assets/96ce3978-169a-45e1-b7bc-6a4bd4a4c57a)


**Açıklama:** En yüksek Silhouette Skoru, 4 kümede elde edilmiştir.

### 6.2. Özelliklerin Sıcaklık ile İlişkisi
Aşağıdaki görsellerde, sıcaklık ile çeşitli hava durumu özellikleri arasındaki ilişkiyi inceleyen scatter plotlar bulunmaktadır:

![image](https://github.com/user-attachments/assets/47140b40-cb62-4739-bdc8-28e330fd4b08)


**Açıklama:** Görseller, belirli hava durumu özelliklerinin sıcaklık üzerindeki etkisini analiz etmek için kullanılmıştır.

---

## 7. Model Performansı: Gerçek ve Tahmin Değerlerinin Karşılaştırılması


# Model Performansı ve Sonuçlar

Bu proje, farklı regresyon modellerinin (Linear Regression, Random Forest, ve Passive Aggressive) ve Kmeans ile sıcaklık tahmini yapılmaya çalışılmıştır.

## Modeller ve Performans Analizi

### 1. Linear Regression:
Linear Regression modelinde, gerçek değerler ile tahmin edilen değerler arasında doğrusal bir ilişki gözlemlenmiştir. Ancak, bazı küçük sapmalar mevcuttur. Bu model, doğrusal ilişkilerle tahmin yapmada etkili olsa da, veri setindeki karmaşıklıklara karşı sınırlı bir genelleme kapasitesine sahiptir.

### 2. Random Forest:
Random Forest modeli, tahmin sonuçlarını gerçek değerlere oldukça yakın yapmıştır ve en yüksek doğruluk oranına ulaşan modeldir. Bu model, verilen veri setindeki doğrusal olmayan ilişkileri etkili bir şekilde öğrenmiş ve karmaşık verilerle daha iyi genelleme yapabilmiştir. Random Forest modelinin doğruluk oranı %98 civarındadır, bu da modelin oldukça iyi performans gösterdiğini ve tahminlerin gerçek verilere çok yakın olduğunu gösterir.

### 3. Passive Aggressive:
Passive Aggressive modelinde, tahmin edilen değerlerin çoğunluğu gerçek değerlere yakın olsa da, bu modelde daha fazla sapma gözlenmiştir. Bu model, doğrusal ilişkileri öğrenmede etkin olabilir ancak, veri setindeki karmaşık yapılar ve doğrusal olmayan ilişkiler karşısında daha zayıf kalmıştır.

# KMeans Algoritması Sonuçları ve Silhouette Skoru Yorumları

### 4. KMeans Algoritması

KMeans algoritması ile elde edilen **Silhouette skoru** 0 ile 1 arasında bir değere sahiptir ve genellikle 0.7 ve üzerindeki değerler **"iyi"** kabul edilir. Bu durumda elde edilen **0.7916**'lık skor, kümeleme modelinin **iyi bir şekilde çalıştığını** ve verilerin kümelerine **iyi bir şekilde uyduğunu** gösterir.

#### Yüksek Silhouette Skoru:
- **Silhouette skoru 0 ile 1 arasında bir değere sahiptir.** Yüksek bir skor, kümeleme işleminin başarılı olduğunu ve kümeler arasındaki ayrımın net olduğunu gösterir.
- Bu durumda elde edilen **0.7916**'lık skor, kümeleme modelinin iyi bir performans sergilediğini ve verilerin kümelere yüksek bir uyum sağladığını ortaya koymaktadır.

#### Ayrıklık ve Uyum:
- **Bu skor, kümelerin birbirinden net bir şekilde ayrıldığını** ve aynı küme içindeki verilerin birbirine oldukça benzer olduğunu gösterir.
- Yani, kümeler arasındaki mesafeler büyük ve her küme içinde **benzer veriler** bulunmaktadır. Bu da modelin doğru şekilde kümeleri ayırdığını gösterir.

#### Veri Yapısı:
- Bu skor, **verinin kümeler oluşturulabilir bir yapıda olduğunu**, yani verinin doğal olarak birkaç ana gruba ayrılabileceğini düşündürmektedir.
  
#### Kümülatif Yorum:
- **Model Başarısı:** Kümeleme algoritması, 4 küme ile oldukça iyi bir performans göstermiştir ve **0.7916'lık bir Silhouette skoru**, modelin kümeleme işleminin genellikle doğru ve uygun olduğunu ortaya koymaktadır.
- Küme içindeki benzerlikler ve küme dışındaki farklılıklar belirgin şekilde ayırt edilebilmektedir.

### Sonuç:
- KMeans algoritması, 4 küme sayısıyla oldukça başarılı bir kümeleme performansı göstermiştir. Bu, verinin kümelenebilir bir yapıya sahip olduğunu ve kümeler arasındaki farklılıkların belirgin olduğunu gösterir.

## Grafikler ve Değerlendirme

![image](https://github.com/user-attachments/assets/74d62da0-9aea-45ba-8baa-62b24858308d)

![image](https://github.com/user-attachments/assets/17b4cdcf-ab61-4e38-bd15-45be50cf1a9a)


Grafiklerde, her bir modelin **gerçek değerler** ile **tahmin edilen değerler** arasındaki ilişkiyi gösteren görseller bulunmaktadır. Kırmızı kesikli çizgi, tahminlerin ideal olarak yer alması gereken doğrusal ilişkiyi temsil etmektedir.

## Sonuç

- **Random Forest**, verilen veri setindeki karmaşıklıkları en iyi şekilde öğrenmiş ve doğrusal olmayan ilişkileri etkili bir biçimde modelleyebilmiştir.
- **Linear Regression** modelinde doğrusal ilişkiler gözlemlense de, karmaşık veri yapıları için yetersiz kalmıştır.
- **Passive Aggressive** modelinde ise, doğrusal ilişkilerle tahmin yapabilme yeteneği sınırlıdır ve daha fazla sapma gözlenmiştir.
- **Kmeans Algoritması** , veri setini belirli sayıda kümeye ayırmada oldukça başarılı olmuştur. Kümeleme sırasında, verinin doğal yapısındaki benzerlikleri ve farklılıkları etkili bir şekilde yakalayabilmiştir.

Bu proje, farklı regresyon modelleri ve kümeleme algoritmasının karşılaştırılmasını sağlamakta olup, Random Forest modelinin doğruluk oranı açısından en başarılı sonuçları verdiğini ortaya koymaktadır.


---

## 8. Gelecekteki Çalışmalar

- Daha fazla hava durumu verisi toplayarak modelin doğruluğunu daha da artırmak.
- Farklı özelliklerin (yağış, nem, rüzgar yönü gibi) dahil edilmesiyle modelin genel başarısını test etmek.
- Diğer makine öğrenmesi teknikleriyle, örneğin derin öğrenme modelleriyle karşılaştırmalar yaparak daha gelişmiş tahminler elde etmek.

---

## 9. Youtube Videosu

Proje detaylarını görmek için aşağıdaki videoyu izleyebilirsiniz:  
[Proje Videosu](https://www.youtube.com/watch?v=ISuI-ZEry_Y)

