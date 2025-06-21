<!DOCTYPE html>
<html lang="tr">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Numeroloji Analizi</title>
  <style>
    body {
      font-family: Georgia, serif;
      background: #f4f4f4;
      margin: 0;
      padding: 0;
    }
    .container {
      max-width: 800px;
      margin: auto;
      background: #ffffff;
      padding: 30px 20px;
      border-radius: 12px;
      box-shadow: 0 0 20px rgba(0,0,0,0.1);
    }
    .logo {
      display: block;
      margin: 0 auto 20px auto;
      max-width: 250px;
      height: auto;
    }
    h2 {
      text-align: center;
      font-size: 26px;
      color: #333;
      margin-bottom: 20px;
    }
    h2::after {
      content: "";
      display: block;
      width: 60px;
      height: 3px;
      background: gold;
      margin: 10px auto;
      border-radius: 2px;
    }
    label {
      display: block;
      margin-top: 20px;
      font-weight: bold;
    }
    input, button {
      width: 100%;
      padding: 12px;
      margin-top: 5px;
      font-size: 16px;
      border-radius: 8px;
      border: 1px solid #ccc;
      box-sizing: border-box;
    }
    button {
      background-color: #f2f2f2;
      cursor: pointer;
      font-weight: bold;
    }
    button:hover {
      background-color: #e0e0e0;
    }
    #result {
      margin-top: 30px;
      white-space: pre-wrap;
      background: #fafafa;
      padding: 20px;
      border-radius: 10px;
      border: 1px solid #ddd;
      font-size: 16px;
    }
    @media (max-width: 600px) {
      .container {
        padding: 20px 10px;
      }
      h2 {
        font-size: 22px;
      }
    }
  </style>
</head>
<body>
  <div class="container">
    <img src="NEWLOGOPNG.png" alt="Logo" class="logo" />
    <h2>🔢 Numeroloji Analizi</h2>

    <label>Ad Soyad:</label>
    <input type="text" id="fullname" placeholder="Örn: Ahmet Yılmaz" />

    <label>Doğum Tarihi (GG.AA.YYYY):</label>
    <input type="text" id="birthdate" placeholder="Örn: 07.02.2001" />

    <button onclick="calculate()">🔍 Analiz Et</button>
    <button onclick="window.print()">🖨️ PDF Olarak Kaydet</button>

    <div id="result"></div>
  </div>

  <script>
    const vowels = ['A','E','I','İ','O','Ö','U','Ü'];
    const letterValues = {
      A:1,B:2,C:3,Ç:3,D:4,E:5,F:6,G:7,Ğ:7,H:8,I:9,İ:9,J:1,K:2,L:3,M:4,N:5,O:6,Ö:6,
      P:7,R:9,S:1,Ş:1,T:2,U:3,Ü:3,V:4,Y:7,Z:8
    };

    const descriptions = {
      1: "Liderlik, bağımsızlık, güçlü irade. Kendi yolunu çizer.",
      2: "Uyum, işbirliği, duyarlılık. Nazik ve uzlaştırıcıdır.",
      3: "Yaratıcılık, iletişim, neşe. Sanata ve sosyalliğe açıktır.",
      4: "Düzen, istikrar, emek. Güvenilir ve sadıktır.",
      5: "Özgürlük, değişim, macera. Meraklı ve hareketlidir.",
      6: "Aile, sorumluluk, sevgi. Koruyucu ve şefkatlidir.",
      7: "Ruhsallık, analiz, içe dönüş. Sezgileri güçlüdür.",
      8: "Başarı, güç, liderlik. Maddi dünyada etkilidir.",
      9: "İyilik, hizmet, evrensel sevgi. Fedakardır.",
      11: "İlham, vizyon, ruhsal sezgi. Ruhsal liderdir.",
      22: "Ustalık, vizyon, yapı kurma. Büyük projeleri taşır.",
      33: "Şifa, sevgi, ruhsal öğretmenlik. İlahi hizmet eder."
    };

    const chakras = {
      1:"Kök Çakra", 2:"Sakral", 3:"Solar Pleksus", 4:"Kalp", 5:"Boğaz",
      6:"Kalp", 7:"Üçüncü Göz", 8:"Güneş Sinirağı", 9:"Taç",
      11:"Taç", 22:"Tüm Çakralar", 33:"Kalp & Taç"
    };

    const stones = {
      1:"Garnet", 2:"Aytaşı", 3:"Sitrin", 4:"Yeşim", 5:"Akuamarin",
      6:"Pembe Kuvars", 7:"Ametist", 8:"Kaplan Gözü", 9:"Kristal Kuvars",
      11:"Labradorit", 22:"Lapis Lazuli", 33:"Selenit"
    };

    const colors = {
      1:"Kırmızı", 2:"Turuncu", 3:"Sarı", 4:"Yeşil", 5:"Açık Mavi",
      6:"Pembe", 7:"Mor", 8:"Altın", 9:"Beyaz",
      11:"Gümüş", 22:"Gece Mavisi", 33:"Saf Beyaz"
    };

    function reduceNumber(n) {
      const master = [11, 22, 33];
      while (n > 9 && !master.includes(n)) {
        n = n.toString().split('').reduce((a,b) => a + +b, 0);
      }
      return n;
    }

    function getCharValues(name, filter) {
      return name.toUpperCase().split('').filter(filter).map(l => letterValues[l] || 0);
    }

    function formatMeaning(label, number) {
      return `${label}: ${number}
→ ${descriptions[number] || ""}
🧘 Çakra: ${chakras[number] || "-"}
💎 Taş: ${stones[number] || "-"}
🎨 Renk: ${colors[number] || "-"}`;
    }

    function calculate() {
      const name = document.getElementById("fullname").value.trim();
      const birth = document.getElementById("birthdate").value.trim();
      const digits = birth.replace(/\D/g, '');

      if (name.length === 0 || digits.length !== 8) {
        document.getElementById("result").innerText = "Lütfen geçerli ad ve tarih giriniz.";
        return;
      }

      const day = parseInt(digits.slice(0,2));
      const month = parseInt(digits.slice(2,4));
      const year = parseInt(digits.slice(4));

      const allDigits = digits.split('').map(n => parseInt(n));
      const lifePath = reduceNumber(allDigits.reduce((a,b)=>a+b,0));
      const destiny = reduceNumber(day + month + year);
      const maturity = reduceNumber(lifePath + destiny);

      const upperName = name.toLocaleUpperCase('tr-TR').replace(/[^A-ZÇĞİÖŞÜ]/g, '');
      const vowelsOnly = getCharValues(upperName, l => vowels.includes(l));
      const consonantsOnly = getCharValues(upperName, l => !vowels.includes(l));
      const allLetters = getCharValues(upperName, l => true);

      const soulUrge = reduceNumber(vowelsOnly.reduce((a,b)=>a+b,0));
      const outerPersonality = reduceNumber(consonantsOnly.reduce((a,b)=>a+b,0));
      const expression = reduceNumber(allLetters.reduce((a,b)=>a+b,0));

      const yearNow = new Date().getFullYear();
      const personalYear = reduceNumber(reduceNumber(day) + reduceNumber(month) + reduceNumber(yearNow));

      const missingLetters = Object.keys(letterValues).filter(l => !upperName.includes(l));

      document.getElementById("result").innerText = 
`👤 İsim: ${name}
📅 Doğum Tarihi: ${birth}

🔹 ${formatMeaning("Yaşam Yolu Sayısı", lifePath)}

🔹 ${formatMeaning("Kader Sayısı", destiny)}

🔹 ${formatMeaning("Olgunluk Sayısı", maturity)}

💖 ${formatMeaning("Ruh İsteği", soulUrge)}

🌐 ${formatMeaning("Dış Yansıma", outerPersonality)}

🗣️ ${formatMeaning("İfade Sayısı", expression)}

📆 ${formatMeaning("Kişisel Yıl", personalYear)}

📉 Eksik Harfler (Karmik Dersler): ${missingLetters.join(', ')}`;
    }
  </script>
</body>
</html>
