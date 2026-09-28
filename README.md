
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Simulador Kardashev Tipo I - Domínio Planetário Pleno</title>
    <style>
        :root {
            --bg-color: #020617;
            --panel-bg: rgba(15, 23, 42, 0.88);
            --border-color: rgba(56, 189, 248, 0.35);
            --accent-cyan: #38bdf8;
            --accent-green: #10b981;
            --accent-gold: #f59e0b;
            --accent-purple: #c084fc;
            --text-main: #f8fafc;
            --text-muted: #94a3b8;
        }

        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
            font-family: 'Segoe UI', system-ui, -apple-system, sans-serif;
            user-select: none;
        }

        body {
            background-color: var(--bg-color);
            color: var(--text-main);
            height: 100vh;
            overflow: hidden;
            display: flex;
            flex-direction: column;
        }

        header {
            height: 60px;
            background: linear-gradient(180deg, rgba(15, 23, 42, 0.95) 0%, rgba(2, 6, 23, 0.8) 100%);
            border-bottom: 1px solid var(--border-color);
            display: flex;
            align-items: center;
            justify-content: space-between;
            padding: 0 25px;
            z-index: 10;
        }

        header h1 {
            font-size: 1.05rem;
            letter-spacing: 2px;
            color: var(--accent-cyan);
            text-transform: uppercase;
        }

        .status-badge {
            font-size: 0.75rem;
            padding: 4px 12px;
            border-radius: 12px;
            background: rgba(16, 185, 129, 0.15);
            border: 1px solid var(--accent-green);
            color: var(--accent-green);
            letter-spacing: 1px;
        }

        .main-container {
            display: grid;
            grid-template-columns: 370px 1fr;
            height: calc(100vh - 60px);
            position: relative;
        }

        /* PAINEL LATERAL DE CONTROLE */
        .control-panel {
            background: var(--panel-bg);
            border-right: 1px solid var(--border-color);
            backdrop-filter: blur(12px);
            padding: 20px;
            display: flex;
            flex-direction: column;
            gap: 16px;
            overflow-y: auto;
            z-index: 5;
        }

        .kardashev-box {
            background: radial-gradient(circle, rgba(56, 189, 248, 0.2) 0%, rgba(2, 6, 23, 0.7) 100%);
            border: 1px solid var(--accent-cyan);
            border-radius: 10px;
            padding: 16px;
            text-align: center;
            box-shadow: 0 0 22px rgba(56, 189, 248, 0.2);
        }

        .kardashev-score {
            font-size: 2.3rem;
            font-family: monospace;
            font-weight: bold;
            color: #fff;
            text-shadow: 0 0 15px var(--accent-cyan);
            margin: 4px 0;
        }

        .section-title {
            font-size: 0.78rem;
            text-transform: uppercase;
            letter-spacing: 1.5px;
            color: var(--accent-cyan);
            border-bottom: 1px solid var(--border-color);
            padding-bottom: 6px;
        }

        .metric-group {
            display: flex;
            flex-direction: column;
            gap: 6px;
        }

        .metric-header {
            display: flex;
            justify-content: space-between;
            font-size: 0.8rem;
        }

        .metric-value {
            font-family: monospace;
            font-weight: bold;
            color: var(--accent-cyan);
        }

        input[type="range"] {
            width: 100%;
            height: 6px;
            border-radius: 3px;
            background: rgba(255, 255, 255, 0.1);
            outline: none;
            accent-color: var(--accent-cyan);
            cursor: pointer;
        }

        .toggle-box {
            display: flex;
            align-items: center;
            justify-content: space-between;
            background: rgba(255, 255, 255, 0.03);
            border: 1px solid rgba(255, 255, 255, 0.08);
            border-radius: 6px;
            padding: 8px 12px;
            font-size: 0.78rem;
        }

        .switch {
            position: relative;
            display: inline-block;
            width: 36px;
            height: 18px;
        }

        .switch input { opacity: 0; width: 0; height: 0; }

        .slider {
            position: absolute; cursor: pointer; top: 0; left: 0; right: 0; bottom: 0;
            background-color: rgba(255,255,255,0.2); transition: .3s; border-radius: 18px;
        }

        .slider:before {
            position: absolute; content: ""; height: 12px; width: 12px; left: 3px; bottom: 3px;
            background-color: white; transition: .3s; border-radius: 50%;
        }

        input:checked + .slider { background-color: var(--accent-green); }
        input:checked + .slider:before { transform: translateX(18px); }

        .info-card {
            background: rgba(0, 0, 0, 0.4);
            border: 1px solid rgba(255, 255, 255, 0.08);
            border-radius: 8px;
            padding: 12px;
            font-size: 0.8rem;
            line-height: 1.45;
            color: var(--text-muted);
        }

        .info-card strong {
            color: var(--text-main);
        }

        /* VIEWPORT CANVAS SIMULAÇÃO */
        .viewport {
            position: relative;
            width: 100%;
            height: 100%;
            background: radial-gradient(circle at center, #0b132b 0%, #020617 100%);
            overflow: hidden;
        }

        canvas {
            width: 100%;
            height: 100%;
            display: block;
        }

        .hud-overlay {
            position: absolute;
            bottom: 20px;
            right: 20px;
            background: var(--panel-bg);
            border: 1px solid var(--border-color);
            border-radius: 8px;
            padding: 15px;
            font-family: monospace;
            font-size: 0.75rem;
            pointer-events: none;
            display: flex;
            flex-direction: column;
            gap: 6px;
            backdrop-filter: blur(10px);
        }

        .hud-line {
            display: flex;
            justify-content: space-between;
            gap: 20px;
        }

        ::-webkit-scrollbar { width: 5px; }
        ::-webkit-scrollbar-track { background: transparent; }
        ::-webkit-scrollbar-thumb { background: var(--border-color); border-radius: 3px; }
    </style>
</head>
<body>

    <header>
        <h1>CIVILIZAÇÃO TIPO I: DOMÍNIO PLANETÁRIO INTEGRAL</h1>
        <div class="status-badge">REDE TERRESTRE: 100% OPERACIONAL</div>
    </header>

    <div class="main-container">
        <!-- PAINEL LATERAL -->
        <aside class="control-panel">
            <div class="kardashev-box">
                <div style="font-size: 0.72rem; color: var(--text-muted); text-transform: uppercase;">Métrica de Kardashev</div>
                <div class="kardashev-score" id="k-score">K 1.000</div>
                <div style="font-size: 0.78rem; color: var(--accent-gold);" id="power-display">1.00 × 10¹⁶ W</div>
            </div>

            <div class="section-title">1. Matriz de Captura Planetária</div>
            
            <div class="metric-group">
                <div class="metric-header">
                    <span>Fluxo Energético Total (10¹⁶ - 10¹⁷ W)</span>
                    <span class="metric-value" id="val-power">10.0 × 10¹⁶ W</span>
                </div>
                <input type="range" id="input-power" min="1.0" max="10.0" step="0.1" value="1.0">
            </div>

            <div class="metric-group">
                <div class="metric-header">
                    <span>Reatores de Fusão Termonuclear Global</span>
                    <span class="metric-value" id="val-fusion">100%</span>
                </div>
                <input type="range" id="input-fusion" min="10" max="100" value="100">
            </div>

            <div class="metric-group">
                <div class="metric-header">
                    <span>Anéis de Renovável & Insolamento Solar</span>
                    <span class="metric-value" id="val-solar">100%</span>
                </div>
                <input type="range" id="input-solar" min="10" max="100" value="100">
            </div>

            <div class="section-title">2. Estabilização e Engenharia Planetária</div>

            <div class="toggle-box">
                <span>Controle Ativo de Vulcões e Terremotos</span>
                <label class="switch"><input type="checkbox" id="sw-seismic" checked><span class="slider"></span></label>
            </div>

            <div class="toggle-box">
                <span>Engenharia Climática e Neutralização de Furacões</span>
                <label class="switch"><input type="checkbox" id="sw-weather" checked><span class="slider"></span></label>
            </div>

            <div class="toggle-box">
                <span>Anel Orbital de Distribuição de Micro-ondas</span>
                <label class="switch"><input type="checkbox" id="sw-ring" checked><span class="slider"></span></label>
            </div>

            <div class="section-title">3. Diagnóstico de Recursos</div>
            <div class="info-card">
                <strong>Status de Sustentabilidade:</strong><br>
                • Reciclagem de matéria e energia em nível atômico.<br>
                • Erradicação completa da escassez energética.<br>
                • Clima controlado sem aquecimento residual.<br>
                • Estabilidade geotectônica absoluta em todas as placas.
            </div>
        </aside>

        <!-- VIEWPORT CANVAS SIMULAÇÃO -->
        <main class="viewport">
            <canvas id="simCanvas"></canvas>

            <div class="hud-overlay">
                <div class="hud-line"><span>AQUECIMENTO GLOBAL:</span><span style="color: var(--accent-green)">ERRADICADO (0.0°C)</span></div>
                <div class="hud-line"><span>TEMPESTADES CATASTRÓFICAS:</span><span id="hud-weather" style="color: var(--accent-green)">NEUTRALIZADAS</span></div>
                <div class="hud-line"><span>ESTABILIDADE TECTÔNICA:</span><span id="hud-seismic" style="color: var(--accent-cyan)">100% RETIDA</span></div>
                <div class="hud-line"><span>RESERVA ENERGÉTICA GLOBAL:</span><span style="color: var(--accent-gold)">ILIMITADA</span></div>
            </div>
        </main>
    </div>

    <script>
        const canvas = document.getElementById('simCanvas');
        const ctx = canvas.getContext('2d');

        // Estado da Simulação
        const state = {
            powerWatts: 1e16,
            fusionPercent: 100,
            solarPercent: 100,
            kardashevScale: 1.0,
            seismicControl: true,
            weatherControl: true,
            orbitalRing: true,
            rotationAngle: 0,
            pulse: 0
        };

        function resizeCanvas() {
            canvas.width = canvas.parentElement.clientWidth;
            canvas.height = canvas.parentElement.clientHeight;
        }
        window.addEventListener('resize', resizeCanvas);
        resizeCanvas();

        // Elementos de Entrada
        const inputPower = document.getElementById('input-power');
        const inputFusion = document.getElementById('input-fusion');
        const inputSolar = document.getElementById('input-solar');
        const swSeismic = document.getElementById('sw-seismic');
        const swWeather = document.getElementById('sw-weather');
        const swRing = document.getElementById('sw-ring');

        // Partículas de Transmissão Energética Orbital
        const beams = [];
        const NUM_BEAMS = 36;
        for (let i = 0; i < NUM_BEAMS; i++) {
            beams.push({
                angle: (i / NUM_BEAMS) * Math.PI * 2,
                progress: Math.random(),
                speed: 0.006 + Math.random() * 0.008
            });
        }

        // Atualização dos Cálculos Físicos
        function updatePhysics() {
            const mult = parseFloat(inputPower.value); // 1.0 a 10.0
            state.powerWatts = mult * 1e16; // 10^16 a 10^17 W

            state.fusionPercent = parseInt(inputFusion.value);
            state.solarPercent = parseInt(inputSolar.value);

            // Fórmula de Kardashev: K = (log10(P) - 6) / 10
            state.kardashevScale = (Math.log10(state.powerWatts) - 6) / 10;

            state.seismicControl = swSeismic.checked;
            state.weatherControl = swWeather.checked;
            state.orbitalRing = swRing.checked;

            // Interface
            document.getElementById('k-score').innerText = `K ${state.kardashevScale.toFixed(3)}`;
            
            const displayVal = (state.powerWatts / 1e16).toFixed(2);
            document.getElementById('power-display').innerText = `${displayVal} × 10¹⁶ W`;
            document.getElementById('val-power').innerText = `${(mult * 10).toFixed(1)} × 10¹⁶ W`;

            document.getElementById('val-fusion').innerText = `${state.fusionPercent}%`;
            document.getElementById('val-solar').innerText = `${state.solarPercent}%`;

            // HUD
            const weatherHUD = document.getElementById('hud-weather');
            weatherHUD.innerText = state.weatherControl ? 'NEUTRALIZADAS' : 'INSTÁVEIS (RISCO)';
            weatherHUD.style.color = state.weatherControl ? 'var(--accent-green)' : '#ef4444';

            const seismicHUD = document.getElementById('hud-seismic');
            seismicHUD.innerText = state.seismicControl ? '100% RETIDA' : 'ATIVIDADE NATURAL';
            seismicHUD.style.color = state.seismicControl ? 'var(--accent-cyan)' : '#f59e0b';
        }

        // Eventos
        inputPower.addEventListener('input', updatePhysics);
        inputFusion.addEventListener('input', updatePhysics);
        inputSolar.addEventListener('input', updatePhysics);
        swSeismic.addEventListener('change', updatePhysics);
        swWeather.addEventListener('change', updatePhysics);
        swRing.addEventListener('change', updatePhysics);

        // Loop de Renderização no Canvas
        function render() {
            ctx.clearRect(0, 0, canvas.width, canvas.height);

            const cx = canvas.width / 2;
            const cy = canvas.height / 2;
            const earthRadius = 120;

            state.rotationAngle += 0.003;
            state.pulse += 0.04;

            // 1. Grade de Fundo Sci-Fi
            ctx.strokeStyle = 'rgba(56, 189, 248, 0.04)';
            ctx.lineWidth = 1;
            const gridSize = 45;
            for (let x = 0; x < canvas.width; x += gridSize) {
                ctx.beginPath(); ctx.moveTo(x, 0); ctx.lineTo(x, canvas.height); ctx.stroke();
            }
            for (let y = 0; y < canvas.height; y += gridSize) {
                ctx.beginPath(); ctx.moveTo(0, y); ctx.lineTo(canvas.width, y); ctx.stroke();
            }

            // 2. Anel Orbital Tecnológico de Transmissão Sem Fio
            const ringRadius = earthRadius + 65;
            if (state.orbitalRing) {
                ctx.beginPath();
                ctx.arc(cx, cy, ringRadius, 0, Math.PI * 2);
                ctx.strokeStyle = `rgba(56, 189, 248, ${0.4 + Math.sin(state.pulse) * 0.15})`;
                ctx.lineWidth = 2;
                ctx.stroke();

                // Coletores e Reatores Orbitais no Anel
                for (let i = 0; i < 12; i++) {
                    const angle = (i / 12) * Math.PI * 2 + state.rotationAngle * 0.5;
                    const rx = cx + Math.cos(angle) * ringRadius;
                    const ry = cy + Math.sin(angle) * ringRadius;

                    ctx.beginPath();
                    ctx.arc(rx, ry, 4, 0, Math.PI * 2);
                    ctx.fillStyle = '#f59e0b';
                    ctx.shadowColor = '#f59e0b';
                    ctx.shadowBlur = 10;
                    ctx.fill();
                    ctx.shadowBlur = 0;
                }
            }

            // 3. Planeta Terra Tipo I
            const earthGrad = ctx.createRadialGradient(cx - 30, cy - 30, 20, cx, cy, earthRadius);
            earthGrad.addColorStop(0, '#0284c7');
            earthGrad.addColorStop(0.65, '#0369a1');
            earthGrad.addColorStop(1, '#0f172a');

            ctx.beginPath();
            ctx.arc(cx, cy, earthRadius, 0, Math.PI * 2);
            ctx.fillStyle = earthGrad;
            ctx.fill();

            // Continentes e Malha Energética de Fusão/Renováveis
            ctx.save();
            ctx.translate(cx, cy);
            ctx.rotate(state.rotationAngle);

            // Continentes estilizados
            ctx.fillStyle = '#16a34a';
            ctx.beginPath();
            ctx.arc(-40, -30, 50, 0, Math.PI * 2);
            ctx.arc(40, 45, 40, 0, Math.PI * 2);
            ctx.arc(45, -50, 32, 0, Math.PI * 2);
            ctx.fill();

            // Rede Iluminada Unificada de Cidades e Fusão (Rede Planetária)
            ctx.strokeStyle = `rgba(56, 189, 248, ${0.3 + (state.fusionPercent / 100) * 0.4})`;
            ctx.lineWidth = 1;
            ctx.beginPath();
            ctx.arc(0, 0, earthRadius - 2, 0, Math.PI * 2);
            ctx.moveTo(-45, -25); ctx.lineTo(35, 45);
            ctx.moveTo(35, 45); ctx.lineTo(45, -45);
            ctx.moveTo(45, -45); ctx.lineTo(-45, -25);
            ctx.stroke();

            // Nós de Reatores de Fusão
            ctx.fillStyle = '#c084fc';
            const nodes = [[-45, -25], [35, 45], [45, -45], [0, 0]];
            nodes.forEach(n => {
                ctx.beginPath();
                ctx.arc(n[0], n[1], 3.5, 0, Math.PI * 2);
                ctx.fill();
            });

            ctx.restore();

            // Atmosfera Estabilizada sem Poluição
            const atmosGrad = ctx.createRadialGradient(cx, cy, earthRadius - 5, cx, cy, earthRadius + 25);
            atmosGrad.addColorStop(0, 'rgba(0,0,0,0)');
            atmosGrad.addColorStop(0.7, 'rgba(56, 189, 248, 0.3)');
            atmosGrad.addColorStop(1, 'rgba(56, 189, 248, 0.0)');

            ctx.beginPath();
            ctx.arc(cx, cy, earthRadius + 25, 0, Math.PI * 2);
            ctx.fillStyle = atmosGrad;
            ctx.fill();

            // 4. Feixes de Energia do Anel para a Terra
            if (state.orbitalRing) {
                beams.forEach(b => {
                    b.progress += b.speed;
                    if (b.progress > 1) {
                        b.progress = 0;
                        b.angle = Math.random() * Math.PI * 2;
                    }

                    const rStart = ringRadius;
                    const rEnd = earthRadius;
                    const currentR = rStart - (rStart - rEnd) * b.progress;

                    const bx = cx + Math.cos(b.angle) * currentR;
                    const by = cy + Math.sin(b.angle) * currentR;

                    ctx.beginPath();
                    ctx.arc(bx, by, 1.8, 0, Math.PI * 2);
                    ctx.fillStyle = `rgba(56, 189, 248, ${1 - b.progress})`;
                    ctx.fill();
                });
            }

            // 5. Redes Geotécnicas e Climáticas
            if (state.seismicControl) {
                ctx.beginPath();
                ctx.arc(cx, cy, earthRadius + 4, 0, Math.PI * 2);
                ctx.strokeStyle = `rgba(16, 185, 129, ${0.25 + Math.sin(state.pulse) * 0.1})`;
                ctx.lineWidth = 1.5;
                ctx.setLineDash([8, 8]);
                ctx.stroke();
                ctx.setLineDash([]);
            }

            requestAnimationFrame(render);
        }

        // Inicialização
        updatePhysics();
        render();
    </script>
</body>
</html>
