<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Modelo de Vivienda Bioclimática</title>
    <style>
        :root {
            --sky-color: #87CEEB;
            --wall-color: #8B7355;
            --roof-color: #2E8B57;
            --cool-color: rgba(0, 150, 255, 0.2);
            --hot-color: rgba(255, 50, 0, 0.4);
        }

        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            margin: 0;
            background-color: #f0f4f8;
            display: flex;
            flex-direction: column;
            align-items: center;
            padding: 20px;
        }

        h1 { color: #333; text-align: center; }
        p.subtitle { color: #666; max-width: 600px; text-align: center; margin-bottom: 20px; }

        /* Contenedor de la Simulación */
        #simulation-container {
            width: 800px;
            height: 500px;
            background-color: var(--sky-color);
            position: relative;
            overflow: hidden;
            border-radius: 12px;
            box-shadow: 0 10px 30px rgba(0,0,0,0.1);
            transition: background-color 1s ease;
        }

        /* El Suelo */
        .ground {
            position: absolute;
            bottom: 0;
            width: 100%;
            height: 80px;
            background-color: #654321;
            z-index: 10;
        }

        /* La Casa */
        .house {
            position: absolute;
            bottom: 80px;
            left: 50%;
            transform: translateX(-50%);
            width: 400px;
            height: 250px;
            background-color: #f5f5dc;
            border: 15px solid var(--wall-color);
            border-bottom: none;
            box-sizing: border-box;
            z-index: 5;
            transition: background-color 0.5s;
        }

        /* Cubierta Verde */
        .green-roof {
            position: absolute;
            top: -35px;
            left: -30px;
            width: 430px;
            height: 20px;
            background-color: var(--roof-color);
            border-radius: 10px;
        }
        .green-roof::before {
            content: "🌿 🌱 🌿 🪴 🌿 🌱";
            position: absolute;
            top: -20px;
            left: 10px;
            font-size: 18px;
            letter-spacing: 25px;
        }

        /* Ventanas para Ventilación Cruzada */
        .window {
            position: absolute;
            top: 60px;
            height: 100px;
            background-color: var(--sky-color);
            border: 4px solid #555;
            transition: background-color 1s ease;
        }
        .window-left { left: -15px; width: 20px; border-left: none; }
        .window-right { right: -15px; width: 20px; border-right: none; }

        /* CAPA 1: Flujo de Aire (Ventilación Cruzada) */
        .air-flow {
            position: absolute;
            top: 90px;
            left: -150px;
            font-size: 30px;
            color: rgba(0, 100, 255, 0.8);
            opacity: 0;
            pointer-events: none;
            z-index: 20;
        }
        .show-air .air-flow {
            animation: blowWind 3s linear infinite;
            opacity: 1;
        }
        .air-flow:nth-child(2) { top: 130px; animation-delay: 1.5s; }

        @keyframes blowWind {
            0% { transform: translateX(0); opacity: 0; }
            20% { opacity: 1; }
            80% { opacity: 1; }
            100% { transform: translateX(700px); opacity: 0; }
        }

        /* CAPA 2: Temperatura (Baja transmitancia térmica) */
        .temp-map {
            position: absolute;
            top: 0; left: 0; width: 100%; height: 100%;
            opacity: 0;
            pointer-events: none;
            transition: opacity 0.8s ease;
        }
        /* Exterior caliente, interior fresco */
        .show-temp .temp-map.outside {
            background: linear-gradient(to bottom, var(--hot-color), transparent);
            opacity: 1;
        }
        .show-temp .house {
            background-color: #a8e6cf; /* Color frío interior */
        }

        /* CAPA 3: Iluminación Solar */
        .sun {
            position: absolute;
            top: 40px;
            left: -100px;
            width: 80px;
            height: 80px;
            background-color: #FFD700;
            border-radius: 50%;
            box-shadow: 0 0 40px #FF8C00;
            transition: all 1.5s ease;
        }
        .sun-ray {
            position: absolute;
            top: 80px;
            left: 20px;
            width: 500px;
            height: 150px;
            background: linear-gradient(110deg, rgba(255,215,0,0.6) 0%, transparent 70%);
            transform: rotate(35deg);
            transform-origin: top left;
            opacity: 0;
            transition: opacity 1.5s ease;
            clip-path: polygon(0 0, 100% 0, 80% 100%, 0% 100%);
            z-index: 8;
        }
        
        .show-sun .sun { left: 50px; }
        .show-sun .sun-ray { opacity: 1; }
        .show-sun #simulation-container { background-color: #87CEEB; }

        /* Panel de Control */
        .controls {
            margin-top: 20px;
            display: flex;
            gap: 15px;
            background: white;
            padding: 15px 30px;
            border-radius: 50px;
            box-shadow: 0 4px 15px rgba(0,0,0,0.05);
        }

        button {
            padding: 10px 20px;
            font-size: 16px;
            font-weight: bold;
            cursor: pointer;
            border: 2px solid #333;
            border-radius: 25px;
            background-color: white;
            transition: all 0.3s ease;
        }

        button.active {
            background-color: #333;
            color: white;
        }

        .labels {
            position: absolute;
            background: rgba(255,255,255,0.9);
            padding: 5px 10px;
            border-radius: 5px;
            font-size: 12px;
            font-weight: bold;
            display: none;
            border: 1px solid #ccc;
        }
        
        .house:hover .labels { display: block; top: -60px; left: 100px; }

    </style>
</head>
<body>

    <h1>Modelo de Vivienda Pasiva</h1>
    <p class="subtitle">Interacciona con los botones para visualizar cómo el diseño arquitectónico logra el confort térmico sin necesidad de aire acondicionado.</p>

    <div id="simulation-container">
        <!-- Mapa de calor exterior -->
        <div class="temp-map outside"></div>
        
        <!-- El Sol -->
        <div class="sun"></div>
        
        <div class="house">
            <div class="labels">↑ Cubierta Verde<br>↔ Muros de baja transmitancia</div>
            
            <div class="green-roof"></div>
            
            <!-- Ventanas -->
            <div class="window window-left"></div>
            <div class="window window-right"></div>
            
            <!-- Rayos de sol entrando -->
            <div class="sun-ray"></div>
        </div>

        <div class="ground"></div>

        <!-- Flechas de viento -->
        <div class="air-flow">💨〰️〰️</div>
        <div class="air-flow">💨〰️〰️</div>
    </div>

    <div class="controls">
        <button id="btn-sun" onclick="toggleLayer('sun', this)">☀️ Asoleamiento</button>
        <button id="btn-wind" onclick="toggleLayer('wind', this)">🌬️ Flujo de Aire</button>
        <button id="btn-temp" onclick="toggleLayer('temp', this)">🌡️ Mapa de Calor</button>
    </div>

    <script>
        function toggleLayer(layer, btnElement) {
            const container = document.getElementById('simulation-container');
            
            // Alternar la clase activa en el botón
            btnElement.classList.toggle('active');

            // Activar/Desactivar clases en el contenedor según el botón presionado
            if (layer === 'wind') {
                container.classList.toggle('show-air');
            } else if (layer === 'temp') {
                container.classList.toggle('show-temp');
            } else if (layer === 'sun') {
                container.classList.toggle('show-sun');
            }
        }

        // Iniciar con el sol encendido por defecto
        window.onload = () => {
            document.getElementById('btn-sun').click();
        }
    </script>

</body>
</html>
