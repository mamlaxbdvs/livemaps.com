<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Location Sharing App</title>
  <style>
    body {
      margin: 0;
      padding: 0;
      font-family: "Segoe UI", sans-serif;
      background: linear-gradient(to right, #667eea, #764ba2);
      color: white;
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: center;
      height: 100vh;
      text-align: center;
    }

    .container {
      background-color: rgba(0, 0, 0, 0.3);
      padding: 2rem;
      border-radius: 15px;
      box-shadow: 0 8px 16px rgba(0,0,0,0.2);
      max-width: 400px;
      width: 90%;
    }

    h1 {
      margin-bottom: 1rem;
    }

    button {
      background-color: #fff;
      color: #333;
      border: none;
      padding: 12px 20px;
      margin: 10px 5px;
      border-radius: 8px;
      cursor: pointer;
      font-size: 16px;
      transition: background 0.3s;
    }

    button:hover {
      background-color: #eee;
    }

    #output {
      margin-top: 20px;
    }

    a {
      color: #ffd700;
      font-weight: bold;
    }

    .hidden {
      display: none;
    }
  </style>
</head>
<body>
  <div class="container">
    <h1>Share Your Location</h1>
    <button onclick="getLocation()">Get My Location</button>
    <div id="output" class="hidden">
      <p id="coords"></p>
      <p><a id="mapLink" href="#" target="_blank">Open in Google Maps</a></p>
      <button onclick="copyLink()">Copy Link</button>
      <button onclick="shareLink()">Share</button>
      <p id="status"></p>
    </div>
  </div>

  <script>
    let currentLink = "";

    function getLocation() {
      const output = document.getElementById('output');
      const coords = document.getElementById('coords');
      const mapLink = document.getElementById('mapLink');
      const status = document.getElementById('status');

      output.classList.remove('hidden');
      coords.textContent = "Getting location...";
      status.textContent = "";

      if (!navigator.geolocation) {
        coords.textContent = "Geolocation not supported.";
        return;
      }

      navigator.geolocation.getCurrentPosition(
        (position) => {
          const lat = position.coords.latitude.toFixed(6);
          const lon = position.coords.longitude.toFixed(6);
          currentLink = `https://www.google.com/maps?q=${lat},${lon}`;

          coords.innerHTML = `<strong>Latitude:</strong> ${lat}<br><strong>Longitude:</strong> ${lon}`;
          mapLink.href = currentLink;
          mapLink.textContent = "Open in Google Maps";
        },
        (err) => {
          coords.textContent = "Unable to retrieve your location.";
          console.error(err);
        }
      );
    }

    function copyLink() {
      if (!currentLink) return;
      navigator.clipboard.writeText(currentLink).then(() => {
        document.getElementById("status").textContent = "Link copied to clipboard!";
      });
    }

    function shareLink() {
      if (navigator.share && currentLink) {
        navigator.share({
          title: "My Location",
          text: "Here's my location:",
          url: currentLink,
        }).catch((err) => console.log("Share failed:", err));
      } else {
        document.getElementById("status").textContent = "Sharing not supported on this device.";
      }
    }
  </script>
</body>
</html>
