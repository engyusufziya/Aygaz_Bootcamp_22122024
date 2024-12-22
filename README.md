# Aygaz_Bootcamp_22122024
Yusuf Ziya Demirel | Görüntü İşleme Proje Teslim

 - Hayvan Sınıflandırma ve Görüntü Manipülasyon Projesi

Merhabalar! Bu projede, on farklı hayvan sınıfını bir derin öğrenme modeli (Convolutional Neural Network, CNN) kullanarak sınıflandırmayı hedefledik. Proje boyunca veri seti hazırlama, veri ön işleme, veri manipülasyonu, veri artırma ve sonuçların değerlendirilmesi gibi aşamalardan geçtik. Şimdi projeye biraz daha yakından bakalım!

1. Proje Hakkında Genel Bakış
Bu proje, Animals with Attributes 2 veri setinden seçtiğimiz on farklı hayvan sınıfına odaklanıyor. Seçilen sınıflar şunlardır:
collie, dolphin, elephant, fox, moose, rabbit, sheep, squirrel, giant+panda, polar+bear
Her bir hayvan sınıfından belirli sayıda (maksimum 650) görüntü alınarak, daha sonra çeşitli manipülasyonlar ve beyaz dengesi işlemleri uygulanmıştır. Model eğitimi için TensorFlow Keras ve OpenCV gibi popüler kütüphanelerden yararlandık.

2. Kurulum ve Gerekli Paketler
Projeyi çalıştırmadan önce aşağıdaki paketlerin kurulu olması gerekir:

OpenCV (cv2)
NumPy
Scikit-Learn
TensorFlow (Keras)
Shutil (klasörleri temizlemek için)
os (dosya, klasör işlemleri için)
Kullanılan paketlerin çoğu Python ekosisteminde yer alan, pip veya conda ile kolayca kurulabilen kütüphanelerdir.

3. Veri Seti Hazırlama
Veri seti işlemleri şu şekilde gerçekleşti:

Görsellerin Seçilmesi ve Yeniden Boyutlandırılması:
Seçtiğimiz hayvan sınıflarının dizinlerindeki görüntüleri, belirlediğimiz maksimum sayıda (650) aldık. OpenCV kullanarak her bir görüntüyü 128x128 boyutuna yeniden boyutlandırdık. Ardından, piksel değerlerini 0-255 aralığından 0-1 aralığına ölçeklendirdik. Bu şekilde normalleştirilmiş görselleri diskte ilgili dizine kaydettik.

Manipüle Edilmiş Görsellerin Oluşturulması:
Manipülasyon aşamasında parlaklık ve kontrast değerlerini değiştirerek (alpha ve beta parametreleri) yeni bir veri seti ürettik. Bu sayede modelin farklı ışık koşullarında daha iyi genellenmesi hedeflendi.

Beyaz Dengesi Düzeltmesi:
Görsellerdeki renk kanallarını, resmin ortalama kanal değerine göre yeniden ölçeklendirerek beyaz dengesini düzeltmeye çalıştık. Böylelikle renk sapmalarını azaltarak veri setinin daha tutarlı hale gelmesini amaçladık.

Her bir aşamadan çıkan yeni veri seti (orijinal, manipüle edilmiş ve beyaz dengesi uygulanmış), eğitimin daha geniş bir varyasyonla yapılmasına olanak tanıdı.

4. Model Mimarisi ve Eğitimi
Kullandığımız derin öğrenme modeli, Keras ile oluşturulmuş bir CNN (Convolutional Neural Network) mimarisidir. Model şu katmanlardan oluşur:

Giriş (Input) Katmanı: 128x128 boyutlu ve 3 kanallı (RGB) resimleri giriş olarak alır.
Conv2D + LeakyReLU + BatchNormalization + MaxPooling2D: 3 defa tekrar eden bir bloktur. Filtre sayısı sırasıyla 32, 64 ve 128 olarak belirlenmiştir.
Flatten Katmanı: 2B veriyi 1B vektöre çevirir.
Dense Katmanı (128 nöron, l2 cezalandırma, Dropout=0.7): Tam bağlantılı bir katmandır.
Çıkış (Dense) Katmanı: 10 sınıf için softmax aktivasyon fonksiyonuna sahiptir.
Modelin optimize edilmesi için Adam optimizatörü (öğrenme oranı 0.0001) ve categorical_crossentropy kayıp fonksiyonu kullandık. Toplam 20 epoch boyunca, batch_size=32 olacak şekilde eğittik.

5. Test Aşaması ve Değerlendirme
Eğitilen modelimizi farklı senaryolarla test ettik:

Orijinal Test Seti: İlk olarak, veri setinin orijinal halinden bir test kümesi oluşturduk ve modeli bu verilerde değerlendirdik.
Manipüle Edilmiş Test Seti: Daha sonra bu test kümesi üzerindeki görüntüleri parlaklık/kontrast değişikliği, döndürme gibi ek manipülasyonlara tabi tuttuk. Modelin, bu değişikliklere rağmen ne kadar başarılı olduğunu gözlemledik.
Beyaz Dengesi Uygulanmış Test Seti: Manipüle edilmiş test setine ayrıca beyaz dengesi işlemi uyguladık. Böylece farklı aşamalarda yapılan renk düzeltmelerinin model performansına etkisini inceledik.
Her bir senaryoda loss (kayıp) ve accuracy (doğruluk) metriğini raporladık. Sonuç olarak projede, orijinal ve manipüle edilmiş/beyaz dengesi uygulanmış görüntüler arasındaki performans farklarını gözlemleme şansı bulduk.

6. Sonuçlar ve Öneriler
Deney sonuçlarında, modelin orijinal veri setindeki doğruluk oranının manipüle edilmiş ve beyaz dengesi uygulanmış veri setlerinde belirli ölçüde farklılaştığını gördük. Bu da modelin farklı ışık koşulları, parlaklık ve renk düzenlemelerine karşı ne kadar dayanıklı olduğunu gösterdi.

Öneriler:

Daha Fazla Veri Çeşitliliği: Veri augmentasyonu tekniklerini zenginleştirerek (örneğin rastgele kırpma, yatay/dikey çevirme, bulanıklaştırma vb.) modelin daha da sağlam hale getirilmesi sağlanabilir.
Daha İyi Model Mimarisi: Modelin katmanlarını daha da derinleştirerek (örneğin ResNet, EfficientNet gibi hazır mimarilerle) doğruluk oranı artırılabilir.
Hiperparametre Optimizasyonu: Öğrenme oranı, batch_size, düzenlileştirme katsayısı (l2) gibi hiperparametreler üzerinde farklı denemeler yaparak en iyi ayarlar bulunabilir.

KAGGLE LINK: https://www.kaggle.com/code/yusufziyademirel/global-ai-hub-g-r-nt-leme-bootcamp
