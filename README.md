Siemens TIA Portal ile Gelişmiş Motor Kontrolü ve Hız İzleme Sistemi
Bu depo, Siemens TIA Portal ortamında geliştirilmiş, endüstriyel uygulamalara yönelik kapsamlı bir motor kontrolü ve hız izleme sistemini içermektedir. Proje, bir Siemens S7-1200/1500 PLC'nin programlanması ve bir Siemens HMI panelinin arayüz tasarımını bir araya getirmektedir. Temel amacı, birden fazla motorun güvenli ve verimli bir şekilde çalıştırılmasını, durdurulmasını, aşırı yük durumlarının yönetilmesini ve hız bilgilerinin gerçek zamanlı olarak izlenmesini sağlamaktır.

Proje Ne İşe Yarar? (Fonksiyonel Amaç)
Bu sistemin temel amacı, endüstriyel motorların operasyonel kontrolünü merkezi ve kullanıcı dostu bir arayüz üzerinden sağlamak, aynı zamanda motorların kritik çalışma parametrelerinden biri olan hızlarını sürekli olarak izlemektir. Proje, aşağıdaki temel ihtiyaçlara cevap verir:

Güvenli Motor Çalıştırma/Durdurma: Operatörlerin motorları güvenli bir şekilde başlatıp durdurmasını sağlar.

Hız İzleme: Motorun anlık hızını (RPS, RPM, RPH cinsinden) doğru bir şekilde ölçer ve görselleştirir. Bu, performans takibi ve olası arızaların erken tespiti için kritik öneme sahiptir.

Aşırı Yük Koruması: Motorların aşırı yüklenmesi durumunda otomatik olarak koruma mekanizmalarını devreye sokarak motorun ve bağlı ekipmanın zarar görmesini engeller.

Operasyonel Görselleştirme: Operatörlere, motorların durumu (çalışıyor, durdu, aşırı yükte) hakkında anlık geri bildirim sunan sezgisel bir HMI arayüzü sağlar.

Sistem Entegrasyonu: PLC ve HMI arasında sorunsuz veri alışverişi ve kontrol akışı sağlar.

Kullanılan Donanım ve Yazılım
PLC: Siemens SIMATIC S7-1200/1500 Serisi CPU (örneğin, CPU 1214C veya CPU 1513-1 PN). Proje, bu PLC'lerin dijital giriş/çıkışlarını ve dahili sayıcı/zamanlayıcı fonksiyonlarını kullanır.

HMI: Siemens SIMATIC HMI Comfort Panel (örneğin, KTP700 Comfort veya TP900 Comfort). Operatörün sistemi izlemesi ve kontrol etmesi için dokunmatik ekran arayüzü sağlar.

Sensör: Motor miline veya döner bir parçaya monte edilmiş bir endüktif yakınlık sensörü. Her bir dönüşte darbe sinyali üreterek hız bilgisini PLC'ye iletir.

Yazılım: Siemens TIA Portal V16 (veya üstü). PLC programlaması (Ladder Logic), HMI tasarımı ve tüm donanım konfigürasyonu bu tek platformda gerçekleştirilmiştir.

Detaylı Çalışma Mantığı (Projenin İç İşleyişi)
Projenin kalbinde, TIA Portal içinde geliştirilen PLC programı ve HMI projesi arasındaki etkileşim yatar.

1. Genel Sistem Akışı ve Veri Akışı
Sistem, saha cihazlarından (sensörler, butonlar) veri toplar, bu verileri PLC'de işler, sonuçları HMI'a gönderir ve HMI'dan gelen komutlara göre saha cihazlarını (motor kontaktörleri) kontrol eder.

Giriş Katmanı:

Yakınlık Sensörü: Motor milinin her turunda bir darbe sinyali (örneğin, dijital '1' sinyali) üretir ve bu sinyal PLC'nin yüksek hızlı sayıcı girişine (%I0.1 gibi) bağlanır.

HMI Kontrol Butonları: HMI ekranındaki 'Start' ve 'Stop' butonları, PLC'deki belirli bellek bitlerini (örneğin, %M0.0 ve %M0.1) tetikler.

Aşırı Yük Sinyali: Motorun termik rölesinden veya motor koruma şalterinden gelen aşırı yük sinyali, PLC'nin dijital girişine bağlanır.

PLC İşleme Katmanı:

Darbe Sayımı: PLC'nin Main [OB1] bloğunda, yakınlık sensöründen gelen darbeler bir CTU (Up Counter) bloğu ("Actual_Counting") tarafından toplanır. Bu sayıcı, motorun belirli bir zaman diliminde kaç darbe ürettiğini belirler.

Zamanlama ve Periyodik Güncelleme: Bir TON (On-Delay Timer) bloğu, hız hesaplamalarının belirli, düzenli aralıklarla (örneğin, her 180 milisaniyede bir) güncellenmesini sağlar. Bu, hız değerlerinin sürekli olarak güncel kalmasını garanti eder.

Hız Birimi Dönüşümü: Sayılan darbe sayısı ("Actual_Counting"), matematiksel işlemlerle (çarpma) farklı hız birimlerine dönüştürülür:

"RPS" (Saniyedeki Devir): Sayılan darbe sayısı doğrudan RPS olarak kabul edilir.

"RPM" (Dakikadaki Devir): "RPS" değeri 60 ile çarpılarak elde edilir.

"RPH" (Saatteki Devir): "RPM" değeri tekrar 60 ile çarpılarak elde edilir.

Motor Kontrol Mantığı:

Başlatma/Durdurma: HMI'dan gelen 'Start' ve 'Stop' komutları, PLC'deki bir mandallama (latch) devresi aracılığıyla motorun ana kontaktörünü ("Pump" çıkışı, %Q0.1) kontrol eder. 'Start' basıldığında motor çalışır ve 'Stop' basılana kadar çalışmaya devam eder.

Aşırı Yük Koruması: Aşırı yük sinyali aktif olduğunda, motorun çalışma mandalı otomatik olarak kırılır ve motor durdurulur. Bu durum, HMI'a bir alarm veya durum bilgisi olarak iletilir.

HMI Arayüz Katmanı:

Veri Görüntüleme: PLC'de hesaplanan "RPS", "RPM" ve "RPH" değerleri, HMI ekranındaki ilgili sayısal giriş/çıkış alanlarına (I/O Fields) bağlanır ve operatöre anlık olarak gösterilir.

Kontrol Elemanları: HMI üzerindeki 'Start' ve 'Stop' butonları, PLC'deki ilgili bellek bitlerine doğrudan etiketlenmiştir. Operatör butona dokunduğunda, PLC'deki ilgili bitin durumu değişir ve kontrol mantığı tetiklenir.

Durum Göstergeleri: Motorun çalışma durumu (çalışıyor/durdu/aşırı yük) HMI üzerindeki renkli göstergeler (örneğin, yeşil LED çalışıyor, kırmızı LED aşırı yük) aracılığıyla görsel olarak operatöre sunulur.
