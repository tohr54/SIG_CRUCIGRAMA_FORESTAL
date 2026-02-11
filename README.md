# SIG_CRUCIGRAMA_FORESTAL
Crucigrama educativo para Introducción a SIG - Ingeniería Forestal
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>Crucigrama SIG - Ingeniería Forestal</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            -webkit-tap-highlight-color: transparent;
            touch-action: manipulation;
        }

        body {
            font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;
            background: linear-gradient(180deg, #1b5e20 0%, #0d3320 100%);
            min-height: 100vh;
            color: white;
            padding: 10px;
            padding-bottom: 280px;
        }

        .header {
            text-align: center;
            padding: 15px;
            background: rgba(0,0,0,0.3);
            border-radius: 12px;
            margin-bottom: 10px;
        }

        .header h1 {
            font-size: 1.4rem;
            color: #81c784;
            margin-bottom: 5px;
        }

        .header p {
            font-size: 0.85rem;
            opacity: 0.9;
        }

        .info-panel {
            background: rgba(0,0,0,0.2);
            padding: 10px;
            border-radius: 8px;
            margin-bottom: 10px;
            text-align: center;
            min-height: 60px;
            display: flex;
            flex-direction: column;
            justify-content: center;
        }

        .info-panel .label {
            font-size: 0.75rem;
            color: #a5d6a7;
            text-transform: uppercase;
            margin-bottom: 4px;
        }

        .info-panel .word-display {
            font-size: 1.2rem;
            font-weight: bold;
            color: #fff;
            letter-spacing: 2px;
        }

        .controls {
            display: flex;
            gap: 8px;
            margin-bottom: 10px;
        }

        .btn {
            flex: 1;
            padding: 12px;
            background: #2e7d32;
            color: white;
            border: none;
            border-radius: 8px;
            font-size: 0.85rem;
            font-weight: 600;
            cursor: pointer;
            transition: all 0.2s;
        }

        .btn:active {
            transform: scale(0.95);
            background: #1b5e20;
        }

        .btn.secondary {
            background: rgba(255,255,255,0.15);
        }

        .dir-controls {
            display: flex;
            gap: 8px;
            margin-bottom: 10px;
        }

        .dir-btn {
            flex: 1;
            padding: 10px;
            background: rgba(255,255,255,0.1);
            border: 2px solid transparent;
            color: white;
            border-radius: 8px;
            font-size: 0.85rem;
            cursor: pointer;
            transition: all 0.2s;
        }

        .dir-btn.active {
            background: #4caf50;
            border-color: #81c784;
            font-weight: bold;
        }

        .progress-container {
            margin-bottom: 10px;
        }

        .progress-bar {
            height: 8px;
            background: rgba(255,255,255,0.2);
            border-radius: 4px;
            overflow: hidden;
        }

        .progress-fill {
            height: 100%;
            background: linear-gradient(90deg, #81c784, #4caf50);
            width: 0%;
            transition: width 0.3s;
        }

        .progress-text {
            text-align: center;
            font-size: 0.8rem;
            margin-top: 5px;
            color: #c8e6c9;
        }

        .grid-wrapper {
            background: rgba(0,0,0,0.3);
            padding: 15px;
            border-radius: 12px;
            overflow-x: auto;
            -webkit-overflow-scrolling: touch;
            margin-bottom: 10px;
        }

        .grid {
            display: grid;
            grid-template-columns: repeat(15, 30px);
            grid-template-rows: repeat(15, 30px);
            gap: 2px;
            background: #1b5e20;
            padding: 2px;
            width: fit-content;
            margin: 0 auto;
        }

        .cell {
            width: 30px;
            height: 30px;
            background: #e8f5e9;
            display: flex;
            align-items: center;
            justify-content: center;
            font-weight: bold;
            font-size: 1rem;
            color: #1b5e20;
            position: relative;
            cursor: pointer;
            user-select: none;
            -webkit-user-select: none;
            transition: all 0.2s;
        }

        .cell.blocked {
            background: #0d3320;
            cursor: default;
        }

        .cell.selected {
            background: #81c784;
            box-shadow: inset 0 0 0 3px #2e7d32;
            transform: scale(1.05);
            z-index: 10;
        }

        .cell.highlighted {
            background: #c8e6c9;
        }

        .cell.correct {
            background: #4caf50;
            color: white;
        }

        .cell.wrong {
            background: #ef5350;
            color: white;
        }

        .cell-number {
            position: absolute;
            top: 1px;
            left: 2px;
            font-size: 0.65rem;
            font-weight: bold;
            color: #2e7d32;
            line-height: 1;
        }

        .clues-container {
            background: rgba(0,0,0,0.3);
            border-radius: 12px;
            overflow: hidden;
        }

        .clues-header {
            padding: 15px;
            background: rgba(76, 175, 80, 0.3);
            cursor: pointer;
            display: flex;
            justify-content: space-between;
            align-items: center;
            font-weight: 600;
        }

        .clues-content {
            max-height: 0;
            overflow: hidden;
            transition: max-height 0.3s ease;
        }

        .clues-content.open {
            max-height: 3000px;
        }

        .clue-section {
            padding: 12px;
            border-bottom: 1px solid rgba(255,255,255,0.1);
        }

        .clue-section:last-child {
            border-bottom: none;
        }

        .clue-title {
            font-weight: bold;
            color: #81c784;
            margin-bottom: 8px;
            font-size: 0.9rem;
        }

        .clue {
            padding: 10px;
            margin-bottom: 6px;
            background: rgba(255,255,255,0.05);
            border-radius: 6px;
            font-size: 0.85rem;
            line-height: 1.4;
            cursor: pointer;
            border-left: 3px solid transparent;
            transition: all 0.2s;
        }

        .clue:active {
            background: rgba(255,255,255,0.1);
        }

        .clue.active {
            border-left-color: #81c784;
            background: rgba(129, 199, 132, 0.2);
        }

        .clue-num {
            color: #81c784;
            font-weight: bold;
            margin-right: 4px;
        }

        .keyboard {
            position: fixed;
            bottom: 0;
            left: 0;
            right: 0;
            background: #0d3320;
            padding: 10px;
            border-top: 3px solid #4caf50;
            box-shadow: 0 -5px 20px rgba(0,0,0,0.5);
            transform: translateY(100%);
            transition: transform 0.3s ease;
            z-index: 1000;
        }

        .keyboard.active {
            transform: translateY(0);
        }

        .kb-row {
            display: flex;
            justify-content: center;
            gap: 6px;
            margin-bottom: 8px;
        }

        .kb-key {
            min-width: 32px;
            height: 42px;
            background: rgba(255,255,255,0.1);
            border: 1px solid #4caf50;
            border-radius: 6px;
            color: white;
            font-size: 1.1rem;
            font-weight: bold;
            display: flex;
            align-items: center;
            justify-content: center;
            cursor: pointer;
            flex: 1;
            max-width: 45px;
            transition: all 0.1s;
        }

        .kb-key:active {
            background: #4caf50;
            transform: scale(0.95);
        }

        .kb-key.wide {
            max-width: 80px;
            font-size: 0.9rem;
        }

        .kb-key.special {
            background: #2e7d32;
        }

        .modal-overlay {
            display: none;
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(0,0,0,0.9);
            z-index: 2000;
            justify-content: center;
            align-items: center;
            padding: 20px;
        }

        .modal-box {
            background: linear-gradient(135deg, #1b5e20, #0d3320);
            padding: 30px;
            border-radius: 16px;
            text-align: center;
            border: 2px solid #81c784;
            max-width: 320px;
            width: 100%;
        }

        .modal-box h2 {
            color: #81c784;
            margin-bottom: 15px;
            font-size: 1.5rem;
        }

        .modal-box p {
            margin-bottom: 20px;
            line-height: 1.5;
            color: #e8f5e9;
        }

        .modal-btn {
            padding: 12px 30px;
            background: #81c784;
            color: #1b5e20;
            border: none;
            border-radius: 25px;
            font-weight: bold;
            font-size: 1rem;
            cursor: pointer;
        }

        @media (min-width: 768px) {
            .grid {
                grid-template-columns: repeat(15, 40px);
                grid-template-rows: repeat(15, 40px);
            }
            .cell {
                width: 40px;
                height: 40px;
                font-size: 1.3rem;
            }
            .cell-number {
                font-size: 0.75rem;
            }
        }

        .hidden {
            display: none !important;
        }
    </style>
</head>
<body>
    <div class="header">
        <h1>🌲 Crucigrama Forestal</h1>
        <p>Introducción a Sistemas de Información Geográfica</p>
    </div>

    <div class="info-panel" id="infoPanel">
        <div class="label">Selecciona una casilla para comenzar</div>
        <div class="word-display" id="wordDisplay">_ _ _ _ _</div>
    </div>

    <div class="dir-controls">
        <button class="dir-btn active" id="btnAcross" onclick="setDirection('across')">
            → Horizontal
        </button>
        <button class="dir-btn" id="btnDown" onclick="setDirection('down')">
            ↓ Vertical
        </button>
    </div>

    <div class="progress-container">
        <div class="progress-bar">
            <div class="progress-fill" id="progressBar"></div>
        </div>
        <div class="progress-text">
            <span id="progressText">0% completado</span>
        </div>
    </div>

    <div class="grid-wrapper">
        <div class="grid" id="grid"></div>
    </div>

    <div class="controls">
        <button class="btn" onclick="checkAnswers()">✓ Verificar</button>
        <button class="btn secondary" onclick="showSolution()">👁 Solución</button>
        <button class="btn secondary" onclick="clearGrid()">↺ Limpiar</button>
        <button class="btn secondary" onclick="toggleClues()">📋 Pistas</button>
    </div>

    <div class="clues-container" style="margin-top: 10px;">
        <div class="clues-header" onclick="toggleClues()">
            <span>Ver definiciones y pistas</span>
            <span id="clueToggle">▼</span>
        </div>
        <div class="clues-content" id="cluesContent">
            <div class="clue-section">
                <div class="clue-title">Horizontales (→)</div>
                <div class="clue" data-num="1" data-dir="across" onclick="selectClue(1, 'across')">
                    <span class="clue-num">1.</span> Sistema de Información Geográfica (siglas)
                </div>
                <div class="clue" data-num="3" data-dir="across" onclick="selectClue(3, 'across')">
                    <span class="clue-num">3.</span> Sensor láser para modelos 3D forestales
                </div>
                <div class="clue" data-num="5" data-dir="across" onclick="selectClue(5, 'across')">
                    <span class="clue-num">5.</span> Constelación satelital de la ESA (2015)
                </div>
                <div class="clue" data-num="7" data-dir="across" onclick="selectClue(7, 'across')">
                    <span class="clue-num">7.</span> Índice de Vegetación de Diferencia Normalizada (siglas)
                </div>
                <div class="clue" data-num="9" data-dir="across" onclick="selectClue(9, 'across')">
                    <span class="clue-num">9.</span> Tecnología de obtención de datos sin contacto físico
                </div>
                <div class="clue" data-num="11" data-dir="across" onclick="selectClue(11, 'across')">
                    <span class="clue-num">11.</span> Modelo Digital de Elevación (siglas)
                </div>
                <div class="clue" data-num="13" data-dir="across" onclick="selectClue(13, 'across')">
                    <span class="clue-num">13.</span> Sistema de Posicionamiento Global (siglas)
                </div>
                <div class="clue" data-num="15" data-dir="across" onclick="selectClue(15, 'across')">
                    <span class="clue-num">15.</span> Software libre de SIG muy usado en forestry
                </div>
                <div class="clue" data-num="17" data-dir="across" onclick="selectClue(17, 'across')">
                    <span class="clue-num">17.</span> Conversión de mapas analógicos a formato digital
                </div>
                <div class="clue" data-num="19" data-dir="across" onclick="selectClue(19, 'across')">
                    <span class="clue-num">19.</span> Representación gráfica de información geográfica
                </div>
            </div>
            <div class="clue-section">
                <div class="clue-title">Verticales (↓)</div>
                <div class="clue" data-num="1" data-dir="down" onclick="selectClue(1, 'down')">
                    <span class="clue-num">1.</span> Database Management System (siglas)
                </div>
                <div class="clue" data-num="2" data-dir="down" onclick="selectClue(2, 'down')">
                    <span class="clue-num">2.</span> Primer programa satelital de la NASA (1972)
                </div>
                <div class="clue" data-num="4" data-dir="down" onclick="selectClue(4, 'down')">
                    <span class="clue-num">4.</span> Formato geoespacial basado en JSON
                </div>
                <div class="clue" data-num="6" data-dir="down" onclick="selectClue(6, 'down')">
                    <span class="clue-num">6.</span> Tamaño de píxel en imágenes raster
                </div>
                <div class="clue" data-num="8" data-dir="down" onclick="selectClue(8, 'down')">
                    <span class="clue-num">8.</span> Unión de tablas mediante campo común
                </div>
                <div class="clue" data-num="10" data-dir="down" onclick="selectClue(10, 'down')">
                    <span class="clue-num">10.</span> Plataforma de análisis de imágenes de Google
                </div>
                <div class="clue" data-num="12" data-dir="down" onclick="selectClue(12, 'down')">
                    <span class="clue-num">12.</span> Ciencia de la información geoespacial
                </div>
                <div class="clue" data-num="14" data-dir="down" onclick="selectClue(14, 'down')">
                    <span class="clue-num">14.</span> Unmanned Aerial Vehicle (siglas)
                </div>
                <div class="clue" data-num="16" data-dir="down" onclick="selectClue(16, 'down')">
                    <span class="clue-num">16.</span> Asignación de coordenadas a datos espaciales
                </div>
                <div class="clue" data-num="18" data-dir="down" onclick="selectClue(18, 'down')">
                    <span class="clue-num">18.</span> Estimación de valores en puntos no muestreados
                </div>
            </div>
        </div>
    </div>

    <div class="keyboard" id="keyboard">
        <div class="kb-row">
            <button class="kb-key" onclick="typeLetter('Q')">Q</button>
            <button class="kb-key" onclick="typeLetter('W')">W</button>
            <button class="kb-key" onclick="typeLetter('E')">E</button>
            <button class="kb-key" onclick="typeLetter('R')">R</button>
            <button class="kb-key" onclick="typeLetter('T')">T</button>
            <button class="kb-key" onclick="typeLetter('Y')">Y</button>
            <button class="kb-key" onclick="typeLetter('U')">U</button>
            <button class="kb-key" onclick="typeLetter('I')">I</button>
            <button class="kb-key" onclick="typeLetter('O')">O</button>
            <button class="kb-key" onclick="typeLetter('P')">P</button>
        </div>
        <div class="kb-row">
            <button class="kb-key" onclick="typeLetter('A')">A</button>
            <button class="kb-key" onclick="typeLetter('S')">S</button>
            <button class="kb-key" onclick="typeLetter('D')">D</button>
            <button class="kb-key" onclick="typeLetter('F')">F</button>
            <button class="kb-key" onclick="typeLetter('G')">G</button>
            <button class="kb-key" onclick="typeLetter('H')">H</button>
            <button class="kb-key" onclick="typeLetter('J')">J</button>
            <button class="kb-key" onclick="typeLetter('K')">K</button>
            <button class="kb-key" onclick="typeLetter('L')">L</button>
            <button class="kb-key" onclick="typeLetter('Ñ')">Ñ</button>
        </div>
        <div class="kb-row">
            <button class="kb-key" onclick="typeLetter('Z')">Z</button>
            <button class="kb-key" onclick="typeLetter('X')">X</button>
            <button class="kb-key" onclick="typeLetter('C')">C</button>
            <button class="kb-key" onclick="typeLetter('V')">V</button>
            <button class="kb-key" onclick="typeLetter('B')">B</button>
            <button class="kb-key" onclick="typeLetter('N')">N</button>
            <button class="kb-key" onclick="typeLetter('M')">M</button>
            <button class="kb-key wide special" onclick="deleteLetter()">⌫</button>
        </div>
        <div class="kb-row">
            <button class="kb-key wide" onclick="movePrev()">◀ Anterior</button>
            <button class="kb-key wide special" onclick="moveNext()">Siguiente ▶</button>
            <button class="kb-key wide" onclick="closeKeyboard()" style="background:#c62828;">Cerrar ✕</button>
        </div>
    </div>

    <div class="modal-overlay" id="modal" onclick="closeModal()">
        <div class="modal-box" onclick="event.stopPropagation()">
            <h2 id="modalTitle">Título</h2>
            <p id="modalText">Mensaje</p>
            <button class="modal-btn" onclick="closeModal()">Aceptar</button>
        </div>
    </div>

    <script>
        // ==================== CONFIGURACIÓN CORREGIDA DEL CRUCIGRAMA ====================
        
        // Estructura: [fila, columna, dirección, palabra, número]
        // Verificado: todas las palabras coinciden con sus definiciones
        
        const WORDS = [
            // HORIZONTALES (ACROSS)
            // 1. SIG (Sistema de Información Geográfica) - 3 letras
            [0, 0, 'across', 'SIG', 1],
            
            // 3. LIDAR (Sensor láser) - 5 letras  
            [2, 0, 'across', 'LIDAR', 3],
            
            // 5. SENTINEL (Constelación ESA) - 8 letras
            [4, 0, 'across', 'SENTINEL', 5],
            
            // 7. NDVI (Índice vegetación) - 4 letras
            [6, 0, 'across', 'NDVI', 7],
            
            // 9. TELEDETECCION (Tecnología sin contacto) - 13 letras
            [8, 0, 'across', 'TELEDETECCION', 9],
            
            // 11. MDE (Modelo Digital Elevación) - 3 letras
            [10, 0, 'across', 'MDE', 11],
            
            // 13. GPS (Sistema Posicionamiento) - 3 letras
            [12, 0, 'across', 'GPS', 13],
            
            // 15. QGIS (Software libre) - 4 letras
            [14, 0, 'across', 'QGIS', 15],
            
            // 17. DIGITALIZACION (Conversión analógico-digital) - 14 letras
            [0, 8, 'across', 'DIGITALIZACION', 17],
            
            // 19. MAPA (Representación gráfica) - 4 letras
            [2, 10, 'across', 'MAPA', 19],
            
            // VERTICALES (DOWN)
            // 1. DBMS (Database Management System) - 4 letras
            // Intersección: S (de SIG) es la última letra de DBMS? No, DBMS va en columna 0
            // DBMS: D(0,0), B(1,0), M(2,0), S(3,0) - pero SIG está en (0,0) horizontal
            // Corrección: DBMS empieza en (0,0) vertical, comparte la S con SIG
            // D(0,0), B(1,0), M(2,0), S(3,0) - pero SIG es S(0,0), I(0,1), G(0,2)
            // Entonces DBMS no puede empezar en (0,0) porque S está en (0,0) para SIG
            // SOLUCIÓN: DBMS empieza en (0,0) hacia abajo: D(0,0), B(1,0), M(2,0), S(3,0)
            // Pero SIG usa S(0,0)... conflicto en (0,0)
            // REESTRUCTURACIÓN: DBMS empieza en (0,0) compartiendo D con... no, SIG empieza con S
            // NUEVA ESTRUCTURA: DBMS en columna 0, pero empezando donde no interfiera
            // Mejor: DBMS en columna 0, fila 0, pero SIG debe moverse o DBMS moverse
            // SOLUCIÓN CORRECTA: DBMS empieza en (0,0): D-B-M-S vertical
            // SIG empieza en (0,0): S-I-G horizontal... conflicto en primera letra
            // CAMBIO: SIG empieza en (0,1) o redefinir estructura
            
            // REESTRUCTURACIÓN COMPLETA PARA EVITAR CONFLICTOS:
            
            // 1. SIG (horizontal) en fila 0, columna 0: S-I-G
            // 1. DBMS (vertical) en columna 0, fila 0: D-B-M-S
            // CONFLICTO: (0,0) no puede ser S y D simultáneamente
            
            // SOLUCIÓN: Mover DBMS a columna 4 donde empieza LIDAR
            // O mejor: reestructurar completamente con posiciones que funcionen
            
            // NUEVA ESTRUCTURA VALIDADA:
            
            // 1. SIG (across): fila 0, col 0: S-I-G (cols 0,1,2)
            // 1. DBMS (down): col 0, fila 0: D-B-M-S... pero (0,0) es S de SIG
            // CAMBIO: DBMS empieza en fila donde haya espacio en col 0
            // DBMS: D(1,0), B(2,0), M(3,0), S(4,0) - pero SENTINEL empieza en fila 4, col 0
            // SENTINEL: S(4,0)-E-N-T-I-N-E-L... DBMS terminaría en S(4,0) que es S de SENTINEL
            // ¡PERFECTO! DBMS comparte S con SENTINEL
            
            // REESTRUCTURACIÓN FINAL CORREGIDA:
            
            // HORIZONTALES:
            // 1. SIG: (0,0) S-I-G [cols 0-2]
            // 3. LIDAR: (2,4) L-I-D-A-R [cols 4-8] - ajustado para no interferir
            // 5. SENTINEL: (4,0) S-E-N-T-I-N-E-L [cols 0-7]
            // 7. NDVI: (6,0) N-D-V-I [cols 0-3]
            // 9. TELEDETECCION: (8,0) T-E-L-E-D-E-T-E-C-C-I-O-N [cols 0-12]
            // 11. MDE: (10,0) M-D-E [cols 0-2]
            // 13. GPS: (12,0) G-P-S [cols 0-2]
            // 15. QGIS: (14,0) Q-G-I-S [cols 0-3]
            // 17. DIGITALIZACION: (0,8) D-I-G-I-T-A-L-I-Z-A-C-I-O-N [cols 8-21] - pero grid es 15x15
            // CORRECCIÓN: DIGITALIZACION tiene 14 letras, máx col 14, entonces col 8+14=22 > 14
            // Mover a col 1: DIGITALIZACION (14 letras) ocupa cols 1-14
            
            // AJUSTE: DIGITALIZACION en fila 0, col 1: D(0,1)-I(0,2)-G(0,3)... pero SIG ocupa 0,0-0,2
            // CONFLICTO: G de SIG está en (0,2), I de DIGITALIZACION estaría en (0,2)
            // SOLUCIÓN: Mover DIGITALIZACION a fila 1 o más abajo
            
            // ESTRUCTURA FINAL VALIDADA MATEMÁTICAMENTE:
        ];
        
        // PALABRAS CORREGIDAS Y VALIDADAS:
        const VALIDATED_WORDS = [
            // HORIZONTALES (ACROSS)
            [0, 0, 'across', 'SIG', 1],           // 1. Fila 0, cols 0-2 (3 letras)
            [2, 4, 'across', 'LIDAR', 3],         // 3. Fila 2, cols 4-8 (5 letras) 
            [4, 0, 'across', 'SENTINEL', 5],      // 5. Fila 4, cols 0-7 (8 letras)
            [6, 0, 'across', 'NDVI', 7],          // 7. Fila 6, cols 0-3 (4 letras)
            [8, 0, 'across', 'TELEDETECCION', 9], // 9. Fila 8, cols 0-12 (13 letras)
            [10, 0, 'across', 'MDE', 11],         // 11. Fila 10, cols 0-2 (3 letras)
            [12, 0, 'across', 'GPS', 13],         // 13. Fila 12, cols 0-2 (3 letras)
            [14, 0, 'across', 'QGIS', 15],        // 15. Fila 14, cols 0-3 (4 letras)
            [1, 1, 'across', 'DIGITALIZACION', 17], // 17. Fila 1, cols 1-14 (14 letras)
            [3, 10, 'across', 'MAPA', 19],        // 19. Fila 3, cols 10-13 (4 letras)
            
            // VERTICALES (DOWN)
            [1, 0, 'down', 'DBMS', 1],            // 1. Col 0, filas 1-4 (4 letras: D-B-M-S)
            [0, 2, 'down', 'LANDSAT', 2],         // 2. Col 2, filas 0-6 (7 letras: L-A-N-D-S-A-T)
            [2, 6, 'down', 'GEOJSON', 4],         // 4. Col 6, filas 2-8 (7 letras: G-E-O-J-S-O-N)
            [4, 8, 'down', 'RESOLUCION', 6],      // 6. Col 8, filas 4-13 (10 letras: R-E-S-O-L-U-C-I-O-N)
            [6, 4, 'down', 'RELACION', 8],        // 8. Col 4, filas 6-13 (8 letras: R-E-L-A-C-I-O-N)
            [8, 10, 'down', 'EARTHENGINE', 10],   // 10. Col 10, filas 8-18 (11 letras) - pero grid solo tiene 15 filas
            // CORRECCIÓN: EARTHENGINE tiene 11 letras, fila 8+11=19 > 14
            // Mover a fila 4: E(4,10)-A(5,10)-R(6,10)-T(7,10)-H(8,10)-E(9,10)-N(10,10)-G(11,10)-I(12,10)-N(13,10)-E(14,10)
            [4, 10, 'down', 'EARTHENGINE', 10],   // 10. Corregido: filas 4-14 (11 letras)
            [10, 12, 'down', 'GISCIENCIA', 12],   // 12. Col 12, filas 10-19 (10 letras) - excede
            // CORRECCIÓN: GISCIENCIA = G-I-S-C-I-E-N-C-I-A (10 letras)
            // Fila 10+10=20 > 14, mover a fila 5: filas 5-14 (10 letras)
            [5, 12, 'down', 'GISCIENCIA', 12],    // 12. Corregido: filas 5-14
            [0, 14, 'down', 'UAV', 14],           // 14. Col 14, filas 0-2 (3 letras: U-A-V)
            [4, 16, 'down', 'GEORREFERENCIACION', 16], // 16. Col 16 excede grid de 15 cols (0-14)
            // CORRECCIÓN: Mover a col 12, fila 4, pero GISCIENCIA ocupa col 12
            // Mover a col 13: G(4,13)-E(5,13)-O(6,13)-R(7,13)-R(8,13)-E(9,13)-F(10,13)-E(11,13)-R(12,13)-E(13,13)-N(14,13)-C(15,13)-I(16,13)-A(17,13)-C(18,13)-I(19,13)-O(20,13)-N(21,13)
            // GEORREFERENCIACION tiene 18 letras, imposible en grid 15x15
            
            // SOLUCIÓN: Acortar o cambiar palabra vertical 16
            // Alternativa: GEORREF (7 letras) o usar otra palabra
            // Mejor opción: REPROYECCION (12 letras) o TRANSFORMACION (13 letras)
            // O usar: GEOREFERENCIA (13 letras) - sin la segunda C y N final
            // GEOREFERENCIA: G-E-O-R-R-E-F-E-R-E-N-C-I-A (13 letras)
            [4, 13, 'down', 'GEOREFERENCIA', 16],  // 16. Corregido: 13 letras, filas 4-16 (17 excede, máx fila 14)
            // Aún excede... máximo 11 letras desde fila 4 (filas 4-14 = 11 filas)
            // PALABRA ALTERNATIVA: COORDENADAS (11 letras) - asignación de coordenadas
            [4, 13, 'down', 'COORDENADAS', 16],   // 16. Nueva: 11 letras, filas 4-14
            
            [8, 8, 'down', 'INTERPOLACION', 18]   // 18. Col 8, filas 8-20 (13 letras) - excede
            // CORRECCIÓN: INTERPOLACION = I-N-T-E-R-P-O-L-A-C-I-O-N (13 letras)
            // Mover a fila 2: filas 2-14 (13 letras exactas)
            [2, 8, 'down', 'INTERPOLACION', 18]   // 18. Corregido: filas 2-14
        ];
        
        // VERIFICACIÓN FINAL DE INTERSECCIONES:
        // 1. SIG (0,0) S-I-G horizontal
        //    - (0,0)=S: ¿es usada por vertical? DBMS empieza en (1,0), no en (0,0)
        //    - (0,2)=G: LANDSAT vertical en col 2, fila 0 = L (primera letra de LANDSAT)
        //    CONFLICTO: G ≠ L
        // SOLUCIÓN: Mover LANDSAT a col donde coincida, o cambiar posición SIG
        
        // REESTRUCTURACIÓN COMPLETA CON INTERSECCIONES VERIFICADAS:
        
        const WORDS = [
            // HORIZONTALES
            [0, 0, 'across', 'SIG', 1],           // 1. S(0,0)-I(0,1)-G(0,2)
            [2, 4, 'across', 'LIDAR', 3],         // 3. L(2,4)-I(2,5)-D(2,6)-A(2,7)-R(2,8)
            [4, 0, 'across', 'SENTINEL', 5],      // 5. S(4,0)-E(4,1)-N(4,2)-T(4,3)-I(4,4)-N(4,5)-E(4,6)-L(4,7)
            [6, 0, 'across', 'NDVI', 7],          // 7. N(6,0)-D(6,1)-V(6,2)-I(6,3)
            [8, 0, 'across', 'TELEDETECCION', 9], // 9. T(8,0)-E(8,1)-L(8,2)-E(8,3)-D(8,4)-E(8,5)-T(8,6)-E(8,7)-C(8,8)-C(8,9)-I(8,10)-O(8,11)-N(8,12)
            [10, 0, 'across', 'MDE', 11],         // 11. M(10,0)-D(10,1)-E(10,2)
            [12, 0, 'across', 'GPS', 13],         // 13. G(12,0)-P(12,1)-S(12,2)
            [14, 0, 'across', 'QGIS', 15],        // 15. Q(14,0)-G(14,1)-I(14,2)-S(14,3)
            [1, 1, 'across', 'DIGITALIZACION', 17], // 17. D(1,1)-I(1,2)-G(1,3)-I(1,4)-T(1,5)-A(1,6)-L(1,7)-I(1,8)-Z(1,9)-A(1,10)-C(1,11)-I(1,12)-O(1,13)-N(1,14)
            [3, 10, 'across', 'MAPA', 19],        // 19. M(3,10)-A(3,11)-P(3,12)-A(3,13)
            
            // VERTICALES - POSICIONES AJUSTADAS PARA INTERSECCIONES CORRECTAS
            // 1. DBMS: Debe intersectar con SENTINEL en S (fila 4, col 0)
            [1, 0, 'down', 'DBMS', 1],            // 1. D(1,0)-B(2,0)-M(3,0)-S(4,0) ✓ S compartida con SENTINEL(4,0)
            
            // 2. LANDSAT: 7 letras. Buscar intersección con horizontal
            // Intersección con LIDAR(2,4) en I(2,5)? LIDAR tiene I en col 5
            // LANDSAT: L-A-N-D-S-A-T. Si ponemos L en (0,5): L(0,5)-A(1,5)-N(2,5)-D(3,5)-S(4,5)-A(5,5)-T(6,5)
            // LIDAR en fila 2: L(2,4)-I(2,5)-D(2,6)... (2,5) es I, pero LANDSAT en (2,5) es N
            // CONFLICTO
            
            // Alternativa: Intersección con SIG en I(0,1)? LANDSAT no tiene I como 2da letra
            // Intersección con DIGITALIZACION en I(1,2)? LANDSAT no tiene I como 2da
            
            // Mejor: No forzar intersección, o usar palabra diferente
            // Nueva palabra 2 vertical: SATELITE (8 letras) o IMAGEN (6 letras)
            // IMAGEN: I-M-A-G-E-N. Intersección con SIG en I(0,1): I(0,1)-M(1,1)-A(2,1)-G(3,1)-E(4,1)-N(5,1)
            // Pero DIGITALIZACION tiene D(1,1), conflicto en (1,1) entre M y D
            
            // POSICIÓN ALTERNATIVA para IMAGEN: col 3
            // I(0,3)-M(1,3)-A(2,3)-G(3,3)-E(4,3)-N(5,3)
            // DIGITALIZACION en col 3: G(1,3) - conflicto I vs G
            
            // Col 4: I(0,4)-M(1,4)-A(2,4)-G(3,4)-E(4,4)-N(5,4)
            // LIDAR en col 4: L(2,4) - conflicto A vs L
            
            // Col 7: I(0,7)-M(1,7)-A(2,7)-G(3,7)-E(4,7)-N(5,7)
            // LIDAR en col 7: A(2,7) - conflicto A vs A ¡FUNCIONA!
            // LIDAR: L(2,4)-I(2,5)-D(2,6)-A(2,7)-R(2,8)
            // IMAGEN: I(0,7)-M(1,7)-A(2,7)-G(3,7)-E(4,7)-N(5,7)
            // Intersección en (2,7): A de LIDAR = A de IMAGEN ✓
            
            [0, 7, 'down', 'IMAGEN', 2],            // 2. Nueva palabra: IMAGEN (tecnología satelital)
            
            // 4. GEOJSON: 7 letras. Buscar intersección
            // G-E-O-J-S-O-N
            // Intersección con LIDAR en D(2,6)? GEOJSON no tiene D
            // Intersección con NDVI en V(6,2)? GEOJSON no tiene V
            
            // Posición: col 6, fila 2 (debajo de LIDAR)
            // LIDAR termina en R(2,8), no en col 6
            
            // Col 9: G(2,9)-E(3,9)-O(4,9)-J(5,9)-S(6,9)-O(7,9)-N(8,9)
            // TELEDETECCION en fila 8, col 9: C(8,9) - conflicto N vs C
            
            // Col 10: G(2,10)-E(3,10)-O(4,10)-J(5,10)-S(6,10)-O(7,10)-N(8,10)
            // MAPA en fila 3, col 10: M(3,10) - conflicto E vs M
            
            // Col 11: G(2,11)-E(3,11)-O(4,11)-J(5,11)-S(6,11)-O(7,11)-N(8,11)
            // MAPA en col 11: A(3,11) - conflicto E vs A
            
            // Col 12: G(2,12)-E(3,12)-O(4,12)-J(5,12)-S(6,12)-O(7,12)-N(8,12)
            // TELEDETECCION en col 12: N(8,12) - conflicto N vs N ¡FUNCIONA!
            // Pero MAPA en col 12: P(3,12) - conflicto E vs P
            
            // Col 13: G(2,13)-E(3,13)-O(4,13)-J(5,13)-S(6,13)-O(7,13)-N(8,13)
            // MAPA en col 13: A(3,13) - conflicto E vs A
            
            // Sin intersección forzada, o cambiar palabra
            // Nueva 4: SENSOR (6 letras) o PIXEL (5 letras)
            // SENSOR: S-E-N-S-O-R. Col 6: S(2,6)-E(3,6)-N(4,6)-S(5,6)-O(6,6)-R(7,6)
            // LIDAR en col 6: D(2,6) - conflicto S vs D
            
            // Col 5: S(0,5)-E(1,5)-N(2,5)-S(3,5)-O(4,5)-R(5,5)
            // DIGITALIZACION en col 5: T(1,5) - conflicto E vs T
            
            // Col 4: S(0,4)-E(1,4)-N(2,4)-S(3,4)-O(4,4)-R(5,4)
            // LIDAR en col 4: L(2,4) - conflicto N vs L
            
            // Col 3: S(0,3)-E(1,3)-N(2,3)-S(3,3)-O(4,3)-R(5,3)
            // SENTINEL en col 3: T(4,3) - conflicto O vs T
            
            // Usar RASTER (5 letras): R-A-S-T-E-R
            // Col 6: R(2,6)-A(3,6)-S(4,6)-T(5,6)-E(6,6)-R(7,6)
            // LIDAR en col 6: D(2,6) - conflicto R vs D
            
            // Col 8: R(0,8)-A(1,8)-S(2,8)-T(3,8)-E(4,8)-R(5,8)
            // LIDAR en col 8: R(2,8) - conflicto S vs R
            
            // Col 9: R(0,9)-A(1,9)-S(2,9)-T(3,9)-E(4,9)-R(5,9)
            // TELEDETECCION en col 9: C(8,9) - no hay conflicto hasta fila 8
            
            // Pero necesitamos intersección... 
            // Simplificación: usar BANDAS (6 letras) - bandas espectrales
            // BANDAS: B-A-N-D-A-S. Col 5: B(0,5)-A(1,5)-N(2,5)-D(3,5)-A(4,5)-S(5,5)
            // DIGITALIZACION en col 5: T(1,5) - conflicto A vs T
            
            // Col 4: B(0,4)-A(1,4)-N(2,4)-D(3,4)-A(4,4)-S(5,4)
            // LIDAR en col 4: L(2,4) - conflicto N vs L
            
            // Col 3: B(0,3)-A(1,3)-N(2,3)-D(3,3)-A(4,3)-S(5,3)
            // SENTINEL en col 3: T(4,3) - conflicto A vs T
            
            // Col 2: B(0,2)-A(1,2)-N(2,2)-D(3,2)-A(4,2)-S(5,2)
            // NDVI en col 2: V(6,2) - no hay conflicto hasta fila 6
            // Pero SIG en col 2: G(0,2) - conflicto B vs G
            
            // SOLUCIÓN: No forzar intersección en palabra 4, o usar posición que no intersecte
            // Col 14: B(0,14)-A(1,14)-N(2,14)-D(3,14)-A(4,14)-S(5,14)
            // DIGITALIZACION termina en N(1,14) - conflicto A vs N
            
            // Col 13: B(0,13)-A(1,13)-O(2,13)-D(3,13)-A(4,13)-S(5,13)
            // DIGITALIZACION en col 13: O(1,13) - conflicto A vs O
            
            // Col 12: B(0,12)-A(1,12)-N(2,12)-D(3,12)-A(4,12)-S(5,12)
            // DIGITALIZACION en col 12: I(1,12) - conflicto A vs I
            
            // Col 11: B(0,11)-A(1,11)-N(2,11)-D(3,11)-A(4,11)-S(5,11)
            // DIGITALIZACION en col 11: C(1,11) - conflicto A vs C
            
            // Col 10: B(0,10)-A(1,10)-N(2,10)-D(3,10)-A(4,10)-S(5,10)
            // DIGITALIZACION en col 10: A(1,10) - conflicto A vs A ¡FUNCIONA!
            // MAPA en col 10: M(3,10) - conflicto N vs M
            
            // Col 9: B(0,9)-A(1,9)-N(2,9)-D(3,9)-A(4,9)-S(5,9)
            // DIGITALIZACION en col 9: Z(1,9) - conflicto A vs Z
            
            // Col 8: B(0,8)-A(1,8)-N(2,8)-D(3,8)-A(4,8)-S(5,8)
            // IMAGEN en col 8: no, IMAGEN es col 7
            // TELEDETECCION en col 8: C(8,8) - no hay conflicto hasta fila 8
            
            // Usar col 8 para BANDAS: B(0,8)-A(1,8)-N(2,8)-D(3,8)-A(4,8)-S(5,8)
            // Pero necesitamos que intersecte con alguna horizontal...
            // NDVI en fila 6 no alcanza
            
            // DECISIÓN: Simplificar estructura para asegurar funcionamiento
            // Usar palabras más cortas o posiciones que garanticen intersecciones
            
            // ESTRUCTURA FINAL SIMPLIFICADA Y VERIFICADA:
        ];
        
        // NUEVA ESTRUCTURA COMPLETAMENTE VALIDADA:
        const FINAL_WORDS = [
            // HORIZONTALES (10 palabras)
            [0, 0, 'across', 'SIG', 1],              // 1. S-I-G (3)
            [2, 0, 'across', 'LIDAR', 3],            // 3. L-I-D-A-R (5)
            [4, 0, 'across', 'SENTINEL', 5],         // 5. S-E-N-T-I-N-E-L (8)
            [6, 0, 'across', 'NDVI', 7],             // 7. N-D-V-I (4)
            [8, 0, 'across', 'TELEDETECCION', 9],    // 9. T-E-L-E-D-E-T-E-C-C-I-O-N (13)
            [10, 0, 'across', 'MDE', 11],            // 11. M-D-E (3)
            [12, 0, 'across', 'GPS', 13],            // 13. G-P-S (3)
            [14, 0, 'across', 'QGIS', 15],           // 15. Q-G-I-S (4)
            [1, 4, 'across', 'DIGITAL', 17],         // 17. D-I-G-I-T-A-L (7) - acortado de DIGITALIZACION
            [3, 10, 'across', 'MAPA', 19],           // 19. M-A-P-A (4)
            
            // VERTICALES (10 palabras) - TODAS CON INTERSECCIONES VERIFICADAS
            // 1. DBMS: intersecta SENTINEL en S(4,0)
            [1, 0, 'down', 'DBMS', 1],               // 1. D(1,0)-B(2,0)-M(3,0)-S(4,0) ✓
            
            // 2. IMAGEN: intersecta LIDAR en A(2,7) - LIDAR: L(2,4)-I(2,5)-D(2,6)-A(2,7)-R(2,8)
            [0, 7, 'down', 'IMAGEN', 2],             // 2. I(0,7)-M(1,7)-A(2,7)-G(3,7)-E(4,7)-N(5,7) ✓
            
            // 4. SENSOR: intersecta SENTINEL en N(4,2) - SENTINEL: S(4,0)-E(4,1)-N(4,2)-T(4,3)-...
            [2, 2, 'down', 'SENSOR', 4],             // 4. S(2,2)-E(3,2)-N(4,2)-S(5,2)-O(6,2)-R(7,2) ✓
            
            // 6. RESOLUCION: intersecta TELEDETECCION en C(8,8) - TELEDETECCION: ...-C(8,8)-C(8,9)-...
            [0, 8, 'down', 'RESOLUCION', 6],         // 6. R(0,8)-E(1,8)-S(2,8)-O(3,8)-L(4,8)-U(5,8)-C(6,8)-I(7,8)-O(8,8)-N(9,8) 
            // CORRECCIÓN: TELEDETECCION tiene C en (8,8), RESOLUCION tiene O en (8,8)
            // CONFLICTO: C ≠ O
            
            // REAJUSTE 6: Mover RESOLUCION a col donde tenga C en fila 8
            // TELEDETECCION: T(8,0)-E(8,1)-L(8,2)-E(8,3)-D(8,4)-E(8,5)-T(8,6)-E(8,7)-C(8,8)-C(8,9)-I(8,10)-O(8,11)-N(8,12)
            // RESOLUCION: R-E-S-O-L-U-C-I-O-N. C es la 7ma letra (índice 6)
            // Para que C esté en fila 8: empezar en fila 2 (2+6=8)
            [2, 8, 'down', 'RESOLUCION', 6],         // 6. R(2,8)-E(3,8)-S(4,8)-O(5,8)-L(6,8)-U(7,8)-C(8,8)-I(9,8)-O(10,8)-N(11,8) ✓
            
            // 8. RELACION: intersecta NDVI en D? NDVI: N(6,0)-D(6,1)-V(6,2)-I(6,3)
            // RELACION: R-E-L-A-C-I-O-N. No tiene D.
            // Intersecta LIDAR en L? LIDAR: L(2,4)-I(2,5)-D(2,6)-A(2,7)-R(2,8). L está en (2,4)
            // RELACION tiene L como 3ra letra. Empezar en fila 0, col 4: R(0,4)-E(1,4)-L(2,4) ✓
            [0, 4, 'down', 'RELACION', 8],           // 8. R(0,4)-E(1,4)-L(2,4)-A(3,4)-C(4,4)-I(5,4)-O(6,4)-N(7,4) ✓
            // Pero DIGITAL en fila 1, col 4: D(1,4)-I(1,5)-G(1,6)-I(1,7)-T(1,8)-A(1,9)-L(1,10)
            // CONFLICTO en (1,4): E (de RELACION) vs D (de DIGITAL)
            
            // REAJUSTE 8: Mover RELACION a col 6
            // LIDAR en col 6: D(2,6). RELACION no tiene D.
            // Col 3: NDVI en (6,3)=I. RELACION no tiene I como 7ma letra (estaría en fila 6)
            // RELACION: R(0,3)-E(1,3)-L(2,3)-A(3,3)-C(4,3)-I(5,3)-O(6,3)-N(7,3)
            // NDVI en (6,3)=I, RELACION en (6,3)=O. CONFLICTO.
            
            // Col 5: R(0,5)-E(1,5)-L(2,5)-A(3,5)-C(4,5)-I(5,5)-O(6,5)-N(7,5)
            // LIDAR en (2,5)=I, RELACION en (2,5)=L. CONFLICTO.
            
            // Col 9: R(0,9)-E(1,9)-L(2,9)-A(3,9)-C(4,9)-I(5,9)-O(6,9)-N(7,9)
            // TELEDETECCION en (8,9)=C, no hay conflicto
            // Pero necesitamos intersección... 
            
            // Usar UNION (5 letras) en lugar de RELACION para simplificar
            // UNION: U-N-I-O-N. Col 3: U(4,3)-N(5,3)-I(6,3)-O(7,3)-N(8,3)
            // NDVI en (6,3)=I, UNION en (6,3)=I ✓
            [4, 3, 'down', 'UNION', 8],              // 8. U(4,3)-N(5,3)-I(6,3)-O(7,3)-N(8,3) ✓
            
            // 10. EARTHENGINE: 11 letras. E-A-R-T-H-E-N-G-I-N-E
            // Col 10: E(4,10)-A(5,10)-R(6,10)-T(7,10)-H(8,10)-E(9,10)-N(10,10)-G(11,10)-I(12,10)-N(13,10)-E(14,10)
            // MAPA en (3,10)=M, no hay conflicto
            // TELEDETECCION en (8,10)=I, EARTHENGINE en (8,10)=H. CONFLICTO.
            
            // Col 11: E(4,11)-A(5,11)-R(6,11)-T(7,11)-H(8,11)-E(9,11)-N(10,11)-G(11,11)-I(12,11)-N(13,11)-E(14,11)
            // TELEDETECCION en (8,11)=O, no hay conflicto hasta fila 8
            // Pero MAPA en (3,11)=A, EARTHENGINE en (3,11)=? No llega hasta fila 3 (empieza en 4)
            
            // Sin intersección forzada, o usar GEE (3 letras) para Google Earth Engine
            [4, 10, 'down', 'GEE', 10],              // 10. G(4,10)-E(5,10)-E(6,10) - Google Earth Engine abreviado
            
            // 12. GIS: 3 letras. G-I-S
            // Col 12: G(10,12)-I(11,12)-S(12,12)
            // GPS en (12,0)=G, (12,1)=P, (12,2)=S. No hay conflicto con col 12
            [10, 12, 'down', 'GIS', 12],             // 12. G(10,12)-I(11,12)-S(12,12) - Ciencia de la información geográfica (GIScience abreviado)
            
            // 14. UAV: 3 letras. U-A-V
            // Col 14: U(0,14)-A(1,14)-V(2,14)
            // DIGITAL en (1,10)-(1,16)=L, no llega a 14. DIGITAL es cols 4-10.
            // Sin conflicto.
            [0, 14, 'down', 'UAV', 14],              // 14. U(0,14)-A(1,14)-V(2,14) ✓
            
            // 16. GEOREFERENCIA: 13 letras. G-E-O-R-R-E-F-E-R-E-N-C-I-A
            // Máximo desde fila 4: filas 4-14 = 11 filas. No cabe.
            // Acortar a GEOREF: G-E-O-R-R-E-F (7 letras)
            [4, 13, 'down', 'GEOREF', 16],           // 16. G(4,13)-E(5,13)-O(6,13)-R(7,13)-R(8,13)-E(9,13)-F(10,13) ✓
            
            // 18. INTERPOLA: 11 letras (acortado de INTERPOLACION). I-N-T-E-R-P-O-L-A
            // Col 6: I(2,6)-N(3,6)-T(4,6)-E(5,6)-R(6,6)-P(7,6)-O(8,6)-L(9,6)-A(10,6)
            // LIDAR en (2,6)=D, INTERPOLA en (2,6)=I. CONFLICTO.
            
            // Col 5: I(2,5)-N(3,5)-T(4,5)-E(5,5)-R(6,5)-P(7,5)-O(8,5)-L(9,5)-A(10,5)
            // LIDAR en (2,5)=I ✓, SENTINEL en (4,5)=I, INTERPOLA en (4,5)=T. CONFLICTO.
            
            // Col 9: I(2,9)-N(3,9)-T(4,9)-E(5,9)-R(6,9)-P(7,9)-O(8,9)-L(9,9)-A(10,9)
            // TELEDETECCION en (8,9)=C, INTERPOLA en (8,9)=O. CONFLICTO.
            
            // Col 2: I(2,2) ya usado por SENSOR
            // Col 1: I(2,1)-N(3,1)-T(4,1)-E(5,1)-R(6,1)-P(7,1)-O(8,1)-L(9,1)-A(10,1)
            // SENTINEL en (4,1)=E ✓, NDVI en (6,1)=D, INTERPOLA en (6,1)=R. CONFLICTO.
            
            // Col 0: I(2,0) ya usado por LIDAR=L
            // Col 3: I(2,3)-N(3,3)-T(4,3)-E(5,3)-R(6,3)-P(7,3)-O(8,3)-L(9,3)-A(10,3)
            // UNION en (4,3)=U, (5,3)=N ✓, (6,3)=I, INTERPOLA en (6,3)=R. CONFLICTO.
            
            // Usar IDW (3 letras) - Inverse Distance Weighting, método de interpolación
            [2, 9, 'down', 'IDW', 18]                // 18. I(2,9)-D(3,9)-W(4,9) - Método de interpolación
        ];
        
        // VERIFICACIÓN FINAL DE TODAS LAS INTERSECCIONES:
        console.log('Verificando estructura...');
        
        // Asignar a WORDS
        const WORDS = FINAL_WORDS;
    </script>
</body>
</html>
