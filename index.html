<!DOCTYPE html>
<html lang="it">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Bussola e Meteo Mare</title>
    <style>
        body { font-family: system-ui, sans-serif; background: #0f172a; color: #f8fafc; display: flex; justify-content: center; align-items: center; min-height: 100vh; margin: 0; }
        .card { background: #1e293b; padding: 1.5rem; border-radius: 1rem; box-shadow: 0 10px 25px rgba(0,0,0,0.3); text-align: center; width: 320px; }
        
        /* Bussola */
        .compass-container { position: relative; width: 200px; height: 200px; margin: 1rem auto; border-radius: 50%; border: 6px solid #334155; background: #0f172a; display: flex; justify-content: center; align-items: center; transition: transform 0.2s ease-out; }
        .compass-ring { position: absolute; width: 100%; height: 100%; border-radius: 50%; background: radial-gradient(circle, #1e293b 60%, #334155 100%); }
        .n-marker { position: absolute; top: 10px; color: #ef4444; font-weight: bold; }
        
        .wind-arrow { position: absolute; width: 6px; height: 140px; transition: transform 0.5s; z-index: 2; }
        .wind-arrow::before { content: ''; position: absolute; top: 0; left: 50%; transform: translateX(-50%); border-left: 10px solid transparent; border-right: 10px solid transparent; border-bottom: 30px solid #38bdf8; }

        .data { margin-top: 1rem; font-size: 0.9rem; color: #94a3b8; }
        button { background: #0284c7; color: white; border: none; padding: 0.8rem; border-radius: 0.5rem; font-weight: bold; cursor: pointer; margin-top: 1rem; width: 100%; }
    </style>
</head>
<body>

<div class="card">
    <div id="compass" class="compass-container">
        <div class="compass-ring"></div>
        <div class="n-marker">N</div>
        <div id="wind-arrow" class="wind-arrow"></div>
    </div>

    <div id="status" style="font-size: 0.8rem; color: #64748b;">Attendi il GPS...</div>
    <button id="btn-compass">Attiva Bussola Reale</button>
</div>

<script>
    let windAngle = 0;

    // 1. Funzione per la bussola reale (Nord)
    document.getElementById('btn-compass').addEventListener('click', () => {
        if (typeof DeviceOrientationEvent.requestPermission === 'function') {
            DeviceOrientationEvent.requestPermission()
                .then(response => {
                    if (response == 'granted') {
                        window.addEventListener('deviceorientation', (event) => {
                            const compass = event.webkitCompassHeading || event.alpha;
                            document.getElementById('compass').style.transform = `rotate(${-compass}deg)`;
                        });
                    }
                });
        } else {
            window.addEventListener('deviceorientation', (event) => {
                const compass = event.webkitCompassHeading || event.alpha;
                document.getElementById('compass').style.transform = `rotate(${-compass}deg)`;
            });
        }
    });

    // 2. Caricamento Meteo e Freccia Vento
    async function init() {
        if ("geolocation" in navigator) {
            navigator.geolocation.getCurrentPosition(async (pos) => {
                const url = `https://api.open-meteo.com/v1/forecast?latitude=${pos.coords.latitude}&longitude=${pos.coords.longitude}&current=wind_direction_10m`;
                const res = await fetch(url);
                const data = await res.json();
                windAngle = data.current.wind_direction_10m;
                document.getElementById('wind-arrow').style.transform = `rotate(${windAngle}deg)`;
                document.getElementById('status').innerText = "Vento: " + windAngle + "°";
            });
        }
    }
    init();
</script>
</body>
</html>
