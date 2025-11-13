<html lang="es">
<head> 
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>BrightWay - Innovación en Seguridad Vial</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            -webkit-tap-highlight-color: transparent;
        }

        :root {
            --primary: #FF6B35;
            --primary-light: #FF8C42;
            --accent: #FFC107;
            --dark: #0A0A0A;
            --darker: #000000;
            --gray-600: #404040;
            --gray-400: #888888;
            --gray-300: #CCCCCC;
            --white: #FFFFFF;
            --success: #10B981;
            --danger: #EF4444;
            --shield: #3B82F6;
        }

        body {
            font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;
            background: linear-gradient(135deg, var(--darker) 0%, var(--dark) 100%);
            color: var(--white);
            min-height: 100vh;
            line-height: 1.6;
            overflow-x: hidden;
        }

        .audio-controls {
            position: fixed;
            top: 20px;
            right: 20px;
            z-index: 2000;
        }

        .audio-btn {
            width: 48px;
            height: 48px;
            background: rgba(255, 255, 255, 0.1);
            border: 2px solid rgba(255, 255, 255, 0.2);
            border-radius: 50%;
            color: var(--white);
            font-size: 1.2rem;
            cursor: pointer;
            transition: all 0.3s ease;
            display: flex;
            align-items: center;
            justify-content: center;
            backdrop-filter: blur(10px);
        }

        .audio-btn:hover {
            background: rgba(255, 107, 53, 0.3);
            border-color: var(--primary);
            transform: scale(1.1);
        }

        .volume-panel {
            position: fixed;
            top: 80px;
            right: 20px;
            z-index: 1999;
            background: rgba(10, 10, 10, 0.95);
            padding: 1.5rem;
            border-radius: 15px;
            border: 1px solid rgba(255, 255, 255, 0.1);
            backdrop-filter: blur(10px);
            display: none;
            min-width: 280px;
            box-shadow: 0 10px 40px rgba(0, 0, 0, 0.5);
        }

        .volume-panel.show {
            display: block;
            animation: slideIn 0.3s ease;
        }

        @keyframes slideIn {
            from { opacity: 0; transform: translateY(-10px); }
            to { opacity: 1; transform: translateY(0); }
        }

        .volume-panel h4 {
            color: var(--primary);
            font-size: 0.9rem;
            margin-bottom: 1rem;
            text-align: center;
        }

        .music-selector {
            display: grid;
            grid-template-columns: repeat(3, 1fr);
            gap: 0.75rem;
            margin-bottom: 1.5rem;
        }

        .music-option {
            width: 100%;
            aspect-ratio: 1;
            background: rgba(255, 255, 255, 0.05);
            border: 2px solid rgba(255, 255, 255, 0.1);
            border-radius: 12px;
            cursor: pointer;
            transition: all 0.3s ease;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 1.5rem;
        }

        .music-option:hover {
            background: rgba(255, 107, 53, 0.2);
            border-color: var(--primary);
            transform: scale(1.05);
        }

        .music-option.active {
            background: rgba(255, 107, 53, 0.3);
            border-color: var(--primary);
            box-shadow: 0 0 15px rgba(255, 107, 53, 0.4);
        }

        .volume-control {
            display: flex;
            flex-direction: column;
            gap: 1rem;
        }

        .volume-row {
            display: flex;
            align-items: center;
            gap: 0.75rem;
        }

        .mute-btn {
            width: 36px;
            height: 36px;
            min-width: 36px;
            background: rgba(255, 255, 255, 0.05);
            border: 1px solid rgba(255, 255, 255, 0.1);
            border-radius: 8px;
            color: var(--white);
            font-size: 1rem;
            cursor: pointer;
            transition: all 0.3s ease;
            display: flex;
            align-items: center;
            justify-content: center;
        }

        .mute-btn:hover {
            background: rgba(255, 107, 53, 0.2);
            border-color: var(--primary);
        }

        .mute-btn.muted {
            background: rgba(239, 68, 68, 0.2);
            border-color: var(--danger);
            color: var(--danger);
        }

        .volume-slider-container {
            flex: 1;
            display: flex;
            flex-direction: column;
            gap: 0.5rem;
        }

        .volume-label {
            font-size: 0.8rem;
            color: var(--gray-400);
            display: flex;
            justify-content: space-between;
            align-items: center;
        }

        .volume-value {
            font-size: 0.8rem;
            color: var(--accent);
            font-weight: 600;
        }

        .volume-slider {
            -webkit-appearance: none;
            appearance: none;
            width: 100%;
            height: 6px;
            border-radius: 3px;
            background: rgba(255, 255, 255, 0.1);
            outline: none;
            cursor: pointer;
        }

        .volume-slider::-webkit-slider-thumb {
            -webkit-appearance: none;
            appearance: none;
            width: 18px;
            height: 18px;
            border-radius: 50%;
            background: linear-gradient(135deg, var(--primary), var(--accent));
            cursor: pointer;
            transition: all 0.3s ease;
        }

        .volume-slider::-webkit-slider-thumb:hover {
            transform: scale(1.2);
            box-shadow: 0 0 15px rgba(255, 107, 53, 0.6);
        }

        .volume-slider::-moz-range-thumb {
            width: 18px;
            height: 18px;
            border-radius: 50%;
            background: linear-gradient(135deg, var(--primary), var(--accent));
            cursor: pointer;
            border: none;
            transition: all 0.3s ease;
        }

        nav {
            position: fixed;
            top: 0;
            width: 100%;
            background: rgba(10, 10, 10, 0.95);
            backdrop-filter: blur(20px);
            border-bottom: 1px solid rgba(255, 255, 255, 0.05);
            z-index: 1000;
            padding: 1.5rem 0;
        }

        .nav-container {
            max-width: 1200px;
            margin: 0 auto;
            display: flex;
            justify-content: center;
            align-items: center;
            padding: 0 1.5rem;
        }

        .logo {
            display: flex;
            align-items: center;
            gap: 0.75rem;
            font-size: 1.75rem;
            font-weight: 700;
            color: var(--primary);
        }

        .logo-icon {
            width: 40px;
            height: 40px;
            background: linear-gradient(135deg, var(--primary), var(--accent));
            border-radius: 10px;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 1.5rem;
        }

        main {
            padding-top: 100px;
            min-height: 100vh;
        }

        .container {
            max-width: 1200px;
            margin: 0 auto;
            padding: 0 1.5rem;
        }

        .hero {
            padding: 4rem 0;
            text-align: center;
        }

        .hero-title {
            font-size: clamp(2.5rem, 8vw, 5rem);
            font-weight: 800;
            background: linear-gradient(135deg, var(--primary), var(--accent));
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            background-clip: text;
            margin-bottom: 1rem;
            line-height: 1.1;
        }

        .hero-subtitle {
            font-size: clamp(1rem, 4vw, 1.25rem);
            color: var(--gray-400);
            margin-bottom: 2rem;
            font-weight: 400;
        }

        .hero-description {
            max-width: 700px;
            margin: 0 auto 3rem;
            font-size: clamp(0.95rem, 3vw, 1.1rem);
            color: var(--gray-300);
            line-height: 1.8;
        }

        .product-showcase {
            margin: 3rem 0;
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 350px;
        }

        .trafitambo {
            position: relative;
            width: 120px;
            height: 250px;
            margin: 0 auto;
            animation: float 6s ease-in-out infinite;
        }

        @keyframes float {
            0%, 100% { transform: translateY(0) rotateY(0deg); }
            33% { transform: translateY(-10px) rotateY(120deg); }
            66% { transform: translateY(5px) rotateY(240deg); }
        }

        .trafitambo-body {
            width: 100%;
            height: 200px;
            background: linear-gradient(180deg, var(--primary) 0%, var(--primary-light) 50%, var(--primary) 100%);
            border-radius: 12px;
            position: relative;
            box-shadow: 0 20px 60px rgba(255, 107, 53, 0.3);
        }

        .reflective-stripe {
            position: absolute;
            width: 100%;
            height: 20px;
            background: var(--white);
            top: 60px;
            opacity: 0.9;
            animation: shine 3s ease-in-out infinite;
        }

        @keyframes shine {
            0%, 100% { opacity: 0.9; }
            50% { opacity: 1; box-shadow: 0 0 20px rgba(255, 255, 255, 0.5); }
        }

        .solar-panel {
            position: absolute;
            top: -30px;
            left: 50%;
            transform: translateX(-50%);
            width: 80px;
            height: 50px;
            background: linear-gradient(135deg, #1E3A8A, #3B82F6);
            border-radius: 8px;
            border: 2px solid #60A5FA;
            box-shadow: 0 10px 30px rgba(59, 130, 246, 0.4);
        }

        .led-strip {
            position: absolute;
            bottom: 20px;
            left: 50%;
            transform: translateX(-50%);
            display: flex;
            gap: 8px;
        }

        .led {
            width: 8px;
            height: 8px;
            background: var(--accent);
            border-radius: 50%;
            box-shadow: 0 0 15px var(--accent);
            animation: blink 2s ease-in-out infinite;
        }

        .led:nth-child(2) { animation-delay: 0.5s; }
        .led:nth-child(3) { animation-delay: 1s; }

        @keyframes blink {
            0%, 50% { opacity: 1; }
            25%, 75% { opacity: 0.3; }
        }

        .btn-play {
            background: linear-gradient(135deg, var(--primary), var(--primary-light));
            color: var(--white);
            border: none;
            padding: 1.25rem 3rem;
            border-radius: 50px;
            font-size: 1.1rem;
            font-weight: 600;
            cursor: pointer;
            transition: all 0.3s ease;
            box-shadow: 0 10px 30px rgba(255, 107, 53, 0.3);
            margin: 0.5rem;
        }

        .btn-play:hover {
            transform: translateY(-5px);
            box-shadow: 0 15px 40px rgba(255, 107, 53, 0.5);
        }

        .features {
            padding: 4rem 0;
            background: rgba(255, 255, 255, 0.02);
        }

        .features-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(280px, 1fr));
            gap: 2rem;
        }

        .feature-card {
            background: rgba(255, 255, 255, 0.03);
            border: 1px solid rgba(255, 255, 255, 0.05);
            border-radius: 16px;
            padding: 2rem;
            transition: all 0.3s ease;
            backdrop-filter: blur(10px);
            cursor: pointer;
        }

        .feature-card:hover {
            transform: translateY(-8px);
            border-color: rgba(255, 107, 53, 0.3);
            box-shadow: 0 20px 60px rgba(255, 107, 53, 0.1);
        }

        .feature-icon {
            width: 48px;
            height: 48px;
            background: linear-gradient(135deg, var(--primary), var(--accent));
            border-radius: 12px;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 1.5rem;
            margin-bottom: 1.5rem;
        }

        .feature-title {
            font-size: 1.25rem;
            font-weight: 600;
            margin-bottom: 0.75rem;
        }

        .feature-description {
            color: var(--gray-400);
            font-size: 0.95rem;
            line-height: 1.6;
        }

        .game-modal {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100vh;
            background: rgba(0, 0, 0, 0.98);
            z-index: 3000;
            display: none;
        }

        .game-modal.active {
            display: block;
        }

        .game-container {
            width: 100%;
            height: 100%;
            position: relative;
        }

        #gameCanvas {
            width: 100%;
            height: 100%;
            display: block;
            position: absolute;
            top: 0;
            left: 0;
            z-index: 1;
        }

        .game-hud {
            position: absolute;
            top: 15px;
            left: 15px;
            right: 15px;
            display: flex;
            justify-content: space-between;
            align-items: flex-start;
            gap: 1rem;
            pointer-events: none;
            z-index: 10;
        }

        .hud-stat {
            background: rgba(10, 10, 10, 0.9);
            backdrop-filter: blur(10px);
            border: 2px solid rgba(255, 107, 53, 0.4);
            border-radius: 16px;
            padding: 0.85rem 1.2rem;
            pointer-events: auto;
            box-shadow: 0 6px 20px rgba(0, 0, 0, 0.6);
            min-width: 110px;
        }

        .hud-label {
            font-size: 0.85rem;
            color: var(--gray-400);
            margin-bottom: 0.25rem;
            text-transform: uppercase;
            letter-spacing: 0.8px;
            font-weight: 600;
        }

        .hud-value {
            font-size: 1.4rem;
            font-weight: 800;
            color: var(--accent);
        }

        .speed-value {
            font-size: 1.8rem;
            font-weight: 900;
            background: linear-gradient(135deg, var(--primary), var(--accent));
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            background-clip: text;
        }

        .speed-unit {
            font-size: 0.85rem;
            color: var(--gray-400);
            margin-left: 3px;
        }

        .shield-indicator {
            background: rgba(59, 130, 246, 0.2);
            border: 2px solid var(--shield);
            display: none;
        }

        .shield-indicator.active {
            display: block;
            animation: shieldPulse 1s ease-in-out infinite;
        }

        .shield-indicator.warning {
            animation: shieldBlink 0.3s ease-in-out infinite;
        }

        @keyframes shieldPulse {
            0%, 100% { box-shadow: 0 0 25px rgba(59, 130, 246, 0.6); }
            50% { box-shadow: 0 0 45px rgba(59, 130, 246, 0.9); }
        }

        @keyframes shieldBlink {
            0%, 100% { opacity: 1; }
            50% { opacity: 0.3; }
        }

        .game-controls {
            position: absolute;
            bottom: 15px;
            left: 15px;
            display: none;
            flex-direction: column;
            gap: 0.75rem;
            z-index: 200;
            pointer-events: auto;
        }
        
        .game-controls.show {
            display: flex;
        }

        .btn {
            background: linear-gradient(135deg, var(--primary), var(--primary-light));
            color: var(--white);
            border: none;
            padding: 0.9rem 1.8rem;
            border-radius: 50px;
            font-size: 1.05rem;
            font-weight: 700;
            cursor: pointer;
            transition: all 0.3s ease;
            min-width: 160px;
            text-align: center;
            box-shadow: 0 4px 15px rgba(255, 107, 53, 0.3);
            pointer-events: auto;
            user-select: none;
            -webkit-user-select: none;
            -webkit-tap-highlight-color: transparent;
        }

        .btn:hover:not(:disabled) {
            transform: translateX(5px);
            box-shadow: 0 6px 25px rgba(255, 107, 53, 0.5);
        }
        
        .btn:active:not(:disabled) {
            transform: translateX(3px) scale(0.98);
        }

        .btn:disabled {
            opacity: 0.5;
            cursor: not-allowed;
        }

        .btn-exit {
            background: linear-gradient(135deg, var(--danger), #DC2626);
        }

        .btn-brightway {
            background: linear-gradient(135deg, var(--accent), #D97706);
            position: relative;
            overflow: hidden;
        }

        .btn-brightway.active {
            animation: pulse 1s ease-in-out infinite;
        }

        @keyframes pulse {
            0%, 100% { box-shadow: 0 0 25px rgba(255, 193, 7, 0.6); }
            50% { box-shadow: 0 0 45px rgba(255, 193, 7, 0.9); }
        }

        .selector-screen {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(0, 0, 0, 0.95);
            display: flex;
            align-items: center;
            justify-content: center;
            z-index: 100;
            padding: 1rem;
        }

        .selector-screen.hidden {
            display: none;
        }

        .selector-content {
            background: rgba(10, 10, 10, 0.95);
            backdrop-filter: blur(20px);
            padding: 2rem;
            border-radius: 20px;
            border: 3px solid var(--primary);
            text-align: center;
            max-width: 90%;
            width: 700px;
            max-height: 90vh;
            overflow-y: auto;
        }

        .selector-content h2 {
            color: var(--primary);
            font-size: clamp(1.5rem, 5vw, 2.2rem);
            margin-bottom: 1rem;
        }

        .selector-content p {
            color: var(--gray-400);
            margin-bottom: 1.5rem;
            font-size: clamp(0.9rem, 3vw, 1.1rem);
        }

        .options-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(150px, 1fr));
            gap: 1rem;
            margin-bottom: 1.5rem;
        }

        .option-card {
            background: rgba(255, 255, 255, 0.05);
            border: 3px solid rgba(255, 255, 255, 0.1);
            border-radius: 16px;
            padding: 1.2rem;
            cursor: pointer;
            transition: all 0.3s ease;
            position: relative;
        }

        .option-card:hover {
            transform: scale(1.05);
            border-color: rgba(255, 255, 255, 0.3);
        }

        .option-card.selected {
            border-color: var(--primary);
            box-shadow: 0 0 30px rgba(255, 107, 53, 0.5);
            transform: scale(1.05);
        }

        .map-preview {
            width: 100%;
            height: 100px;
            border-radius: 12px;
            margin-bottom: 0.8rem;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 3rem;
            position: relative;
            overflow: hidden;
        }

        .map-preview.city {
            background: linear-gradient(135deg, #1a1a2e, #16213e);
        }

        .map-preview.snow {
            background: linear-gradient(135deg, #e0f2fe, #bae6fd);
        }

        .map-preview.volcano {
            background: linear-gradient(135deg, #450a0a, #7f1d1d);
        }

        .map-preview.desert {
            background: linear-gradient(135deg, #fbbf24, #f59e0b);
        }

        .car-preview {
            width: 100%;
            height: 60px;
            border-radius: 12px;
            margin-bottom: 0.8rem;
            display: flex;
            align-items: center;
            justify-content: center;
            position: relative;
            overflow: hidden;
        }

        .car-preview::before {
            content: '';
            position: absolute;
            top: 0;
            left: 0;
            right: 0;
            height: 35%;
            background: linear-gradient(180deg, rgba(0,0,0,0.9) 0%, rgba(0,0,0,0.3) 100%);
            border-radius: 12px 12px 0 0;
        }

        .car-preview.blue {
            background: linear-gradient(135deg, #3B82F6, #1E40AF);
            box-shadow: 0 8px 24px rgba(59, 130, 246, 0.4);
        }

        .car-preview.red {
            background: linear-gradient(135deg, #EF4444, #DC2626);
            box-shadow: 0 8px 24px rgba(239, 68, 68, 0.4);
        }

        .car-preview.yellow {
            background: linear-gradient(135deg, #FBBF24, #F59E0B);
            box-shadow: 0 8px 24px rgba(251, 191, 36, 0.4);
        }

        .car-preview.green {
            background: linear-gradient(135deg, #059669, #047857);
            box-shadow: 0 8px 24px rgba(5, 150, 105, 0.4);
        }

        .option-card h3 {
            color: var(--white);
            font-size: clamp(0.9rem, 3vw, 1.15rem);
            margin: 0;
        }

        .game-over-screen {
            position: absolute;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            background: rgba(10, 10, 10, 0.97);
            backdrop-filter: blur(20px);
            padding: 3rem;
            border-radius: 24px;
            border: 3px solid var(--primary);
            text-align: center;
            display: none;
            z-index: 100;
            max-width: 90%;
        }

        .game-over-screen.show {
            display: block;
            animation: fadeInScale 0.4s ease;
        }

        @keyframes fadeInScale {
            0% { opacity: 0; transform: translate(-50%, -50%) scale(0.8); }
            100% { opacity: 1; transform: translate(-50%, -50%) scale(1); }
        }

        .game-over-screen h2 {
            color: var(--primary);
            font-size: clamp(1.8rem, 6vw, 2.8rem);
            margin-bottom: 1.5rem;
        }

        .final-score {
            font-size: clamp(2.5rem, 8vw, 3.5rem);
            color: var(--accent);
            font-weight: 800;
            margin-bottom: 2rem;
        }

        footer {
            padding: 3rem 0 2rem;
            border-top: 1px solid rgba(255, 255, 255, 0.05);
            text-align: center;
            color: var(--gray-400);
            background: rgba(255, 255, 255, 0.02);
        }

        .footer-logo {
            display: flex;
            justify-content: center;
            align-items: center;
            gap: 0.5rem;
            margin-bottom: 1rem;
        }

        .footer-logo .company-name {
            font-size: 1.25rem;
            font-weight: 700;
            background: linear-gradient(135deg, var(--primary), var(--accent));
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            background-clip: text;
        }

        .instructions {
            background: rgba(255, 255, 255, 0.05);
            border: 2px solid rgba(255, 255, 255, 0.1);
            border-radius: 15px;
            padding: 2rem;
            margin-top: 2rem;
            text-align: left;
            max-width: 100%;
            max-height: 100%;
            overflow-y: auto;
            margin-left: auto;
            margin-right: auto;
        }

        .instructions h4 {
            color: var(--primary);
            font-size: 1.3rem;
            margin-bottom: 1rem;
        }

        .instructions p {
            color: var(--gray-300);
            font-size: 1rem;
            margin-bottom: 0.8rem;
            line-height: 1.6;
        }

        .combo-indicator {
            position: absolute;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            font-size: 3rem;
            font-weight: 900;
            color: var(--accent);
            text-shadow: 0 0 20px rgba(255, 193, 7, 0.8);
            opacity: 0;
            pointer-events: none;
            z-index: 50;
        }

        .combo-indicator.show {
            animation: comboAppear 1s ease-out;
        }

        @keyframes comboAppear {
            0% { opacity: 0; transform: translate(-50%, -50%) scale(0.5); }
            50% { opacity: 1; transform: translate(-50%, -50%) scale(1.2); }
            100% { opacity: 0; transform: translate(-50%, -50%) scale(1); }
        }

        @media (max-width: 768px) {
            .options-grid {
                grid-template-columns: repeat(2, 1fr);
                gap: 0.8rem;
            }

            .selector-content {
                padding: 1.5rem;
            }

            .hud-stat {
                padding: 0.6rem 0.9rem;
                min-width: 90px;
            }

            .hud-label {
                font-size: 0.7rem;
            }

            .hud-value {
                font-size: 1.1rem;
            }

            .speed-value {
                font-size: 1.4rem;
            }

            .btn {
                padding: 0.8rem 1.5rem;
                font-size: 0.95rem;
                min-width: 140px;
            }

            .audio-controls {
                top: 80px;
            }
        }
    </style>
</head>
<body>
    <div class="audio-controls">
        <button class="audio-btn" id="volumeBtn" title="Volumen">🎧</button>
    </div>

    <div class="volume-panel" id="volumePanel">
        <h4>Control de Volumen</h4>
        
        <div class="music-selector">
            <div class="music-option" id="marioMusic" title="Super Mario Bros">🍄</div>
            <div class="music-option active" id="megaloMusic" title="Megalovania">💀</div>
            <div class="music-option" id="starWarsMusic" title="Star Wars">⭐</div>
            <div class="music-option" id="tetrisMusic" title="Tetris">🧱</div>
            <div class="music-option" id="piratesMusic" title="Pirates">🏴‍☠️</div>
            <div class="music-option" id="pokemonMusic" title="Pokémon">⚡</div>
        </div>
        
        <div class="volume-control">
            <div class="volume-row">
                <button class="mute-btn" id="musicMuteBtn" title="Silenciar música">🎵</button>
                <div class="volume-slider-container">
                    <div class="volume-label">
                        <span>Música</span>
                        <span class="volume-value" id="musicVolumeValue">30%</span>
                    </div>
                    <input type="range" min="0" max="100" value="30" class="volume-slider" id="musicVolume">
                </div>
            </div>
            <div class="volume-row">
                <button class="mute-btn" id="soundMuteBtn" title="Silenciar efectos">🔊</button>
                <div class="volume-slider-container">
                    <div class="volume-label">
                        <span>Efectos</span>
                        <span class="volume-value" id="soundVolumeValue">50%</span>
                    </div>
                    <input type="range" min="0" max="100" value="50" class="volume-slider" id="soundVolume">
                </div>
            </div>
        </div>
    </div>

    <nav>
        <div class="nav-container">
            <div class="logo">
                <div class="logo-icon">💡</div>
                BrightWay
            </div>
        </div>
    </nav>

    <main>
        <div class="container">
            <div class="hero">
                <h1 class="hero-title">BrightWay</h1>
                <p class="hero-subtitle">Dale luz a tu camino</p>
                <p class="hero-description">
                    BrightWay vela por tu seguridad al implementar tecnologías modernas y buenas para el ambiente. 
                    Un innovador sistema de señalización vial que combina un trafitambo tradicional con tecnología 
                    solar avanzada, proporcionando iluminación autónoma y sostenible para mejorar la seguridad vial.
                </p>
            </div>

            <div class="product-showcase">
                <div class="trafitambo">
                    <div class="solar-panel"></div>
                    <div class="trafitambo-body">
                        <div class="reflective-stripe"></div>
                        <div class="led-strip">
                            <div class="led"></div>
                            <div class="led"></div>
                            <div class="led"></div>
                        </div>
                    </div>
                </div>
            </div>

            <div style="text-align: center;">
                <button class="btn-play" id="playGameBtn">🎮 Jugar Racing 3D</button>
                <button class="btn-play" id="instructionsBtn" style="background: rgba(255, 255, 255, 0.1);">📖 Controles</button>
            </div>
        </div>

        <div class="features">
            <div class="container">
                <div class="features-grid">
                    <div class="feature-card">
                        <div class="feature-icon">☀️</div>
                        <h3 class="feature-title">Energía Solar</h3>
                        <p class="feature-description">Panel solar móvil con servomotores para optimizar la captación de energía durante todo el día.</p>
                    </div>
                    <div class="feature-card">
                        <div class="feature-icon">💡</div>
                        <h3 class="feature-title">Iluminación LED</h3>
                        <p class="feature-description">Sistema de LEDs de alta eficiencia con sensor de luz para máxima visibilidad nocturna.</p>
                    </div>
                    <div class="feature-card">
                        <div class="feature-icon">🌱</div>
                        <h3 class="feature-title">Eco-Amigable</h3>
                        <p class="feature-description">Tecnología 100% sustentable que no requiere conexión eléctrica externa.</p>
                    </div>
                    <div class="feature-card">
                        <div class="feature-icon">🔧</div>
                        <h3 class="feature-title">Arduino Nano</h3>
                        <p class="feature-description">Control inteligente mediante microcontrolador para gestión automática del sistema.</p>
                    </div>
                </div>
            </div>
        </div>
    </main>

    <footer>
        <div class="container">
            <div class="footer-logo">
                <div class="logo-icon">💡</div>
                <span class="company-name">BrightWay Technologies</span>
            </div>
            <div style="color: var(--gray-400); margin: 1rem 0;">
                Desarrollado por: <strong style="color: var(--primary);">Pablo Nicolas Garcia Huerta</strong> y <strong style="color: var(--primary);">Angel Antonio Ruiz Garcia</strong>
            </div>
            <div style="font-size: 0.85rem; margin-bottom: 1rem;">Universidad Autónoma de Querétaro - Facultad de Ingeniería</div>
            <div style="font-size: 0.8rem; padding-top: 1rem; border-top: 1px solid rgba(255, 255, 255, 0.05);">
                © 2025 BrightWay Technologies. Todos los derechos reservados.
            </div>
        </div>
    </footer>

    <div class="game-modal" id="gameModal">
        <div class="game-container">
            <div class="selector-screen" id="mapSelectorScreen">
                <div class="selector-content">
                    <h2>🗺️ Elige tu Mapa</h2>
                    <p>Selecciona el escenario donde quieres correr</p>
                    
                    <div class="options-grid">
                        <div class="option-card" data-map="city">
                            <div class="map-preview city">🌃</div>
                            <h3>Ciudad Nocturna</h3>
                        </div>
                        <div class="option-card" data-map="snow">
                            <div class="map-preview snow">🏔️</div>
                            <h3>Montañas Nevadas</h3>
                        </div>
                        <div class="option-card" data-map="volcano">
                            <div class="map-preview volcano">🌋</div>
                            <h3>Volcán Ardiente</h3>
                        </div>
                        <div class="option-card" data-map="desert">
                            <div class="map-preview desert">🏜️</div>
                            <h3>Desierto Dorado</h3>
                        </div>
                    </div>
                </div>
            </div>

            <div class="selector-screen hidden" id="carSelectorScreen">
                <div class="selector-content">
                    <h2>🎮 Elige tu Auto</h2>
                    <p>Selecciona el color de tu vehículo para comenzar</p>
                    
                    <div class="options-grid">
                        <div class="option-card" data-color="blue">
                            <div class="car-preview blue"></div>
                            <h3>Azul Eléctrico</h3>
                        </div>
                        <div class="option-card" data-color="red">
                            <div class="car-preview red"></div>
                            <h3>Rojo Furioso</h3>
                        </div>
                        <div class="option-card" data-color="yellow">
                            <div class="car-preview yellow"></div>
                            <h3>Amarillo Solar</h3>
                        </div>
                        <div class="option-card" data-color="green">
                            <div class="car-preview green"></div>
                            <h3>Verde Esmeralda</h3>
                        </div>
                    </div>
                </div>
            </div>

            <canvas id="gameCanvas"></canvas>
            
            <div class="game-hud">
                <div class="hud-stat">
                    <div class="hud-label">Puntos</div>
                    <div class="hud-value" id="score">0</div>
                </div>
                <div class="hud-stat shield-indicator" id="shieldIndicator">
                    <div class="hud-label">🛡️ Escudo</div>
                    <div class="hud-value" id="shieldTime">10</div>
                </div>
                <div class="hud-stat">
                    <div class="hud-label">Record</div>
                    <div class="hud-value" id="highScore">0</div>
                </div>
                <div class="hud-stat">
                    <div class="hud-label">Velocidad</div>
                    <div>
                        <span class="speed-value" id="speedValue">0</span>
                        <span class="speed-unit">km/h</span>
                    </div>
                </div>
            </div>

            <div class="combo-indicator" id="comboIndicator">COMBO x2!</div>
            
            <div class="game-controls" id="gameControls">
                <button class="btn btn-brightway" id="brightwayBtn">💡 BrightWay</button>
                <button class="btn" id="pauseBtn">⏸ Pausar</button>
                <button class="btn btn-exit" id="menuBtn">🚪 Salir</button>
            </div>
            
            <div class="game-over-screen" id="gameOverScreen">
                <h2>¡Juego Terminado!</h2>
                <div class="final-score">Puntuación: <span id="finalScore">0</span></div>
                <button class="btn" id="exitGameBtn" style="font-size: 1.1rem; padding: 1rem 2.5rem; background: linear-gradient(135deg, var(--danger), #DC2626);">🚪 Salir al Menú</button>
            </div>
        </div>
    </div>

    <div class="game-modal" id="instructionsModal">
        <div class="game-container" style="display: flex; align-items: center; justify-content: center; background: rgba(0,0,0,0.95);">
            <div class="instructions">
                <h4>Controles del Juego</h4>
                <p><strong>Teclado PC:</strong></p>
                <p>• <strong>← →</strong> - Mover el auto izquierda/derecha</p>
                <p>• <strong>↑</strong> - Saltar sobre obstáculos</p>
                <p>• <strong>ESPACIO</strong> - Activar/Desactivar BrightWay (iluminación especial)</p>
                
                <h4 style="margin-top: 1.5rem;">Móvil/Táctil:</h4>
                <p>• Tocar izquierda/derecha de la pantalla para girar</p>
                <p>• Doble toque en la pantalla para saltar</p>
                <p>• Botón 💡 BrightWay para activar/desactivar iluminación</p>
                <p>• El auto acelera automáticamente</p>
                
                <h4 style="margin-top: 1.5rem;">Poderes y Objetos:</h4>
                <p>• <strong>🛡️ Escudo (Recolectable):</strong> Aparece en la carretera. Recógelo para obtener protección de 10 segundos. El campo parpadea cuando está por acabarse.</p>
                <p>• <strong>💡 BrightWay:</strong> Ilumina el camino y otorga puntos extra. Se puede activar/desactivar en cualquier momento.</p>
                <p>• <strong>⭐ Combo:</strong> Recolecta escudos consecutivamente para multiplicar tus puntos.</p>
                <p>• <strong>🦘 Salto:</strong> Salta sobre los obstáculos para obtener 500 puntos extra.</p>
                
                <h4 style="margin-top: 1.5rem;">Características Nuevas:</h4>
                <p>• <strong>Mapa Desierto:</strong> Nuevo escenario con dunas y tormentas de arena</p>
                <p>• <strong>Sistema de Combos:</strong> Recolecta objetos seguidos para bonificaciones</p>
                <p>• <strong>Dificultad Progresiva:</strong> Más obstáculos a mayor velocidad</p>
                
                <h4 style="margin-top: 1.5rem;">Objetivo:</h4>
                <p>• Esquiva los obstáculos en la carretera</p>
                <p>• Recolecta escudos para protección temporal</p>
                <p>• Usa los poderes estratégicamente</p>
                <p>• La velocidad aumenta progresivamente sin límite</p>
                <p>• ¡Consigue el máximo puntaje!</p>
                
                <div style="text-align: center; margin-top: 2rem;">
                    <button class="btn-play" onclick="document.getElementById('instructionsModal').classList.remove('active')">Entendido</button>
                </div>
            </div>
        </div>
    </div>

    <script src="https://cdnjs.cloudflare.com/ajax/libs/three.js/r128/three.min.js"></script>
    <script>
        const audioCtx = new (window.AudioContext || window.webkitAudioContext)();
        let musicOn = true;
        let soundOn = true;
        let bgMusic = null;
        let musicVol = 0.3;
        let soundVol = 0.5;
        let panelOpen = false;
        let musicType = 'megalo';

        const marioNotes = [
            {f:659.25,d:0.12},{f:659.25,d:0.24},{f:659.25,d:0.24},{f:523.25,d:0.12},
            {f:659.25,d:0.24},{f:783.99,d:0.48},{f:392,d:0.48},{f:523.25,d:0.36},
            {f:392,d:0.36},{f:329.63,d:0.36},{f:440,d:0.36},{f:493.88,d:0.32},
            {f:466.16,d:0.28},{f:440,d:0.32},{f:392,d:0.24},{f:659.25,d:0.24},
            {f:783.99,d:0.24},{f:880,d:0.28},{f:698.46,d:0.24},{f:783.99,d:0.24},
            {f:659.25,d:0.32},{f:523.25,d:0.24},{f:587.33,d:0.24},{f:493.88,d:0.32},
            {f:523.25,d:0.36},{f:392,d:0.36},{f:329.63,d:0.36},{f:440,d:0.36},
            {f:493.88,d:0.32},{f:466.16,d:0.28},{f:440,d:0.32},{f:392,d:0.24},
            {f:659.25,d:0.24},{f:783.99,d:0.24},{f:880,d:0.28},{f:698.46,d:0.24},
            {f:783.99,d:0.24},{f:659.25,d:0.32},{f:523.25,d:0.24},{f:587.33,d:0.24},
            {f:493.88,d:0.32},{f:783.99,d:0.24},{f:740,d:0.24},{f:698.46,d:0.24},
            {f:622.25,d:0.32},{f:659.25,d:0.24},{f:415.3,d:0.18},{f:440,d:0.18},
            {f:523.25,d:0.32},{f:440,d:0.24},{f:523.25,d:0.24},{f:587.33,d:0.48},
            {f:783.99,d:0.24},{f:740,d:0.24},{f:698.46,d:0.24},{f:622.25,d:0.32},
            {f:659.25,d:0.24},{f:1046.5,d:0.18},{f:1046.5,d:0.18},{f:1046.5,d:0.32},
            {f:783.99,d:0.48},{f:783.99,d:0.24},{f:740,d:0.24},{f:698.46,d:0.24},
            {f:622.25,d:0.32},{f:659.25,d:0.24},{f:415.3,d:0.18},{f:440,d:0.18},
            {f:523.25,d:0.32},{f:440,d:0.24},{f:523.25,d:0.24},{f:587.33,d:0.48}
        ];

        const megaloNotes = [
            {f:293.66,d:0.15},{f:293.66,d:0.15},{f:587.33,d:0.3},{f:440,d:0.3},
            {f:415.3,d:0.25},{f:392,d:0.25},{f:349.23,d:0.25},{f:293.66,d:0.15},
            {f:349.23,d:0.15},{f:392,d:0.15},{f:261.63,d:0.15},{f:261.63,d:0.15},
            {f:587.33,d:0.3},{f:440,d:0.3},{f:415.3,d:0.25},{f:392,d:0.25},
            {f:349.23,d:0.25},{f:293.66,d:0.15},{f:349.23,d:0.15},{f:392,d:0.15},
            {f:246.94,d:0.15},{f:246.94,d:0.15},{f:587.33,d:0.3},{f:440,d:0.3},
            {f:415.3,d:0.25},{f:392,d:0.25},{f:349.23,d:0.25},{f:293.66,d:0.15},
            {f:349.23,d:0.15},{f:392,d:0.15},{f:220,d:0.15},{f:220,d:0.15},
            {f:587.33,d:0.3},{f:440,d:0.3},{f:415.3,d:0.25},{f:392,d:0.25},
            {f:349.23,d:0.25},{f:293.66,d:0.15},{f:349.23,d:0.15},{f:392,d:0.15},
            {f:207.65,d:0.15},{f:207.65,d:0.15},{f:587.33,d:0.3},{f:440,d:0.3},
            {f:415.3,d:0.25},{f:392,d:0.25},{f:349.23,d:0.25},{f:293.66,d:0.15},
            {f:349.23,d:0.15},{f:392,d:0.15},{f:293.66,d:0.15},{f:293.66,d:0.15},
            {f:587.33,d:0.3},{f:440,d:0.3},{f:415.3,d:0.25},{f:392,d:0.25},
            {f:349.23,d:0.25},{f:293.66,d:0.15},{f:349.23,d:0.15},{f:392,d:0.15}
        ];

        const starWarsNotes = [
            {f:392,d:0.3},{f:392,d:0.3},{f:392,d:0.3},{f:523.25,d:0.5},
            {f:783.99,d:0.5},{f:698.46,d:0.25},{f:659.25,d:0.25},{f:587.33,d:0.25},
            {f:1046.5,d:0.5},{f:783.99,d:0.4},{f:698.46,d:0.25},{f:659.25,d:0.25},
            {f:587.33,d:0.25},{f:1046.5,d:0.5},{f:783.99,d:0.4},{f:698.46,d:0.25},
            {f:659.25,d:0.25},{f:698.46,d:0.25},{f:587.33,d:0.6},
            {f:392,d:0.3},{f:392,d:0.3},{f:392,d:0.3},{f:523.25,d:0.5},
            {f:783.99,d:0.5},{f:698.46,d:0.25},{f:659.25,d:0.25},{f:587.33,d:0.25},
            {f:1046.5,d:0.5},{f:783.99,d:0.4},{f:698.46,d:0.25},{f:659.25,d:0.25},
            {f:587.33,d:0.25},{f:1046.5,d:0.5},{f:783.99,d:0.4},{f:698.46,d:0.25},
            {f:659.25,d:0.25},{f:698.46,d:0.25},{f:587.33,d:0.6},
            {f:783.99,d:0.3},{f:783.99,d:0.3},{f:783.99,d:0.3},{f:880,d:0.5},
            {f:1046.5,d:0.5},{f:987.77,d:0.25},{f:880,d:0.25},{f:783.99,d:0.25},
            {f:1174.7,d:0.5},{f:1046.5,d:0.4},{f:987.77,d:0.25},{f:880,d:0.25},
            {f:783.99,d:0.25},{f:1174.7,d:0.5},{f:1046.5,d:0.4}
        ];

        const tetrisNotes = [
            {f:659.25,d:0.4},{f:493.88,d:0.2},{f:523.25,d:0.2},{f:587.33,d:0.4},
            {f:523.25,d:0.2},{f:493.88,d:0.2},{f:440,d:0.4},{f:440,d:0.2},
            {f:523.25,d:0.2},{f:659.25,d:0.4},{f:587.33,d:0.2},{f:523.25,d:0.2},
            {f:493.88,d:0.6},{f:523.25,d:0.2},{f:587.33,d:0.4},{f:659.25,d:0.4},
            {f:523.25,d:0.4},{f:440,d:0.4},{f:440,d:0.4},{f:0,d:0.2},
            {f:587.33,d:0.4},{f:698.46,d:0.2},{f:880,d:0.4},{f:783.99,d:0.2},
            {f:698.46,d:0.2},{f:659.25,d:0.6},{f:523.25,d:0.2},{f:659.25,d:0.4},
            {f:587.33,d:0.2},{f:523.25,d:0.2},{f:493.88,d:0.4},{f:493.88,d:0.2},
            {f:523.25,d:0.2},{f:587.33,d:0.4},{f:659.25,d:0.4},{f:523.25,d:0.4},
            {f:440,d:0.4},{f:440,d:0.4},{f:0,d:0.4},{f:659.25,d:0.4},
            {f:493.88,d:0.2},{f:523.25,d:0.2},{f:587.33,d:0.4},{f:523.25,d:0.2},
            {f:493.88,d:0.2},{f:440,d:0.4},{f:440,d:0.2},{f:523.25,d:0.2},
            {f:659.25,d:0.4},{f:587.33,d:0.2},{f:523.25,d:0.2},{f:493.88,d:0.6},
            {f:523.25,d:0.2},{f:587.33,d:0.4},{f:659.25,d:0.4},{f:523.25,d:0.4},
            {f:440,d:0.4},{f:440,d:0.6}
        ];

        const piratesNotes = [
            {f:220,d:0.2},{f:261.63,d:0.2},{f:293.66,d:0.15},{f:293.66,d:0.15},
            {f:293.66,d:0.2},{f:329.63,d:0.2},{f:349.23,d:0.15},{f:349.23,d:0.15},
            {f:349.23,d:0.2},{f:392,d:0.2},{f:220,d:0.2},{f:261.63,d:0.2},
            {f:293.66,d:0.15},{f:293.66,d:0.15},{f:293.66,d:0.2},{f:329.63,d:0.2},
            {f:293.66,d:0.4},{f:261.63,d:0.3},{f:220,d:0.2},{f:261.63,d:0.2},
            {f:293.66,d:0.15},{f:293.66,d:0.15},{f:293.66,d:0.2},{f:329.63,d:0.2},
            {f:349.23,d:0.15},{f:349.23,d:0.15},{f:349.23,d:0.2},{f:392,d:0.2},
            {f:220,d:0.2},{f:261.63,d:0.2},{f:293.66,d:0.15},{f:293.66,d:0.15},
            {f:329.63,d:0.4},{f:293.66,d:0.4},{f:220,d:0.2},{f:261.63,d:0.2},
            {f:293.66,d:0.15},{f:293.66,d:0.15},{f:293.66,d:0.2},{f:329.63,d:0.2},
            {f:349.23,d:0.15},{f:349.23,d:0.15},{f:349.23,d:0.2},{f:392,d:0.2},
            {f:220,d:0.2},{f:261.63,d:0.2},{f:293.66,d:0.15},{f:293.66,d:0.15},
            {f:293.66,d:0.2},{f:329.63,d:0.2},{f:293.66,d:0.4},{f:261.63,d:0.3},
            {f:392,d:0.3},{f:440,d:0.3},{f:466.16,d:0.3},{f:440,d:0.3},
            {f:392,d:0.3},{f:349.23,d:0.3},{f:329.63,d:0.6}
        ];

        const pokemonNotes = [
            {f:440,d:0.3},{f:440,d:0.2},{f:440,d:0.3},{f:440,d:0.3},{f:392,d:0.2},
            {f:329.63,d:0.2},{f:261.63,d:0.35},{f:261.63,d:0.3},{f:440,d:0.2},
            {f:440,d:0.3},{f:392,d:0.2},{f:349.23,d:0.2},{f:392,d:0.35},
            {f:349.23,d:0.3},{f:466.16,d:0.3},{f:466.16,d:0.3},{f:466.16,d:0.3},
            {f:440,d:0.2},{f:392,d:0.2},{f:349.23,d:0.35},{f:349.23,d:0.3},
            {f:440,d:0.2},{f:440,d:0.3},{f:392,d:0.2},{f:349.23,d:0.2},{f:440,d:0.35},
            {f:440,d:0.2},{f:523.25,d:0.2},{f:587.33,d:0.4},{f:440,d:0.2},
            {f:440,d:0.3},{f:523.25,d:0.3},{f:587.33,d:0.3},{f:587.33,d:0.2},
            {f:523.25,d:0.35},{f:523.25,d:0.3},{f:523.25,d:0.3},{f:587.33,d:0.3},
            {f:587.33,d:0.2},{f:523.25,d:0.35},{f:523.25,d:0.3},{f:587.33,d:0.3},
            {f:659.25,d:0.3},{f:698.46,d:0.3},{f:659.25,d:0.2},{f:587.33,d:0.2},
            {f:523.25,d:0.4},{f:440,d:0.2},{f:523.25,d:0.2},{f:587.33,d:0.4},
            {f:440,d:0.2},{f:440,d:0.3},{f:523.25,d:0.3},{f:587.33,d:0.3},
            {f:587.33,d:0.2},{f:523.25,d:0.35}
        ];

        const musicLibrary = {mario: marioNotes, megalo: megaloNotes, starWars: starWarsNotes, tetris: tetrisNotes, pirates: piratesNotes, pokemon: pokemonNotes};

        function playClick() {
            if (!soundOn) return;
            const o = audioCtx.createOscillator();
            const g = audioCtx.createGain();
            o.connect(g);
            g.connect(audioCtx.destination);
            o.frequency.value = 900;
            o.type = 'sine';
            g.gain.setValueAtTime(0.15 * soundVol, audioCtx.currentTime);
            g.gain.exponentialRampToValueAtTime(0.01, audioCtx.currentTime + 0.08);
            o.start();
            o.stop(audioCtx.currentTime + 0.08);
        }

        function playHitSound() {
            if (!soundOn) return;
            const o = audioCtx.createOscillator();
            const g = audioCtx.createGain();
            o.connect(g);
            g.connect(audioCtx.destination);
            o.frequency.value = 150;
            o.type = 'sawtooth';
            g.gain.setValueAtTime(0.25 * soundVol, audioCtx.currentTime);
            g.gain.exponentialRampToValueAtTime(0.01, audioCtx.currentTime + 0.4);
            o.start();
            o.stop(audioCtx.currentTime + 0.4);
        }

        function playScoreSound() {
            if (!soundOn) return;
            [800, 1000].forEach((freq, i) => {
                const o = audioCtx.createOscillator();
                const g = audioCtx.createGain();
                o.connect(g);
                g.connect(audioCtx.destination);
                o.frequency.value = freq;
                o.type = 'sine';
                const startTime = audioCtx.currentTime + (i * 0.08);
                g.gain.setValueAtTime(0.12 * soundVol, startTime);
                g.gain.exponentialRampToValueAtTime(0.01, startTime + 0.15);
                o.start(startTime);
                o.stop(startTime + 0.15);
            });
        }

        function playBrightWaySound() {
            if (!soundOn) return;
            [500, 650, 800, 1000].forEach((freq, i) => {
                const o = audioCtx.createOscillator();
                const g = audioCtx.createGain();
                o.connect(g);
                g.connect(audioCtx.destination);
                o.frequency.value = freq;
                o.type = 'sine';
                const startTime = audioCtx.currentTime + (i * 0.1);
                g.gain.setValueAtTime(0.18 * soundVol, startTime);
                g.gain.exponentialRampToValueAtTime(0.01, startTime + 0.35);
                o.start(startTime);
                o.stop(startTime + 0.35);
            });
        }

        function playShieldSound() {
            if (!soundOn) return;
            [400, 600, 800].forEach((freq, i) => {
                const o = audioCtx.createOscillator();
                const g = audioCtx.createGain();
                o.connect(g);
                g.connect(audioCtx.destination);
                o.frequency.value = freq;
                o.type = 'sine';
                const startTime = audioCtx.currentTime + (i * 0.08);
                g.gain.setValueAtTime(0.2 * soundVol, startTime);
                g.gain.exponentialRampToValueAtTime(0.01, startTime + 0.3);
                o.start(startTime);
                o.stop(startTime + 0.3);
            });
        }

        function playComboSound() {
            if (!soundOn) return;
            [600, 800, 1000, 1200].forEach((freq, i) => {
                const o = audioCtx.createOscillator();
                const g = audioCtx.createGain();
                o.connect(g);
                g.connect(audioCtx.destination);
                o.frequency.value = freq;
                o.type = 'triangle';
                const startTime = audioCtx.currentTime + (i * 0.06);
                g.gain.setValueAtTime(0.15 * soundVol, startTime);
                g.gain.exponentialRampToValueAtTime(0.01, startTime + 0.2);
                o.start(startTime);
                o.stop(startTime + 0.2);
            });
        }

        function updateMusicVol() {
            if (bgMusic && bgMusic.g) {
                bgMusic.g.gain.value = Math.min(musicVol * 1.5, 0.7);
            }
        }

        function stopMusic() {
            if (bgMusic) {
                if (bgMusic.g) bgMusic.g.disconnect();
                if (bgMusic.stop) bgMusic.stop();
                bgMusic = null;
            }
        }

        function startMusic() {
            if (!musicOn) return;
            stopMusic();
            bgMusic = { g: audioCtx.createGain(), stop: null };
            bgMusic.g.connect(audioCtx.destination);
            bgMusic.g.gain.value = Math.min(musicVol * 1.5, 0.7);
            const notes = musicLibrary[musicType];
            let shouldStop = false;
            bgMusic.stop = () => { shouldStop = true; };
            async function playMelody() {
                if (shouldStop || !musicOn) return;
                for (let note of notes) {
                    if (shouldStop || !musicOn) break;
                    await new Promise(resolve => {
                        setTimeout(() => {
                            if (shouldStop || !musicOn || !bgMusic) {
                                resolve();
                                return;
                            }
                            const o = audioCtx.createOscillator();
                            const g = audioCtx.createGain();
                            o.connect(g);
                            g.connect(bgMusic.g);
                            o.frequency.value = note.f;
                            o.type = 'triangle';
                            const now = audioCtx.currentTime;
                            g.gain.setValueAtTime(0, now);
                            g.gain.linearRampToValueAtTime(0.4, now + 0.05);
                            g.gain.linearRampToValueAtTime(0, now + note.d - 0.05);
                            o.start(now);
                            o.stop(now + note.d);
                            setTimeout(resolve, note.d * 1000);
                        }, 0);
                    });
                }
                await new Promise(r => setTimeout(r, 400));
                if (musicOn && bgMusic && !shouldStop) playMelody();
            }
            playMelody();
        }

        let scene, camera, renderer, car, obstacles = [], brightwayPosts = [], particles = [], shieldMesh = null, shieldPickups = [];
        let gameState = {
            running: false, paused: false, score: 0, highScore: 0, speed: 60, maxSpeed: 60, 
            acceleration: 0.5, carPosition: 0, carColor: null, mapType: null,
            brightwayActive: false, shieldActive: false, shieldTimeLeft: 0,
            frameCount: 0, isMobile: window.innerWidth <= 768, obstacleFrequency: 120,
            combo: 0, lastPickupTime: 0,
            isJumping: false, jumpHeight: 0, jumpVelocity: 0
        };
        const keys = {left: false, right: false, up: false, down: false, space: false};
        const carColorMap = {blue: 0x3B82F6, red: 0xEF4444, yellow: 0xFBBF24, green: 0x059669};
        let lastTapTime = 0, lastTapX = 0;

        document.addEventListener('DOMContentLoaded', function() {
            document.addEventListener('click', function(e) {
                if (e.target.closest('button, .feature-card, .option-card, .music-option, a')) {
                    playClick();
                }
            });

            document.getElementById('volumeBtn').onclick = (e) => {
                e.stopPropagation();
                panelOpen = !panelOpen;
                const panel = document.getElementById('volumePanel');
                if (panelOpen) {
                    panel.classList.add('show');
                    // Iniciar música automáticamente con Megalovania
                    if (audioCtx.state === 'suspended') {
                        audioCtx.resume().then(() => {
                            if (!musicOn) {
                                musicOn = true;
                                document.getElementById('musicMuteBtn').classList.remove('muted');
                                document.getElementById('musicMuteBtn').innerHTML = '🎵';
                            }
                            musicType = 'megalo';
                            document.querySelectorAll('.music-option').forEach(opt => opt.classList.remove('active'));
                            document.getElementById('megaloMusic').classList.add('active');
                            startMusic();
                        });
                    } else {
                        if (!musicOn) {
                            musicOn = true;
                            document.getElementById('musicMuteBtn').classList.remove('muted');
                            document.getElementById('musicMuteBtn').innerHTML = '🎵';
                        }
                        if (!bgMusic) {
                            musicType = 'megalo';
                            document.querySelectorAll('.music-option').forEach(opt => opt.classList.remove('active'));
                            document.getElementById('megaloMusic').classList.add('active');
                            startMusic();
                        }
                    }
                } else {
                    panel.classList.remove('show');
                }
            };

            document.onclick = (e) => {
                const panel = document.getElementById('volumePanel');
                const btn = document.getElementById('volumeBtn');
                if (panelOpen && !panel.contains(e.target) && e.target !== btn) {
                    panelOpen = false;
                    panel.classList.remove('show');
                }
            };

            document.getElementById('musicVolume').oninput = (e) => {
                musicVol = e.target.value / 100;
                document.getElementById('musicVolumeValue').textContent = e.target.value + '%';
                updateMusicVol();
            };

            document.getElementById('soundVolume').oninput = (e) => {
                soundVol = e.target.value / 100;
                document.getElementById('soundVolumeValue').textContent = e.target.value + '%';
            };

            document.getElementById('musicMuteBtn').onclick = function() {
                musicOn = !musicOn;
                this.classList.toggle('muted');
                this.innerHTML = musicOn ? '🎵' : '🔇';
                musicOn ? startMusic() : stopMusic();
            };

            document.getElementById('soundMuteBtn').onclick = function() {
                soundOn = !soundOn;
                this.classList.toggle('muted');
                this.innerHTML = soundOn ? '🔊' : '🔇';
            };

            function selectMusic(type) {
                musicType = type;
                document.querySelectorAll('.music-option').forEach(opt => opt.classList.remove('active'));
                // Siempre iniciar la música cuando se selecciona una canción
                if (!musicOn) {
                    musicOn = true;
                    document.getElementById('musicMuteBtn').classList.remove('muted');
                    document.getElementById('musicMuteBtn').innerHTML = '🎵';
                }
                startMusic();
            }

            document.getElementById('marioMusic').onclick = () => {selectMusic('mario');document.getElementById('marioMusic').classList.add('active');};
            document.getElementById('megaloMusic').onclick = () => {selectMusic('megalo');document.getElementById('megaloMusic').classList.add('active');};
            document.getElementById('starWarsMusic').onclick = () => {selectMusic('starWars');document.getElementById('starWarsMusic').classList.add('active');};
            document.getElementById('tetrisMusic').onclick = () => {selectMusic('tetris');document.getElementById('tetrisMusic').classList.add('active');};
            document.getElementById('piratesMusic').onclick = () => {selectMusic('pirates');document.getElementById('piratesMusic').classList.add('active');};
            document.getElementById('pokemonMusic').onclick = () => {selectMusic('pokemon');document.getElementById('pokemonMusic').classList.add('active');};

            document.getElementById('playGameBtn').addEventListener('click', function() {
                document.getElementById('gameModal').classList.add('active');
                document.getElementById('mapSelectorScreen').classList.remove('hidden');
            });
            
            document.getElementById('instructionsBtn').addEventListener('click', function() {
                document.getElementById('instructionsModal').classList.add('active');
            });

            document.querySelectorAll('#mapSelectorScreen .option-card').forEach(card => {
                card.addEventListener('click', function() {
                    document.querySelectorAll('#mapSelectorScreen .option-card').forEach(c => c.classList.remove('selected'));
                    this.classList.add('selected');
                    gameState.mapType = this.dataset.map;
                    setTimeout(() => {
                        document.getElementById('mapSelectorScreen').classList.add('hidden');
                        document.getElementById('carSelectorScreen').classList.remove('hidden');
                    }, 300);
                });
            });

            document.querySelectorAll('#carSelectorScreen .option-card').forEach(card => {
                card.addEventListener('click', function() {
                    document.querySelectorAll('#carSelectorScreen .option-card').forEach(c => c.classList.remove('selected'));
                    this.classList.add('selected');
                    gameState.carColor = this.dataset.color;
                    setTimeout(() => {
                        document.getElementById('carSelectorScreen').classList.add('hidden');
                        initGame();
                    }, 300);
                });
            });

            setTimeout(() => {
                if (audioCtx.state === 'suspended') {
                    audioCtx.resume().then(() => startMusic());
                } else {
                    startMusic();
                }
            }, 500);
        });

        function initGame() {
            scene = new THREE.Scene();
            
            if (gameState.mapType === 'city') {
                scene.fog = new THREE.Fog(0x050510, 80, 280);
                scene.background = new THREE.Color(0x020208);
            } else if (gameState.mapType === 'snow') {
                scene.fog = new THREE.Fog(0x4a5560, 100, 300);
                scene.background = new THREE.Color(0x2a3540);
            } else if (gameState.mapType === 'volcano') {
                scene.fog = new THREE.Fog(0x2a0505, 60, 220);
                scene.background = new THREE.Color(0x0a0202);
            } else if (gameState.mapType === 'desert') {
                scene.fog = new THREE.Fog(0x6a5540, 100, 300);
                scene.background = new THREE.Color(0x4a3520);
            }
            
            camera = new THREE.PerspectiveCamera(60, window.innerWidth / window.innerHeight, 0.1, 1000);
            camera.position.set(0, 6, 14);
            camera.lookAt(0, 1, -10);
            renderer = new THREE.WebGLRenderer({canvas: document.getElementById('gameCanvas'), antialias: true, powerPreference: "high-performance"});
            renderer.setSize(window.innerWidth, window.innerHeight);
            renderer.setPixelRatio(Math.min(window.devicePixelRatio, 2));
            renderer.shadowMap.enabled = true;
            renderer.shadowMap.type = THREE.PCFSoftShadowMap;
            
            if (gameState.mapType === 'city') {
                const ambientLight = new THREE.AmbientLight(0x2a2a40, 0.2);
                scene.add(ambientLight);
                const directionalLight = new THREE.DirectionalLight(0x5a5a8a, 0.25);
                directionalLight.position.set(20, 40, 20);
                directionalLight.castShadow = true;
                configureShadow(directionalLight);
                scene.add(directionalLight);
                const hemisphereLight = new THREE.HemisphereLight(0x0a0a1a, 0x050510, 0.2);
                scene.add(hemisphereLight);
            } else if (gameState.mapType === 'snow') {
                const ambientLight = new THREE.AmbientLight(0x1a1a30, 0.12);
                scene.add(ambientLight);
                const directionalLight = new THREE.DirectionalLight(0x3a3a50, 0.18);
                directionalLight.position.set(15, 50, 25);
                directionalLight.castShadow = true;
                configureShadow(directionalLight);
                scene.add(directionalLight);
                const hemisphereLight = new THREE.HemisphereLight(0x1a1a2a, 0x0a0a15, 0.15);
                scene.add(hemisphereLight);
            } else if (gameState.mapType === 'volcano') {
                const ambientLight = new THREE.AmbientLight(0x8a2200, 0.15);
                scene.add(ambientLight);
                const directionalLight = new THREE.DirectionalLight(0xaa3300, 0.3);
                directionalLight.position.set(20, 40, 20);
                directionalLight.castShadow = true;
                configureShadow(directionalLight);
                scene.add(directionalLight);
                const hemisphereLight = new THREE.HemisphereLight(0x4a1010, 0x2a0505, 0.2);
                scene.add(hemisphereLight);
            } else if (gameState.mapType === 'desert') {
                const ambientLight = new THREE.AmbientLight(0x2a1a0a, 0.12);
                scene.add(ambientLight);
                const directionalLight = new THREE.DirectionalLight(0x4a3a2a, 0.18);
                directionalLight.position.set(25, 60, 15);
                directionalLight.castShadow = true;
                configureShadow(directionalLight);
                scene.add(directionalLight);
                const hemisphereLight = new THREE.HemisphereLight(0x1a1a0a, 0x0a0a05, 0.15);
                scene.add(hemisphereLight);
            }
            
            createEnvironment();
            createRoad();
            createCar();
            createBrightwayPosts();
            createParticles();
            
            // Configurar controles solo la primera vez
            if (!controlsSetup) {
                setupControls();
            }
            
            // Inicializar valores del juego
            gameState.running = true;
            gameState.speed = 60; // VELOCIDAD FIJA
            gameState.maxSpeed = 60;
            gameState.highScore = parseInt(localStorage.getItem('brightwayHighScore3D') || '0');
            
            // Mostrar controles del juego
            document.getElementById('gameControls').classList.add('show');
            
            // Actualizar UI inicial
            document.getElementById('highScore').textContent = gameState.highScore;
            document.getElementById('speedValue').textContent = '60';
            document.getElementById('score').textContent = '0';
            
            animate();
        }

        function configureShadow(light) {
            light.shadow.camera.left = -60;
            light.shadow.camera.right = 60;
            light.shadow.camera.top = 60;
            light.shadow.camera.bottom = -60;
            light.shadow.mapSize.width = 2048;
            light.shadow.mapSize.height = 2048;
            light.shadow.bias = -0.0001;
        }

        function createEnvironment() {
            let groundColor, skyElements;
            
            if (gameState.mapType === 'city') {
                groundColor = 0x0a1a0a;
                skyElements = 'stars';
                createCityEnvironment();
            } else if (gameState.mapType === 'snow') {
                groundColor = 0xFFFFFF;
                skyElements = 'snow';
                createSnowEnvironment();
            } else if (gameState.mapType === 'volcano') {
                groundColor = 0x1a0a0a;
                skyElements = 'lava';
                createVolcanoEnvironment();
            } else if (gameState.mapType === 'desert') {
                groundColor = 0xF4A460;
                skyElements = 'sun';
                createDesertEnvironment();
            }
            
            // Suelo base
            const groundGeo = new THREE.PlaneGeometry(400, 600);
            const groundMat = new THREE.MeshStandardMaterial({ 
                color: groundColor, 
                roughness: 0.95,
                metalness: 0
            });
            const ground = new THREE.Mesh(groundGeo, groundMat);
            ground.rotation.x = -Math.PI / 2;
            ground.position.z = -200;
            ground.receiveShadow = true;
            scene.add(ground);
            
            // Elementos del cielo comunes
            if (skyElements === 'stars') {
                createStars();
            } else if (skyElements === 'sun') {
                createSun();
            }
        }

        function createCityEnvironment() {
            // Crear edificios simplificados - REDUCIDOS DE 30 A 12
            const buildingTypes = [
                { width: 8, depth: 8, height: 40, color: 0x1a1a2a },
                { width: 10, depth: 10, height: 50, color: 0x0f0f1f },
                { width: 7, depth: 7, height: 35, color: 0x12121e }
            ];
            
            for (let i = 0; i < 12; i++) {
                const buildingType = buildingTypes[Math.floor(Math.random() * buildingTypes.length)];
                
                // Solo estructura principal, sin ventanas detalladas
                const buildingMesh = new THREE.Mesh(
                    new THREE.BoxGeometry(buildingType.width, buildingType.height, buildingType.depth),
                    new THREE.MeshStandardMaterial({
                        color: buildingType.color,
                        roughness: 0.8,
                        metalness: 0.2
                    })
                );
                buildingMesh.position.y = buildingType.height / 2;
                buildingMesh.castShadow = false; // Sin sombras para ahorrar recursos
                buildingMesh.receiveShadow = false;
                
                // EDIFICIOS SOLO EN LOS COSTADOS LEJANOS
                const side = Math.random() > 0.5 ? 1 : -1;
                buildingMesh.position.set(
                    side * (50 + Math.random() * 90),
                    0,
                    -Math.random() * 500
                );
                scene.add(buildingMesh);
            }
            
            // ÁRBOLES REDUCIDOS DE 60 A 25
            for (let i = 0; i < 25; i++) {
                const treeGroup = new THREE.Group();
                const trunkHeight = 5 + Math.random() * 2;
                
                // Tronco simplificado
                const trunk = new THREE.Mesh(
                    new THREE.CylinderGeometry(0.4, 0.5, trunkHeight, 6),
                    new THREE.MeshStandardMaterial({
                        color: 0x2a1a0a,
                        roughness: 0.9
                    })
                );
                trunk.position.y = trunkHeight / 2;
                trunk.castShadow = false;
                treeGroup.add(trunk);
                
                // Follaje simplificado
                const leavesSize = 2.5 + Math.random() * 1;
                const leaves = new THREE.Mesh(
                    new THREE.SphereGeometry(leavesSize, 6, 6),
                    new THREE.MeshStandardMaterial({
                        color: 0x1a3a1a,
                        roughness: 0.85
                    })
                );
                leaves.position.y = trunkHeight + leavesSize * 0.5;
                leaves.castShadow = false;
                treeGroup.add(leaves);
                
                const side = Math.random() > 0.5 ? 1 : -1;
                treeGroup.position.set(
                    side * (16 + Math.random() * 20),
                    0,
                    -Math.random() * 500
                );
                scene.add(treeGroup);
            }
            
            // Luces de calle REDUCIDAS DE 20 A 10
            for (let i = 0; i < 10; i++) {
                const streetLight = new THREE.Group();
                
                const pole = new THREE.Mesh(
                    new THREE.CylinderGeometry(0.2, 0.2, 6, 6),
                    new THREE.MeshStandardMaterial({ color: 0x2a2a2a })
                );
                pole.position.y = 3;
                streetLight.add(pole);
                
                const lamp = new THREE.Mesh(
                    new THREE.SphereGeometry(0.5, 6, 6),
                    new THREE.MeshBasicMaterial({
                        color: 0xFFDD88,
                        transparent: true,
                        opacity: 0.8
                    })
                );
                lamp.position.y = 6;
                streetLight.add(lamp);
                
                const side = i % 2 === 0 ? -1 : 1;
                streetLight.position.set(
                    side * 15,
                    0,
                    -i * 50
                );
                scene.add(streetLight);
            }
        }

        function createSnowEnvironment() {
            // Montañas nevadas de fondo - REDUCIDAS DE 15 A 8
            for (let i = 0; i < 8; i++) {
                const mountainHeight = 50 + Math.random() * 60;
                const mountainWidth = 30 + Math.random() * 40;
                
                const mountain = new THREE.Mesh(
                    new THREE.ConeGeometry(mountainWidth, mountainHeight, 6),
                    new THREE.MeshStandardMaterial({
                        color: 0xE8E8E8,
                        roughness: 0.9
                    })
                );
                const side = Math.random() > 0.5 ? 1 : -1;
                mountain.position.set(
                    side * (80 + Math.random() * 150),
                    mountainHeight / 2,
                    -200 - Math.random() * 200
                );
                mountain.castShadow = false;
                scene.add(mountain);
                
                // Nieve en el pico
                const snowCap = new THREE.Mesh(
                    new THREE.ConeGeometry(mountainWidth * 0.6, mountainHeight * 0.4, 6),
                    new THREE.MeshStandardMaterial({
                        color: 0xFFFFFF,
                        roughness: 0.7
                    })
                );
                snowCap.position.set(
                    mountain.position.x,
                    mountainHeight * 0.8,
                    mountain.position.z
                );
                scene.add(snowCap);
            }
            
            // Pinos nevados - REDUCIDOS DE 100 A 35
            for (let i = 0; i < 35; i++) {
                const pineTree = new THREE.Group();
                
                // Tronco simplificado
                const trunk = new THREE.Mesh(
                    new THREE.CylinderGeometry(0.3, 0.4, 5, 6),
                    new THREE.MeshStandardMaterial({
                        color: 0x2a1a0a,
                        roughness: 0.95
                    })
                );
                trunk.position.y = 2.5;
                pineTree.add(trunk);
                
                // Solo 2 capas de follaje en lugar de 4
                const foliageLayers = [
                    { radius: 2.5, height: 4, yPos: 5 },
                    { radius: 1.5, height: 3, yPos: 8 }
                ];
                
                foliageLayers.forEach(layer => {
                    const foliage = new THREE.Mesh(
                        new THREE.ConeGeometry(layer.radius, layer.height, 6),
                        new THREE.MeshStandardMaterial({
                            color: 0x0d3d0d,
                            roughness: 0.9
                        })
                    );
                    foliage.position.y = layer.yPos;
                    pineTree.add(foliage);
                });
                
                const side = Math.random() > 0.5 ? 1 : -1;
                pineTree.position.set(
                    side * (20 + Math.random() * 50),
                    0,
                    -Math.random() * 500
                );
                scene.add(pineTree);
            }
            
            // Montículos de nieve - REDUCIDOS DE 30 A 15
            for (let i = 0; i < 15; i++) {
                const snowPile = new THREE.Mesh(
                    new THREE.SphereGeometry(2 + Math.random() * 3, 6, 6),
                    new THREE.MeshStandardMaterial({
                        color: 0xF0F0F0,
                        roughness: 0.8
                    })
                );
                snowPile.scale.y = 0.4;
                const side = Math.random() > 0.5 ? 1 : -1;
                snowPile.position.set(
                    side * (18 + Math.random() * 50),
                    0,
                    -Math.random() * 500
                );
                scene.add(snowPile);
            }
        }

        function createVolcanoEnvironment() {
            // Volcán grande - A UN COSTADO, NO EN MEDIO
            const volcanoHeight = 100;
            const volcanoRadius = 60;
            
            const volcano = new THREE.Mesh(
                new THREE.ConeGeometry(volcanoRadius, volcanoHeight, 8),
                new THREE.MeshStandardMaterial({
                    color: 0x1a0a0a,
                    roughness: 0.95,
                    metalness: 0
                })
            );
            // VOLCÁN EN EL COSTADO IZQUIERDO
            volcano.position.set(-120, volcanoHeight / 2, -400);
            volcano.castShadow = true;
            volcano.receiveShadow = true;
            scene.add(volcano);
            
            // Cráter con lava
            const crater = new THREE.Mesh(
                new THREE.CylinderGeometry(15, 8, 10, 8),
                new THREE.MeshStandardMaterial({
                    color: 0x2a0a0a,
                    roughness: 0.9
                })
            );
            crater.position.set(-120, volcanoHeight - 5, -400);
            scene.add(crater);
            
            // Lava en el cráter
            const lava = new THREE.Mesh(
                new THREE.CylinderGeometry(14, 7, 1, 16),
                new THREE.MeshBasicMaterial({
                    color: 0xFF4500,
                    transparent: true,
                    opacity: 0.9
                })
            );
            lava.position.set(-120, volcanoHeight - 10, -400);
            scene.add(lava);
            
            // Luz del cráter
            const craterLight = new THREE.PointLight(0xFF4500, 3, 150);
            craterLight.position.set(-120, volcanoHeight - 5, -400);
            scene.add(craterLight);
            
            // Ríos de lava bajando
            for (let i = 0; i < 5; i++) {
                const lavaRiver = new THREE.Mesh(
                    new THREE.BoxGeometry(3, 0.5, 80),
                    new THREE.MeshBasicMaterial({
                        color: 0xFF6347,
                        transparent: true,
                        opacity: 0.8
                    })
                );
                const angle = (i / 5) * Math.PI * 2;
                lavaRiver.position.set(
                    -120 + Math.cos(angle) * 25,
                    volcanoHeight / 3,
                    -400 + Math.sin(angle) * 25
                );
                lavaRiver.rotation.y = angle;
                lavaRiver.rotation.x = -Math.PI / 6;
                scene.add(lavaRiver);
            }
            
            // Rocas volcánicas - EN LOS COSTADOS (AUMENTADAS de 50 a 120)
            for (let i = 0; i < 120; i++) {
                const rock = new THREE.Mesh(
                    new THREE.DodecahedronGeometry(1 + Math.random() * 3, 0),
                    new THREE.MeshStandardMaterial({
                        color: Math.random() > 0.5 ? 0x2a1a1a : 0x1a0a0a,
                        roughness: 0.95
                    })
                );
                const side = Math.random() > 0.5 ? 1 : -1;
                rock.position.set(
                    side * (18 + Math.random() * 70),
                    0,
                    -Math.random() * 500
                );
                rock.rotation.set(
                    Math.random() * Math.PI,
                    Math.random() * Math.PI,
                    Math.random() * Math.PI
                );
                rock.castShadow = true;
                rock.receiveShadow = true;
                scene.add(rock);
            }
            
            // Columnas de humo/vapor - EN LOS COSTADOS
            for (let i = 0; i < 10; i++) {
                const smoke = new THREE.Mesh(
                    new THREE.CylinderGeometry(2, 0.5, 15, 8),
                    new THREE.MeshBasicMaterial({
                        color: 0x3a1a1a,
                        transparent: true,
                        opacity: 0.3
                    })
                );
                const side = Math.random() > 0.5 ? 1 : -1;
                smoke.position.set(
                    side * (20 + Math.random() * 50),
                    10,
                    -Math.random() * 400
                );
                scene.add(smoke);
            }
        }

        function createDesertEnvironment() {
            // Montañas de arena grandes de fondo - EN LOS COSTADOS
            for (let i = 0; i < 20; i++) {
                const sandMountain = new THREE.Group();
                
                const mountainBase = new THREE.Mesh(
                    new THREE.SphereGeometry(25 + Math.random() * 30, 12, 12),
                    new THREE.MeshStandardMaterial({
                        color: 0xD2B48C,
                        roughness: 0.95,
                        metalness: 0
                    })
                );
                mountainBase.scale.y = 0.6;
                mountainBase.scale.z = 1.5;
                mountainBase.position.y = -5;
                mountainBase.castShadow = true;
                mountainBase.receiveShadow = true;
                sandMountain.add(mountainBase);
                
                // Capas superiores
                const topLayer = new THREE.Mesh(
                    new THREE.SphereGeometry(15 + Math.random() * 15, 12, 12),
                    new THREE.MeshStandardMaterial({
                        color: 0xE5C07B,
                        roughness: 0.9
                    })
                );
                topLayer.scale.y = 0.5;
                topLayer.position.y = 10;
                topLayer.castShadow = true;
                sandMountain.add(topLayer);
                
                const side = Math.random() > 0.5 ? 1 : -1;
                sandMountain.position.set(
                    side * (80 + Math.random() * 120),
                    0,
                    -200 - Math.random() * 250
                );
                scene.add(sandMountain);
            }
            
            // Dunas medianas - EN LOS COSTADOS
            for (let i = 0; i < 35; i++) {
                const dune = new THREE.Mesh(
                    new THREE.SphereGeometry(4 + Math.random() * 6, 12, 12),
                    new THREE.MeshStandardMaterial({
                        color: 0xEDC967,
                        roughness: 0.95,
                        metalness: 0
                    })
                );
                dune.scale.y = 0.4;
                dune.scale.z = 1.3;
                const side = Math.random() > 0.5 ? 1 : -1;
                dune.position.set(
                    side * (18 + Math.random() * 40),
                    -1,
                    -Math.random() * 500
                );
                dune.rotation.y = Math.random() * Math.PI;
                dune.castShadow = true;
                dune.receiveShadow = true;
                scene.add(dune);
            }
            
            // Cactus variados - EN LOS COSTADOS
            for (let i = 0; i < 25; i++) {
                const cactusGroup = new THREE.Group();
                
                const mainHeight = 3 + Math.random() * 3;
                const mainBody = new THREE.Mesh(
                    new THREE.CylinderGeometry(0.35, 0.4, mainHeight, 8),
                    new THREE.MeshStandardMaterial({
                        color: 0x2E7D32,
                        roughness: 0.9,
                        metalness: 0
                    })
                );
                mainBody.position.y = mainHeight / 2;
                mainBody.castShadow = true;
                cactusGroup.add(mainBody);
                
                // Brazos laterales
                const numArms = Math.floor(Math.random() * 3) + 1;
                for (let j = 0; j < numArms; j++) {
                    const armHeight = 1 + Math.random() * 2;
                    const arm = new THREE.Mesh(
                        new THREE.CylinderGeometry(0.25, 0.25, armHeight, 8),
                        new THREE.MeshStandardMaterial({
                            color: 0x2E7D32,
                            roughness: 0.9
                        })
                    );
                    arm.rotation.z = Math.PI / 2;
                    const side = j % 2 === 0 ? 1 : -1;
                    arm.position.set(
                        side * 0.6,
                        mainHeight * 0.6 - j * 0.5,
                        0
                    );
                    cactusGroup.add(arm);
                    
                    // Parte vertical del brazo
                    const armUp = new THREE.Mesh(
                        new THREE.CylinderGeometry(0.25, 0.25, armHeight * 0.8, 8),
                        new THREE.MeshStandardMaterial({
                            color: 0x2E7D32,
                            roughness: 0.9
                        })
                    );
                    armUp.position.set(
                        side * (0.6 + armHeight / 2),
                        mainHeight * 0.6 - j * 0.5 + armHeight * 0.4,
                        0
                    );
                    cactusGroup.add(armUp);
                }
                
                // Flores en la punta
                if (Math.random() > 0.6) {
                    const flower = new THREE.Mesh(
                        new THREE.SphereGeometry(0.3, 6, 6),
                        new THREE.MeshBasicMaterial({
                            color: Math.random() > 0.5 ? 0xFF69B4 : 0xFFD700
                        })
                    );
                    flower.position.y = mainHeight;
                    cactusGroup.add(flower);
                }
                
                // CACTUS EN LOS COSTADOS
                const side = Math.random() > 0.5 ? 1 : -1;
                cactusGroup.position.set(
                    side * (18 + Math.random() * 45),
                    0,
                    -Math.random() * 500
                );
                scene.add(cactusGroup);
            }
            
            // Rocas del desierto - EN LOS COSTADOS
            for (let i = 0; i < 30; i++) {
                const rock = new THREE.Mesh(
                    new THREE.DodecahedronGeometry(0.8 + Math.random() * 1.5, 0),
                    new THREE.MeshStandardMaterial({
                        color: 0x8B7355,
                        roughness: 0.95
                    })
                );
                const side = Math.random() > 0.5 ? 1 : -1;
                rock.position.set(
                    side * (16 + Math.random() * 60),
                    0.3,
                    -Math.random() * 500
                );
                rock.rotation.set(
                    Math.random() * Math.PI,
                    Math.random() * Math.PI,
                    Math.random() * Math.PI
                );
                rock.castShadow = true;
                rock.receiveShadow = true;
                scene.add(rock);
            }
            
            // Plantas rodadoras (tumbleweeds) - EN LOS COSTADOS
            for (let i = 0; i < 10; i++) {
                const tumbleweed = new THREE.Mesh(
                    new THREE.IcosahedronGeometry(1, 0),
                    new THREE.MeshStandardMaterial({
                        color: 0x8B7355,
                        roughness: 0.9,
                        wireframe: true
                    })
                );
                const side = Math.random() > 0.5 ? 1 : -1;
                tumbleweed.position.set(
                    side * (20 + Math.random() * 40),
                    0.5,
                    -Math.random() * 400
                );
                scene.add(tumbleweed);
            }
        }

        function createStars() {
            const starMaterial = new THREE.MeshBasicMaterial({ 
                color: 0xFFFFFF,
                transparent: true,
                opacity: 0.9
            });
            // Estrellas REDUCIDAS DE 200 A 80
            for (let i = 0; i < 80; i++) {
                const starSize = 0.15 + Math.random() * 0.25;
                const star = new THREE.Mesh(
                    new THREE.SphereGeometry(starSize, 4, 4), 
                    starMaterial
                );
                star.position.set(
                    (Math.random() - 0.5) * 400, 
                    35 + Math.random() * 70, 
                    -Math.random() * 600
                );
                scene.add(star);
            }
            
            const moonGeometry = new THREE.SphereGeometry(8, 16, 16);
            const moonMaterial = new THREE.MeshBasicMaterial({ 
                color: 0xFFFAF0,
                transparent: true,
                opacity: 0.95
            });
            const moon = new THREE.Mesh(moonGeometry, moonMaterial);
            moon.position.set(80, 60, -250);
            scene.add(moon);
            
            const moonGlow = new THREE.Mesh(
                new THREE.SphereGeometry(10, 16, 16),
                new THREE.MeshBasicMaterial({
                    color: 0xFFFFDD,
                    transparent: true,
                    opacity: 0.15
                })
            );
            moonGlow.position.copy(moon.position);
            scene.add(moonGlow);
        }

        function createSun() {
            const sunGeometry = new THREE.SphereGeometry(12, 32, 32);
            const sunMaterial = new THREE.MeshBasicMaterial({
                color: 0xFFDD44,
                transparent: true,
                opacity: 0.9
            });
            const sun = new THREE.Mesh(sunGeometry, sunMaterial);
            sun.position.set(100, 80, -300);
            scene.add(sun);
            
            const sunGlow = new THREE.Mesh(
                new THREE.SphereGeometry(15, 32, 32),
                new THREE.MeshBasicMaterial({
                    color: 0xFFAA00,
                    transparent: true,
                    opacity: 0.3
                })
            );
            sunGlow.position.copy(sun.position);
            scene.add(sunGlow);
        }

        function createParticles() {
            if (gameState.mapType === 'snow') {
                // Copos de nieve REDUCIDOS DE 800 A 300
                for (let i = 0; i < 300; i++) {
                    const snowflake = new THREE.Mesh(
                        new THREE.SphereGeometry(0.08 + Math.random() * 0.12, 4, 4),
                        new THREE.MeshBasicMaterial({ 
                            color: 0xFFFFFF,
                            transparent: true,
                            opacity: 0.8 + Math.random() * 0.2
                        })
                    );
                    snowflake.position.set(
                        (Math.random() - 0.5) * 120,
                        Math.random() * 100,
                        -Math.random() * 450
                    );
                    snowflake.userData.velocity = 0.04 + Math.random() * 0.08;
                    snowflake.userData.drift = (Math.random() - 0.5) * 0.02;
                    scene.add(snowflake);
                    particles.push(snowflake);
                }
            } else if (gameState.mapType === 'volcano') {
                // Partículas de lava REDUCIDAS DE 40 A 25
                for (let i = 0; i < 25; i++) {
                    const lavaParticle = new THREE.Mesh(
                        new THREE.SphereGeometry(0.4 + Math.random() * 0.9, 6, 6),
                        new THREE.MeshBasicMaterial({ 
                            color: Math.random() > 0.5 ? 0xFF4500 : 0xFF6347,
                            transparent: true,
                            opacity: 0.7 + Math.random() * 0.2
                        })
                    );
                    lavaParticle.position.set(
                        (Math.random() - 0.5) * 90,
                        8 + Math.random() * 15,
                        -Math.random() * 350 - 100
                    );
                    lavaParticle.userData.velocity = {
                        x: (Math.random() - 0.5) * 0.25,
                        y: 0.35 + Math.random() * 0.5,
                        z: -0.15
                    };
                    lavaParticle.userData.life = 0;
                    scene.add(lavaParticle);
                    particles.push(lavaParticle);
                }
                
                // Ceniza REDUCIDA DE 100 A 50
                for (let i = 0; i < 50; i++) {
                    const ash = new THREE.Mesh(
                        new THREE.SphereGeometry(0.05 + Math.random() * 0.1, 3, 3),
                        new THREE.MeshBasicMaterial({
                            color: 0x2a1a1a,
                            transparent: true,
                            opacity: 0.4
                        })
                    );
                    ash.position.set(
                        (Math.random() - 0.5) * 100,
                        Math.random() * 50,
                        -Math.random() * 400
                    );
                    ash.userData.velocity = {
                        x: (Math.random() - 0.5) * 0.1,
                        y: 0.05 + Math.random() * 0.1,
                        z: 0
                    };
                    scene.add(ash);
                    particles.push(ash);
                }
            } else if (gameState.mapType === 'desert') {
                // Partículas de arena REDUCIDAS DE 200 A 100
                for (let i = 0; i < 100; i++) {
                    const sandParticle = new THREE.Mesh(
                        new THREE.SphereGeometry(0.12 + Math.random() * 0.15, 3, 3),
                        new THREE.MeshBasicMaterial({
                            color: Math.random() > 0.5 ? 0xF4A460 : 0xE5C07B,
                            transparent: true,
                            opacity: 0.3 + Math.random() * 0.2
                        })
                    );
                    sandParticle.position.set(
                        (Math.random() - 0.5) * 80,
                        Math.random() * 20,
                        -Math.random() * 450
                    );
                    sandParticle.userData.velocity = {
                        x: 0.15 + Math.random() * 0.25,
                        y: (Math.random() - 0.5) * 0.15,
                        z: 0
                    };
                    scene.add(sandParticle);
                    particles.push(sandParticle);
                }
            } else if (gameState.mapType === 'city') {
                // Luces de neón REDUCIDAS DE 50 A 25
                for (let i = 0; i < 25; i++) {
                    const neonLight = new THREE.Mesh(
                        new THREE.SphereGeometry(0.2, 4, 4),
                        new THREE.MeshBasicMaterial({
                            color: Math.random() > 0.5 ? 0x88CCFF : 0xFFDD88,
                            transparent: true,
                            opacity: 0.6
                        })
                    );
                    neonLight.position.set(
                        (Math.random() - 0.5) * 100,
                        15 + Math.random() * 30,
                        -Math.random() * 400
                    );
                    neonLight.userData.bobSpeed = 0.01 + Math.random() * 0.02;
                    neonLight.userData.bobOffset = Math.random() * Math.PI * 2;
                    scene.add(neonLight);
                    particles.push(neonLight);
                }
            }
        }

        function createRoad() {
            const roadColor = 0x0a0a0a;
            const road = new THREE.Mesh(
                new THREE.PlaneGeometry(14, 500), 
                new THREE.MeshStandardMaterial({ 
                    color: roadColor, 
                    roughness: 0.95,
                    metalness: 0.05
                })
            );
            road.rotation.x = -Math.PI / 2;
            road.position.z = -200;
            road.position.y = 0.05;
            road.receiveShadow = true;
            scene.add(road);
            
            for (let i = 0; i < 50; i++) {
                const line = new THREE.Mesh(
                    new THREE.BoxGeometry(0.5, 0.25, 6), 
                    new THREE.MeshBasicMaterial({ 
                        color: 0xFFFF00,
                        transparent: true,
                        opacity: 0.9
                    })
                );
                line.position.set(0, 0.2, -i * 10);
                scene.add(line);
            }
            
            const shoulderColor = 0x1a1a1a;
            const shoulderL = new THREE.Mesh(
                new THREE.PlaneGeometry(6, 500), 
                new THREE.MeshStandardMaterial({ 
                    color: shoulderColor,
                    roughness: 0.9,
                    metalness: 0.02
                })
            );
            shoulderL.rotation.x = -Math.PI / 2;
            shoulderL.position.set(-10, 0.02, -200);
            shoulderL.receiveShadow = true;
            scene.add(shoulderL);
            
            const shoulderR = new THREE.Mesh(
                new THREE.PlaneGeometry(6, 500), 
                new THREE.MeshStandardMaterial({ 
                    color: shoulderColor,
                    roughness: 0.9,
                    metalness: 0.02
                })
            );
            shoulderR.rotation.x = -Math.PI / 2;
            shoulderR.position.set(10, 0.02, -200);
            shoulderR.receiveShadow = true;
            scene.add(shoulderR);
        }

        function createCar() {
            const carGroup = new THREE.Group();
            const bodyMat = new THREE.MeshStandardMaterial({ color: carColorMap[gameState.carColor], metalness: 0.8, roughness: 0.2 });
            const body = new THREE.Mesh(new THREE.BoxGeometry(2, 1, 4), bodyMat);
            body.position.y = 0.7;
            body.castShadow = true;
            carGroup.add(body);
            carGroup.userData.bodyMaterial = bodyMat;
            const cabinMat = new THREE.MeshStandardMaterial({ color: 0x111111, metalness: 0.95, roughness: 0.05, transparent: true, opacity: 0.9 });
            const cabin = new THREE.Mesh(new THREE.BoxGeometry(1.7, 0.9, 2), cabinMat);
            cabin.position.set(0, 1.5, -0.3);
            cabin.castShadow = true;
            carGroup.add(cabin);
            const windowMat = new THREE.MeshStandardMaterial({ color: 0x000000, metalness: 0.95, roughness: 0.05, transparent: true, opacity: 0.8 });
            const frontWindow = new THREE.Mesh(new THREE.BoxGeometry(1.5, 0.7, 0.9), windowMat);
            frontWindow.position.set(0, 1.5, -1.3);
            carGroup.add(frontWindow);
            const wheelMat = new THREE.MeshStandardMaterial({ color: 0x1a1a1a, metalness: 0.3, roughness: 0.8 });
            const wheelPositions = [
                {pos: [-1.1, 0.4, 1.4], rot: Math.PI / 2},
                {pos: [1.1, 0.4, 1.4], rot: Math.PI / 2},
                {pos: [-1.1, 0.4, -1.4], rot: Math.PI / 2},
                {pos: [1.1, 0.4, -1.4], rot: Math.PI / 2}
            ];
            carGroup.userData.wheels = [];
            wheelPositions.forEach(wheel => {
                const wheelMesh = new THREE.Mesh(new THREE.CylinderGeometry(0.4, 0.4, 0.4, 16), wheelMat);
                wheelMesh.rotation.z = wheel.rot;
                wheelMesh.rotation.y = 0;
                wheelMesh.position.set(...wheel.pos);
                wheelMesh.castShadow = true;
                carGroup.add(wheelMesh);
                carGroup.userData.wheels.push(wheelMesh);
            });
            
            const headlightMat = new THREE.MeshStandardMaterial({ color: 0xFF8C00, emissive: 0xFF8C00, emissiveIntensity: 1 });
            [-0.7, 0.7].forEach(x => {
                const headlight = new THREE.Mesh(new THREE.BoxGeometry(0.3, 0.25, 0.15), headlightMat);
                headlight.position.set(x, 0.7, 2.05);
                carGroup.add(headlight);
                const light = new THREE.SpotLight(0xFFFFDD, 2.0, 60, Math.PI / 8, 0.5, 1);
                light.position.set(x, 0.7, 2.1);
                light.target.position.set(x, 0, -40);
                light.castShadow = true;
                light.shadow.mapSize.width = 1024;
                light.shadow.mapSize.height = 1024;
                light.shadow.camera.near = 0.5;
                light.shadow.camera.far = 60;
                carGroup.add(light);
                carGroup.add(light.target);
            });
            
            const rearLightMat = new THREE.MeshStandardMaterial({ color: 0xFF0000, emissive: 0xFF0000, emissiveIntensity: 0.8 });
            [-0.7, 0.7].forEach(x => {
                const rearLight = new THREE.Mesh(new THREE.BoxGeometry(0.3, 0.25, 0.15), rearLightMat);
                rearLight.position.set(x, 0.7, -2.05);
                rearLight.castShadow = false;
                carGroup.add(rearLight);
            });
            
            const shieldGeometry = new THREE.SphereGeometry(2.8, 32, 32);
            const shieldMaterial = new THREE.MeshBasicMaterial({
                color: 0x3B82F6,
                transparent: true,
                opacity: 0,
                side: THREE.DoubleSide,
                depthWrite: false
            });
            shieldMesh = new THREE.Mesh(shieldGeometry, shieldMaterial);
            shieldMesh.visible = false;
            carGroup.add(shieldMesh);
            
            carGroup.position.set(0, 0, 0);
            scene.add(carGroup);
            car = carGroup;
        }

        function createBrightwayPosts() {
            for (let i = 0; i < 15; i++) {
                const postGroup = new THREE.Group();
                const body = new THREE.Mesh(new THREE.BoxGeometry(0.8, 3, 0.8), new THREE.MeshStandardMaterial({ color: 0xFF6B35, metalness: 0.5, roughness: 0.4 }));
                body.position.y = 1.5;
                body.castShadow = true;
                postGroup.add(body);
                const panel = new THREE.Mesh(new THREE.BoxGeometry(1, 0.6, 0.1), new THREE.MeshStandardMaterial({ color: 0x1E3A8A, metalness: 0.6, roughness: 0.3 }));
                panel.position.set(0, 3.3, 0);
                panel.castShadow = true;
                postGroup.add(panel);
                const ledMat = new THREE.MeshStandardMaterial({ color: 0xFFC107, emissive: 0xFFC107, emissiveIntensity: 0.3 });
                postGroup.userData.leds = [];
                for (let j = 0; j < 3; j++) {
                    const led = new THREE.Mesh(new THREE.SphereGeometry(0.15, 8, 8), ledMat.clone());
                    led.position.set((j - 1) * 0.3, 2, 0.45);
                    postGroup.add(led);
                    postGroup.userData.leds.push(led);
                }
                const side = i % 2 === 0 ? -1 : 1;
                postGroup.position.set(side * 12, 0, -i * 30 - 20);
                scene.add(postGroup);
                brightwayPosts.push(postGroup);
            }
        }

        function createShieldPickup() {
            const shieldGroup = new THREE.Group();
            
            const outerRing = new THREE.Mesh(
                new THREE.TorusGeometry(1.2, 0.15, 16, 32),
                new THREE.MeshStandardMaterial({
                    color: 0x3B82F6,
                    emissive: 0x3B82F6,
                    emissiveIntensity: 0.5,
                    metalness: 0.8,
                    roughness: 0.2
                })
            );
            outerRing.rotation.x = Math.PI / 2;
            shieldGroup.add(outerRing);
            
            const shieldCore = new THREE.Mesh(
                new THREE.SphereGeometry(0.6, 16, 16),
                new THREE.MeshBasicMaterial({
                    color: 0x60A5FA,
                    transparent: true,
                    opacity: 0.6
                })
            );
            shieldGroup.add(shieldCore);
            
            shieldGroup.userData.rotation = 0;
            shieldGroup.userData.bobOffset = Math.random() * Math.PI * 2;
            
            const lanes = [-4.5, 0, 4.5];
            const lane = lanes[Math.floor(Math.random() * lanes.length)];
            shieldGroup.position.set(lane, 1.5, -200);
            shieldGroup.userData.type = 'shield';
            
            scene.add(shieldGroup);
            shieldPickups.push(shieldGroup);
        }

        function createObstacle() {
            const colors = [0xEF4444, 0x10B981, 0xFBBF24, 0x8B5CF6];
            const color = colors[Math.floor(Math.random() * colors.length)];
            const carGroup = new THREE.Group();
            const body = new THREE.Mesh(new THREE.BoxGeometry(2, 1, 4), new THREE.MeshStandardMaterial({ color, metalness: 0.7, roughness: 0.3 }));
            body.position.y = 0.7;
            body.castShadow = true;
            carGroup.add(body);
            const cabin = new THREE.Mesh(new THREE.BoxGeometry(1.8, 0.8, 2), new THREE.MeshStandardMaterial({ color: 0x111111, metalness: 0.9, roughness: 0.1, transparent: true, opacity: 0.9 }));
            cabin.position.set(0, 1.4, 0.4);
            cabin.castShadow = true;
            carGroup.add(cabin);
            const wheelMat = new THREE.MeshStandardMaterial({ color: 0x1a1a1a });
            const wheelPositions = [[-1, 0.4, 1.5], [1, 0.4, 1.5], [-1, 0.4, -1.5], [1, 0.4, -1.5]];
            carGroup.userData.wheels = [];
            wheelPositions.forEach(pos => {
                const wheel = new THREE.Mesh(new THREE.CylinderGeometry(0.4, 0.4, 0.4, 12), wheelMat);
                wheel.rotation.z = Math.PI / 2;
                wheel.position.set(...pos);
                wheel.castShadow = true;
                carGroup.add(wheel);
                carGroup.userData.wheels.push(wheel);
            });
            
            const rearLightMat = new THREE.MeshStandardMaterial({ color: 0xFF0000, emissive: 0xFF0000, emissiveIntensity: 0.6 });
            [-0.7, 0.7].forEach(x => {
                const rearLight = new THREE.Mesh(new THREE.BoxGeometry(0.3, 0.25, 0.15), rearLightMat);
                rearLight.position.set(x, 0.7, 1.9);
                rearLight.castShadow = false;
                carGroup.add(rearLight);
            });
            const lanes = [-4.5, 0, 4.5];
            const lane = lanes[Math.floor(Math.random() * lanes.length)];
            carGroup.position.set(lane, 0.7, -200);
            carGroup.userData = { type: 'car', lane };
            scene.add(carGroup);
            obstacles.push(carGroup);
        }

        let controlsSetup = false;
        
        function setupControls() {
            if (controlsSetup) return;
            
            document.addEventListener('keydown', (e) => {
                if (e.key === 'ArrowLeft') keys.left = true;
                if (e.key === 'ArrowRight') keys.right = true;
                if (e.key === 'ArrowUp') { 
                    e.preventDefault();
                    keys.up = true;
                    if (gameState.running && !gameState.paused && !gameState.isJumping) {
                        jump();
                    }
                }
                if (e.key === 'ArrowDown') keys.down = true;
                if (e.key === ' ') { e.preventDefault(); keys.space = true; toggleBrightWay(); }
            });
            document.addEventListener('keyup', (e) => {
                if (e.key === 'ArrowLeft') keys.left = false;
                if (e.key === 'ArrowRight') keys.right = false;
                if (e.key === 'ArrowUp') keys.up = false;
                if (e.key === 'ArrowDown') keys.down = false;
                if (e.key === ' ') keys.space = false;
            });
            
            const canvas = document.getElementById('gameCanvas');
            canvas.addEventListener('touchstart', (e) => {
                const touch = e.touches[0];
                const rect = canvas.getBoundingClientRect();
                const x = touch.clientX - rect.left;
                const currentTime = new Date().getTime();
                const tapLength = currentTime - lastTapTime;
                
                if (tapLength < 300 && tapLength > 0 && Math.abs(x - lastTapX) < 50) {
                    if (gameState.running && !gameState.paused && !gameState.isJumping) {
                        jump();
                    }
                    lastTapTime = 0;
                } else {
                    if (x < rect.width / 2) keys.left = true;
                    else keys.right = true;
                    lastTapTime = currentTime;
                    lastTapX = x;
                }
            });
            canvas.addEventListener('touchend', () => { keys.left = false; keys.right = false; });
            
            // Botones del juego con prevención de propagación
            const brightwayBtn = document.getElementById('brightwayBtn');
            const pauseBtn = document.getElementById('pauseBtn');
            const menuBtn = document.getElementById('menuBtn');
            const exitGameBtn = document.getElementById('exitGameBtn');
            
            brightwayBtn.onclick = function(e) {
                e.preventDefault();
                e.stopPropagation();
                toggleBrightWay();
            };
            
            pauseBtn.onclick = function(e) {
                e.preventDefault();
                e.stopPropagation();
                if (!gameState.running) return;
                gameState.paused = !gameState.paused;
                this.innerHTML = gameState.paused ? '▶ Reanudar' : '⏸ Pausar';
            };
            
            menuBtn.onclick = function(e) {
                e.preventDefault();
                e.stopPropagation();
                returnToMenu();
            };
            
            exitGameBtn.onclick = function(e) {
                e.preventDefault();
                e.stopPropagation();
                returnToMenu();
            };
            
            controlsSetup = true;
        }

        function toggleBrightWay() {
            if (!gameState.running || gameState.paused) {
                console.log('No se puede activar BrightWay: juego no activo o pausado');
                return;
            }
            
            playClick();
            
            if (gameState.brightwayActive) {
                gameState.brightwayActive = false;
                deactivateBrightWay();
                console.log('BrightWay desactivado');
            } else {
                gameState.brightwayActive = true;
                activateBrightWay();
                console.log('BrightWay activado');
            }
        }

        // Configuración de iluminación para cada mapa
        const mapLightingConfig = {
            city: {
                normal: {
                    fog: 0x050510,
                    background: 0x020208,
                    directional: { intensity: 0.2, color: 0x5a5a8a },
                    ambient: { intensity: 0.18, color: 0x2a2a40 },
                    hemisphere: { intensity: 0.18 }
                },
                brightway: {
                    fog: 0x8a7050,
                    background: 0x2a2015,
                    directional: { intensity: 1.8, color: 0xCCCC88 },
                    ambient: { intensity: 0.6 },
                    hemisphere: { intensity: 0.55 }
                }
            },
            snow: {
                normal: {
                    fog: 0x4a5560,
                    background: 0x2a3540,
                    directional: { intensity: 0.22, color: 0x8090a0 },
                    ambient: { intensity: 0.15, color: 0x5a6a7a },
                    hemisphere: { intensity: 0.18, groundColor: 0x3a4550, skyColor: 0x5a6a7a }
                },
                brightway: {
                    fog: 0x9a9580,
                    background: 0x6a7580,
                    directional: { intensity: 1.0, color: 0xBBBB99 },
                    ambient: { intensity: 0.5, color: 0x9a9a88 },
                    hemisphere: { intensity: 0.45, groundColor: 0x7a8090, skyColor: 0xa0b0c0 }
                }
            },
            volcano: {
                normal: {
                    fog: 0x2a0505,
                    background: 0x0a0202,
                    directional: { intensity: 0.35, color: 0xaa3300 },
                    ambient: { intensity: 0.18, color: 0x8a2200 },
                    hemisphere: { intensity: 0.22 }
                },
                brightway: {
                    fog: 0xaa5020,
                    background: 0x3a1005,
                    directional: { intensity: 1.8, color: 0xCCCC88 },
                    ambient: { intensity: 0.55 },
                    hemisphere: { intensity: 0.5 }
                }
            },
            desert: {
                normal: {
                    fog: 0x6a5540,
                    background: 0x4a3520,
                    directional: { intensity: 0.22, color: 0x8a7050 },
                    ambient: { intensity: 0.15, color: 0x6a5540 },
                    hemisphere: { intensity: 0.18, groundColor: 0x5a4530, skyColor: 0x6a5540 }
                },
                brightway: {
                    fog: 0xaa9570,
                    background: 0x7a6040,
                    directional: { intensity: 0.9, color: 0xBBBB99 },
                    ambient: { intensity: 0.48, color: 0x9a8870 },
                    hemisphere: { intensity: 0.42, groundColor: 0x7a6a50, skyColor: 0xa09080 }
                }
            }
        };

        function applyLighting(mode) {
            const config = mapLightingConfig[gameState.mapType];
            if (!config) {
                console.error('No hay configuración para el mapa:', gameState.mapType);
                return;
            }
            
            const lighting = config[mode];
            if (!lighting) {
                console.error('No hay configuración de iluminación para modo:', mode);
                return;
            }
            
            console.log(`Aplicando iluminación ${mode} para mapa ${gameState.mapType}`);
            
            // Aplicar niebla y fondo
            if (scene.fog) scene.fog.color.setHex(lighting.fog);
            if (scene.background) scene.background.setHex(lighting.background);
            
            // Aplicar a todas las luces de la escena
            scene.children.forEach(child => {
                if (child instanceof THREE.DirectionalLight) {
                    child.intensity = lighting.directional.intensity;
                    if (lighting.directional.color) child.color.setHex(lighting.directional.color);
                    console.log('DirectionalLight actualizada:', child.intensity);
                } else if (child instanceof THREE.AmbientLight) {
                    child.intensity = lighting.ambient.intensity;
                    if (lighting.ambient.color) child.color.setHex(lighting.ambient.color);
                    console.log('AmbientLight actualizada:', child.intensity);
                } else if (child instanceof THREE.HemisphereLight) {
                    child.intensity = lighting.hemisphere.intensity;
                    if (lighting.hemisphere.groundColor) child.groundColor.setHex(lighting.hemisphere.groundColor);
                    if (lighting.hemisphere.skyColor) child.color.setHex(lighting.hemisphere.skyColor);
                    console.log('HemisphereLight actualizada:', child.intensity);
                }
            });
            
            // Aplicar a LEDs de postes
            const ledIntensity = mode === 'brightway' ? 3 : 0.3;
            brightwayPosts.forEach(post => {
                if (post.userData.leds) {
                    post.userData.leds.forEach(led => {
                        led.material.emissiveIntensity = ledIntensity;
                    });
                }
            });
            
            console.log(`Iluminación ${mode} aplicada correctamente`);
        }

        function activateBrightWay() {
            if (!gameState.running || gameState.paused) return;
            
            playBrightWaySound();
            gameState.score += 1000;
            
            const btn = document.getElementById('brightwayBtn');
            btn.classList.add('active');
            
            applyLighting('brightway');
            
            console.log('BrightWay activado - Iluminación mejorada');
        }

        function deactivateBrightWay() {
            const btn = document.getElementById('brightwayBtn');
            btn.classList.remove('active');
            
            applyLighting('normal');
            
            console.log('BrightWay desactivado - Iluminación normal');
        }

        function jump() {
            if (gameState.isJumping || !gameState.running || gameState.paused) return;
            playJumpSound();
            gameState.isJumping = true;
            gameState.jumpVelocity = 0.35;
        }

        function playJumpSound() {
            if (!soundOn) return;
            const o = audioCtx.createOscillator();
            const g = audioCtx.createGain();
            o.connect(g);
            g.connect(audioCtx.destination);
            o.frequency.setValueAtTime(300, audioCtx.currentTime);
            o.frequency.exponentialRampToValueAtTime(800, audioCtx.currentTime + 0.2);
            o.type = 'sine';
            g.gain.setValueAtTime(0.3 * soundVol, audioCtx.currentTime);
            g.gain.exponentialRampToValueAtTime(0.01, audioCtx.currentTime + 0.2);
            o.start();
            o.stop(audioCtx.currentTime + 0.2);
        }

        function activateShield() {
            if (gameState.shieldActive || !gameState.running) return;
            playShieldSound();
            gameState.shieldActive = true;
            gameState.shieldTimeLeft = 10;
            
            const shieldIndicator = document.getElementById('shieldIndicator');
            shieldIndicator.classList.add('active');
            shieldIndicator.classList.remove('warning');
            
            if (shieldMesh) {
                shieldMesh.visible = true;
                shieldMesh.material.opacity = 0.4;
            }
            
            const shieldInterval = setInterval(() => {
                if (!gameState.running || gameState.paused || !gameState.shieldActive) {
                    clearInterval(shieldInterval);
                    return;
                }
                
                gameState.shieldTimeLeft--;
                document.getElementById('shieldTime').textContent = gameState.shieldTimeLeft;
                
                if (gameState.shieldTimeLeft <= 3 && gameState.shieldTimeLeft > 0) {
                    shieldIndicator.classList.add('warning');
                    if (shieldMesh) {
                        shieldMesh.material.opacity = (gameState.shieldTimeLeft % 2 === 0) ? 0.6 : 0.2;
                    }
                }
                
                if (gameState.shieldTimeLeft <= 0) {
                    gameState.shieldActive = false;
                    shieldIndicator.classList.remove('active', 'warning');
                    if (shieldMesh) {
                        shieldMesh.visible = false;
                        shieldMesh.material.opacity = 0;
                    }
                    clearInterval(shieldInterval);
                }
            }, 1000);
        }

        function showComboIndicator() {
            const comboEl = document.getElementById('comboIndicator');
            comboEl.textContent = `COMBO x${gameState.combo}!`;
            comboEl.classList.remove('show');
            void comboEl.offsetWidth;
            comboEl.classList.add('show');
        }

        function updateGame() {
            if (!gameState.running || gameState.paused) return;
            gameState.frameCount++;
            if (keys.left && gameState.carPosition > -4.5) gameState.carPosition -= 0.25;
            if (keys.right && gameState.carPosition < 4.5) gameState.carPosition += 0.25;
            car.position.x += (gameState.carPosition - car.position.x) * 0.12;
            
            // Velocidad FIJA en 60 km/h - SIN INCREMENTOS
            gameState.speed = 60;
            
            // Física del salto
            if (gameState.isJumping) {
                gameState.jumpHeight += gameState.jumpVelocity;
                gameState.jumpVelocity -= 0.02; // Gravedad
                
                if (gameState.jumpHeight <= 0) {
                    gameState.jumpHeight = 0;
                    gameState.jumpVelocity = 0;
                    gameState.isJumping = false;
                }
                
                car.position.y = gameState.jumpHeight;
            }
            
            const elapsedTime = gameState.frameCount / 60;
            gameState.obstacleFrequency = Math.max(25, 120 - (elapsedTime * 3));
            
            if (gameState.frameCount % Math.floor(gameState.obstacleFrequency) === 0) {
                createObstacle();
                if (elapsedTime > 20 && Math.random() > 0.4) {
                    createObstacle();
                }
                if (elapsedTime > 40 && Math.random() > 0.6) {
                    createObstacle();
                }
            }
            
            if (gameState.frameCount % 180 === 0 && Math.random() > 0.3) {
                createShieldPickup();
            }
            
            obstacles.forEach((obstacle, index) => {
                obstacle.position.z += gameState.speed * 0.04;
                if (obstacle.userData.wheels) {
                    obstacle.userData.wheels.forEach(wheel => {
                        wheel.rotation.x += gameState.speed * 0.012;
                    });
                }
                if (obstacle.position.z > 20) {
                    scene.remove(obstacle);
                    obstacles.splice(index, 1);
                    gameState.score += 200;
                    playScoreSound();
                }
                if (Math.abs(obstacle.position.z - car.position.z) < 3 && Math.abs(obstacle.position.x - car.position.x) < 2.2) {
                    if (gameState.isJumping && gameState.jumpHeight > 2) {
                        // Saltó exitosamente sobre el obstáculo
                        gameState.score += 500;
                        playScoreSound();
                    } else if (gameState.shieldActive) {
                        scene.remove(obstacle);
                        obstacles.splice(index, 1);
                        gameState.score += 500;
                        playScoreSound();
                    } else {
                        endGame();
                    }
                }
            });
            
            shieldPickups.forEach((pickup, index) => {
                pickup.position.z += gameState.speed * 0.04;
                
                pickup.userData.rotation += 0.02;
                pickup.rotation.y = pickup.userData.rotation;
                pickup.position.y = 1.5 + Math.sin(Date.now() * 0.003 + pickup.userData.bobOffset) * 0.3;
                
                if (pickup.position.z > 20) {
                    scene.remove(pickup);
                    shieldPickups.splice(index, 1);
                }
                
                if (Math.abs(pickup.position.z - car.position.z) < 2 && Math.abs(pickup.position.x - car.position.x) < 2) {
                    scene.remove(pickup);
                    shieldPickups.splice(index, 1);
                    
                    const currentTime = Date.now();
                    if (currentTime - gameState.lastPickupTime < 3000) {
                        gameState.combo++;
                        if (gameState.combo >= 2) {
                            gameState.score += 500 * gameState.combo;
                            playComboSound();
                            showComboIndicator();
                        }
                    } else {
                        gameState.combo = 1;
                    }
                    gameState.lastPickupTime = currentTime;
                    
                    activateShield();
                }
            });
            
            particles.forEach((particle, index) => {
                if (gameState.mapType === 'snow') {
                    particle.position.y -= particle.userData.velocity;
                    particle.position.x += particle.userData.drift;
                    particle.position.z += gameState.speed * 0.016;
                    if (particle.position.y < 0) {
                        particle.position.y = 100;
                        particle.position.x = (Math.random() - 0.5) * 120;
                        particle.position.z = -Math.random() * 450;
                    }
                } else if (gameState.mapType === 'volcano') {
                    particle.position.x += particle.userData.velocity.x;
                    particle.position.y += particle.userData.velocity.y;
                    particle.position.z += particle.userData.velocity.z + gameState.speed * 0.016;
                    particle.userData.velocity.y -= 0.018;
                    particle.userData.life++;
                    
                    // Parpadeo de lava
                    if (particle.material.color.r > 0.5) {
                        particle.material.opacity = 0.6 + Math.sin(Date.now() * 0.005 + index) * 0.2;
                    }
                    
                    if (particle.position.y < 0 || particle.userData.life > 220) {
                        particle.position.set(
                            (Math.random() - 0.5) * 90,
                            8 + Math.random() * 15,
                            -Math.random() * 350 - 100
                        );
                        particle.userData.velocity = {
                            x: (Math.random() - 0.5) * 0.25,
                            y: 0.35 + Math.random() * 0.5,
                            z: -0.15
                        };
                        particle.userData.life = 0;
                    }
                } else if (gameState.mapType === 'desert') {
                    particle.position.x += particle.userData.velocity.x;
                    particle.position.y += particle.userData.velocity.y;
                    particle.position.z += gameState.speed * 0.016;
                    
                    if (particle.position.x > 40 || particle.position.z > 30) {
                        particle.position.set(
                            (Math.random() - 0.5) * 80,
                            Math.random() * 20,
                            -Math.random() * 450
                        );
                    }
                } else if (gameState.mapType === 'city') {
                    // Movimiento de flotación para luces de neón
                    particle.position.y += Math.sin(Date.now() * particle.userData.bobSpeed + particle.userData.bobOffset) * 0.05;
                    particle.position.z += gameState.speed * 0.016;
                    
                    // Parpadeo suave
                    particle.material.opacity = 0.5 + Math.sin(Date.now() * 0.003 + particle.userData.bobOffset) * 0.3;
                    
                    if (particle.position.z > 30) {
                        particle.position.z = -Math.random() * 400;
                        particle.position.x = (Math.random() - 0.5) * 100;
                    }
                }
            });
            
            brightwayPosts.forEach(post => {
                post.position.z += gameState.speed * 0.04;
                if (post.position.z > 30) post.position.z -= 450;
            });
            scene.children.forEach(child => {
                if (child.geometry && child.geometry.type === 'BoxGeometry' && child.material.color && child.material.color.getHex() === 0xFFFF00) {
                    child.position.z += gameState.speed * 0.04;
                    if (child.position.z > 30) child.position.z -= 500;
                }
            });
            if (car.userData.wheels) {
                car.userData.wheels.forEach(wheel => {
                    wheel.rotation.x += gameState.speed * 0.008;
                });
            }
            
            if (shieldMesh && gameState.shieldActive) {
                shieldMesh.rotation.y += 0.03;
                if (gameState.shieldTimeLeft > 3) {
                    shieldMesh.material.opacity = 0.4 + Math.sin(Date.now() * 0.005) * 0.15;
                }
            }
            
            gameState.score += Math.floor(gameState.speed * 0.08);
            if (gameState.brightwayActive) {
                gameState.score += Math.floor(gameState.speed * 0.12);
            }
            
            // Actualizar UI - asegurar que velocidad sea un entero
            document.getElementById('score').textContent = Math.floor(gameState.score);
            document.getElementById('speedValue').textContent = Math.floor(gameState.speed);
            camera.position.z = car.position.z + 14 + (gameState.speed * 0.02);
            camera.position.y = 6 + (gameState.speed * 0.01);
            camera.position.x += (car.position.x * 0.15 - camera.position.x) * 0.1;
            camera.lookAt(car.position.x * 0.3, 1, car.position.z - 15);
        }

        function animate() {
            if (!gameState.running && !gameState.paused) return;
            requestAnimationFrame(animate);
            if (!gameState.paused) updateGame();
            renderer.render(scene, camera);
        }

        function endGame() {
            gameState.running = false;
            gameState.paused = false;
            
            // Velocidad FIJA en 60
            gameState.speed = 60;
            gameState.maxSpeed = 60;
            
            playHitSound();
            
            // Ocultar controles del juego
            document.getElementById('gameControls').classList.remove('show');
            
            const finalScore = Math.floor(gameState.score);
            document.getElementById('finalScore').textContent = finalScore;
            
            if (finalScore > gameState.highScore) {
                gameState.highScore = finalScore;
                document.getElementById('highScore').textContent = finalScore;
                localStorage.setItem('brightwayHighScore3D', finalScore);
            }
            
            document.getElementById('gameOverScreen').classList.add('show');
        }

        function resetGameState() {
            // Detener el loop de animación primero
            gameState.running = false;
            gameState.paused = false;
            
            // Desactivar BrightWay completamente antes de limpiar
            if (gameState.brightwayActive) {
                gameState.brightwayActive = false;
                document.getElementById('brightwayBtn').classList.remove('active');
                
                // Restaurar iluminación a estado inicial según el mapa
                if (scene && gameState.mapType) {
                    if (gameState.mapType === 'city') {
                        scene.fog.color.setHex(0x0a0a1a);
                        scene.background.setHex(0x050510);
                    } else if (gameState.mapType === 'snow') {
                        scene.fog.color.setHex(0x0a0a1a);
                        scene.background.setHex(0x0d1117);
                    } else if (gameState.mapType === 'volcano') {
                        scene.fog.color.setHex(0x450a0a);
                        scene.background.setHex(0x1a0505);
                    } else if (gameState.mapType === 'desert') {
                        scene.fog.color.setHex(0x1a1410);
                        scene.background.setHex(0x0f0c09);
                    }
                }
            }
            
            // Limpiar completamente todos los objetos
            obstacles.forEach(obstacle => {
                if (obstacle.geometry) obstacle.geometry.dispose();
                if (obstacle.material) {
                    if (Array.isArray(obstacle.material)) {
                        obstacle.material.forEach(mat => mat.dispose());
                    } else {
                        obstacle.material.dispose();
                    }
                }
                scene.remove(obstacle);
            });
            obstacles = [];
            
            shieldPickups.forEach(pickup => {
                if (pickup.geometry) pickup.geometry.dispose();
                if (pickup.material) {
                    if (Array.isArray(pickup.material)) {
                        pickup.material.forEach(mat => mat.dispose());
                    } else {
                        pickup.material.dispose();
                    }
                }
                scene.remove(pickup);
            });
            shieldPickups = [];
            
            // Resetear TODOS los valores del estado del juego
            gameState.score = 0;
            gameState.speed = 40;
            gameState.maxSpeed = 60;
            gameState.acceleration = 0.5;
            gameState.carPosition = 0;
            gameState.brightwayActive = false;
            gameState.shieldActive = false;
            gameState.shieldTimeLeft = 0;
            gameState.frameCount = 0;
            gameState.obstacleFrequency = 120;
            gameState.combo = 0;
            gameState.lastPickupTime = 0;
            gameState.isJumping = false;
            gameState.jumpHeight = 0;
            gameState.jumpVelocity = 0;
            
            // Resetear controles
            keys.left = false;
            keys.right = false;
            keys.up = false;
            keys.down = false;
            keys.space = false;
            
            // Resetear UI
            document.getElementById('brightwayBtn').classList.remove('active');
            document.getElementById('shieldIndicator').classList.remove('active', 'warning');
            document.getElementById('pauseBtn').innerHTML = '⏸ Pausar';
            document.getElementById('score').textContent = '0';
            document.getElementById('speedValue').textContent = '40';
            document.getElementById('shieldTime').textContent = '10';
            document.getElementById('gameOverScreen').classList.remove('show');
            document.getElementById('gameControls').classList.remove('show');
            
            console.log('Estado del juego reseteado completamente');
            
            if (shieldMesh) {
                shieldMesh.visible = false;
                shieldMesh.material.opacity = 0;
            }
            
            if (car) {
                car.position.set(0, 0, 0);
                car.rotation.set(0, 0, 0);
            }
            
            if (camera) {
                camera.position.set(0, 6, 14);
                camera.lookAt(0, 1, -10);
            }
        }

        function restartGame() {
            document.getElementById('gameOverScreen').classList.remove('show');
            
            // Resetear completamente el estado
            resetGameState();
            
            // Velocidad FIJA en 60
            gameState.speed = 60;
            gameState.maxSpeed = 60;
            
            // Asegurar que BrightWay esté desactivado
            gameState.brightwayActive = false;
            document.getElementById('brightwayBtn').classList.remove('active');
            
            // Mostrar controles del juego
            document.getElementById('gameControls').classList.add('show');
            
            // Actualizar UI
            document.getElementById('speedValue').textContent = '60';
            document.getElementById('score').textContent = '0';
            
            // Reiniciar juego
            gameState.running = true;
            gameState.paused = false;
            
            animate();
        }

        function returnToMenu() {
            // Resetear completamente el estado
            resetGameState();
            
            // Limpiar postes y partículas
            brightwayPosts.forEach(post => scene.remove(post));
            brightwayPosts = [];
            particles.forEach(particle => scene.remove(particle));
            particles = [];
            
            // Limpiar renderizador
            if (renderer) {
                renderer.dispose();
                scene.traverse(object => {
                    if (object.geometry) object.geometry.dispose();
                    if (object.material) {
                        if (Array.isArray(object.material)) object.material.forEach(material => material.dispose());
                        else object.material.dispose();
                    }
                });
            }
            
            // Ocultar modal de juego y pantalla de game over
            document.getElementById('gameModal').classList.remove('active');
            document.getElementById('gameOverScreen').classList.remove('show');
            
            // Ocultar controles del juego
            document.getElementById('gameControls').classList.remove('show');
            
            // Mostrar selector de mapas
            document.getElementById('mapSelectorScreen').classList.remove('hidden');
            document.getElementById('carSelectorScreen').classList.add('hidden');
            
            // Limpiar selecciones
            document.querySelectorAll('.option-card').forEach(c => c.classList.remove('selected'));
            
            // Resetear UI de botones
            document.getElementById('brightwayBtn').classList.remove('active');
            document.getElementById('shieldIndicator').classList.remove('active', 'warning');
            document.getElementById('pauseBtn').innerHTML = '⏸ Pausar';
            
            // Resetear estado del juego
            gameState.carColor = null;
            gameState.mapType = null;
            gameState.shieldActive = false;
            gameState.shieldTimeLeft = 0;
            gameState.combo = 0;
            gameState.lastPickupTime = 0;
            gameState.brightwayActive = false;
            
            console.log('Regresando al menú principal');
        }

        window.addEventListener('resize', () => {
            if (camera && renderer) {
                camera.aspect = window.innerWidth / window.innerHeight;
                camera.updateProjectionMatrix();
                renderer.setSize(window.innerWidth, window.innerHeight);
                renderer.setPixelRatio(Math.min(window.devicePixelRatio, 2));
                gameState.isMobile = window.innerWidth <= 768;
            }
        });

        document.addEventListener('click', function initAudio() {
            if (audioCtx.state === 'suspended') {
                audioCtx.resume().then(() => { if (musicOn) startMusic(); });
            }
        }, { once: true });

        // Detectar cuando el usuario sale de la aplicación
        document.addEventListener('visibilitychange', function() {
            if (document.hidden) {
                // Usuario salió de la app - detener música
                stopMusic();
                console.log('App en segundo plano - música detenida');
            } else {
                // Usuario regresó a la app - reanudar música si estaba activa
                if (musicOn && !bgMusic) {
                    startMusic();
                    console.log('App en primer plano - música reanudada');
                }
            }
        });

        // Para navegadores móviles (iOS/Android)
        window.addEventListener('blur', function() {
            stopMusic();
            console.log('Ventana perdió foco - música detenida');
        });

        window.addEventListener('focus', function() {
            if (musicOn && !bgMusic) {
                startMusic();
                console.log('Ventana ganó foco - música reanudada');
            }
        });
    </script>
</body>
</html>
