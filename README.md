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

- BFS Algoritması Nedir ve Neden Kullandık
BFS, graf üzerinde genişlik öncelikli arama yapan bir algoritmadır. Temel amacı, başlangıç noktasından hedef noktaya kadar olan en kısa yolu bulmaktır. Bu algoritmanın ana mantığı, ilk olarak başlangıç noktasına en yakın komşuları keşfetmek ve sonra sırayla daha uzak komşuları keşfetmektir. BFS, her seviyede (yani, her mesafede) komşu düğümleri keşfeder.

- A* Algoritması Nedir ve Neden Kullandık


## Örnek Kullanım ve Test Sonuçları








