<!DOCTYPE html>
<html lang="tr">
<head>
  <meta charset="UTF-8">
  <title>Canvas'da Yaz</title>
  <style>
    body {
      background-color: #f4f4f4;
      display: flex;
      justify-content: center;
      align-items: center;
      min-height: 100vh;
      margin: 0;
    }
    canvas {
      border: 1px solid #000;
      background-color: #fff;
    }
  </style>
</head>
<body>
  <canvas id="myCanvas" width="800" height="1200"></canvas>
  
  <script>
    const canvas = document.getElementById("myCanvas");
    const ctx = canvas.getContext("2d");

    // Metnin tamamı (README.md içeriği) - uzun metni backtick (`) içinde saklıyoruz.
    const text = `# Wear OS İçin Duyarlı Resim Galerisi

Bu depo, Wear OS akıllı saatler gibi küçük ekranlı cihazlar için optimize edilmiş basit ve duyarlı bir resim galeri web uygulamasını içermektedir. Uygulama, CSS Grid kullanılarak düzenlenmiş, lightbox modal (büyütme) özelliğine sahip ve dokunmatik etkileşimler için animasyonlar içermektedir.

## Özellikler

- **Duyarlı Tasarım:** CSS Grid ve medya sorguları kullanılarak küçük ekranlara uyum sağlayacak şekilde tasarlanmıştır.
- **Lightbox Modal:** Herhangi bir resme tıkladığınızda, resmi büyütülmüş bir lightbox üzerinde görüntüleyebilirsiniz.
- **Dokunmatik Etkileşim:** Resme dokunulduğunda ölçeklendirme animasyonu ile etkileşim deneyimi geliştirilmiştir.
- **Wear OS Uyumlu:** Wear OS gibi küçük ekran boyutlarına sahip cihazlarda optimal görüntüleme sağlamak için tasarlanmıştır.

## Demo

### Galeri Görünümü

Aşağıdaki GIF, galeri görünümünü göstermektedir.  
*(Buraya galeri görünümünü gösteren animasyonlu GIF'in URL'sini ekleyin.)*

![Galeri Görünümü](https://your-image-url.com/gallery-demo.gif)

### Lightbox Modal

Aşağıdaki GIF, lightbox modal'ın nasıl çalıştığını göstermektedir.  
*(Buraya lightbox modal işlevini gösteren animasyonlu GIF'in URL'sini ekleyin.)*

![Lightbox Modal](https://your-image-url.com/lightbox-demo.gif)

### Animasyonlu Etkileşim

Aşağıdaki GIF, resme dokunulduğunda gerçekleşen animasyonlu etkileşimi göstermektedir.  
*(Buraya dokunmatik etkileşim animasyonunu gösteren animasyonlu GIF'in URL'sini ekleyin.)*

![Animasyonlu Etkileşim](https://your-image-url.com/interaction-demo.gif)

## Başlangıç

### Kurulum

1. **Depoyu klonlayın:**

   \`\`\`bash
   git clone https://github.com/kullaniciadiniz/wearos-gallery.git
   \`\`\`

2. **Proje dizinine geçin:**

   \`\`\`bash
   cd wearos-gallery
   \`\`\`

3. **Uygulamayı açın:**
   - \`index.html\` dosyasını tarayıcınızda açarak galeriyi görüntüleyebilirsiniz.
   - Alternatif olarak, klasörü statik sunucu ile çalıştırabilirsiniz (örneğin, VS Code için [Live Server](https://marketplace.visualstudio.com/items?itemName=ritwickdey.LiveServer) eklentisini kullanarak).

### Dosya Yapısı

\`\`\`
.
├── index.html         # Galeri ve lightbox modal kodlarını içeren ana HTML dosyası
├── style.css          # (Opsiyonel) Ek stil düzenlemeleri için harici CSS dosyası (CSS kodları index.html'e gömülü olarak da bulunabilir)
├── images/            # Galeri için kullanılan resim dosyalarının bulunduğu dizin
└── README.md          # Proje dokümantasyonu
\`\`\`

## Özelleştirme

- **Resimler:** \`images/\` dizinindeki resimleri kendi resimlerinizle değiştirebilirsiniz. HTML'deki \`<img>\` etiketlerindeki \`src\` özniteliklerini güncellemeyi unutmayın.
- **Stiller:** CSS kodlarını \`index.html\` içerisinden \`style.css\` dosyasına taşıyabilir veya doğrudan düzenleyebilirsiniz.
- **Animasyonlar:** CSS \`transition\` özelliklerini düzenleyerek resimlerin ölçeklendirme animasyonlarını ihtiyacınıza göre özelleştirebilirsiniz.

## Katkıda Bulunma

Katkılarınız memnuniyetle karşılanır! Öneri, hata raporu veya iyileştirme için pull request gönderebilirsiniz.

## Lisans

Bu proje [MIT Lisansı](LICENSE) kapsamında lisanslanmıştır.

## Teşekkür

- Bu proje, küçük ekranlı cihazlar için minimal ve verimli tasarım gereksinimlerinden ilham alınarak geliştirilmiştir.
- Açık kaynak topluluğuna, projeyi geliştirmemde yardımcı olan kaynaklar ve araçlar için teşekkür ederim.
`;

    // Canvas ayarları
    ctx.font = "16px Arial";
    ctx.fillStyle = "#000";
    const lineHeight = 20;
    const maxWidth = canvas.width - 40; // Kenar boşlukları için

    // Metni satırlara bölme (kelime kaydırmalı yazdırma)
    function wrapText(context, text, x, y, maxWidth, lineHeight) {
      const words = text.split(" ");
      let line = "";

      for (let n = 0; n < words.length; n++) {
        const testLine = line + words[n] + " ";
        const metrics = context.measureText(testLine);
        const testWidth = metrics.width;
        if (testWidth > maxWidth && n > 0) {
          context.fillText(line, x, y);
          line = words[n] + " ";
          y += lineHeight;
        } else {
          line = testLine;
        }
      }
      context.fillText(line, x, y);
      return y;
    }

    // Metni satır satır çizmek için önce \n karakterlerine göre bölüyoruz
    const paragraphs = text.split("\n");
    let x = 20;
    let y = 30;

    paragraphs.forEach(paragraph => {
      if (paragraph.trim() === "") {
        // Boş satır ise biraz boşluk bırak
        y += lineHeight;
      } else {
        y = wrapText(ctx, paragraph, x, y, maxWidth, lineHeight) + lineHeight;
      }
    });
  </script>
</body>
</html>
