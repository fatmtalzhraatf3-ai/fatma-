<!DOCTYPE html><html lang="ar" dir="rtl">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Find My Doctor</title>
  <link rel="stylesheet" href="https://unpkg.com/leaflet@1.9.4/dist/leaflet.css" />
  <style>
    body{margin:0;font-family:Arial;background:#f4f6f8}
    .screen{display:none;padding:20px}
    .active{display:block}
    button{padding:12px 18px;border:none;border-radius:10px;background:#0d6efd;color:#fff;font-size:16px}
    select{width:100%;padding:12px;margin:15px 0;border-radius:8px}
    #map{height:60vh;border-radius:12px}
    h1,h2{text-align:center}
  </style>
</head>
<body><!-- Screen 1 --><div id="screen1" class="screen active">
  <h1>🩺 Find My Doctor</h1>
  <p style="text-align:center">اعرف أقرب دكتور حسب الأعراض</p>
  <button onclick="goToSymptoms()">ابدأ</button>
</div><!-- Screen 2 --><div id="screen2" class="screen">
  <h2>اختاري العرض</h2>
  <select id="symptom">
    <option value="">-- اختاري --</option>
    <option value="باطنة">مغص</option>
    <option value="صدر">كحة / ضيق تنفس</option>
    <option value="مخ وأعصاب">صداع / دوخة</option>
    <option value="أسنان">ألم أسنان</option>
  </select>
  <button onclick="findDoctor()">التالي</button>
</div><!-- Screen 3 --><div id="screen3" class="screen">
  <h2 id="result"></h2>
  <div id="map"></div>
</div><script src="https://unpkg.com/leaflet@1.9.4/dist/leaflet.js"></script><script>
  const screens = [screen1, screen2, screen3];
  function show(n){screens.forEach(s=>s.classList.remove('active'));screens[n].classList.add('active');}
  function goToSymptoms(){show(1);}

  const doctors = {
    "باطنة": {name:"د. أحمد علي",lat:26.155,lng:32.716},
    "صدر": {name:"د. محمد حسن",lat:26.160,lng:32.720},
    "مخ وأعصاب": {name:"د. سارة محمود",lat:26.150,lng:32.710},
    "أسنان": {name:"د. ريم حسين",lat:26.158,lng:32.718}
  };

  function findDoctor(){
    const spec=document.getElementById('symptom').value;
    if(!spec){alert('اختاري عرض');return;}
    show(2);
    document.getElementById('result').innerText='التخصص المناسب: '+spec;

    navigator.geolocation.getCurrentPosition(pos=>{
      const user=[pos.coords.latitude,pos.coords.longitude];
      const doc=doctors[spec];
      const map=L.map('map').setView(user,14);
      L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png').addTo(map);
      L.marker(user).addTo(map).bindPopup('موقعك');
      L.marker([doc.lat,doc.lng]).addTo(map).bindPopup(doc.name);
    });
  }
</script></body>
</html>
