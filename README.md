<!DOCTYPE html>
<html lang="ar">
<head>
  <meta charset="UTF-8">
  <title>أوجد أقرب طبيب</title>
  <meta name="viewport" content="width=device-width, initial-scale=1.0">

  <!-- Leaflet Map -->
  <link rel="stylesheet" href="https://unpkg.com/leaflet@1.9.4/dist/leaflet.css"/>

  <style>
    body {
      font-family: Arial, sans-serif;
      margin: 0;
      background: #f5f6fa;
      direction: rtl;
    }

    header {
      background: #2c7be5;
      color: white;
      padding: 15px;
      text-align: center;
      font-size: 18px;
    }

    .container {
      padding: 15px;
    }

    select, button {
      width: 100%;
      padding: 12px;
      margin-top: 10px;
      font-size: 16px;
    }

    button {
      background: #2c7be5;
      color: white;
      border: none;
      border-radius: 6px;
      cursor: pointer;
    }

    button:hover {
      background: #1a5fd0;
    }

    #result {
      margin-top: 15px;
      font-size: 16px;
      font-weight: bold;
    }

    #map {
      height: 300px;
      margin-top: 15px;
      border-radius: 10px;
    }
  </style>
</head>

<body>

<header>
  🩺 أوجد أقرب طبيب
</header>

<div class="container">

  <label>اختاري العَرَض:</label>
  <select id="symptom">
    <option value="">-- اختاري --</option>
    <option value="باطنة">صداع / تعب عام</option>
    <option value="صدرية">كحة / ضيق تنفس</option>
    <option value="جلدية">حساسية / طفح جلدي</option>
    <option value="عظام">آلام مفاصل</option>
  </select>

  <button onclick="findDoctor()">ابحث عن أقرب طبيب</button>

  <div id="result"></div>
  <div id="map"></div>

</div>

<script src="https://unpkg.com/leaflet@1.9.4/dist/leaflet.js"></script>

<script>
  let map;

  function findDoctor() {
    const symptom = document.getElementById("symptom").value;
    const result = document.getElementById("result");

    if (!symptom) {
      alert("من فضلك اختاري العَرَض");
      return;
    }

    if (!navigator.geolocation) {
      alert("الموقع غير مدعوم على جهازك");
      return;
    }

    navigator.geolocation.getCurrentPosition(
      position => {
        const userLat = position.coords.latitude;
        const userLng = position.coords.longitude;

        result.innerHTML = "✔️ التخصص المناسب: " + symptom;

        // موقع دكتور تجريبي قريب
        const doctorLat = userLat + 0.005;
        const doctorLng = userLng + 0.005;

        if (map) {
          map.remove();
        }

        map = L.map('map').setView([userLat, userLng], 14);

        L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
          attribution: '© OpenStreetMap'
        }).addTo(map);

        L.marker([userLat, userLng])
          .addTo(map)
          .bindPopup("📍 موقعك")
          .openPopup();

        L.marker([doctorLat, doctorLng])
          .addTo(map)
          .bindPopup("🧑‍⚕️ طبيب " + symptom);

      },
      () => {
        alert("لم يتم تحديد موقعك");
      }
    );
  }
</script>

</body>
</html>
