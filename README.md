# Sürücüsüz Metro Simülasyonu (Rota Optimizasyonu) 

Bu proje, bir şehirdeki metro ağı verilerini modellemek ve analiz etmek için yazılmış bir Python uygulamasıdır. Kullanıcılar, belirli duraklar arasındaki rotaları hesaplayabilir ve ağ üzerinde çeşitli sorgular gerçekleştirebilirler.

## Kullanılan Teknolojiler ve Kütüphaneler

Projeyi yazmak için kullanılan ana programlama dili Python'dır.

- **heapq**: Python'da yerleşik olarak bulunan bu kütüphane, min-heap veri yapısını sağlar. Min-heap, her zaman en küçük öğeyi en üstte tutar. Bu, özellikle A* algoritmasında, en hızlı rotayı takip etmek için en uygun istasyonu hızlıca seçmek için kullanılır.
- **collections**: `deque`, Bu veri yapısı, çift uçlu kuyruk anlamına gelir. Yani, hem baştan hem de sondan veri ekleyip çıkarabilirsiniz. BFS (Breadth-First Search) algoritmasında, genişlik öncelikli arama yaparken her seviyedeki komşuları keşfetmek için deque kullanılır.genişlik öncelikli arama (BFS) gibi algoritmalarda, istasyonlar arasındaki en kısa rotayı bulmak için kuyruk veri yapısını sağlar. Ayrıca, **defaultdict** sınıfı, Metro ağı örneğinde, defaultdict(list) kullanarak, yeni bir hat eklendiğinde otomatik olarak boş bir liste oluşturulmasını sağlar. Bu, her defasında listeyi başlatma gerekliliğini ortadan kaldırır..
- **typing**: `Dict`, `List`, `Tuple`, `Optional` gibi tür ipuçları (type hints) kullanılarak, fonksiyonların ve sınıfların türleri belirtilmiştir. Bu, kodun daha anlaşılır olmasını sağlar.
  - Dict: Anahtar-değer çiftlerini temsil eder
  - List: Bir listeyi temsil eder.
  - Tuple: Sabit uzunluktaki, değiştirilemez veri yapılarıdır.
  - Optional: Bir değişkenin belirli bir türde olabileceği ancak aynı zamanda None olabileceği durumları belirtmek için kullanılır

## Algoritmaların Çalışma Mantığı

- BFS Algoritması Nedir 
BFS, graf üzerinde genişlik öncelikli arama yapan bir algoritmadır. Temel amacı, başlangıç noktasından hedef noktaya kadar olan en kısa yolu bulmaktır. Bu algoritmanın ana mantığı, ilk olarak başlangıç noktasına en yakın komşuları keşfetmek ve sonra sırayla daha uzak komşuları keşfetmektir. BFS, her seviyede (yani, her mesafede) komşu düğümleri keşfeder.BFS algoritması, özellikle en az aktarma yapılan rotaları bulmak için idealdir. Bu, metro gibi ağlarda, kullanıcıların daha az değişiklik yaparak istedikleri yere ulaşmalarını sağlamak için gereklidir.

BFS Algoritmasının İşleyişi:
1. **Başlangıç İstasyonunu Kuyruğa Ekleyin**:
Başlangıç istasyonu, BFS algoritmasında ilk olarak kuyruktan çıkarılır ve ziyaret edilmemiş olarak işaretlenir. Başlangıç istasyonu, kuyruktan çıkarılmadan önce **kuyruğa** eklenir.
   ```python
   kuyruk = deque([(baslangic, [baslangic])])
   
2. **Komşu İstasyonları Keşfedin**:
Mevcut istasyonun komşuları (yani, bağlı olduğu diğer istasyonlar) sırayla keşfedilir. Her bir komşu, henüz ziyaret edilmediyse kuyrukta bir sonraki adımlar için bekler.
   ```python
      for komsu, _ in mevcut.komsular:
         if komsu not in ziyaret_edildi:
            kuyruk.append((komsu, yol + [komsu]))
 
3. **Hedefe Ulaşana Kadar Devam Et**:

Eğer mevcut istasyon, hedef istasyonu ile eşleşirse, algoritma işlemi sonlandırır ve en kısa yolu döndürür.
    ```python
          for komsu, _ in mevcut.komsular:
           if komsu not in ziyaret_edildi:
               kuyruk.append((komsu, yol + [komsu]))
 
4. **Sonuç**:
 En kısa yol (en az aktarma) bulunur ve sonuç olarak döndürülür.

   ```python
          return None
   

- A* Algoritması Nedir
  A* algoritması, genişlik öncelikli arama ile birlikte, her istasyonun hedefe olan tahmini mesafesini dikkate alır. Bu algoritma, en hızlı rotayı bulmaya yöneliktir ve her istasyonu seçerken maliyeti (gerçek maliyet ve tahmin edilen maliyet) dikkate alır.A* algoritması ise, her istasyonun hedefe olan tahmini mesafesini göz önünde bulundurarak en hızlı rotayı bulur. Bu, zaman açısından verimli bir rota arayışı için kullanışlıdır, çünkü her bir istasyonun maliyetini ve hedefe olan uzaklığını dikkate alır.

A* Algoritmasının İşleyişi:
1. Başlangıç İstasyonunu Kuyruğa Ekleyin:
Başlangıç istasyonu, A* algoritmasında f-skoru sıfır olarak kuyruğa eklenir. Bu skor, her istasyon için toplam maliyeti gösterir.

    ```python
         pq = [(0, id(baslangic), baslangic, [baslangic])]

2. Hedefe Giden Maliyet Hesaplanır:
A*, her bir istasyon için g-skoru (gerçek maliyet) ve h-skoru (hedefe tahmini maliyet) hesaplar. Aşağıdaki gibi f-skoru (g + h) hesaplanır:

    ```python
        toplam_sure = g_skoru + h_skoru          
 
3. Komşu İstasyonları ve Maliyetleri Hesaplayın:
Mevcut istasyonun komşuları sırasıyla işlenir. Her bir komşunun maliyeti, kuyruktaki önceki istasyonlardan gelen maliyetle birleştirilir.

    ```python
          for komsu, sure in mevcut.komsular:
    heapq.heappush(pq, (toplam_sure + sure, id(komsu), komsu, yol + [komsu]))
 
4. Hedefe Ulaşana Kadar Devam Et:
Eğer mevcut istasyon hedefe ulaşıyorsa, algoritma işlemi sonlandırır ve en hızlı rotayı döndürür.

   ```python
          if mevcut == hedef:
    return yol, toplam_sure

5. Sonuç:

En hızlı rota ve bu rotanın toplam süresi döndürülür.

    ```python
          return None

## Örnek Kullanım ve Test Sonuçları
Metro Ağı Yapısı
Programda aşağıdaki metro hatları ve istasyonlar tanımlanmıştır:

- Kırmızı Hat
  -  K1: Kızılay
  -  K2: Ulus
  -  K3: Demetevler
  - K4: OSB

- Mavi Hat
  - M1: AŞTİ
  - M2: Kızılay (Aktarma noktası)
  - M3: Sıhhiye
  - M4: Gar

- Turuncu Hat
  - T1: Batıkent
  - T2: Demetevler (Aktarma noktası)
  - T3: Gar (Aktarma noktası)
  - T4: Keçiören

Test Sonuçları

1. AŞTİ'den OSB'ye
- En Az Aktarmalı Rota:

Başlangıç: AŞTİ (Mavi Hat)
Hedef: OSB (Kırmızı Hat)
Rota: AŞTİ → Kızılay (Aktarma) → Demetevler → OSB
Açıklama: Bu rota, AŞTİ'den başlayarak, Kızılay'da aktarma yaparak Kırmızı Hat’a geçer ve hedef istasyon olan OSB'ye ulaşır. 1 aktarma yapılır.

- En Hızlı Rota:

Başlangıç: AŞTİ (Mavi Hat) <br>
Hedef: OSB (Kırmızı Hat) <br>
Rota: AŞTİ → Kızılay → Demetevler → OSB <br>
Süre: 21 dakika (örnek bir süre hesaplaması) <br>
Açıklama: En hızlı rota da aynı şekilde bir aktarma gerektiren yolculuğu ifade eder, ancak bu rota toplamda daha kısa sürede tamamlanır.

2.  Batıkent'ten Keçiören'e
   
- En Az Aktarmalı Rota:

Başlangıç: Batıkent (Turuncu Hat) <br>
Hedef: Keçiören (Turuncu Hat) <br>
Rota: Batıkent → Demetevler → Gar → Keçiören <br>
Açıklama: Batıkent ve Keçiören aynı hat üzerinde yer aldıkları için aktarma yapılmaz ve doğrudan bu hat üzerinden seyahat edilir. <br>

- En Hızlı Rota:

Başlangıç: Batıkent (Turuncu Hat) <br>
Hedef: Keçiören (Turuncu Hat) <br>
Rota: Batıkent → Demetevler → Gar → Keçiören <br>
Süre: 21 dakika (örnek bir süre hesaplaması) <br>
Açıklama: Hızlı rota, doğrudan aynı hattı kullanarak Batıkent’ten Keçiören’e ulaşır ve aktarma yapılmaz.

3.  Keçiören'den AŞTİ'ye
- En Az Aktarmalı Rota:

Başlangıç: Keçiören (Turuncu Hat) <br>
Hedef: AŞTİ (Mavi Hat) <br>
Rota: Keçiören → Gar (Aktarma) → Kızılay → AŞTİ <br>
Açıklama: Keçiören’den Gar istasyonuna kadar Turuncu Hat kullanılır ve Gar’dan sonra Mavi Hat’a geçilir. Bu rotada 2 aktarma yapılır: biri Gar’da, diğeri Kızılay’da.

- En Hızlı Rota:

Başlangıç: Keçiören (Turuncu Hat) <br>
Hedef: AŞTİ (Mavi Hat) <br>
Rota: Keçiören → Gar (Aktarma) → AŞTİ <br>
Süre: 19 dakika (örnek bir süre hesaplaması) <br>
Açıklama: En hızlı rota da aynı aktarma noktalarına sahiptir, ancak süre açısından daha kısa bir yolculuk yapılır.


## Projeyi Geliştirme Fikirleri

Bu projede  ağa dinamik yapı kazandırılarak kullanıcıların yeni hatlar ve istasyonlar eklemesine imkan tanınabilir.Ayrıca metro hatlarının bakımda ya da kapalı olduğu durumlar da sisteme dahil edilerek, bu tür güncel bilgiler rota hesaplamalarına entegre edilebilir. Metro dışında otobüs, tramvay gibi farklı ulaşım araçları da sisteme dahil edilerek, en hızlı veya en ekonomik rota alternatifleri sunulabilir. Yoğun saatlerdeki kalabalık durumlarına göre, kullanıcılara daha uygun ve alternatif rotalar önerilerek daha verimli bir ulaşım deneyimi sağlanabilir.




