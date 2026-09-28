
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Simulador Kardashev: Nível 1.0 - Civilização Planetária</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <style>
        :root {
            --m3c5: #090d16;
            --m3c6: #111827;
            --m3c7: #1f2937;
            --m3c9: #f3f4f6;
            --m3c10: #9ca3af;
            --m3c18: #06b6d4;
            --m3c23: #3b82f6;
        }

        body {
            background-color: var(--m3c5);
            color: var(--m3c9);
            font-family: 'Segoe UI', Roboto, Helvetica, Arial, sans-serif;
        }

        .card {
            background-color: var(--m3c6);
            border: 1px solid var(--m3c7);
        }

        .custom-slider {
            accent-color: var(--m3c18);
        }

        .glow-cyan {
            box-shadow: 0 0 15px rgba(6, 182, 212, 0.25);
        }
    </style>
</head>
<body class="min-h-screen flex flex-col p-4 md:p-6">

    <!-- Top Bar Header -->
    <header class="max-w-7xl mx-auto w-full mb-6 flex flex-col md:flex-row justify-between items-start md:items-center border-b border-gray-800 pb-4 gap-4">
        <div>
            <span class="text-xs font-mono uppercase tracking-widest text-cyan-400">Simulador de Evolução de Civilização</span>
            <h1 class="text-2xl md:text-3xl font-bold flex items-center gap-3">
                Escala Kardashev: <span id="kardashevDisplay" class="text-cyan-400 font-mono">1.000</span>
                <span class="text-xs px-2.5 py-1 rounded-full bg-cyan-500/20 text-cyan-300 border border-cyan-500/40 font-semibold">Nível 1.0 - Civilização Planetária</span>
            </h1>
        </div>
        <div class="flex items-center gap-4">
            <button id="btnAdvance" onclick="advanceYear()" class="bg-cyan-500 hover:bg-cyan-400 text-gray-950 font-bold px-5 py-2.5 rounded-lg shadow-lg shadow-cyan-500/20 transition cursor-pointer flex items-center gap-2">
                <span>Avançar 1 Ano</span>
                <svg class="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M13 5l7 7-7 7M5 5l7 7-7 7"></path></svg>
            </button>
            <button id="btnReset" onclick="resetSimulation()" class="bg-gray-800 hover:bg-gray-700 text-gray-300 px-3 py-2.5 rounded-lg text-sm transition cursor-pointer border border-gray-700">
                Reiniciar
            </button>
        </div>
    </header>

    <!-- Main Dashboard Grid -->
    <main class="max-w-7xl mx-auto w-full grid grid-cols-1 lg:grid-cols-3 gap-6 flex-1">
        
        <!-- Left Column: Technological Pillars & Critical Needs -->
        <section class="space-y-6">
            <!-- Pillars Status -->
            <div class="card p-5 rounded-xl glow-cyan">
                <h2 class="text-md font-bold text-cyan-400 mb-3 uppercase tracking-wider text-xs">⚡ Pilares Tecnológicos do Nível 1.0</h2>
                <div class="space-y-2 text-xs">
                    <div class="flex justify-between items-center bg-gray-800/50 p-2 rounded border border-gray-700">
                        <span>Antimatéria em Pequena Escala</span>
                        <span class="text-emerald-400 font-mono font-bold">Domínio Ativo</span>
                    </div>
                    <div class="flex justify-between items-center bg-gray-800/50 p-2 rounded border border-gray-700">
                        <span>Computação Quântica Universal</span>
                        <span class="text-emerald-400 font-mono font-bold">Operacional</span>
                    </div>
                    <div class="flex justify-between items-center bg-gray-800/50 p-2 rounded border border-gray-700">
                        <span>AGI Integrada Planetária</span>
                        <span class="text-cyan-400 font-mono font-bold">Sincronizada</span>
                    </div>
                </div>
            </div>

            <!-- Controls for Critical Needs -->
            <div class="card p-5 rounded-xl space-y-5">
                <h2 class="text-lg font-bold text-cyan-400 flex items-center gap-2">
                    <span>🛠️ Necessidades Críticas para Avançar</span>
                </h2>
                
                <!-- Megamedium Engineering -->
                <div>
                    <div class="flex justify-between items-center text-sm mb-1">
                        <span class="font-medium text-gray-200">Engenharia de Megameios</span>
                        <span id="megamediumVal" class="font-mono text-cyan-400">20%</span>
                    </div>
                    <input type="range" id="megamediumSlider" min="0" max="100" value="20" class="w-full custom-slider" oninput="updateControls()">
                    <p class="text-xs text-gray-400 mt-1">Manipulação geológica e atmosférica para neutralizar desastres naturais.</p>
                </div>

                <!-- Space Elevator -->
                <div>
                    <div class="flex justify-between items-center text-sm mb-1">
                        <span class="font-medium text-gray-200">Elevadores Espaciais</span>
                        <span id="elevatorVal" class="font-mono text-cyan-400">15%</span>
                    </div>
                    <input type="range" id="elevatorSlider" min="0" max="100" value="15" class="w-full custom-slider" oninput="updateControls()">
                    <p class="text-xs text-gray-400 mt-1">Cabos de nanotubos de carbono/grafeno para transporte barato e contínuo ao espaço.</p>
                </div>

                <!-- Near-Space Control -->
                <div>
                    <div class="flex justify-between items-center text-sm mb-1">
                        <span class="font-medium text-gray-200">Domínio do Espaço Próximo</span>
                        <span id="spaceControlVal" class="font-mono text-cyan-400">30%</span>
                    </div>
                    <input type="range" id="spaceControlSlider" min="0" max="100" value="30" class="w-full custom-slider" oninput="updateControls()">
                    <p class="text-xs text-gray-400 mt-1">Varredura de lixo espacial e construção de usinas/indústrias orbitais.</p>
                </div>
            </div>

            <!-- Planetary Metrics -->
            <div class="card p-5 rounded-xl space-y-4">
                <h3 class="text-sm font-bold text-gray-300 uppercase tracking-wider">Métricas da Infraestrutura Planetária</h3>
                
                <div>
                    <div class="flex justify-between text-xs mb-1">
                        <span>Estabilidade Geoclimática</span>
                        <span id="geoStabilityDisplay" class="font-mono font-bold text-emerald-400">65%</span>
                    </div>
                    <div class="w-full bg-gray-800 h-2 rounded-full overflow-hidden">
                        <div id="geoBar" class="bg-emerald-500 h-full transition-all" style="width: 65%"></div>
                    </div>
                </div>

                <div>
                    <div class="flex justify-between text-xs mb-1">
                        <span>Custo de Carga em Órbita ($/kg)</span>
                        <span id="launchCostDisplay" class="font-mono font-bold text-cyan-400">$2,400</span>
                    </div>
                    <div class="w-full bg-gray-800 h-2 rounded-full overflow-hidden">
                        <div id="launchBar" class="bg-cyan-500 h-full transition-all" style="width: 50%"></div>
                    </div>
                </div>

                <div>
                    <div class="flex justify-between text-xs mb-1">
                        <span>Segurança da Órbita Terrestre (Lixo Removido)</span>
                        <span id="orbitSafetyDisplay" class="font-mono font-bold text-indigo-400">30%</span>
                    </div>
                    <div class="w-full bg-gray-800 h-2 rounded-full overflow-hidden">
                        <div id="orbitBar" class="bg-indigo-500 h-full transition-all" style="width: 30%"></div>
                    </div>
                </div>
            </div>
        </section>

        <!-- Center Column: Domain Impact & Event Log -->
        <section class="space-y-6">
            <div class="card p-5 rounded-xl">
                <h2 class="text-lg font-bold text-cyan-400 mb-4">🌍 Status de Megainfraestrutura</h2>
                
                <div class="space-y-4 text-sm">
                    <!-- Geodynamics -->
                    <div class="p-3 bg-gray-800/60 rounded-lg border border-gray-700">
                        <div class="flex justify-between items-center mb-1">
                            <span class="font-bold text-teal-300">🌋 Controle Geodinâmico</span>
                            <span id="geoBadge" class="text-xs px-2 py-0.5 rounded bg-amber-500/20 text-amber-300 border border-amber-500/30">Fase Teórica</span>
                        </div>
                        <p id="geoDesc" class="text-xs text-gray-300">Aceleração de simulações quânticas ativas. Desastres vulcânicos e sismos ainda parcialmente contidos.</p>
                    </div>

                    <!-- Carbon Cable Structure -->
                    <div class="p-3 bg-gray-800/60 rounded-lg border border-gray-700">
                        <div class="flex justify-between items-center mb-1">
                            <span class="font-bold text-sky-300">🏗️ Elevadores de Grafeno</span>
                            <span id="elevatorBadge" class="text-xs px-2 py-0.5 rounded bg-amber-500/20 text-amber-300 border border-amber-500/30">Protótipo</span>
                        </div>
                        <p id="elevatorDesc" class="text-xs text-gray-300">Cabos de teste ancorados no equador. Foguetes pesados ainda são necessários para cargas industriais.</p>
                    </div>

                    <!-- Orbital Sweeping -->
                    <div class="p-3 bg-gray-800/60 rounded-lg border border-gray-700">
                        <div class="flex justify-between items-center mb-1">
                            <span class="font-bold text-purple-300">🛰️ Espaço Próximo & Indústria</span>
                            <span id="spaceBadge" class="text-xs px-2 py-0.5 rounded bg-amber-500/20 text-amber-300 border border-amber-500/30">Em Limpeza</span>
                        </div>
                        <p id="spaceDesc" class="text-xs text-gray-300">Laser e enxames AGI removendo entulho orbital. Primeiras manufaturas de microgravidade em expansão.</p>
                    </div>

                    <!-- AGI Ecosystem -->
                    <div class="p-3 bg-gray-800/60 rounded-lg border border-gray-700">
                        <div class="flex justify-between items-center mb-1">
                            <span class="font-bold text-cyan-300">🧠 Rede Planetária AGI</span>
                            <span class="text-xs px-2 py-0.5 rounded bg-cyan-500/20 text-cyan-300 border border-cyan-500/30">Sincronizada</span>
                        </div>
                        <p class="text-xs text-gray-300">A AGI gerencia a alocação de energia limpa, logística de suprimentos e otimização dos centros de pesquisa quântica.</p>
                    </div>
                </div>
            </div>

            <!-- Simulation Log -->
            <div class="card p-5 rounded-xl">
                <h3 class="text-md font-bold mb-2 text-gray-200">Diário de Avanço Planetário</h3>
                <div id="simLog" class="h-44 overflow-y-auto space-y-2 text-xs font-mono bg-gray-950/70 p-3 rounded border border-gray-800">
                    <p class="text-cyan-400">[Ano 2100] Início da civilização Tipo 1.0. AGI planetária ativa, energia limpa a 100%.</p>
                </div>
            </div>
        </section>

        <!-- Right Column: Trajectory Chart & Future Outcome -->
        <section class="space-y-6">
            <div class="card p-5 rounded-xl flex flex-col justify-between">
                <div>
                    <h2 class="text-lg font-bold text-cyan-400 mb-1">📈 Rumo ao Tipo II (Estelar)</h2>
                    <p class="text-xs text-gray-400 mb-4">Projeção da escala Kardashev e eficiência da capacidade orbital.</p>
                </div>
                <div class="h-64">
                    <canvas id="kardashevChart"></canvas>
                </div>
            </div>

            <div id="outcomeBox" class="card p-5 rounded-xl border-cyan-500/40">
                <h3 id="outcomeTitle" class="font-bold text-md text-cyan-400 mb-1">Status da Expansão Planetária</h3>
                <p id="outcomeText" class="text-xs text-gray-300 leading-relaxed">
                    A civilização atingiu o Tipo 1.0. Construa os Elevadores Espaciais e domine a Engenharia de Megameios para iniciar megastruturas no Sistema Solar.
                </p>
            </div>
        </section>

    </main>

    <script>
        let year = 2100;
        let kardashevLevel = 1.000;
        let geoStability = 65;
        let launchCost = 2400; // in $/kg
        let orbitSafety = 30;

        let ctx = document.getElementById('kardashevChart').getContext('2d');
        let chart = new Chart(ctx, {
            type: 'line',
            data: {
                labels: [2100],
                datasets: [
                    {
                        label: 'Nível Kardashev',
                        data: [1.000],
                        borderColor: '#06b6d4',
                        backgroundColor: 'rgba(6, 182, 212, 0.1)',
                        yAxisID: 'y',
                        tension: 0.25,
                        fill: true
                    },
                    {
                        label: 'Segurança Órbita (%)',
                        data: [30],
                        borderColor: '#a855f7',
                        borderDash: [4, 4],
                        yAxisID: 'y1',
                        tension: 0.25
                    }
                ]
            },
            options: {
                responsive: true,
                maintainAspectRatio: false,
                scales: {
                    x: {
                        ticks: { color: '#9ca3af' },
                        grid: { color: '#1f2937' }
                    },
                    y: {
                        type: 'linear',
                        display: true,
                        position: 'left',
                        min: 0.99,
                        max: 1.30,
                        ticks: { color: '#06b6d4' },
                        grid: { color: '#1f2937' }
                    },
                    y1: {
                        type: 'linear',
                        display: true,
                        position: 'right',
                        min: 0,
                        max: 100,
                        ticks: { color: '#a855f7' },
                        grid: { drawOnChartArea: false }
                    }
                },
                plugins: {
                    legend: { labels: { color: '#f3f4f6', boxWidth: 12 } }
                }
            }
        });

        function updateControls() {
            let mega = parseInt(document.getElementById('megamediumSlider').value);
            let elev = parseInt(document.getElementById('elevatorSlider').value);
            let space = parseInt(document.getElementById('spaceControlSlider').value);

            document.getElementById('megamediumVal').innerText = mega + '%';
            document.getElementById('elevatorVal').innerText = elev + '%';
            document.getElementById('spaceControlVal').innerText = space + '%';

            // Metrics Logic
            geoStability = Math.min(100, Math.round(50 + (mega * 0.5)));
            launchCost = Math.max(5, Math.round(2500 - (elev * 24.5)));
            orbitSafety = Math.min(100, Math.round(15 + (space * 0.85)));

            // Update UI Bars
            document.getElementById('geoStabilityDisplay').innerText = geoStability + '%';
            document.getElementById('geoBar').style.width = geoStability + '%';

            document.getElementById('launchCostDisplay').innerText = '$' + launchCost + ' / kg';
            document.getElementById('launchBar').style.width = Math.max(2, (launchCost / 2500) * 100) + '%';

            document.getElementById('orbitSafetyDisplay').innerText = orbitSafety + '%';
            document.getElementById('orbitBar').style.width = orbitSafety + '%';

            updateDomainStates(mega, elev, space);
        }

        function updateDomainStates(mega, elev, space) {
            // Geodynamics
            let geoBadge = document.getElementById('geoBadge');
            let geoDesc = document.getElementById('geoDesc');
            if (mega > 70) {
                geoBadge.innerText = "Controle Pleno";
                geoBadge.className = "text-xs px-2 py-0.5 rounded bg-emerald-500/20 text-emerald-300 border border-emerald-500/30";
                geoDesc.innerText = "Geologia e atmosfera totalmente moduladas. Mórmons, terremotos e ciclones foram erradicados.";
            } else if (mega > 35) {
                geoBadge.innerText = "Estabilização Parcial";
                geoBadge.className = "text-xs px-2 py-0.5 rounded bg-cyan-500/20 text-cyan-300 border border-cyan-500/30";
                geoDesc.innerText = "Grandes tempestades e atividades vulcânicas severas são atenuadas antecipadamente por AGI.";
            } else {
                geoBadge.innerText = "Fase Teórica";
                geoBadge.className = "text-xs px-2 py-0.5 rounded bg-amber-500/20 text-amber-300 border border-amber-500/30";
                geoDesc.innerText = "Aceleração de simulações quânticas ativas. Desastres vulcânicos e sismos ainda parcialmente contidos.";
            }

            // Space Elevator
            let elevBadge = document.getElementById('elevatorBadge');
            let elevDesc = document.getElementById('elevatorDesc');
            if (elev > 75) {
                elevBadge.innerText = "Rede Operacional";
                elevBadge.className = "text-xs px-2 py-0.5 rounded bg-emerald-500/20 text-emerald-300 border border-emerald-500/30";
                elevDesc.innerText = "Múltiplos elevadores espaciais de grafeno movem gigatons de material à órbita sem combustão.";
            } else if (elev > 35) {
                elevBadge.innerText = "Espinha Dorsal Pronta";
                elevBadge.className = "text-xs px-2 py-0.5 rounded bg-cyan-500/20 text-cyan-300 border border-cyan-500/30";
                elevDesc.innerText = "Primeiro elevador comercial ativo no equador, reduzindo drasticamente custos astronômicos.";
            } else {
                elevBadge.innerText = "Protótipo";
                elevBadge.className = "text-xs px-2 py-0.5 rounded bg-amber-500/20 text-amber-300 border border-amber-500/30";
                elevDesc.innerText = "Cabos de teste ancorados no equador. Foguetes pesados ainda são necessários para cargas industriais.";
            }

            // Orbit & Industry
            let spaceBadge = document.getElementById('spaceBadge');
            let spaceDesc = document.getElementById('spaceDesc');
            if (space > 70) {
                spaceBadge.innerText = "Órbita Limpa & Industrial";
                spaceBadge.className = "text-xs px-2 py-0.5 rounded bg-purple-500/20 text-purple-300 border border-purple-500/30";
                spaceDesc.innerText = "Órbita totalmente limpa. Fornos solares e fundições orbitais abastecem a construção de frotas.";
            } else {
                spaceBadge.innerText = "Em Limpeza";
                spaceBadge.className = "text-xs px-2 py-0.5 rounded bg-amber-500/20 text-amber-300 border border-amber-500/30";
                spaceDesc.innerText = "Laser e enxames AGI removendo entulho orbital. Primeiras manufaturas de microgravidade em expansão.";
            }
        }

        function advanceYear() {
            year += 1;

            let mega = parseInt(document.getElementById('megamediumSlider').value);
            let elev = parseInt(document.getElementById('elevatorSlider').value);
            let space = parseInt(document.getElementById('spaceControlSlider').value);

            // Calculation of Kardashev progression above Level 1.0
            let growth = (mega * 0.001) + (elev * 0.0012) + (space * 0.001);
            kardashevLevel = Math.min(1.30, Math.round((kardashevLevel + growth) * 1000) / 1000);

            document.getElementById('kardashevDisplay').innerText = kardashevLevel.toFixed(3);

            // Log
            let log = document.getElementById('simLog');
            let newLog = document.createElement('p');

            if (elev > 70 && space > 70 && mega > 70) {
                newLog.className = "text-emerald-400 font-bold";
                newLog.innerText = `[Ano ${year}] MARCO DE EXPANSÃO! Início da construção da Esfera de Dyson Parcial em torno do Sol!`;
            } else {
                newLog.className = "text-gray-300";
                newLog.innerText = `[Ano ${year}] Kardashev: ${kardashevLevel.toFixed(3)} | Custo Órbita: $${launchCost}/kg | Segurança Órbita: ${orbitSafety}%`;
            }
            log.prepend(newLog);

            // Chart update
            chart.data.labels.push(year);
            chart.data.datasets[0].data.push(kardashevLevel);
            chart.data.datasets[1].data.push(orbitSafety);
            chart.update();

            checkStatus();
        }

        function checkStatus() {
            let title = document.getElementById('outcomeTitle');
            let text = document.getElementById('outcomeText');
            let box = document.getElementById('outcomeBox');

            if (kardashevLevel >= 1.20) {
                title.innerText = "🚀 Rumo ao Tipo II (Civilização Estelar)";
                title.className = "font-bold text-md text-purple-400 mb-1";
                box.className = "card p-5 rounded-xl border-purple-500/50 bg-purple-950/10";
                text.innerText = "A humanidade superou os limites planetários. Os elevadores espaciais operam continuamente e a indústria orbital está pronta para colonizar o Sistema Solar.";
            } else if (launchCost < 100 && geoStability > 80) {
                title.innerText = "⚡ Infraestrutura Planetária Consolidada";
                title.className = "font-bold text-md text-emerald-400 mb-1";
                box.className = "card p-5 rounded-xl border-emerald-500/50 bg-emerald-950/10";
                text.innerText = "A Terra está protegida contra desastres naturais e o acesso ao espaço tornou-se tão simples e acessível quanto o transporte marítimo.";
            } else {
                title.innerText = "Status da Expansão Planetária";
                title.className = "font-bold text-md text-cyan-400 mb-1";
                box.className = "card p-5 rounded-xl border-cyan-500/40";
                text.innerText = `Progresso no Nível ${kardashevLevel.toFixed(3)}. Aumente os investimentos nos Elevadores Espaciais e no Controle do Espaço Próximo para acelerar a expansão.`;
            }
        }

        function resetSimulation() {
            year = 2100;
            kardashevLevel = 1.000;

            document.getElementById('megamediumSlider').value = 20;
            document.getElementById('elevatorSlider').value = 15;
            document.getElementById('spaceControlSlider').value = 30;

            document.getElementById('simLog').innerHTML = '<p class="text-cyan-400">[Ano 2100] Simulação reiniciada no Nível Kardashev 1.000.</p>';
            
            chart.data.labels = [2100];
            chart.data.datasets[0].data = [1.000];
            chart.data.datasets[1].data = [30];
            chart.update();

            updateControls();
            document.getElementById('kardashevDisplay').innerText = '1.000';
        }

        // Initial setup
        updateControls();
    </script>
</body>
</html>
