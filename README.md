<!DOCTYPE html>
<html lang="it">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Meteo e Bussola Vento</title>
    <style>
        body { 
            font-family: sans-serif; 
            text-align: center; 
            background-color: #eef2f3; 
            padding: 20px; 
            color: #333;
        }
        .card { 
            background: white; 
            padding: 25px; 
            border-radius: 15px; 
            box-shadow: 0 4px 12px rgba(0,0,0,0.1); 
            max-width: 350px; 
            margin: 0 auto; 
        }
        h1 { font-size: 22px; margin-bottom: 20px; color: #005f99; }
        .data { font-size: 18px; margin: 10px 0; font-weight: bold; }
        
        /* Stile della Bussola */
        .compass-container { 
            margin: 40px auto 20px; 
            width: 200px; 
            height: 200px; 
            border-radius: 50%; 
            border: 5px solid #005f99; 
            position: relative; 
            /* Sfondo con i punti cardinali */
            background: url('data:image/svg+xml;utf8,<svg viewBox="0 0 100 100" xmlns="http://www.w3.org/2000/svg" fill="%23333" font-family="sans-serif" font-weight="bold" font-size="12"><text x="45" y="15">N</text><text x="88" y="55">E</text><text x="45" y="95">S</text><text x="5" y="55">O</text></svg>') center/cover; 
            box-shadow: inset 0 0 10px rgba(0,0,0,0.1);
        }
        /* L'ago della bussola */
        .arrow { 
            position: absolute; 
            width: 6px; 
            height: 140px; 
            background: linear-gradient(to bottom, red 50%, #333 50%); 
            top: 30px; 
            left: 97px; 
            border-radius: 3px;
            transform-origin: center center; /* Il centro esatto di rotazione */
            transition: transform 0.1s ease-out; 
        }
        
        .btn { 
            background: #005f99; 
            color: white; 
            border: none; 
            padding: 12px 20px; 
            border-radius: 8px; 
            font-size: 16px; 
            cursor: pointer; 
            margin-top: 15px; 
            width: 100%;
        }
        .note { font-size: 12px; color: #777; margin-top: 15px; }
    </style>
</head>
<body>

<div class="card">
    <h1>Condizioni Attuali</h1>
    
    <div class="data" id="temp">Temperatura: -- °C</div>
    <div class="data" id="windSpeed">Vento: -- km/h</div>
    <div class="data" id="windDir">Dir. Vento: -- °</div>

    <div class="compass-container">
        <div class="arrow" id="arrow"></div>
    </div>

    <button class="btn" id="startBtn">Attiva Posizione e Bussola</button>
    <p class="note">Premi il pulsante e consenti l'accesso alla posizione per caricare i dati e calibrare la bussola.</p>
</div>

<script>
    const startBtn = document.getElementById('startBtn');
    
    startBtn.addEventListener('click', () => {
        // 1. Richiedi la posizione GPS
        if (navigator.geolocation) {
            startBtn.innerText = "Caricamento...";
            navigator.geolocation.getCurrentPosition(position => {
                const lat = position.coords.latitude;
                const lon = position.coords.longitude;
                getWeather(lat, lon);
                startBtn.style.display = 'none'; // Nascondi il pulsante a caricamento completato
            }, error => {
                alert("Devi autorizzare la posizione per vedere il meteo locale.");
                startBtn.innerText = "Riprova";
            });
        } else {
            alert("Il tuo browser non supporta la geolocalizzazione.");
        }

        // 2. Attiva la Bussola
        // Per Android (come il tuo Samsung) usa deviceorientationabsolute
        window.addEventListener('deviceorientationabsolute', handleOrientation);
        
        // Fallback per dispositivi iOS (che richiedono un permesso speciale)
        if (typeof DeviceOrientationEvent !== 'undefined' && typeof DeviceOrientationEvent.requestPermission === 'function') {
            DeviceOrientationEvent.requestPermission().then(permissionState => {
                if (permissionState === 'granted') {
                    window.addEventListener('deviceorientation', handleOrientation);
                }
            }).catch(console.error);
        } else {
            // Fallback generico
            window.addEventListener('deviceorientation', handleOrientation);
        }
    });

    // Funzione per recuperare il meteo da Open-Meteo
    function getWeather(lat, lon) {
        const url = `https://api.open-meteo.com/v1/forecast?latitude=${lat}&longitude=${lon}&current_weather=true&windspeed_unit=kmh`;
        
        fetch(url)
            .then(response => response.json())
            .then(data => {
                const weather = data.current_weather;
                document.getElementById('temp').innerText = `Temperatura: ${weather.temperature} °C`;
                document.getElementById('windSpeed').innerText = `Vento: ${weather.windspeed} km/h`;
                document.getElementById('windDir').innerText = `Dir. Vento: ${weather.winddirection}°`;
            })
            .catch(error => console.error("Errore meteo:", error));
    }

    // Funzione per far ruotare l'ago della bussola
    function handleOrientation(event) {
        // Calcola i gradi (webkitCompassHeading per iOS, alpha per Android)
        let compass = event.webkitCompassHeading || Math.abs(event.alpha - 360);
        if (compass !== null) {
            document.getElementById('arrow').style.transform = `rotate(${compass}deg)`;
        }
    }
</script>

</body>
</html>

    function getWeather(lat, lon) {
        const url = `https://api.open-meteo.com/v1/forecast?latitude=${lat}&longitude=${lon}&current_weather=true&windspeed_unit=kmh`;
        
        fetch(url)
            .then(response => response.json())
            .then(data => {
                const weather = data.current_weather;
                document.getElementById('temp').innerText = `Temperatura: ${weather.temperature} °C`;
                document.getElementById('windSpeed').innerText = `Vento: ${weather.windspeed} km/h`;
                document.getElementById('windDir').innerText = `Dir. Vento: ${weather.winddirection}°`;
            })
            .catch(error => console.error("Errore meteo:", error));
    }

    // Funzione per far ruotare l'ago della bussola
    function handleOrientation(event) {
        // Calcola i gradi (webkitCompassHeading per iOS, alpha per Android)
        let compass = event.webkitCompassHeading || Math.abs(event.alpha - 360);
        if (compass !== null) {
            document.getElementById('arrow').style.transform = `rotate(${compass}deg)`;
        }
    }
</script>

</body>
</html>
