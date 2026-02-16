<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Mono Electric | Конфигуратор</title>
    <style>
        :root {
            --bg-color: #f0f2f5;
            --accent-color: #2563eb;
        }

        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background-color: var(--bg-color);
            margin: 0;
            display: flex;
            align-items: center;
            justify-content: center;
            min-height: 100vh;
            color: #333;
        }

        .configurator-card {
            background: white;
            padding: 30px;
            border-radius: 24px;
            box-shadow: 0 20px 40px rgba(0,0,0,0.1);
            max-width: 450px;
            width: 90%;
            text-align: center;
        }

        h1 { font-size: 1.5rem; margin-bottom: 5px; color: #111; }
        p { color: #666; font-size: 0.9rem; margin-bottom: 25px; }

        /* Сцена с визуализацией */
        .preview-zone {
            width: 100%;
            height: 250px;
            border-radius: 16px;
            display: flex;
            align-items: center;
            justify-content: center;
            transition: background 0.5s cubic-bezier(0.4, 0, 0.2, 1);
            margin-bottom: 30px;
            position: relative;
            overflow: hidden;
            border: 1px solid rgba(0,0,0,0.05);
        }

        /* Реалистичная розетка */
        .socket-frame {
            width: 110px;
            height: 110px;
            border-radius: 14px;
            background: white;
            display: flex;
            align-items: center;
            justify-content: center;
            box-shadow: 
                0 10px 20px rgba(0,0,0,0.15),
                inset 0 1px 2px rgba(255,255,255,0.8);
            transition: background 0.3s ease;
            position: relative;
        }

        .socket-inner {
            width: 75px;
            height: 75px;
            border-radius: 50%;
            background: inherit;
            filter: brightness(97%);
            box-shadow: inset 0 2px 4px rgba(0,0,0,0.1);
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 15px;
        }

        .socket-inner::before, .socket-inner::after {
            content: '';
            width: 10px;
            height: 10px;
            background: rgba(0,0,0,0.15);
            border-radius: 50%;
            box-shadow: inset 0 1px 2px rgba(0,0,0,0.3);
        }

        /* Группы кнопок */
        .controls-group {
            margin-bottom: 20px;
            text-align: left;
        }

        .label {
            font-weight: 600;
            font-size: 0.8rem;
            text-transform: uppercase;
            letter-spacing: 1px;
            margin-bottom: 10px;
            display: block;
            color: #888;
        }

        .swatch-list {
            display: flex;
            flex-wrap: wrap;
            gap: 10px;
        }

        .swatch {
            width: 36px;
            height: 36px;
            border-radius: 50%;
            border: 2px solid white;
            outline: 1px solid #ddd;
            cursor: pointer;
            transition: 0.2s;
            padding: 0;
        }

        .swatch:hover { transform: scale(1.1); }
        .swatch.active { outline: 2px solid var(--accent-color); outline-offset: 2px; }

    </style>
</head>
<body>

<div class="configurator-card">
    <h1>Mono Electric</h1>
    <p>Создайте идеальное сочетание для вашего интерьера</p>

    <div class="preview-zone" id="wall">
        <div class="socket-frame" id="socket">
            <div class="socket-inner"></div>
        </div>
    </div>

    <div class="controls-group">
        <span class="label">Цвет стены</span>
        <div class="swatch-list" id="wall-colors">
            </div>
    </div>

    <div class="controls-group">
        <span class="label">Цвет розетки</span>
        <div class="swatch-list" id="socket-colors">
            </div>
    </div>
</div>

<script>
    const wallColors = [
        { name: 'Песочный', hex: '#e8d8c3' },
        { name: 'Серый шелк', hex: '#cccccc' },
        { name: 'Графит', hex: '#4a4a4a' },
        { name: 'Теракот', hex: '#b36a5e' },
        { name: 'Хвоя', hex: '#5e716a' }
    ];

    const socketColors = [
        { name: 'Белый', hex: '#ffffff' },
        { name: 'Черный матовый', hex: '#1a1a1a' },
        { name: 'Серебро', hex: '#a6a6a6' },
        { name: 'Золото', hex: '#d4af37' }
    ];

    function initConfigurator() {
        renderSwatches('wall-colors', wallColors, changeWall);
        renderSwatches('socket-colors', socketColors, changeSocket);
        
        // Установка начальных значений
        changeWall(wallColors[0].hex, document.querySelector('#wall-colors .swatch'));
        changeSocket(socketColors[0].hex, document.querySelector('#socket-colors .swatch'));
    }

    function renderSwatches(containerId, colors, callback) {
        const container = document.getElementById(containerId);
        colors.forEach(color => {
            const btn = document.createElement('button');
            btn.className = 'swatch';
            btn.style.background = color.hex;
            btn.title = color.name;
            btn.onclick = (e) => callback(color.hex, e.target);
            container.appendChild(btn);
        });
    }

    function changeWall(color, el) {
        document.getElementById("wall").style.background = color;
        updateActiveState('wall-colors', el);
    }

    function changeSocket(color, el) {
        const socket = document.getElementById("socket");
        socket.style.background = color;
        // Автоматическая подстройка цвета отверстий для темных розеток
        socket.style.color = (color === '#1a1a1a') ? 'rgba(255,255,255,0.2)' : 'rgba(0,0,0,0.15)';
        updateActiveState('socket-colors', el);
    }

    function updateActiveState(containerId, activeEl) {
        document.querySelectorAll(`#${containerId} .swatch`).forEach(btn => btn.classList.remove('active'));
        activeEl.classList.add('active');
    }

    initConfigurator();
</script>

</body>
</html>
