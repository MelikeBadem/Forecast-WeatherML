# Forecast-WeatherML

**Forecast Weather** isimli hava durumu sitesinden alınan **San Francisco Downtown**'a ait geçen 5 yılın sıcaklık verileri ve bu sıcaklığa etki eden faktörler elde edilmiştir. Bu veriler üzerinde çeşitli **data hazırlama teknikleri** kullanılarak model eğitimi için uygun hale getirilmiştir. Proje kapsamında 3 farklı regresyon modeli kullanılarak sıcaklık tahminleri yapılmıştır. Sonuçlar kapsamlı bir şekilde analiz edilmiştir.

### 2. Veri Seti ve Kaynağı
Veri setine ait hava durumu verilerine şu linkten ulaşılabilir: [San Francisco Downtown Hava Durumu Tahmin Verisi](https://forecast.weather.gov/MapClick.php?lat=37.7749&lon=-122.4194)

### Kullanılan Veri Setleri:
- **gecmis_hava_durumu.csv**: Kazıma yöntemiyle elde edilen hava durumu verileri.
- **güncellenmis_hava_durumu.csv**: Model eğitiminde kullanılan işlenmiş veri seti.

## Kullanılan Modeller
- **Linear Regression (Doğrusal Regresyon)**
- **Random Forest Regressor**
- **Passive Aggressive Regressor**

Bu modeller, hava durumu verilerinden sıcaklık tahmini yapmak için karşılaştırılmıştır. Modellerin doğruluk oranları, farklı özelliklerin modele dahil edilmesiyle elde edilmiştir.

## Veri Seti
Veri seti, San Francisco'nun Downtown bölgesine ait hava durumu verilerini içerir. Bu veriler, tarih, zaman dilimi (gün ve gece), hava durumu özeti, sıcaklık ve rüzgar hızı gibi temel özellikleri içerir.

### Veri Temizleme ve İşleme
- **Rüzgar Hızı (Null Değerler):** Rüzgar hızı ölçülmeyen günler için, o gün ölçülen rüzgar hızlarının ortalaması alınarak eksik değerler doldurulmuştur.
- **Mevsimsel Kategoriler:** Hava sıcaklığı üzerinde etkili olan mevsimsel değişim göz önüne alınarak, her tarihe uygun mevsim belirlenmiştir. Mevsimsel etkiler, sıcaklık tahminlerini iyileştiren önemli faktörlerdir. San Francisco'nun iklimine göre aşağıdaki tarihlerde mevsimler belirlenmiştir:
  - **Yaz:** 13 Haziran - 24 Ekim (Bu dönemde sıcaklık daha yüksektir.)
  - **Serin:** 4 Aralık - 4 Şubat (Bu dönemde sıcaklık genellikle daha düşüktür.)
  - **Kış:** Diğer tarihlerde Kış mevsimi geçerlidir.
- **Zaman Dilimi (Night / Non-Night):** Veri setindeki her zaman dilimi, günün hangi kısmında (gündüz veya gece) hava durumu ölçüldüğünü belirtir. Bu bilgi, **"Night"** ve **"Non-Night"** olmak üzere iki yeni sütuna dönüştürülmüştür. Ancak **"Non-Night"** sütunu modelde kullanılmamıştır çünkü "Night" olmayan her zaman dilimi zaten "Non-Night"tır.
- **Hava Durumu Özeti:** Hava durumu, kategorik bir veri olarak işlenmiş ve daha sonra sayısal verilere dönüştürülmüştür. Örneğin, güneşli hava genellikle daha yüksek sıcaklıklarla ilişkilendirilmiştir, bu nedenle "Güneşli" gibi hava durumu özetleri, model için sayısal değerler haline getirilmiştir.

### Özellikler ve Etkileri
- **Rüzgar Hızı:** Rüzgar hızı eksik verilerle karşılaşıldığında, bu eksiklikler ortalama rüzgar hızıyla doldurulmuştur.
- **Mevsimsel Etkiler:** Mevsimler, sıcaklık düzeylerini etkileyen önemli bir faktördür. Yaz, Kış ve Serin mevsimlerine göre veriler kategorize edilmiştir.
- **Zaman Dilimi:** Zaman dilimi bilgisi (gündüz/gece), sıcaklık tahmininde önemli bir rol oynar. Gece sıcaklıkları genellikle daha düşüktür.


## Model Performansı
Veri seti **4-5 yıl** süresine ait olduğu için, model doğruluğu, kullanılan modele göre değişiklik göstermektedir:
- **Random Forest** ve **Linear Regression** modelleri, daha büyük veri seti ile **%97** civarında doğruluk oranları elde etmiştir.
- **Passive Aggressive Regressor** modeli ise doğruluk oranı bakımından daha düşük kalmıştır, yaklaşık **%94** civarındadır.
- Veri seti 1-2 yıl civarına kısıldığında, doğruluk oranları şu şekilde değişmektedir:
  - **Random Forest:** **%90** civarında
  - **Linear Regression:** **%90** civarında
  - **Passive Aggressive:** **%88** civarında


## Sonuçlar
- **En İyi Model:** **Random Forest** modeli, en yüksek doğruluğa sahip model olup **%97.97** doğruluk oranı elde etmiştir.
- **Linear Regression** ise **%96.98** doğruluk oranı ile başarılı bir modeldir.
- **Passive Aggressive Regressor** ise **%94.92** doğruluk oranına sahip olup, daha hızlı çalışmasıyla dikkat çekmektedir.
![image](https://github.com/user-attachments/assets/635f126d-09d6-418d-b086-58df9360c8fd)


## Veri Setinin Güncellenebilirliği
- Şu an görmekte olduğunuz veri seti, **4-5 yıl** civarına ait verilerdir. Ancak bu set, istediğiniz tarihlere göre arttırılabilir ya da azaltılabilir. **1-2 yıllık** veri seti kullanıldığında, doğruluk oranları şu şekilde gerçekleşmektedir:
  - **Random Forest** ve **Linear Regression** modelleri için **%90** civarında doğruluk oranı elde edilmektedir.
  - **Passive Aggressive Regressor** modeli de **%88** doğruluk seviyelerine yakın sonuçlar vermektedir.
    
### Sonuç:
Bu proje, hava durumu verilerinden sıcaklık tahminleri yapmak için kullanılan üç farklı regresyon modelini karşılaştırarak, her birinin doğruluğunu değerlendirmiştir. **Random Forest Regressor**, en yüksek doğruluğu sağlayarak modelleme sürecinde en güçlü seçenek olmuştur. **Linear Regression**, daha basit ve hızlı çözümler arayanlar için uygun olsa da, karmaşık veri setlerinde doğruluk açısından yetersiz kalabilmektedir. **Passive Aggressive Regressor** ise, özellikle düşük iterasyonlarla çalışabilen, ancak zaman zaman yüksek doğruluk sağlamayan bir model olarak öne çıkmaktadır.

Bu doğrultuda, önerilen iyileştirmelerle model doğruluğu artırılabilir ve daha doğru sıcaklık tahminleri yapılabilir.

## Gelecekteki Çalışmalar
- Daha fazla hava durumu verisi toplayarak modelin doğruluğunu daha da artırmak.
- Farklı özelliklerin (yağış, nem, rüzgar yönü gibi) dahil edilmesiyle modelin genel başarısını test etmek.
- Diğer makine öğrenmesi teknikleriyle, örneğin derin öğrenme modelleriyle karşılaştırmalar yaparak daha gelişmiş tahminler elde etmek.



Bu proje, hava durumu verilerinin işlenmesi ve farklı regresyon modelleri ile sıcaklık tahminleri yapılmasını sağlayarak, hava tahmini konusundaki farklı yöntemlerin karşılaştırılmasını sunmaktadır.


