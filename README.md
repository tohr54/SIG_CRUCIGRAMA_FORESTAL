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
        // ==================== CONFIGURACIÓN DEL CRUCIGRAMA ====================
        
        // Definición de palabras: [fila, columna, dirección, palabra, número]
        const WORDS = [
            // Horizontales (ACROSS)
            [0, 0, 'across', 'SIG', 1],
            [2, 0, 'across', 'LIDAR', 3],
            [4, 0, 'across', 'SENTINEL', 5],
            [6, 0, 'across', 'NDVI', 7],
            [8, 0, 'across', 'TELEDETECCION', 9],
            [10, 0, 'across', 'MDE', 11],
            [12, 0, 'across', 'GPS', 13],
            [14, 0, 'across', 'QGIS', 15],
            [0, 8, 'across', 'DIGITALIZACION', 17],
            [2, 10, 'across', 'MAPA', 19],
            
            // Verticales (DOWN)
            [0, 0, 'down', 'DBMS', 1],
            [0, 2, 'down', 'LANDSAT', 2],
            [2, 4, 'down', 'GEOJSON', 4],
            [4, 6, 'down', 'RESOLUCION', 6],
            [6, 8, 'down', 'RELACION', 8],
            [8, 10, 'down', 'EARTHENGINE', 10],
            [10, 12, 'down', 'GISCIENCIA', 12],
            [0, 14, 'down', 'UAV', 14],
            [4, 16, 'down', 'GEORREFERENCIACION', 16],
            [8, 18, 'down', 'INTERPOLACION', 18]
        ];

        // ==================== ESTADO GLOBAL ====================
        
        let gridData = []; // Información de cada celda
        let userAnswers = []; // Respuestas del usuario
        let selectedCell = null; // Celda seleccionada {r, c}
        let currentDirection = 'across'; // Dirección actual
        let currentWord = null; // Palabra activa

        // ==================== INICIALIZACIÓN ====================
        
        function init() {
            console.log('Inicializando crucigrama...');
            createGridStructure();
            renderGrid();
            updateProgress();
            console.log('Crucigrama listo');
        }

        // Crear estructura de datos del grid
        function createGridStructure() {
            // Inicializar matrices vacías
            gridData = Array(15).fill(null).map(() => Array(15).fill(null));
            userAnswers = Array(15).fill(null).map(() => Array(15).fill(''));
            
            // Procesar cada palabra
            WORDS.forEach(([startRow, startCol, direction, word, number]) => {
                for (let i = 0; i < word.length; i++) {
                    let row = direction === 'across' ? startRow : startRow + i;
                    let col = direction === 'across' ? startCol + i : startCol;
                    
                    // Verificar límites
                    if (row >= 15 || col >= 15) {
                        console.error(`Palabra ${number} fuera de límites en ${row},${col}`);
                        continue;
                    }
                    
                    // Crear o actualizar celda
                    if (!gridData[row][col]) {
                        gridData[row][col] = {
                            isActive: true,
                            numbers: [],
                            correctChar: word[i],
                            acrossWord: null,
                            downWord: null
                        };
                    }
                    
                    // Asignar número si es primera letra
                    if (i === 0) {
                        gridData[row][col].numbers.push(number);
                    }
                    
                    // Asignar referencia a palabra
                    if (direction === 'across') {
                        gridData[row][col].acrossWord = {
                            startRow, startCol, word, number, index: i
                        };
                    } else {
                        gridData[row][col].downWord = {
                            startRow, startCol, word, number, index: i
                        };
                    }
                }
            });
        }

        // Renderizar el grid visual
        function renderGrid() {
            const gridEl = document.getElementById('grid');
            gridEl.innerHTML = '';
            
            for (let r = 0; r < 15; r++) {
                for (let c = 0; c < 15; c++) {
                    const cell = document.createElement('div');
                    cell.className = 'cell';
                    cell.dataset.row = r;
                    cell.dataset.col = c;
                    
                    const data = gridData[r][c];
                    
                    if (data && data.isActive) {
                        // Agregar número si existe
                        if (data.numbers.length > 0) {
                            const numSpan = document.createElement('span');
                            numSpan.className = 'cell-number';
                            numSpan.textContent = data.numbers.join(',');
                            cell.appendChild(numSpan);
                        }
                        
                        // Agregar letra si existe respuesta
                        if (userAnswers[r][c]) {
                            cell.textContent = userAnswers[r][c];
                            // Re-agregar número si existe
                            if (data.numbers.length > 0) {
                                const numSpan = document.createElement('span');
                                numSpan.className = 'cell-number';
                                numSpan.textContent = data.numbers.join(',');
                                cell.appendChild(numSpan);
                            }
                        }
                        
                        // Evento de clic
                        cell.onclick = function(e) {
                            e.preventDefault();
                            e.stopPropagation();
                            handleCellClick(r, c);
                        };
                        
                        // Evento táctil para móvil
                        cell.ontouchstart = function(e) {
                            e.preventDefault();
                            handleCellClick(r, c);
                        };
                        
                    } else {
                        cell.classList.add('blocked');
                    }
                    
                    gridEl.appendChild(cell);
                }
            }
        }

        // ==================== MANEJO DE INTERACCIÓN ====================
        
        function handleCellClick(row, col) {
            console.log(`Celda clickeada: ${row}, ${col}`);
            
            const data = gridData[row][col];
            if (!data || !data.isActive) {
                console.log('Celda inactiva, ignorando');
                return;
            }
            
            // Actualizar selección
            selectedCell = { row, col };
            
            // Limpiar selecciones anteriores
            document.querySelectorAll('.cell').forEach(c => {
                c.classList.remove('selected', 'highlighted');
            });
            
            // Marcar celda actual
            const cellIndex = row * 15 + col;
            const cells = document.querySelectorAll('.cell');
            if (cells[cellIndex]) {
                cells[cellIndex].classList.add('selected');
            }
            
            // Determinar palabra actual
            determineCurrentWord(row, col);
            
            // Resaltar palabra completa
            highlightCurrentWord();
            
            // Actualizar display
            updateInfoPanel();
            
            // Mostrar teclado
            showKeyboard();
            
            console.log(`Seleccionada: ${row},${col}, dir: ${currentDirection}, palabra: ${currentWord ? currentWord.number : 'ninguna'}`);
        }

        function determineCurrentWord(row, col) {
            const data = gridData[row][col];
            
            // Intentar dirección actual primero
            let wordInfo = currentDirection === 'across' ? data.acrossWord : data.downWord;
            
            // Si no hay palabra en dirección actual, cambiar dirección
            if (!wordInfo) {
                currentDirection = currentDirection === 'across' ? 'down' : 'across';
                wordInfo = currentDirection === 'across' ? data.acrossWord : data.downWord;
                updateDirectionButtons();
            }
            
            if (wordInfo) {
                currentWord = {
                    ...wordInfo,
                    direction: currentDirection
                };
            } else {
                currentWord = null;
            }
        }

        function highlightCurrentWord() {
            if (!currentWord) return;
            
            const { startRow, startCol, word, direction } = currentWord;
            const cells = document.querySelectorAll('.cell');
            
            for (let i = 0; i < word.length; i++) {
                const row = direction === 'across' ? startRow : startRow + i;
                const col = direction === 'across' ? startCol + i : startCol;
                const index = row * 15 + col;
                
                if (cells[index]) {
                    cells[index].classList.add('highlighted');
                }
            }
        }

        function updateInfoPanel() {
            const label = document.querySelector('#infoPanel .label');
            const display = document.getElementById('wordDisplay');
            
            if (!currentWord) {
                label.textContent = 'Selecciona una casilla';
                display.textContent = '_ _ _ _ _';
                return;
            }
            
            const { number, direction, word } = currentWord;
            label.textContent = `Palabra ${number} - ${direction === 'across' ? 'Horizontal' : 'Vertical'}`;
            
            // Mostrar progreso de la palabra
            let displayStr = '';
            for (let i = 0; i < word.length; i++) {
                const row = direction === 'across' ? currentWord.startRow : currentWord.startRow + i;
                const col = direction === 'across' ? currentWord.startCol + i : currentWord.startCol;
                const char = userAnswers[row][col];
                displayStr += (char || '_') + ' ';
            }
            display.textContent = displayStr.trim();
        }

        // ==================== CONTROLES DE DIRECCIÓN ====================
        
        function setDirection(dir) {
            if (!selectedCell) {
                currentDirection = dir;
                updateDirectionButtons();
                return;
            }
            
            const { row, col } = selectedCell;
            const data = gridData[row][col];
            const wordInfo = dir === 'across' ? data.acrossWord : data.downWord;
            
            if (wordInfo) {
                currentDirection = dir;
                currentWord = { ...wordInfo, direction: dir };
                updateDirectionButtons();
                
                // Actualizar visual
                document.querySelectorAll('.cell').forEach(c => c.classList.remove('highlighted'));
                highlightCurrentWord();
                updateInfoPanel();
            }
        }

        function updateDirectionButtons() {
            const btnAcross = document.getElementById('btnAcross');
            const btnDown = document.getElementById('btnDown');
            
            if (btnAcross && btnDown) {
                btnAcross.classList.toggle('active', currentDirection === 'across');
                btnDown.classList.toggle('active', currentDirection === 'down');
            }
        }

        // ==================== TECLADO ====================
        
        function showKeyboard() {
            const kb = document.getElementById('keyboard');
            if (kb) kb.classList.add('active');
        }

        function closeKeyboard() {
            const kb = document.getElementById('keyboard');
            if (kb) kb.classList.remove('active');
            
            // Limpiar selección visual pero mantener datos
            document.querySelectorAll('.cell').forEach(c => {
                c.classList.remove('selected', 'highlighted');
            });
        }

        function typeLetter(letter) {
            if (!selectedCell) {
                showModal('Atención', 'Primero selecciona una casilla del crucigrama');
                return;
            }
            
            const { row, col } = selectedCell;
            userAnswers[row][col] = letter.toUpperCase();
            
            // Actualizar celda visualmente
            updateCellVisual(row, col);
            
            // Actualizar progreso
            updateProgress();
            updateInfoPanel();
            
            // Mover a siguiente celda
            moveNext();
        }

        function deleteLetter() {
            if (!selectedCell) return;
            
            const { row, col } = selectedCell;
            
            if (userAnswers[row][col]) {
                // Si tiene contenido, borrarlo
                userAnswers[row][col] = '';
                updateCellVisual(row, col);
                updateProgress();
                updateInfoPanel();
            } else {
                // Si está vacía, mover atrás
                movePrev();
            }
        }

        function updateCellVisual(row, col) {
            const index = row * 15 + col;
            const cells = document.querySelectorAll('.cell');
            const cell = cells[index];
            
            if (!cell) return;
            
            const data = gridData[row][col];
            const char = userAnswers[row][col];
            
            // Limpiar contenido actual
            cell.innerHTML = '';
            
            // Agregar número si existe
            if (data && data.numbers.length > 0) {
                const numSpan = document.createElement('span');
                numSpan.className = 'cell-number';
                numSpan.textContent = data.numbers.join(',');
                cell.appendChild(numSpan);
            }
            
            // Agregar letra si existe
            if (char) {
                cell.textContent = char;
                // Re-agregar número
                if (data && data.numbers.length > 0) {
                    const numSpan = document.createElement('span');
                    numSpan.className = 'cell-number';
                    numSpan.textContent = data.numbers.join(',');
                    cell.appendChild(numSpan);
                }
            }
            
            // Limpiar estados de validación
            cell.classList.remove('correct', 'wrong');
        }

        // ==================== NAVEGACIÓN ====================
        
        function moveNext() {
            if (!currentWord || !selectedCell) return;
            
            const { row, col } = selectedCell;
            const { direction, startRow, startCol, word } = currentWord;
            
            // Calcular índice actual en la palabra
            let currentIndex;
            if (direction === 'across') {
                currentIndex = col - startCol;
            } else {
                currentIndex = row - startRow;
            }
            
            // Verificar si hay siguiente celda
            if (currentIndex < word.length - 1) {
                const nextRow = direction === 'across' ? row : row + 1;
                const nextCol = direction === 'across' ? col + 1 : col;
                handleCellClick(nextRow, nextCol);
            }
        }

        function movePrev() {
            if (!currentWord || !selectedCell) return;
            
            const { row, col } = selectedCell;
            const { direction, startRow, startCol } = currentWord;
            
            // Calcular índice actual
            let currentIndex;
            if (direction === 'across') {
                currentIndex = col - startCol;
            } else {
                currentIndex = row - startRow;
            }
            
            // Verificar si hay celda anterior
            if (currentIndex > 0) {
                const prevRow = direction === 'across' ? row : row - 1;
                const prevCol = direction === 'across' ? col - 1 : col;
                handleCellClick(prevRow, prevCol);
            }
        }

        // ==================== FUNCIONES DE PISTAS ====================
        
        function toggleClues() {
            const content = document.getElementById('cluesContent');
            const arrow = document.getElementById('clueToggle');
            
            if (content && arrow) {
                if (content.classList.contains('open')) {
                    content.classList.remove('open');
                    arrow.textContent = '▼';
                } else {
                    content.classList.add('open');
                    arrow.textContent = '▲';
                    closeKeyboard();
                }
            }
        }

        function selectClue(number, direction) {
            // Encontrar la palabra
            const word = WORDS.find(w => w[4] === number && w[2] === direction);
            
            if (!word) {
                console.error(`Palabra no encontrada: ${number} ${direction}`);
                return;
            }
            
            // Establecer dirección
            currentDirection = direction;
            updateDirectionButtons();
            
            // Seleccionar primera celda
            const [startRow, startCol] = word;
            handleCellClick(startRow, startCol);
            
            // Cerrar panel de pistas en móvil
            const content = document.getElementById('cluesContent');
            const arrow = document.getElementById('clueToggle');
            if (content && window.innerWidth < 768) {
                content.classList.remove('open');
                if (arrow) arrow.textContent = '▼';
            }
            
            // Scroll al grid
            document.querySelector('.grid-wrapper').scrollIntoView({ behavior: 'smooth' });
            
            // Marcar pista activa
            document.querySelectorAll('.clue').forEach(c => c.classList.remove('active'));
            if (event && event.currentTarget) {
                event.currentTarget.classList.add('active');
            }
        }

        // ==================== VERIFICACIÓN Y SOLUCIÓN ====================
        
        function checkAnswers() {
            let correctCount = 0;
            let totalCount = 0;
            let wrongCells = [];
            
            WORDS.forEach(([startRow, startCol, direction, word, number]) => {
                for (let i = 0; i < word.length; i++) {
                    const row = direction === 'across' ? startRow : startRow + i;
                    const col = direction === 'across' ? startCol + i : startCol;
                    const index = row * 15 + col;
                    
                    const userChar = userAnswers[row][col];
                    const correctChar = word[i];
                    
                    totalCount++;
                    
                    const cell = document.querySelectorAll('.cell')[index];
                    if (!cell) continue;
                    
                    cell.classList.remove('correct', 'wrong');
                    
                    if (userChar === correctChar) {
                        correctCount++;
                        cell.classList.add('correct');
                    } else if (userChar) {
                        wrongCells.push(index);
                    }
                }
            });
            
            const percentage = Math.round((correctCount / totalCount) * 100);
            
            if (percentage === 100) {
                showModal('¡Excelente! 🌲', `¡Perfecto! Has completado el crucigrama con 100% de acierto. Dominas los conceptos de SIG.`);
            } else if (percentage >= 70) {
                showModal('¡Muy bien! 🌿', `Tienes ${percentage}% de aciertos. Las celdas en rojo necesitan revisión.`);
                wrongCells.forEach(idx => {
                    const cell = document.querySelectorAll('.cell')[idx];
                    if (cell) cell.classList.add('wrong');
                });
            } else {
                showModal('Sigue practicando 🍃', `Tienes ${percentage}% de aciertos. Usa las pistas y sigue intentando.`);
                wrongCells.forEach(idx => {
                    const cell = document.querySelectorAll('.cell')[idx];
                    if (cell) cell.classList.add('wrong');
                });
            }
        }

        function showSolution() {
            WORDS.forEach(([startRow, startCol, direction, word]) => {
                for (let i = 0; i < word.length; i++) {
                    const row = direction === 'across' ? startRow : startRow + i;
                    const col = direction === 'across' ? startCol + i : startCol;
                    userAnswers[row][col] = word[i];
                    updateCellVisual(row, col);
                }
            });
            
            updateProgress();
            showModal('Solución revelada', 'Se han mostrado todas las respuestas correctas. Estúdialas y vuelve a intentarlo.');
        }

        function clearGrid() {
            // Limpiar respuestas
            for (let r = 0; r < 15; r++) {
                for (let c = 0; c < 15; c++) {
                    if (userAnswers[r][c]) {
                        userAnswers[r][c] = '';
                        updateCellVisual(r, c);
                    }
                }
            }
            
            // Limpiar estados visuales
            document.querySelectorAll('.cell').forEach(c => {
                c.classList.remove('correct', 'wrong', 'selected', 'highlighted');
            });
            
            selectedCell = null;
            currentWord = null;
            updateProgress();
            updateInfoPanel();
            closeKeyboard();
        }

        function updateProgress() {
            let filled = 0;
            let total = 0;
            
            WORDS.forEach(([startRow, startCol, direction, word]) => {
                for (let i = 0; i < word.length; i++) {
                    const row = direction === 'across' ? startRow : startRow + i;
                    const col = direction === 'across' ? startCol + i : startCol;
                    if (userAnswers[row][col]) filled++;
                    total++;
                }
            });
            
            const percentage = Math.round((filled / total) * 100);
            
            const bar = document.getElementById('progressBar');
            const text = document.getElementById('progressText');
            
            if (bar) bar.style.width = percentage + '%';
            if (text) text.textContent = `${percentage}% completado (${filled}/${total} letras)`;
        }

        // ==================== MODAL ====================
        
        function showModal(title, text) {
            const modal = document.getElementById('modal');
            const titleEl = document.getElementById('modalTitle');
            const textEl = document.getElementById('modalText');
            
            if (titleEl) titleEl.textContent = title;
            if (textEl) textEl.textContent = text;
            if (modal) modal.style.display = 'flex';
        }

        function closeModal() {
            const modal = document.getElementById('modal');
            if (modal) modal.style.display = 'none';
        }

        // ==================== INICIALIZACIÓN ====================
        
        // Iniciar cuando el DOM esté listo
        if (document.readyState === 'loading') {
            document.addEventListener('DOMContentLoaded', init);
        } else {
            init();
        }

        // Prevenir zoom en doble tap en móviles
        let lastTouchEnd = 0;
        document.addEventListener('touchend', function(e) {
            const now = Date.now();
            if (now - lastTouchEnd <= 300) {
                e.preventDefault();
            }
            lastTouchEnd = now;
        }, false);
    </script>
</body>
</html>>
