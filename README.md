# 🎬 Ruh Haline Göre Otomatik Film Önerici

Kameradan yüz ifadeni okuyup, anlık ruh haline göre otomatik film öneren bir web uygulaması.

🔗 **Canlı demo:** https://saidbilaldariyemez.github.io/histen-film-onerici/

---

## Proje Hakkında

Bu proje, **BTK Akademi**'deki "LLM ve Yapay Zeka" kursu kapsamında,
**Dr. Murat Altun**'un anlattığı yapay zeka temellerinden yola çıkılarak
geliştirilmiş uygulamalı bir projedir.

- Eğitmen: [Dr. Murat Altun — LinkedIn](https://www.linkedin.com/in/drmurataltun/) · [GitHub](https://github.com/yapayzekaokulum)
- Kurum: BTK Akademi

## Nasıl Çalışıyor?

1. **Kamerayı aç** — tarayıcı üzerinden çalışan yapay zeka destekli bir
   duygu tanıma modeli, yüz ifadeni gerçek zamanlı olarak analiz eder
   (mutlu, üzgün, şaşkın, nötr gibi kategoriler).
2. **Ruh haline göre eşleme** — tespit edilen duygu durumu, film
   türleriyle akıllıca eşleştirilir (örneğin üzgün hissediyorsan
   varsayılan olarak seni neşelendirecek bir film önerilir; istersen
   "ruh halimi pekiştir" seçeneğiyle tam tersi de yapılabilir).
3. **Film önerisi** — binlerce gerçek IMDb filminden oluşan bir veri
   havuzundan, seçtiğin filtrelere (tür, minimum puan, sıralama) uygun,
   daha önce önerilmemiş bir film seçilir ve doğrudan IMDb linkiyle
   sunulur.

Her şey tarayıcında çalışır — herhangi bir kurulum ya da sunucuya
ihtiyaç duymadan, tek tıkla deneyebilirsin.

## Kullanılan Model
Duygu tanıma için, Hugging Face üzerinde barındırılan önceden eğitilmiş
bir yapay zeka modelinden yararlanılıyor:
[`mo-thecreator/vit-Facial-Expression-Recognition`](https://huggingface.co/mo-thecreator/vit-Facial-Expression-Recognition)
(bu model, projenin Python/Colab sürümünde kullanılıyor; web üzerindeki
canlı demo ise tarayıcı içi çalışan ayrı bir modelle çalışır).

## Kullanılan Teknolojiler
- Yapay zeka destekli gerçek zamanlı yüz ifadesi analizi
- JavaScript (istemci taraflı, framework yok)
- Gerçek IMDb film verisi
- GitHub Pages üzerinden ücretsiz barındırma
