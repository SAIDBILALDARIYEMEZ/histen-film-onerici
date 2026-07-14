# 🎬 Ruh Haline Göre Otomatik Film Önerici

Kameradan yüz ifadeni okuyup, anlık ruh haline göre otomatik film öneren
bir web uygulaması. **BTK Akademi**'deki "LLM ve Yapay Zeka" kursu
kapsamında, **Dr. Murat Altun**'un anlattığı yapay zeka/LLM temellerinden
yola çıkılarak geliştirilmiş bir uygulamalı proje.

- Eğitmen: [Dr. Murat Altun — LinkedIn](https://www.linkedin.com/in/drmurataltun/) · GitHub: _(link eklenecek)_
- Kurum: BTK Akademi

## Nasıl çalışıyor?
1. **Yüz ifadesi tanıma**: Tarayıcıda çalışan `face-api.js` (TensorFlow.js
   tabanlı bir Vision Transformer/CNN modeli) kamera görüntüsünden
   duygu tahmini yapıyor — mutlu, üzgün, nötr, şaşkın, kızgın vb.
   Bu bir LLM (dil modeli) değil, bir **görüntü sınıflandırma** modeli;
   sadece yüz ifadesine bakıp bir etiket üretiyor, hiç metin üretmiyor.
2. **Ruh hali → tür eşleme**: Tespit edilen duygu, önceden tanımlı bir
   mantıkla film türlerine eşleniyor (örn. üzgün → varsayılan olarak
   neşelendirici komedi/animasyon önerilir; istenirse "ruh halimi
   pekiştir" seçeneğiyle tam tersi de yapılabilir).
3. **Film verisi**: ~4450 gerçek IMDb filmi (tür, IMDb puanı, IMDb ID)
   içeren, halka açık ve ücretsiz bir veri setinden geliyor — sayfayla
   birlikte yükleniyor, canlı bir API çağrısına gerek yok.
4. **Öneri mantığı**: Seçilen tür/ruh hali kategorisine uyan filmlerden
   oluşan bir havuzdan, daha önce gösterilmemiş bir film rastgele
   seçiliyor — hep aynı 1-2 filme takılı kalmıyor.

## Kullanılan teknikler
- **face-api.js** (TensorFlow.js) — tarayıcı içi, sunucu gerektirmeyen
  gerçek zamanlı yüz ifadesi tanıma
- **Vanilla JavaScript** — framework yok, tek dosyalık statik site
- **Statik veri seti** (JSON) — canlı API bağımlılığı olmadan çalışan
  film önerisi
- **GitHub Pages** — tamamen ücretsiz, sunucusuz barındırma

Projenin bir de Python/Gradio ile yerel bilgisayarda veya Google
Colab/Kaggle üzerinde çalışan bir sürümü var (`app.py`) — bu repoda
sadece dosya olarak duruyor, GitHub Pages onu çalıştırmıyor/görmezden
geliyor, siteyi etkilemiyor.

---

## Yenilikler (v8)
- **Poster görseli eklendi** (opsiyonel): OMDb API kullanılıyor — TMDb'den
  FARKLI, ayrı ücretsiz bir servis, IMDb ID ile doğrudan çalışıyor:
  1. http://www.omdbapi.com/apikey.aspx adresinden "FREE" planı seç,
     e-postanı gir, gelen onay linkine tıkla, anahtarını al.
  2. Terminalde: `export OMDB_API_KEY="senin_anahtarin"`
  3. `python app.py` ile başlat — artık kartta poster de görünecek.
  Anahtar girmezsen uygulama yine sorunsuz çalışır, sadece poster
  görünmez.
- **Duygu etiketi Türkçeleşti**: "Şu an: disgust" yerine "Şu an: tiksinme"
  gibi gösteriliyor.
- **Öneri geçmişi filtreye göre gruplandı**: hangi filtrelerle
  (tür/puan/ruh hali hedefi) önerildiği değiştiğinde geçmişte
  "🔧 Yeni filtre: ..." başlığı beliriyor, aynı filtre altındaki
  öneriler o başlığın altında birikiyor.

## Yenilikler (v7)
- **TMDb API'sine artık gerek yok.** Film verisi yerel bir dosyada:
  `movies_dataset.csv` (Python için) / `movies_data.js` (statik/JS için).
  ~4450 gerçek IMDb filmi, 24 tür, IMDb puanı ve doğrudan IMDb ID içeriyor.
  Kaynak: GitHub'da halka açık, ücretsiz bir IMDb film veri seti
  (sundeepblue/movie_rating_prediction). Artık API anahtarı girmene,
  internet bağlantısına (Python sürümünde sadece ilk model indirmesi
  hariç) gerek yok.
- **Yüz tanıma isabeti artırıldı**: Python sürümünde artık kamera
  karesinin tamamı değil, önce OpenCV ile tespit edilen YÜZ BÖLGESİ
  kırpılıp modele veriliyor. Bu modeller sıkı kırpılmış yüz
  fotoğraflarıyla eğitiliyor; ham kareyi (arka plan dahil) vermek
  isabeti ciddi düşürüyor — "her zaman nötr" sorununun asıl sebebi
  büyük ihtimalle buydu. Ayrıca model `HardlyHumans/Facial-expression-detection`
  ile değiştirildi (FER2013+AffectNet, %92 civarı bildirilen doğruluk).
- **Öneri çeşitliliği**: her kategori artık binlerce filmlik bir
  havuzdan rastgele seçim yapıyor (bkz. v6 notları), asla aynı 1-2
  filme takılı kalmıyor.
- **Poster kaldırıldı** (TMDb olmadan poster görseli yok), yerine daha
  büyük/okunaklı bir metin kartı + doğrudan tıklanabilir IMDb linki var.

## Bu proje neden "LLM eğitimi" gerektirmiyor?
Yüz ifadesinden duygu tahmini bir **görüntü sınıflandırma** işi (ViT —
Vision Transformer), dil modeli değil. Hazır, küçük bir model
kullanılıyor. Film önerisi de yerel veri setini filtreleyen basit bir
mantık, LLM yok.

---

## 1) Python/Gradio sürümü — yerel / Colab / Kaggle

### Kendi bilgisayarında (localhost)
```bash
pip install -r requirements.txt
python app.py
```
`movies_dataset.csv`'nin `app.py` ile AYNI klasörde olduğundan emin ol.
Terminalde `http://localhost:7860` gibi bir adres çıkar, tarayıcından
o adrese git. Artık hiçbir API anahtarı sormayacak.

### Google Colab / Kaggle
`colab_kaggle_notebook.ipynb`'yi yükle, **`movies_dataset.csv`'yi de
aynı çalışma klasörüne yükle**, hücreleri sırayla çalıştır. İkinci
hücrede `SHARE_PUBLIC_LINK="1"` ile gelen `https://xxxxx.gradio.live`
linkine kendi tarayıcından gir (kamera senin bilgisayarından açılır).

### Nasıl çalışıyor
1. Filtreleri (ruh hali hedefi, tür, min. puan, sıralama) en başta bir
   kez ayarlarsın.
2. Kamera her ~2 saniyede bir kare alır, OpenCV ile yüzü bulup kırpar,
   `HardlyHumans/Facial-expression-detection` modeline verir.
3. Tespit edilen ifade bir "hedef ruh hali kategorisi"ne çevrilir.
4. Kategori değiştiyse, o kategorinin film havuzundan daha önce
   gösterilmemiş rastgele bir film seçilir ve karta yazılır.
5. Geçmiş sayfa açıkken birikir, yenilenince sıfırlanır.

---

## 2) Statik HTML/JS sürümü — GitHub Pages (herkese açık)

`index.html` + `movies_data.js` tamamen tarayıcıda çalışıyor, sunucuya
ihtiyaç yok:
- Yüz ifadesi tanıma: `face-api.js` (tarayıcıda TensorFlow.js), CDN'den
  otomatik yükleniyor.
- Film verisi: `movies_data.js` (aynı ~4450 filmlik veri, sayfayla
  birlikte yükleniyor, hiçbir API çağrısı yok).
- Artık hiçbir API anahtarı girmen gerekmiyor.

### GitHub Pages'e yükleme
1. GitHub'da yeni bir repo aç.
2. **HEM `index.html` HEM `movies_data.js`'i** o repoya yükle (ikisi de
   gerekli, movies_data.js olmadan sayfa çalışmaz).
3. Repo **Settings → Pages** → Source: `main` dalı, `/ (root)` klasörü,
   kaydet.
4. 1-2 dakika sonra `https://kullanici-adin.github.io/repo-adi/`
   adresinde yayına girer.

---

## Dosyalar
- `app.py` — Python/Gradio uygulaması (yerel/Colab/Kaggle)
- `movies_dataset.csv` — Python sürümü için film veri seti (~4450 film)
- `colab_kaggle_notebook.ipynb` — Colab/Kaggle için hazır not defteri
- `requirements.txt` — Python bağımlılıkları
- `index.html` + `movies_data.js` — Statik tarayıcı sürümü (GitHub Pages)
- `movies.csv` — v1'den kalma eski küçük örnek, artık kullanılmıyor, silebilirsin

## Sonraki adımlar için fikirler
- Poster görseli istersen: OMDb API'sinden (ayrı, ücretsiz bir anahtar
  gerektirir) poster URL'si çekilebilir — istersen ekleyebilirim.
- Son birkaç karenin ortalama duygusunu almak (daha kararlı sonuç,
  tek kare titremesini azaltır).
- Veri setini genişletmek istersen: aynı işlemi resmi IMDb
  non-commercial veri setleriyle (datasets.imdbws.com) de yapabiliriz,
  o çok daha büyük ve güncel ama işlenmesi biraz daha uğraştırıcı.
