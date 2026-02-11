# SIG_CRUCIGRAMA_FORESTAL
Crucigrama educativo para Introducción a SIG - Ingeniería Forestal
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>Crucigrama SIG - Ingeniería Forestal</title>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Inter:wght@400;600;700&display=swap');
        
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            -webkit-tap-highlight-color: transparent;
        }

        body {
            font-family: 'Inter', sans-serif;
            background: linear-gradient(135deg, #1a472a 0%, #0d3320 100%);
            min-height: 100vh;
            color: #e8f5e9;
            padding: 10px;
            padding-bottom: 100px;
        }

        header {
            text-align: center;
            padding: 15px;
            background: rgba(0,0,0,0.3);
            border-radius: 12px;
            margin-bottom: 10px;
        }

        h1 {
            font-size: 1.3rem;
            color: #81c784;
            margin-bottom: 5px;
        }

        .subtitle {
            font-size: 0.8rem;
            opacity: 0.9;
        }

        .controls {
            display: flex;
            gap: 8px;
            margin-bottom: 10px;
            flex-wrap: wrap;
        }

        .btn {
            flex: 1;
            min-width: 80px;
            padding: 10px;
            background: #2e7d32;
            color: white;
            border: none;
            border-radius: 8px;
            font-size: 0.85rem;
            font-weight: 600;
            cursor: pointer;
        }

        .btn:active {
            transform: scale(0.95);
        }

        .btn.secondary {
            background: rgba(255,255,255,0.2);
        }

        .direction-toggle {
            display: flex;
            gap: 8px;
            margin-bottom: 10px;
        }

        .dir-btn {
            flex: 1;
            padding: 8px;
            background: rgba(255,255,255,0.1);
            border: 2px solid transparent;
            color: white;
            border-radius: 8px;
            font-size: 0.8rem;
        }

        .dir-btn.active {
            background: #4caf50;
            border-color: #81c784;
        }

        .progress-bar {
            height: 6px;
            background: rgba(255,255,255,0.2);
            border-radius: 3px;
            margin-bottom: 10px;
            overflow: hidden;
        }

        .progress-fill {
            height: 100%;
            background: #81c784;
            width: 0%;
            transition: width 0.3s;
        }

        .grid-container {
            background: rgba(0,0,0,0.3);
            padding: 10px;
            border-radius: 12px;
            overflow-x: auto;
            margin-bottom: 10px;
        }

        .grid {
            display: grid;
            grid-template-columns: repeat(15, 32px);
            grid-template-rows: repeat(15, 32px);
            gap: 1px;
            background: #1b5e20;
            width: fit-content;
            margin: 0 auto;
        }

        .cell {
            background: #e8f5e9;
            display: flex;
            align-items: center;
            justify-content: center;
            font-weight: bold;
            font-size: 1.1rem;
            color: #1b5e20;
            position: relative;
            cursor: pointer;
            user-select: none;
        }

        .cell.blocked {
            background: #0d3320;
        }

        .cell.selected {
            background: #81c784;
            box-shadow: inset 0 0 0 3px #2e7d32;
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

        .number {
            position: absolute;
            top: 1px;
            left: 2px;
            font-size: 0.6rem;
            font-weight: bold;
        }

        .current-word {
            background: rgba(0,0,0,0.3);
            padding: 10px;
            border-radius: 8px;
            margin-bottom: 10px;
            text-align: center;
            font-size: 0.9rem;
            min-height: 50px;
        }

        .keyboard {
            position: fixed;
            bottom: 0;
            left: 0;
            right: 0;
            background: #0d3320;
            padding: 8px;
            border-top: 2px solid #4caf50;
            transform: translateY(100%);
            transition: transform 0.3s;
        }

        .keyboard.active {
            transform: translateY(0);
        }

        .kb-row {
            display: flex;
            justify-content: center;
            gap: 4px;
            margin-bottom: 4px;
        }

        .key {
            min-width: 32px;
            height: 40px;
            background: rgba(255,255,255,0.1);
            border: 1px solid #4caf50;
            border-radius: 6px;
            color: white;
            font-size: 1rem;
            font-weight: bold;
            display: flex;
            align-items: center;
            justify-content: center;
            cursor: pointer;
        }

        .key:active {
            background: #4caf50;
        }

        .key.wide {
            min-width: 60px;
            font-size: 0.8rem;
        }

        .clues-section {
            background: rgba(0,0,0,0.3);
            border-radius: 12px;
            overflow: hidden;
        }

        .clues-header {
            padding: 12px;
            background: rgba(76, 175, 80, 0.3);
            cursor: pointer;
            display: flex;
            justify-content: space-between;
            align-items: center;
        }

        .clues-content {
            max-height: 0;
            overflow: hidden;
            transition: max-height 0.3s;
        }

        .clues-content.open {
            max-height: 2000px;
        }

        .clue-group {
            padding: 10px;
            border-bottom: 1px solid rgba(255,255,255,0.1);
        }

        .clue-group-title {
            font-weight: bold;
            color: #81c784;
            margin-bottom: 8px;
            font-size: 0.9rem;
        }

        .clue {
            padding: 8px;
            margin-bottom: 5px;
            background: rgba(255,255,255,0.05);
            border-radius: 6px;
            font-size: 0.85rem;
            line-height: 1.3;
            cursor: pointer;
            border-left: 3px solid transparent;
        }

        .clue.active {
            border-left-color: #81c784;
            background: rgba(129, 199, 132, 0.2);
        }

        .clue-num {
            color: #81c784;
            font-weight: bold;
        }

        .modal {
            display: none;
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(0,0,0,0.9);
            z-index: 1000;
            justify-content: center;
            align-items: center;
            padding: 20px;
        }

        .modal-content {
            background: linear-gradient(135deg, #1b5e20, #0d3320);
            padding: 30px;
            border-radius: 16px;
            text-align: center;
            border: 2px solid #81c784;
            max-width: 300px;
        }

        .modal h2 {
            color: #81c784;
            margin-bottom: 15px;
        }

        .modal p {
            margin-bottom: 20px;
            font-size: 0.9rem;
        }

        .modal-btn {
            padding: 12px 24px;
            background: #81c784;
            color: #1b5e20;
            border: none;
            border-radius: 20px;
            font-weight: bold;
            cursor: pointer;
        }

        @media (min-width: 768px) {
            .grid {
                grid-template-columns: repeat(15, 40px);
                grid-template-rows: repeat(15, 40px);
            }
            .cell {
                font-size: 1.3rem;
            }
        }
    </style>
</head>
<body>
    <header>
        <h1>🌲 Crucigrama Forestal</h1>
        <div class="subtitle">Introducción a SIG - 4to Semestre</div>
    </header>

    <div class="progress-bar">
        <div class="progress-fill" id="progress"></div>
    </div>

    <div class="direction-toggle">
        <button class="dir-btn active" onclick="setDir('across')">→ Horizontal</button>
        <button class="dir-btn" onclick="setDir('down')">↓ Vertical</button>
    </div>

    <div class="current-word" id="currentWord">
        Toca una casilla para empezar
    </div>

    <div class="grid-container">
        <div class="grid" id="grid"></div>
    </div>

    <div class="controls">
        <button class="btn" onclick="check()">✓ Verificar</button>
        <button class="btn secondary" onclick="solve()">👁 Solución</button>
        <button class="btn secondary" onclick="clearAll()">↺ Limpiar</button>
        <button class="btn secondary" onclick="toggleClues()">📋 Pistas</button>
    </div>

    <div class="clues-section" style="margin-top: 10px;">
        <div class="clues-header" onclick="toggleClues()">
            <span>Definiciones</span>
            <span id="clueArrow">▼</span>
        </div>
        <div class="clues-content" id="cluesContent">
            <div class="clue-group">
                <div class="clue-group-title">Horizontales →</div>
                <div class="clue" onclick="goToClue(1, 'across')">
                    <span class="clue-num">1.</span> Sistema de Información Geográfica (siglas)
                </div>
                <div class="clue" onclick="goToClue(3, 'across')">
                    <span class="clue-num">3.</span> Sensor láser para modelos 3D forestales
                </div>
                <div class="clue" onclick="goToClue(5, 'across')">
                    <span class="clue-num">5.</span> Constelación satelital ESA 2015
                </div>
                <div class="clue" onclick="goToClue(7, 'across')">
                    <span class="clue-num">7.</span> Índice de vegetación (siglas)
                </div>
                <div class="clue" onclick="goToClue(9, 'across')">
                    <span class="clue-num">9.</span> Obtención de datos sin contacto físico
                </div>
                <div class="clue" onclick="goToClue(11, 'across')">
                    <span class="clue-num">11.</span> Modelo Digital de Elevación (siglas)
                </div>
                <div class="clue" onclick="goToClue(13, 'across')">
                    <span class="clue-num">13.</span> Sistema de Posicionamiento Global (siglas)
                </div>
                <div class="clue" onclick="goToClue(15, 'across')">
                    <span class="clue-num">15.</span> Software SIG libre y gratuito
                </div>
                <div class="clue" onclick="goToClue(17, 'across')">
                    <span class="clue-num">17.</span> Conversión de mapas analógicos a digitales
                </div>
                <div class="clue" onclick="goToClue(19, 'across')">
                    <span class="clue-num">19.</span> Representación gráfica de datos geográficos
                </div>
            </div>
            <div class="clue-group">
                <div class="clue-group-title">Verticales ↓</div>
                <div class="clue" onclick="goToClue(1, 'down')">
                    <span class="clue-num">1.</span> Sistema de Gestión de Bases de Datos (siglas)
                </div>
                <div class="clue" onclick="goToClue(2, 'down')">
                    <span class="clue-num">2.</span> Primer satélite de la NASA (1972)
                </div>
                <div class="clue" onclick="goToClue(4, 'down')">
                    <span class="clue-num">4.</span> Formato de datos geoespaciales JSON
                </div>
                <div class="clue" onclick="goToClue(6, 'down')">
                    <span class="clue-num">6.</span> Tamaño de celda en imágenes raster
                </div>
                <div class="clue" onclick="goToClue(8, 'down')">
                    <span class="clue-num">8.</span> Unión de tablas mediante campo común
                </div>
                <div class="clue" onclick="goToClue(10, 'down')">
                    <span class="clue-num">10.</span> Plataforma Google para imágenes satelitales
                </div>
                <div class="clue" onclick="goToClue(12, 'down')">
                    <span class="clue-num">12.</span> Ciencia de la información geográfica
                </div>
                <div class="clue" onclick="goToClue(14, 'down')">
                    <span class="clue-num">14.</span> Vehículo Aéreo No Tripulado (siglas)
                </div>
                <div class="clue" onclick="goToClue(16, 'down')">
                    <span class="clue-num">16.</span> Asignación de coordenadas geográficas
                </div>
                <div class="clue" onclick="goToClue(18, 'down')">
                    <span class="clue-num">18.</span> Predicción de valores en ubicaciones desconocidas
                </div>
            </div>
        </div>
    </div>

    <div class="keyboard" id="keyboard">
        <div class="kb-row">
            <button class="key" onclick="type('Q')">Q</button>
            <button class="key" onclick="type('W')">W</button>
            <button class="key" onclick="type('E')">E</button>
            <button class="key" onclick="type('R')">R</button>
            <button class="key" onclick="type('T')">T</button>
            <button class="key" onclick="type('Y')">Y</button>
            <button class="key" onclick="type('U')">U</button>
            <button class="key" onclick="type('I')">I</button>
            <button class="key" onclick="type('O')">O</button>
            <button class="key" onclick="type('P')">P</button>
        </div>
        <div class="kb-row">
            <button class="key" onclick="type('A')">A</button>
            <button class="key" onclick="type('S')">S</button>
            <button class="key" onclick="type('D')">D</button>
            <button class="key" onclick="type('F')">F</button>
            <button class="key" onclick="type('G')">G</button>
            <button class="key" onclick="type('H')">H</button>
            <button class="key" onclick="type('J')">J</button>
            <button class="key" onclick="type('K')">K</button>
            <button class="key" onclick="type('L')">L</button>
            <button class="key" onclick="type('Ñ')">Ñ</button>
        </div>
        <div class="kb-row">
            <button class="key" onclick="type('Z')">Z</button>
            <button class="key" onclick="type('X')">X</button>
            <button class="key" onclick="type('C')">C</button>
            <button class="key" onclick="type('V')">V</button>
            <button class="key" onclick="type('B')">B</button>
            <button class="key" onclick="type('N')">N</button>
            <button class="key" onclick="type('M')">M</button>
            <button class="key wide" onclick="backspace()">⌫</button>
        </div>
        <div class="kb-row">
            <button class="key wide" onclick="prev()">◀ Ant</button>
            <button class="key wide" onclick="next()">Sig ▶</button>
            <button class="key wide" onclick="closeKB()" style="background:#ef5350;">Cerrar</button>
        </div>
    </div>

    <div class="modal" id="modal" onclick="closeModal()">
        <div class="modal-content" onclick="event.stopPropagation()">
            <h2 id="modTitle">¡Listo!</h2>
            <p id="modMsg">Mensaje</p>
            <button class="modal-btn" onclick="closeModal()">Aceptar</button>
        </div>
    </div>

    <script>
        // Definición del crucigrama - Estructura corregida
        const words = [
            // [fila, columna, dirección, palabra, número]
            // Horizontales
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
            
            // Verticales
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

        let grid = Array(15).fill().map(() => Array(15).fill(null));
        let answers = Array(15).fill().map(() => Array(15).fill(''));
        let selected = null;
        let currentDir = 'across';
        let currentWord = null;

        // Construir estructura de datos
        function init() {
            // Marcar celdas ocupadas
            words.forEach(([r, c, dir, word, num]) => {
                for (let i = 0; i < word.length; i++) {
                    let row = dir === 'across' ? r : r + i;
                    let col = dir === 'across' ? c + i : c;
                    if (row < 15 && col < 15) {
                        if (!grid[row][col]) {
                            grid[row][col] = { nums: [], char: word[i] };
                        }
                        if (i === 0) grid[row][col].nums.push(num);
                    }
                }
            });
            
            render();
            updateProgress();
        }

        function render() {
            const g = document.getElementById('grid');
            g.innerHTML = '';
            
            for (let r = 0; r < 15; r++) {
                for (let c = 0; c < 15; c++) {
                    const cell = document.createElement('div');
                    cell.className = 'cell';
                    cell.dataset.r = r;
                    cell.dataset.c = c;
                    
                    if (grid[r][c]) {
                        if (grid[r][c].nums.length > 0) {
                            const n = document.createElement('span');
                            n.className = 'number';
                            n.textContent = grid[r][c].nums[0];
                            cell.appendChild(n);
                        }
                        if (answers[r][c]) {
                            cell.textContent = answers[r][c];
                            cell.appendChild(cell.firstChild); // Reordenar número
                        }
                        cell.onclick = () => select(r, c);
                    } else {
                        cell.classList.add('blocked');
                    }
                    g.appendChild(cell);
                }
            }
        }

        function select(r, c) {
            if (!grid[r][c]) return;
            
            selected = { r, c };
            
            // Limpiar selección anterior
            document.querySelectorAll('.cell').forEach(cel => {
                cel.classList.remove('selected', 'highlighted');
            });
            
            // Seleccionar actual
            const idx = r * 15 + c;
            document.querySelectorAll('.cell')[idx].classList.add('selected');
            
            // Determinar palabra actual
            findWord(r, c);
            highlightWord();
            updateDisplay();
            
            // Mostrar teclado
            document.getElementById('keyboard').classList.add('active');
        }

        function findWord(r, c) {
            // Buscar en dirección actual
            for (let w of words) {
                let [wr, wc, dir, word, num] = w;
                if (dir === currentDir) {
                    if (currentDir === 'across' && wr === r && wc <= c && c < wc + word.length) {
                        currentWord = w;
                        return;
                    }
                    if (currentDir === 'down' && wc === c && wr <= r && r < wr + word.length) {
                        currentWord = w;
                        return;
                    }
                }
            }
            
            // Si no, buscar en dirección opuesta
            let other = currentDir === 'across' ? 'down' : 'across';
            for (let w of words) {
                let [wr, wc, dir, word, num] = w;
                if (dir === other) {
                    if (other === 'across' && wr === r && wc <= c && c < wc + word.length) {
                        currentWord = w;
                        currentDir = other;
                        updateDirButtons();
                        return;
                    }
                    if (other === 'down' && wc === c && wr <= r && r < wr + word.length) {
                        currentWord = w;
                        currentDir = other;
                        updateDirButtons();
                        return;
                    }
                }
            }
        }

        function highlightWord() {
            if (!currentWord) return;
            let [r, c, dir, word] = currentWord;
            
            for (let i = 0; i < word.length; i++) {
                let row = dir === 'across' ? r : r + i;
                let col = dir === 'across' ? c + i : c;
                let idx = row * 15 + col;
                document.querySelectorAll('.cell')[idx].classList.add('highlighted');
            }
        }

        function updateDisplay() {
            const d = document.getElementById('currentWord');
            if (!currentWord) {
                d.innerHTML = 'Toca una casilla para empezar';
                return;
            }
            
            let [r, c, dir, word, num] = currentWord;
            let filled = word.split('').map((ch, i) => {
                let row = dir === 'across' ? r : r + i;
                let col = dir === 'across' ? c + i : c;
                return answers[row][col] || '_';
            }).join('');
            
            d.innerHTML = `<div style="font-size:0.8rem;opacity:0.8">Palabra ${num} ${dir === 'across' ? '→' : '↓'}</div><div style="font-size:1.1rem;margin-top:4px">${filled}</div>`;
        }

        function setDir(dir) {
            currentDir = dir;
            updateDirButtons();
            if (selected) {
                findWord(selected.r, selected.c);
                highlightWord();
                updateDisplay();
            }
        }

        function updateDirButtons() {
            document.getElementById('btn-across')?.classList.toggle('active', currentDir === 'across');
            document.getElementById('btn-down')?.classList.toggle('active', currentDir === 'down');
        }

        function type(ch) {
            if (!selected) return;
            
            let { r, c } = selected;
            answers[r][c] = ch;
            
            let idx = r * 15 + c;
            let cell = document.querySelectorAll('.cell')[idx];
            cell.textContent = ch;
            if (grid[r][c].nums.length) {
                let n = document.createElement('span');
                n.className = 'number';
                n.textContent = grid[r][c].nums[0];
                cell.appendChild(n);
            }
            cell.classList.remove('wrong');
            
            updateProgress();
            updateDisplay();
            next();
        }

        function backspace() {
            if (!selected) return;
            let { r, c } = selected;
            answers[r][c] = '';
            
            let idx = r * 15 + c;
            let cell = document.querySelectorAll('.cell')[idx];
            cell.textContent = '';
            if (grid[r][c].nums.length) {
                let n = document.createElement('span');
                n.className = 'number';
                n.textContent = grid[r][c].nums[0];
                cell.appendChild(n);
            }
            
            updateProgress();
            updateDisplay();
            prev();
        }

        function next() {
            if (!currentWord) return;
            let [r, c, dir, word] = currentWord;
            let { r: cr, c: cc } = selected;
            
            let idx = dir === 'across' ? (cc - c) : (cr - r);
            if (idx < word.length - 1) {
                let nr = dir === 'across' ? cr : cr + 1;
                let nc = dir === 'across' ? cc + 1 : cc;
                select(nr, nc);
            }
        }

        function prev() {
            if (!currentWord) return;
            let [r, c, dir, word] = currentWord;
            let { r: cr, c: cc } = selected;
            
            let idx = dir === 'across' ? (cc - c) : (cr - r);
            if (idx > 0) {
                let nr = dir === 'across' ? cr : cr - 1;
                let nc = dir === 'across' ? cc - 1 : cc;
                select(nr, nc);
            }
        }

        function closeKB() {
            document.getElementById('keyboard').classList.remove('active');
            document.querySelectorAll('.cell').forEach(c => c.classList.remove('selected', 'highlighted'));
            selected = null;
            currentWord = null;
            document.getElementById('currentWord').textContent = 'Toca una casilla para empezar';
        }

        function check() {
            let correct = 0, total = 0, wrong = [];
            
            words.forEach(([r, c, dir, word]) => {
                for (let i = 0; i < word.length; i++) {
                    let row = dir === 'across' ? r : r + i;
                    let col = dir === 'across' ? c + i : c;
                    let idx = row * 15 + col;
                    let cell = document.querySelectorAll('.cell')[idx];
                    
                    if (answers[row][col] === word[i]) {
                        correct++;
                        cell.classList.add('correct');
                    } else if (answers[row][col]) {
                        wrong.push(idx);
                    }
                    total++;
                }
            });
            
            let pct = Math.round((correct / total) * 100);
            
            if (pct === 100) {
                showModal('¡Excelente! 🌲', `Perfecto: 100% de aciertos`);
            } else if (pct >= 70) {
                showModal('¡Bien! 🌿', `${pct}% correcto. Sigue practicando`);
                wrong.forEach(i => document.querySelectorAll('.cell')[i].classList.add('wrong'));
            } else {
                showModal('Sigue intentando 🍃', `${pct}% correcto. Usa las pistas`);
                wrong.forEach(i => document.querySelectorAll('.cell')[i].classList.add('wrong'));
            }
        }

        function solve() {
            words.forEach(([r, c, dir, word]) => {
                for (let i = 0; i < word.length; i++) {
                    let row = dir === 'across' ? r : r + i;
                    let col = dir === 'across' ? c + i : c;
                    answers[row][col] = word[i];
                }
            });
            render();
            updateProgress();
            showModal('Solución revelada', 'Estudia las respuestas');
        }

        function clearAll() {
            answers = Array(15).fill().map(() => Array(15).fill(''));
            render();
            updateProgress();
            closeKB();
        }

        function updateProgress() {
            let filled = 0, total = 0;
            words.forEach(([r, c, dir, word]) => {
                for (let i = 0; i < word.length; i++) {
                    let row = dir === 'across' ? r : r + i;
                    let col = dir === 'across' ? c + i : c;
                    if (answers[row][col]) filled++;
                    total++;
                }
            });
            let pct = Math.round((filled / total) * 100);
            document.getElementById('progress').style.width = pct + '%';
        }

        function toggleClues() {
            const c = document.getElementById('cluesContent');
            const a = document.getElementById('clueArrow');
            if (c.classList.contains('open')) {
                c.classList.remove('open');
                a.textContent = '▼';
            } else {
                c.classList.add('open');
                a.textContent = '▲';
                closeKB();
            }
        }

        function goToClue(num, dir) {
            for (let w of words) {
                if (w[4] === num && w[2] === dir) {
                    currentDir = dir;
                    updateDirButtons();
                    select(w[0], w[1]);
                    document.getElementById('cluesContent').classList.remove('open');
                    document.getElementById('clueArrow').textContent = '▼';
                    document.querySelector('.grid-container').scrollIntoView({ behavior: 'smooth' });
                    break;
                }
            }
            document.querySelectorAll('.clue').forEach(c => c.classList.remove('active'));
            event.currentTarget.classList.add('active');
        }

        function showModal(t, m) {
            document.getElementById('modTitle').textContent = t;
            document.getElementById('modMsg').textContent = m;
            document.getElementById('modal').style.display = 'flex';
        }

        function closeModal() {
            document.getElementById('modal').style.display = 'none';
        }

        // Iniciar
        init();
    </script>
</body>
</html>
