# jinja-checkin
<!DOCTYPE html>
<html lang="ja">
<head>
  <meta charset="UTF-8">
  <title>三社神社 チェックイン判定</title>
</head>
<body>
  <h1>三社神社 チェックイン</h1>
  <p id="status">位置情報を取得中...</p>
  <script>
    const shrineLat = 34.666472;
    const shrineLng = 135.460167;
    function calculateDistance(lat1, lon1, lat2, lon2) {
      const R = 6371;
      const dLat = (lat2 - lat1) * Math.PI / 180;
      const dLon = (lon2 - lon1) * Math.PI / 180;
      const a =
        Math.sin(dLat/2) * Math.sin(dLat/2) +
        Math.cos(lat1 * Math.PI / 180) *
        Math.cos(lat2 * Math.PI / 180) *
        Math.sin(dLon/2) * Math.sin(dLon/2);
      const c = 2 * Math.atan2(Math.sqrt(a), Math.sqrt(1-a));
      return R * c;
    }
    if (navigator.geolocation) {
      navigator.geolocation.getCurrentPosition(
        (pos) => {
          const userLat = pos.coords.latitude;
          const userLng = pos.coords.longitude;
          const distance = calculateDistance(userLat, userLng, shrineLat, shrineLng);
          const status = document.getElementById("status");
          if (distance < 0.1) {
            status.textContent = `チェックイン成功！距離：約${(distance*1000).toFixed(1)}m`;
          } else {
            status.textContent = `チェックイン範囲外です（距離：約${(distance*1000).toFixed(1)}m）`;
          }
        },
        () => {
          document.getElementById("status").textContent = "位置情報の取得に失敗しました。";
        }
      );
    } else {
      document.getElementById("status").textContent = "この端末では位置情報が使えません。";
    }
  </script>
</body>
</html>
<script>
  // ユーザーに位置情報の許可を求める
  navigator.geolocation.getCurrentPosition(
    function (position) {
      const lat = position.coords.latitude;
      const lng = position.coords.longitude;

      // 地図上に現在地をマーカー表示（例）
      const marker = L.marker([lat, lng]).addTo(map);
      marker.bindPopup("あなたの現在地").openPopup();

      // 地図の中心に移動
      map.setView([lat, lng], 15);
    },
    function (error) {
      alert("位置情報の取得に失敗しました：" + error.message);
    }
  );
</script>
