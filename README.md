
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Matriz de Riesgos Interactiva</title>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/html2canvas/1.4.1/html2canvas.min.js"></script>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
        }

        body {
            background-color: #f5f7fa;
            color: #333;
            line-height: 1.6;
            padding: 20px;
        }

        .container {
            max-width: 1400px;
            margin: 0 auto;
            background: white;
            border-radius: 12px;
            box-shadow: 0 5px 15px rgba(0, 0, 0, 0.08);
            overflow: hidden;
        }

        header {
            background: linear-gradient(135deg, #2c3e50, #4a6491);
            color: white;
            padding: 25px 30px;
            text-align: center;
        }

        h1 {
            font-size: 2.2rem;
            margin-bottom: 10px;
        }

        .subtitle {
            font-size: 1.1rem;
            opacity: 0.9;
            max-width: 800px;
            margin: 0 auto;
        }

        .content {
            display: grid;
            grid-template-columns: 1fr 1.2fr;
            gap: 25px;
            padding: 25px;
        }

        @media (max-width: 1100px) {
            .content {
                grid-template-columns: 1fr;
            }
        }

        .matrix-section {
            background: #f8f9fa;
            border-radius: 10px;
            padding: 20px;
            box-shadow: 0 3px 10px rgba(0, 0, 0, 0.05);
        }

        .controls-section {
            background: #f8f9fa;
            border-radius: 10px;
            padding: 20px;
            box-shadow: 0 3px 10px rgba(0, 0, 0, 0.05);
        }

        h2 {
            color: #2c3e50;
            margin-bottom: 20px;
            padding-bottom: 10px;
            border-bottom: 2px solid #eaecef;
            font-size: 1.5rem;
        }

        .matrix-container {
            display: flex;
            justify-content: center;
            margin: 20px 0;
            overflow: auto;
        }

        .risk-matrix {
            border-collapse: collapse;
            width: 100%;
            max-width: 700px;
            box-shadow: 0 3px 10px rgba(0, 0, 0, 0.1);
            font-size: 14px;
        }

        .risk-matrix th, .risk-matrix td {
            border: 1px solid #ddd;
            padding: 12px;
            text-align: center;
            position: relative;
            min-width: 80px;
        }

        .risk-matrix th {
            background-color: #2c3e50;
            color: white;
            font-weight: 600;
        }

        .risk-matrix td {
            height: 80px;
            font-weight: bold;
            font-size: 16px;
        }

        .green {
            background-color: #4CAF50;
            color: white;
        }

        .yellow {
            background-color: #FFC107;
            color: #333;
        }

        .orange {
            background-color: #FF9800;
            color: white;
        }

        .red {
            background-color: #F44336;
            color: white;
        }

        .risk-marker {
            position: absolute;
            width: 26px;
            height: 26px;
            border-radius: 50%;
            background: #2c3e50;
            color: white;
            display: flex;
            align-items: center;
            justify-content: center;
            font-weight: bold;
            font-size: 12px;
            cursor: pointer;
            box-shadow: 0 2px 5px rgba(0, 0, 0, 0.2);
            z-index: 10;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
        }

        .risk-marker:hover {
            transform: translate(-50%, -50%) scale(1.1);
            z-index: 20;
        }

        .legend {
            display: flex;
            justify-content: center;
            gap: 15px;
            margin-top: 20px;
            flex-wrap: wrap;
        }

        .legend-item {
            display: flex;
            align-items: center;
            gap: 5px;
        }

        .color-box {
            width: 20px;
            height: 20px;
            border-radius: 4px;
        }

        .form-group {
            margin-bottom: 20px;
        }

        label {
            display: block;
            margin-bottom: 8px;
            font-weight: 600;
            color: #2c3e50;
        }

        input, select, textarea {
            width: 100%;
            padding: 12px;
            border: 1px solid #ddd;
            border-radius: 6px;
            font-size: 16px;
            transition: border 0.3s;
        }

        input:focus, select:focus, textarea:focus {
            border-color: #4a6491;
            outline: none;
            box-shadow: 0 0 0 2px rgba(74, 100, 145, 0.2);
        }

        textarea {
            height: 100px;
            resize: vertical;
        }

        .btn {
            background: #4a6491;
            color: white;
            border: none;
            padding: 12px 20px;
            border-radius: 6px;
            cursor: pointer;
            font-size: 16px;
            font-weight: 600;
            transition: background 0.3s;
            display: inline-flex;
            align-items: center;
            justify-content: center;
            gap: 8px;
        }

        .btn:hover {
            background: #3a5379;
        }

        .btn-secondary {
            background: #6c757d;
        }

        .btn-secondary:hover {
            background: #5a6268;
        }

        .btn-danger {
            background: #e74c3c;
        }

        .btn-danger:hover {
            background: #c0392b;
        }

        .btn-success {
            background: #27ae60;
        }

        .btn-success:hover {
            background: #219653;
        }

        .btn-ai {
            background: #9c27b0;
        }

        .btn-ai:hover {
            background: #7b1fa2;
        }

        .buttons {
            display: flex;
            gap: 10px;
            margin-top: 20px;
            flex-wrap: wrap;
        }

        .risk-list {
            margin-top: 20px;
            max-height: 300px;
            overflow-y: auto;
            border: 1px solid #eaecef;
            border-radius: 6px;
        }

        .risk-item {
            padding: 12px 15px;
            border-bottom: 1px solid #eaecef;
            cursor: pointer;
            transition: background 0.2s;
        }

        .risk-item:hover {
            background: #f1f5f9;
        }

        .risk-item.selected {
            background: #e3f2fd;
            border-left: 4px solid #4a6491;
        }

        .risk-info {
            display: flex;
            justify-content: space-between;
            align-items: center;
        }

        .risk-name {
            font-weight: 600;
        }

        .risk-score {
            font-weight: bold;
            padding: 4px 10px;
            border-radius: 20px;
            color: white;
        }

        .risk-details {
            font-size: 0.85rem;
            color: #666;
            margin-top: 5px;
        }

        .result {
            margin-top: 20px;
            padding: 15px;
            border-radius: 6px;
            display: none;
        }

        .tolerable {
            background: #e8f5e9;
            color: #2e7d32;
            border-left: 4px solid #4caf50;
        }

        .significant {
            background: #fff3e0;
            color: #ef6c00;
            border-left: 4px solid #ff9800;
        }

        .unacceptable {
            background: #ffebee;
            color: #c62828;
            border-left: 4px solid #f44336;
        }

        .highlighted {
            box-shadow: inset 0 0 0 3px #2c3e50;
        }

        .empty-message {
            text-align: center;
            padding: 20px;
            color: #777;
            font-style: italic;
        }

        .pdf-section {
            margin-top: 30px;
            padding-top: 20px;
            border-top: 1px solid #eaecef;
            text-align: center;
        }

        footer {
            text-align: center;
            padding: 20px;
            color: #666;
            font-size: 0.9rem;
            border-top: 1px solid #eaecef;
            margin-top: 20px;
        }

        .risk-count {
            display: inline-block;
            background: #4a6491;
            color: white;
            border-radius: 50%;
            width: 24px;
            height: 24px;
            text-align: center;
            line-height: 24px;
            font-size: 0.8rem;
            margin-left: 5px;
        }
        
        /* Estilos para el PDF */
        .pdf-template {
            position: fixed;
            top: -10000px;
            left: -10000px;
            width: 800px;
            padding: 20px;
            background: white;
            z-index: -1000;
        }
        
        /* Nuevos estilos para el análisis de concentración */
        .analysis-section {
            background: #f8f9fa;
            border-radius: 10px;
            padding: 20px;
            margin-top: 25px;
            box-shadow: 0 3px 10px rgba(0, 0, 0, 0.05);
        }
        
        .analysis-grid {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 20px;
            margin-top: 15px;
        }
        
        @media (max-width: 768px) {
            .analysis-grid {
                grid-template-columns: 1fr;
            }
        }
        
        .analysis-chart {
            display: flex;
            flex-direction: column;
            gap: 10px;
        }
        
        .chart-bar {
            display: flex;
            align-items: center;
            gap: 10px;
        }
        
        .chart-label {
            width: 120px;
            font-weight: 600;
        }
        
        .chart-bar-container {
            flex: 1;
            height: 24px;
            background: #e9ecef;
            border-radius: 4px;
            overflow: hidden;
        }
        
        .chart-bar-fill {
            height: 100%;
            display: flex;
            align-items: center;
            justify-content: flex-end;
            padding-right: 8px;
            font-size: 12px;
            font-weight: bold;
            color: white;
            transition: width 0.5s ease;
        }
        
        .chart-bar-fill.green { background-color: #4CAF50; }
        .chart-bar-fill.yellow { background-color: #FFC107; color: #333; }
        .chart-bar-fill.orange { background-color: #FF9800; }
        .chart-bar-fill.red { background-color: #F44336; }
        
        .analysis-recommendations {
            background: white;
            padding: 15px;
            border-radius: 8px;
            border-left: 4px solid #4a6491;
        }
        
        .recommendation-item {
            margin-bottom: 10px;
            padding-left: 15px;
            position: relative;
        }
        
        .recommendation-item:before {
            content: "•";
            position: absolute;
            left: 0;
            color: #4a6491;
            font-weight: bold;
        }
        
        .analysis-summary {
            display: flex;
            justify-content: space-between;
            margin-top: 15px;
            padding-top: 15px;
            border-top: 1px solid #eaecef;
        }
        
        .summary-item {
            text-align: center;
            flex: 1;
        }
        
        .summary-value {
            font-size: 1.5rem;
            font-weight: bold;
            margin-bottom: 5px;
        }
        
        .summary-label {
            font-size: 0.85rem;
            color: #666;
        }
        
        .priority-high {
            color: #F44336;
        }
        
        .priority-medium {
            color: #FF9800;
        }
        
        .priority-low {
            color: #4CAF50;
        }
        
        /* Estilos para la sección de IA */
        .ai-section {
            background: #f8f9fa;
            border-radius: 10px;
            padding: 20px;
            margin-top: 25px;
            box-shadow: 0 3px 10px rgba(0, 0, 0, 0.05);
        }
        
        .ai-analysis {
            background: white;
            padding: 20px;
            border-radius: 8px;
            border-left: 4px solid #9c27b0;
            margin-top: 15px;
            max-height: 400px;
            overflow-y: auto;
        }
        
        .ai-loading {
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            padding: 40px;
            color: #666;
        }
        
        .spinner {
            border: 4px solid #f3f3f3;
            border-top: 4px solid #9c27b0;
            border-radius: 50%;
            width: 40px;
            height: 40px;
            animation: spin 2s linear infinite;
            margin-bottom: 15px;
        }
        
        @keyframes spin {
            0% { transform: rotate(0deg); }
            100% { transform: rotate(360deg); }
        }
        
        .ai-conclusion {
            margin-bottom: 15px;
            padding-bottom: 15px;
            border-bottom: 1px solid #eaecef;
        }
        
        .ai-conclusion:last-child {
            margin-bottom: 0;
            padding-bottom: 0;
            border-bottom: none;
        }
        
        .conclusion-title {
            font-weight: bold;
            color: #2c3e50;
            margin-bottom: 8px;
            display: flex;
            align-items: center;
            gap: 8px;
        }
        
        .conclusion-title i {
            color: #9c27b0;
        }
        
        .conclusion-content {
            line-height: 1.6;
            color: #444;
        }
        
        .ai-error {
            background: #ffebee;
            color: #c62828;
            padding: 15px;
            border-radius: 6px;
            border-left: 4px solid #f44336;
        }
        
        .ai-success {
            background: #e8f5e9;
            color: #2e7d32;
            padding: 10px 15px;
            border-radius: 6px;
            border-left: 4px solid #4caf50;
            margin-bottom: 15px;
        }
    </style>
</head>
<body>
    <div class="container">
        <header>
            <h1 style="text-align:center;">
                Matriz de Riesgos Interactiva <br>
                ELABORADA POR ING. RENATO ALTAMIRANO
            </h1>
            <p class="subtitle">Identifique, evalúe y gestione riesgos utilizando esta herramienta interactiva. Agregue riesgos manualmente, calcule su nivel de criticidad y genere un informe PDF.</p>
        </header>

        <div class="content">
            <div class="matrix-section">
                <h2>Matriz de Riesgos</h2>
                <div class="matrix-container">
                    <table class="risk-matrix" id="riskMatrix">
                        <thead>
                            <tr>
                                <th rowspan="2">PROBABILIDAD \ CONSECUENCIA</th>
                                <th colspan="5">CONSECUENCIA — Lesiones / Impacto</th>
                            </tr>
                            <tr>
                                <th>1<br>Leves</th>
                                <th>2<br>Menores</th>
                                <th>3<br>Graves</th>
                                <th>4<br>Muy graves</th>
                                <th>5<br>Catastróficas</th>
                            </tr>
                        </thead>
                        <tbody>
                            <tr>
                                <th>1 — Raro</th>
                                <td class="green" data-prob="1" data-cons="1">1</td>
                                <td class="green" data-prob="1" data-cons="2">2</td>
                                <td class="green" data-prob="1" data-cons="3">3</td>
                                <td class="green" data-prob="1" data-cons="4">4</td>
                                <td class="green" data-prob="1" data-cons="5">5</td>
                            </tr>
                            <tr>
                                <th>2 — Poco probable</th>
                                <td class="green" data-prob="2" data-cons="1">2</td>
                                <td class="green" data-prob="2" data-cons="2">4</td>
                                <td class="green" data-prob="2" data-cons="3">6</td>
                                <td class="yellow" data-prob="2" data-cons="4">8</td>
                                <td class="yellow" data-prob="2" data-cons="5">10</td>
                            </tr>
                            <tr>
                                <th>3 — Probable</th>
                                <td class="green" data-prob="3" data-cons="1">3</td>
                                <td class="green" data-prob="3" data-cons="2">6</td>
                                <td class="yellow" data-prob="3" data-cons="3">9</td>
                                <td class="yellow" data-prob="3" data-cons="4">12</td>
                                <td class="orange" data-prob="3" data-cons="5">15</td>
                            </tr>
                            <tr>
                                <th>4 — Ocasional</th>
                                <td class="green" data-prob="4" data-cons="1">4</td>
                                <td class="yellow" data-prob="4" data-cons="2">8</td>
                                <td class="yellow" data-prob="4" data-cons="3">12</td>
                                <td class="orange" data-prob="4" data-cons="4">16</td>
                                <td class="red" data-prob="4" data-cons="5">20</td>
                            </tr>
                            <tr>
                                <th>5 — Frecuente</th>
                                <td class="green" data-prob="5" data-cons="1">5</td>
                                <td class="yellow" data-prob="5" data-cons="2">10</td>
                                <td class="orange" data-prob="5" data-cons="3">15</td>
                                <td class="red" data-prob="5" data-cons="4">20</td>
                                <td class="red" data-prob="5" data-cons="5">25</td>
                            </tr>
                        </tbody>
                    </table>
                </div>

                <div class="legend">
                    <div class="legend-item">
                        <div class="color-box green"></div>
                        <span>Tolerable (1-6)</span>
                    </div>
                    <div class="legend-item">
                        <div class="color-box yellow"></div>
                        <span>Significativo (8-12)</span>
                    </div>
                    <div class="legend-item">
                        <div class="color-box orange"></div>
                        <span>Alto (15-16)</span>
                    </div>
                    <div class="legend-item">
                        <div class="color-box red"></div>
                        <span>Inaceptable (20-25)</span>
                    </div>
                </div>

                <div class="pdf-section">
                    <button id="generatePdf" class="btn btn-success">
                        Generar Informe PDF
                    </button>
                </div>
            </div>

            <div class="controls-section">
                <h2>Gestión de Riesgos</h2>

                <div class="form-group">
                    <label for="riskName">Nombre del Riesgo:</label>
                    <input type="text" id="riskName" placeholder="Ej: Derrame de producto químico">
                </div>

                <div class="form-group">
                    <label for="riskDescription">Descripción del Riesgo:</label>
                    <textarea id="riskDescription" placeholder="Describa el riesgo identificado..."></textarea>
                </div>

                <div class="form-group">
                    <label for="probability">Probabilidad (1-5):</label>
                    <select id="probability">
                        <option value="1">1 — Raro (Ocurre excepcionalmente)</option>
                        <option value="2">2 — Poco probable (Podría ocurrir ocasionalmente)</option>
                        <option value="3">3 — Probable (Podría ocurrir en algún momento)</option>
                        <option value="4">4 — Ocasional (Ocurre varias veces al año)</option>
                        <option value="5">5 — Frecuente (Ocurre regularmente)</option>
                    </select>
                </div>

                <div class="form-group">
                    <label for="consequence">Consecuencia (1-5):</label>
                    <select id="consequence">
                        <option value="1">1 — Leves (Sin lesiones, impacto mínimo)</option>
                        <option value="2">2 — Menores (Lesiones leves, impacto bajo)</option>
                        <option value="3">3 — Graves (Lesiones serias, impacto moderado)</option>
                        <option value="4">4 — Muy graves (Lesiones permanentes, impacto alto)</option>
                        <option value="5">5 — Catastróficas (Muerte, impacto muy alto)</option>
                    </select>
                </div>

                <div class="buttons">
                    <button id="calculateRisk" class="btn">Calcular Riesgo</button>
                    <button id="addRisk" class="btn btn-secondary">Agregar a la Lista</button>
                </div>

                <div id="result" class="result"></div>

                <h3>Riesgos Identificados <span id="riskCounter" class="risk-count">0</span></h3>
                <div class="risk-list" id="riskList">
                    <div class="empty-message">No hay riesgos identificados. Agregue el primer riesgo usando el formulario.</div>
                </div>

                <div class="buttons">
                    <button id="clearRisks" class="btn btn-danger">Limpiar Todos los Riesgos</button>
                </div>
            </div>
        </div>
        
        <!-- Nueva sección de análisis de concentración -->
        <div class="analysis-section">
            <h2>Análisis de Concentración de Riesgos</h2>
            <p>Distribución porcentual de riesgos por categoría para determinar dónde enfocar los esfuerzos de gestión.</p>
            
            <div class="analysis-grid">
                <div class="analysis-chart">
                    <div class="chart-bar">
                        <div class="chart-label">Tolerable (1-6)</div>
                        <div class="chart-bar-container">
                            <div id="tolerable-bar" class="chart-bar-fill green" style="width: 0%">0%</div>
                        </div>
                    </div>
                    <div class="chart-bar">
                        <div class="chart-label">Significativo (8-12)</div>
                        <div class="chart-bar-container">
                            <div id="significant-bar" class="chart-bar-fill yellow" style="width: 0%">0%</div>
                        </div>
                    </div>
                    <div class="chart-bar">
                        <div class="chart-label">Alto (15-16)</div>
                        <div class="chart-bar-container">
                            <div id="high-bar" class="chart-bar-fill orange" style="width: 0%">0%</div>
                        </div>
                    </div>
                    <div class="chart-bar">
                        <div class="chart-label">Inaceptable (20-25)</div>
                        <div class="chart-bar-container">
                            <div id="unacceptable-bar" class="chart-bar-fill red" style="width: 0%">0%</div>
                        </div>
                    </div>
                </div>
                
                <div class="analysis-recommendations">
                    <h3>Recomendaciones de Priorización</h3>
                    <div id="recommendations-list">
                        <div class="recommendation-item">Agregue riesgos para ver recomendaciones de priorización</div>
                    </div>
                </div>
            </div>
            
            <div class="analysis-summary">
                <div class="summary-item">
                    <div id="priority-risks" class="summary-value priority-high">0</div>
                    <div class="summary-label">Riesgos Prioritarios</div>
                </div>
                <div class="summary-item">
                    <div id="total-risks" class="summary-value">0</div>
                    <div class="summary-label">Total de Riesgos</div>
                </div>
                <div class="summary-item">
                    <div id="priority-percentage" class="summary-value">0%</div>
                    <div class="summary-label">Porcentaje Prioritario</div>
                </div>
            </div>
        </div>
        
        <!-- Nueva sección de análisis con IA -->
        <div class="ai-section">
            <h2>Análisis Avanzado con IA</h2>
            <p>Obtenga conclusiones y recomendaciones detalladas utilizando inteligencia artificial.</p>
            
            <div class="buttons">
                <button id="analyzeWithAI" class="btn btn-ai">Analizar con IA</button>
            </div>
            
            <div class="ai-analysis" id="aiAnalysis">
                <div class="empty-message">
                    Haga clic en "Analizar con IA" para obtener conclusiones avanzadas sobre sus riesgos.
                    <br><br>
                    <small>El análisis se incluirá automáticamente en el informe PDF.</small>
                </div>
            </div>
        </div>

        <footer>
            <p><strong>Nota:</strong> Esta matriz sigue la escala estándar de evaluación de riesgos: 1-6 Tolerable, 8-12 Significativo, 15-16 Alto, 20-25 Inaceptable.</p>
            <p>Los riesgos marcados como "Inaceptable" requieren acción inmediata y medidas de control prioritarias.</p>
        </footer>
    </div>

    <!-- Template para el PDF (oculto) -->
    <div id="pdfTemplate" class="pdf-template"></div>

    <script>
        // Inicialización de variables
        let risks = [];
        let riskCounter = 1;
        let aiAnalysis = null;
        const matrixValues = [
            [1, 2, 3, 4, 5],
            [2, 4, 6, 8, 10],
            [3, 6, 9, 12, 15],
            [4, 8, 12, 16, 20],
            [5, 10, 15, 20, 25]
        ];

        // API Key integrada
        const API_KEY = "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";

        // Elementos DOM
        const riskNameInput = document.getElementById('riskName');
        const riskDescriptionInput = document.getElementById('riskDescription');
        const probabilitySelect = document.getElementById('probability');
        const consequenceSelect = document.getElementById('consequence');
        const calculateButton = document.getElementById('calculateRisk');
        const addRiskButton = document.getElementById('addRisk');
        const clearRisksButton = document.getElementById('clearRisks');
        const generatePdfButton = document.getElementById('generatePdf');
        const analyzeWithAIButton = document.getElementById('analyzeWithAI');
        const resultDiv = document.getElementById('result');
        const riskListDiv = document.getElementById('riskList');
        const riskCounterSpan = document.getElementById('riskCounter');
        const riskMatrix = document.getElementById('riskMatrix');
        const pdfTemplate = document.getElementById('pdfTemplate');
        const aiAnalysisDiv = document.getElementById('aiAnalysis');
        
        // Elementos para el análisis de concentración
        const tolerableBar = document.getElementById('tolerable-bar');
        const significantBar = document.getElementById('significant-bar');
        const highBar = document.getElementById('high-bar');
        const unacceptableBar = document.getElementById('unacceptable-bar');
        const recommendationsList = document.getElementById('recommendations-list');
        const priorityRisks = document.getElementById('priority-risks');
        const totalRisks = document.getElementById('total-risks');
        const priorityPercentage = document.getElementById('priority-percentage');

        // Función para determinar la categoría del riesgo
        function getRiskCategory(score) {
            if (score <= 6) return { label: 'TOLERABLE', class: 'tolerable' };
            if (score <= 12) return { label: 'SIGNIFICATIVO', class: 'significant' };
            if (score <= 16) return { label: 'ALTO', class: 'unacceptable' };
            return { label: 'INACEPTABLE', class: 'unacceptable' };
        }

        // Función para calcular el riesgo
        function calculateRisk() {
            const probability = parseInt(probabilitySelect.value);
            const consequence = parseInt(consequenceSelect.value);
            
            if (isNaN(probability) || isNaN(consequence)) {
                alert('Por favor, seleccione probabilidad y consecuencia.');
                return null;
            }
            
            const score = matrixValues[probability-1][consequence-1];
            const category = getRiskCategory(score);
            
            return {
                probability,
                consequence,
                score,
                category: category.label,
                categoryClass: category.class
            };
        }

        // Función para mostrar el resultado del cálculo
        function showCalculationResult(riskResult) {
            if (!riskResult) return;
            
            resultDiv.innerHTML = `
                <h3>Resultado del Cálculo:</h3>
                <p><strong>Puntuación:</strong> ${riskResult.score}</p>
                <p><strong>Categoría:</strong> ${riskResult.category}</p>
                <p><strong>Probabilidad:</strong> ${riskResult.probability} (${probabilitySelect.options[probabilitySelect.selectedIndex].text.split('—')[1].trim()})</p>
                <p><strong>Consecuencia:</strong> ${riskResult.consequence} (${consequenceSelect.options[consequenceSelect.selectedIndex].text.split('—')[1].trim()})</p>
            `;
            resultDiv.className = `result ${riskResult.categoryClass}`;
            resultDiv.style.display = 'block';
        }

        // Función para agregar un riesgo a la lista
        function addRisk() {
            const name = riskNameInput.value.trim();
            const description = riskDescriptionInput.value.trim();
            
            if (!name) {
                alert('Por favor, ingrese un nombre para el riesgo.');
                return;
            }
            
            const riskResult = calculateRisk();
            if (!riskResult) return;
            
            const risk = {
                id: riskCounter++,
                name,
                description,
                ...riskResult
            };
            
            risks.push(risk);
            updateRiskList();
            addRiskToMatrix(risk);
            updateRiskCounter();
            updateRiskAnalysis();
            
            // Limpiar formulario
            riskNameInput.value = '';
            riskDescriptionInput.value = '';
            probabilitySelect.selectedIndex = 0;
            consequenceSelect.selectedIndex = 0;
            resultDiv.style.display = 'none';
        }

        // Función para actualizar la lista de riesgos
        function updateRiskList() {
            if (risks.length === 0) {
                riskListDiv.innerHTML = '<div class="empty-message">No hay riesgos identificados. Agregue el primer riesgo usando el formulario.</div>';
                return;
            }
            
            riskListDiv.innerHTML = risks.map(risk => `
                <div class="risk-item" data-id="${risk.id}">
                    <div class="risk-info">
                        <span class="risk-name">${risk.name}</span>
                        <span class="risk-score ${risk.categoryClass}">${risk.score}</span>
                    </div>
                    <div class="risk-details">
                        Prob: ${risk.probability} | Cons: ${risk.consequence} | ${risk.category}
                    </div>
                </div>
            `).join('');
            
            // Agregar event listeners a los elementos de riesgo
            document.querySelectorAll('.risk-item').forEach(item => {
                item.addEventListener('click', function() {
                    const riskId = parseInt(this.getAttribute('data-id'));
                    const risk = risks.find(r => r.id === riskId);
                    if (risk) {
                        showRiskDetails(risk);
                        highlightRiskInMatrix(risk);
                    }
                });
            });
        }

        // Función para agregar un marcador de riesgo a la matriz
        function addRiskToMatrix(risk) {
            const cell = riskMatrix.querySelector(`td[data-prob="${risk.probability}"][data-cons="${risk.consequence}"]`);
            if (!cell) return;
            
            // Eliminar marcador existente para este riesgo (si existe)
            const existingMarker = cell.querySelector(`.risk-marker[data-risk-id="${risk.id}"]`);
            if (existingMarker) existingMarker.remove();
            
            // Crear nuevo marcador
            const marker = document.createElement('div');
            marker.className = 'risk-marker';
            marker.setAttribute('data-risk-id', risk.id);
            marker.textContent = risk.id;
            marker.style.backgroundColor = getRiskColor(risk.category);
            marker.title = `${risk.name} (Puntuación: ${risk.score})`;
            
            // Posicionar aleatoriamente dentro de la celda
            const offsetX = Math.random() * 60 + 20;
            const offsetY = Math.random() * 60 + 20;
            marker.style.left = `${offsetX}%`;
            marker.style.top = `${offsetY}%`;
            
            // Agregar evento para mostrar detalles al hacer clic
            marker.addEventListener('click', function(e) {
                e.stopPropagation();
                showRiskDetails(risk);
                highlightRiskInMatrix(risk);
            });
            
            cell.appendChild(marker);
        }

        // Función para obtener el color según la categoría del riesgo
        function getRiskColor(category) {
            switch(category) {
                case 'TOLERABLE': return '#4CAF50';
                case 'SIGNIFICATIVO': return '#FFC107';
                case 'ALTO': return '#FF9800';
                case 'INACEPTABLE': return '#F44336';
                default: return '#2c3e50';
            }
        }

        // Función para mostrar detalles de un riesgo
        function showRiskDetails(risk) {
            resultDiv.innerHTML = `
                <h3>Detalles del Riesgo #${risk.id}</h3>
                <p><strong>Nombre:</strong> ${risk.name}</p>
                <p><strong>Descripción:</strong> ${risk.description || 'No proporcionada'}</p>
                <p><strong>Puntuación:</strong> ${risk.score}</p>
                <p><strong>Categoría:</strong> ${risk.category}</p>
                <p><strong>Probabilidad:</strong> ${risk.probability}</p>
                <p><strong>Consecuencia:</strong> ${risk.consequence}</p>
            `;
            resultDiv.className = `result ${risk.categoryClass}`;
            resultDiv.style.display = 'block';
            
            // Resaltar elemento en la lista
            document.querySelectorAll('.risk-item').forEach(item => {
                item.classList.remove('selected');
            });
            document.querySelector(`.risk-item[data-id="${risk.id}"]`).classList.add('selected');
        }

        // Función para resaltar un riesgo en la matriz
        function highlightRiskInMatrix(risk) {
            // Quitar resaltado anterior
            document.querySelectorAll('.highlighted').forEach(cell => {
                cell.classList.remove('highlighted');
            });
            
            // Resaltar celda actual
            const cell = riskMatrix.querySelector(`td[data-prob="${risk.probability}"][data-cons="${risk.consequence}"]`);
            if (cell) {
                cell.classList.add('highlighted');
            }
        }

        // Función para actualizar el contador de riesgos
        function updateRiskCounter() {
            riskCounterSpan.textContent = risks.length;
        }

        // Función para actualizar el análisis de concentración de riesgos
        function updateRiskAnalysis() {
            // Calcular distribución por categorías
            const total = risks.length;
            const tolerableCount = risks.filter(r => r.score <= 6).length;
            const significantCount = risks.filter(r => r.score >= 8 && r.score <= 12).length;
            const highCount = risks.filter(r => r.score >= 15 && r.score <= 16).length;
            const unacceptableCount = risks.filter(r => r.score >= 20).length;
            
            // Calcular porcentajes
            const tolerablePercent = total > 0 ? Math.round((tolerableCount / total) * 100) : 0;
            const significantPercent = total > 0 ? Math.round((significantCount / total) * 100) : 0;
            const highPercent = total > 0 ? Math.round((highCount / total) * 100) : 0;
            const unacceptablePercent = total > 0 ? Math.round((unacceptableCount / total) * 100) : 0;
            
            // Actualizar barras de gráfico
            tolerableBar.style.width = `${tolerablePercent}%`;
            tolerableBar.textContent = `${tolerablePercent}%`;
            
            significantBar.style.width = `${significantPercent}%`;
            significantBar.textContent = `${significantPercent}%`;
            
            highBar.style.width = `${highPercent}%`;
            highBar.textContent = `${highPercent}%`;
            
            unacceptableBar.style.width = `${unacceptablePercent}%`;
            unacceptableBar.textContent = `${unacceptablePercent}%`;
            
            // Actualizar resumen
            totalRisks.textContent = total;
            
            const priorityCount = highCount + unacceptableCount;
            priorityRisks.textContent = priorityCount;
            
            const priorityPercent = total > 0 ? Math.round((priorityCount / total) * 100) : 0;
            priorityPercentage.textContent = `${priorityPercent}%`;
            
            // Actualizar color del contador de riesgos prioritarios
            if (priorityPercent >= 50) {
                priorityRisks.className = 'summary-value priority-high';
            } else if (priorityPercent >= 25) {
                priorityRisks.className = 'summary-value priority-medium';
            } else {
                priorityRisks.className = 'summary-value priority-low';
            }
            
            // Actualizar recomendaciones
            updateRecommendations(tolerablePercent, significantPercent, highPercent, unacceptablePercent, priorityPercent);
        }

        // Función para actualizar las recomendaciones de priorización
        function updateRecommendations(tolerablePercent, significantPercent, highPercent, unacceptablePercent, priorityPercent) {
            let recommendationsHTML = '';
            
            if (risks.length === 0) {
                recommendationsHTML = '<div class="recommendation-item">Agregue riesgos para ver recomendaciones de priorización</div>';
            } else {
                // Recomendaciones basadas en la distribución de riesgos
                if (unacceptablePercent > 0) {
                    recommendationsHTML += `<div class="recommendation-item">Concentre el <strong>${unacceptablePercent}%</strong> de sus recursos en los riesgos INACEPTABLES (prioridad máxima).</div>`;
                }
                
                if (highPercent > 0) {
                    recommendationsHTML += `<div class="recommendation-item">Asigne el <strong>${highPercent}%</strong> de sus esfuerzos a los riesgos ALTOS (prioridad alta).</div>`;
                }
                
                if (significantPercent > 0) {
                    recommendationsHTML += `<div class="recommendation-item">Mantenga un monitoreo regular sobre el <strong>${significantPercent}%</strong> de riesgos SIGNIFICATIVOS.</div>`;
                }
                
                if (tolerablePercent > 0) {
                    recommendationsHTML += `<div class="recommendation-item">El <strong>${tolerablePercent}%</strong> de riesgos TOLERABLES pueden gestionarse con controles básicos.</div>`;
                }
                
                // Recomendación general
                if (priorityPercent >= 50) {
                    recommendationsHTML += `<div class="recommendation-item"><strong>ALERTA:</strong> Más de la mitad de sus riesgos son de alta prioridad. Considere reasignar recursos.</div>`;
                } else if (priorityPercent >= 25) {
                    recommendationsHTML += `<div class="recommendation-item"><strong>ATENCIÓN:</strong> Una cuarta parte de sus riesgos requieren atención prioritaria.</div>`;
                } else {
                    recommendationsHTML += `<div class="recommendation-item">Su perfil de riesgo es manejable. Enfóquese en mantener los controles existentes.</div>`;
                }
            }
            
            recommendationsList.innerHTML = recommendationsHTML;
        }

        // Función para limpiar todos los riesgos
        function clearAllRisks() {
            if (risks.length === 0) {
                alert('No hay riesgos para limpiar.');
                return;
            }
            
            if (confirm('¿Está seguro de que desea eliminar todos los riesgos?')) {
                risks = [];
                riskCounter = 1;
                aiAnalysis = null;
                updateRiskList();
                updateRiskCounter();
                updateRiskAnalysis();
                resultDiv.style.display = 'none';
                
                // Eliminar todos los marcadores de la matriz
                document.querySelectorAll('.risk-marker').forEach(marker => {
                    marker.remove();
                });
                
                // Quitar resaltado de celdas
                document.querySelectorAll('.highlighted').forEach(cell => {
                    cell.classList.remove('highlighted');
                });
                
                // Limpiar análisis de IA
                aiAnalysisDiv.innerHTML = '<div class="empty-message">Haga clic en "Analizar con IA" para obtener conclusiones avanzadas sobre sus riesgos.</div>';
            }
        }

        // Función para obtener texto de probabilidad
        function getProbabilityText(prob) {
            const texts = {
                1: 'Raro (Ocurre excepcionalmente)',
                2: 'Poco probable (Podría ocurrir ocasionalmente)',
                3: 'Probable (Podría ocurrir en algún momento)',
                4: 'Ocasional (Ocurre varias veces al año)',
                5: 'Frecuente (Ocurre regularmente)'
            };
            return texts[prob] || '';
        }

        // Función para obtener texto de consecuencia
        function getConsequenceText(cons) {
            const texts = {
                1: 'Leves (Sin lesiones, impacto mínimo)',
                2: 'Menores (Lesiones leves, impacto bajo)',
                3: 'Graves (Lesiones serias, impacto moderado)',
                4: 'Muy graves (Lesiones permanentes, impacto alto)',
                5: 'Catastróficas (Muerte, impacto muy alto)'
            };
            return texts[cons] || '';
        }

        // Función para analizar riesgos con IA
        async function analyzeRisksWithAI() {
            if (risks.length === 0) {
                alert('No hay riesgos para analizar. Agregue al menos un riesgo primero.');
                return;
            }
            
            // Mostrar estado de carga
            aiAnalysisDiv.innerHTML = `
                <div class="ai-loading">
                    <div class="spinner"></div>
                    <p>Analizando riesgos con IA...</p>
                    <small>Esto puede tomar unos segundos</small>
                </div>
            `;
            
            try {
                // Usar OpenAI API con la clave integrada
                const analysis = await callOpenAI();
                
                // Guardar el análisis para incluirlo en el PDF
                aiAnalysis = analysis;
                
                // Mostrar el análisis
                displayAIAnalysis(analysis);
                
                // Mostrar mensaje de éxito
                const successMsg = document.createElement('div');
                successMsg.className = 'ai-success';
                successMsg.innerHTML = '✅ Análisis de IA generado exitosamente. Este análisis se incluirá en el informe PDF.';
                aiAnalysisDiv.insertBefore(successMsg, aiAnalysisDiv.firstChild);
                
            } catch (error) {
                console.error('Error en el análisis con IA:', error);
                aiAnalysisDiv.innerHTML = `
                    <div class="ai-error">
                        <h4>Error en el análisis</h4>
                        <p>No se pudo completar el análisis: ${error.message}</p>
                        <p>Se ha generado un análisis básico en su lugar.</p>
                    </div>
                `;
                
                // Mostrar análisis básico como fallback
                const basicAnalysis = generateBasicAnalysis();
                aiAnalysis = basicAnalysis;
                displayAIAnalysis(basicAnalysis);
            }
        }

        // Función para llamar a la API de OpenAI
        async function callOpenAI() {
            // Preparar datos de riesgos para enviar a la API
            const riskData = risks.map(risk => ({
                name: risk.name,
                description: risk.description,
                probability: risk.probability,
                consequence: risk.consequence,
                score: risk.score,
                category: risk.category
            }));
            
            // Calcular estadísticas
            const total = risks.length;
            const tolerableCount = risks.filter(r => r.score <= 6).length;
            const significantCount = risks.filter(r => r.score >= 8 && r.score <= 12).length;
            const highCount = risks.filter(r => r.score >= 15 && r.score <= 16).length;
            const unacceptableCount = risks.filter(r => r.score >= 20).length;
            
            const prompt = `
            Eres un experto en gestión de riesgos. Analiza la siguiente matriz de riesgos y proporciona un análisis ejecutivo completo en español que incluya:

            1. RESUMEN EJECUTIVO: Perfil general de riesgo y situación actual
            2. DIAGNÓSTICO PRINCIPAL: Análisis de las áreas de mayor preocupación
            3. RECOMENDACIONES ESTRATÉGICAS: Acciones específicas para cada categoría de riesgo
            4. PLAN DE ACCIÓN PRIORIZADO: Pasos concretos a seguir

            DATOS DE RIESGOS:
            ${JSON.stringify(riskData, null, 2)}
            
            ESTADÍSTICAS:
            - Total de riesgos: ${total}
            - Riesgos tolerables: ${tolerableCount} (${Math.round((tolerableCount/total)*100)}%)
            - Riesgos significativos: ${significantCount} (${Math.round((significantCount/total)*100)}%)
            - Riesgos altos: ${highCount} (${Math.round((highCount/total)*100)}%)
            - Riesgos inaceptables: ${unacceptableCount} (${Math.round((unacceptableCount/total)*100)}%)

            Proporciona un análisis estructurado, profesional y accionable. Usa un lenguaje ejecutivo pero claro.
            `;
            
            const response = await fetch('https://api.openai.com/v1/chat/completions', {
                method: 'POST',
                headers: {
                    'Content-Type': 'application/json',
                    'Authorization': `Bearer ${API_KEY}`
                },
                body: JSON.stringify({
                    model: 'gpt-3.5-turbo',
                    messages: [
                        {
                            role: 'system',
                            content: 'Eres un consultor experto en gestión de riesgos con más de 15 años de experiencia. Proporcionas análisis profundos, prácticos y accionables en español.'
                        },
                        {
                            role: 'user',
                            content: prompt
                        }
                    ],
                    max_tokens: 2000,
                    temperature: 0.7
                })
            });
            
            if (!response.ok) {
                const errorData = await response.json();
                throw new Error(`Error en la API: ${response.status} - ${errorData.error?.message || 'Error desconocido'}`);
            }
            
            const data = await response.json();
            return data.choices[0].message.content;
        }

        // Función para generar análisis básico (fallback)
        function generateBasicAnalysis() {
            const total = risks.length;
            const tolerableCount = risks.filter(r => r.score <= 6).length;
            const significantCount = risks.filter(r => r.score >= 8 && r.score <= 12).length;
            const highCount = risks.filter(r => r.score >= 15 && r.score <= 16).length;
            const unacceptableCount = risks.filter(r => r.score >= 20).length;
            
            const tolerablePercent = Math.round((tolerableCount/total)*100);
            const significantPercent = Math.round((significantCount/total)*100);
            const highPercent = Math.round((highCount/total)*100);
            const unacceptablePercent = Math.round((unacceptableCount/total)*100);
            const priorityPercent = Math.round(((highCount + unacceptableCount)/total)*100);
            
            return `
            ANÁLISIS DE RIESGOS - RESUMEN EJECUTIVO

            PERFIL GENERAL DE RIESGO:
            Su organización tiene un total de ${total} riesgos identificados, con la siguiente distribución:
            - ${tolerablePercent}% Riesgos Tolerables (${tolerableCount})
            - ${significantPercent}% Riesgos Significativos (${significantCount}) 
            - ${highPercent}% Riesgos Altos (${highCount})
            - ${unacceptablePercent}% Riesgos Inaceptables (${unacceptableCount})

            DIAGNÓSTICO PRINCIPAL:
            ${priorityPercent >= 50 ? 
                '🚨 ALERTA CRÍTICA: Más del 50% de sus riesgos son de alta prioridad. Se requiere intervención inmediata y reasignación de recursos.' :
            priorityPercent >= 25 ?
                '⚠️ ATENCIÓN REQUERIDA: Una cuarta parte de sus riesgos requieren acción prioritaria. Se recomienda revisión de controles existentes.' :
                '✅ SITUACIÓN MANEJABLE: Su perfil de riesgo es aceptable. Enfóquese en mantener y mejorar los controles actuales.'}

            RECOMENDACIONES ESTRATÉGICAS:

            1. PRIORIZACIÓN DE RECURSOS:
               - Asigne el ${unacceptablePercent}% de recursos a riesgos inaceptables (acción inmediata requerida)
               - Dedique el ${highPercent}% de esfuerzos a riesgos altos (planificación a corto plazo)
               - Monitoree el ${significantPercent}% de riesgos significativos (controles periódicos)
               - Mantenga controles básicos para el ${tolerablePercent}% de riesgos tolerables

            2. PLAN DE ACCIÓN INMEDIATO:
               ${unacceptableCount > 0 ? `• Implemente medidas de control críticas para los ${unacceptableCount} riesgos inaceptables` : ''}
               ${highCount > 0 ? `• Desarrolle planes de mitigación detallados para los ${highCount} riesgos altos` : ''}
               ${significantCount > 0 ? `• Revise y fortalezca controles existentes para los ${significantCount} riesgos significativos` : ''}

            3. SEGUIMIENTO Y MONITOREO:
               • Establezca revisiones trimestrales para riesgos altos e inaceptables
               • Implemente indicadores de desempeño para medir efectividad de controles
               • Considere transferencia de riesgo para exposiciones críticas

            Este análisis se basa en la distribución porcentual de riesgos identificados.
            `;
        }

        // Función para mostrar el análisis de IA
        function displayAIAnalysis(analysis) {
            // Convertir el texto en HTML formateado
            const formattedAnalysis = formatAIAnalysis(analysis);
            aiAnalysisDiv.innerHTML = formattedAnalysis;
        }

        // Función para formatear el análisis de IA
        function formatAIAnalysis(analysis) {
            // Dividir el análisis en secciones
            const sections = analysis.split('\n\n').filter(section => section.trim());
            
            let html = '';
            
            sections.forEach(section => {
                if (section.includes('RESUMEN EJECUTIVO') || section.includes('DIAGNÓSTICO PRINCIPAL') || 
                    section.includes('RECOMENDACIONES ESTRATÉGICAS') || section.includes('PLAN DE ACCIÓN')) {
                    
                    const title = section.split('\n')[0];
                    const content = section.split('\n').slice(1).join('<br>');
                    
                    html += `<div class="ai-conclusion">
                        <div class="conclusion-title">
                            <i>📊</i>
                            ${title}
                        </div>
                        <div class="conclusion-content">
                            ${content}
                        </div>
                    </div>`;
                } else {
                    html += `<div class="conclusion-content">${section.replace(/\n/g, '<br>')}</div>`;
                }
            });
            
            return html;
        }

        // Función para crear la matriz del PDF
        function createPDFMatrix() {
            let matrixHTML = `
                <h1 style="text-align: center; color: #2c3e50; margin-bottom: 10px;">Informe de Matriz de Riesgos</h1>
                <p style="text-align: center; margin-bottom: 20px;">Generado el: ${new Date().toLocaleDateString()}</p>
                
                <table style="border-collapse: collapse; width: 100%; margin-bottom: 20px; font-size: 12px;">
                    <thead>
                        <tr>
                            <th rowspan="2" style="border: 1px solid #ddd; padding: 8px; background: #2c3e50; color: white;">PROBABILIDAD \\ CONSECUENCIA</th>
                            <th colspan="5" style="border: 1px solid #ddd; padding: 8px; background: #2c3e50; color: white;">CONSECUENCIA — Lesiones / Impacto</th>
                        </tr>
                        <tr>
                            <th style="border: 1px solid #ddd; padding: 8px; background: #2c3e50; color: white;">1<br>Leves</th>
                            <th style="border: 1px solid #ddd; padding: 8px; background: #2c3e50; color: white;">2<br>Menores</th>
                            <th style="border: 1px solid #ddd; padding: 8px; background: #2c3e50; color: white;">3<br>Graves</th>
                            <th style="border: 1px solid #ddd; padding: 8px; background: #2c3e50; color: white;">4<br>Muy graves</th>
                            <th style="border: 1px solid #ddd; padding: 8px; background: #2c3e50; color: white;">5<br>Catastróficas</th>
                        </tr>
                    </thead>
                    <tbody>
            `;
            
            const rows = [
                ['1 — Raro', 'green', 'green', 'green', 'green', 'green'],
                ['2 — Poco probable', 'green', 'green', 'green', 'yellow', 'yellow'],
                ['3 — Probable', 'green', 'green', 'yellow', 'yellow', 'orange'],
                ['4 — Ocasional', 'green', 'yellow', 'yellow', 'orange', 'red'],
                ['5 — Frecuente', 'green', 'yellow', 'orange', 'red', 'red']
            ];
            
            rows.forEach((row, rowIndex) => {
                matrixHTML += `
                    <tr>
                        <th style="border: 1px solid #ddd; padding: 8px; background: #2c3e50; color: white;">${row[0]}</th>
                `;
                
                for (let i = 0; i < 5; i++) {
                    const cellRisks = risks.filter(r => 
                        r.probability === rowIndex + 1 && r.consequence === i + 1
                    );
                    
                    const markers = cellRisks.map(r => 
                        `<span style="display: inline-block; width: 18px; height: 18px; border-radius: 50%; background: ${getRiskColor(r.category)}; color: white; font-size: 10px; line-height: 18px; margin: 1px;">${r.id}</span>`
                    ).join(' ');
                    
                    matrixHTML += `
                        <td style="border: 1px solid #ddd; padding: 8px; text-align: center; font-weight: bold; background: ${getColorValue(row[i+1])}; color: ${getTextColor(row[i+1])};">
                            ${matrixValues[rowIndex][i]}<br>${markers}
                        </td>
                    `;
                }
                
                matrixHTML += `</tr>`;
            });
            
            matrixHTML += `
                    </tbody>
                </table>
                <h2 style="margin-top: 30px; color: #2c3e50; border-bottom: 1px solid #ddd; padding-bottom: 5px;">
                    Lista de Riesgos Identificados (Total: ${risks.length})
                </h2>
            `;
            
            return matrixHTML;
        }

        // Función auxiliar para obtener color de fondo
        function getColorValue(color) {
            switch(color) {
                case 'green': return '#4CAF50';
                case 'yellow': return '#FFC107';
                case 'orange': return '#FF9800';
                case 'red': return '#F44336';
                default: return '#f8f9fa';
            }
        }

        // Función auxiliar para obtener color de texto
        function getTextColor(color) {
            return color === 'yellow' ? '#333' : 'white';
        }

        // Función para crear el análisis de concentración para el PDF
        function createPDFAnalysis() {
            // Calcular distribución por categorías
            const total = risks.length;
            const tolerableCount = risks.filter(r => r.score <= 6).length;
            const significantCount = risks.filter(r => r.score >= 8 && r.score <= 12).length;
            const highCount = risks.filter(r => r.score >= 15 && r.score <= 16).length;
            const unacceptableCount = risks.filter(r => r.score >= 20).length;
            
            // Calcular porcentajes
            const tolerablePercent = total > 0 ? Math.round((tolerableCount / total) * 100) : 0;
            const significantPercent = total > 0 ? Math.round((significantCount / total) * 100) : 0;
            const highPercent = total > 0 ? Math.round((highCount / total) * 100) : 0;
            const unacceptablePercent = total > 0 ? Math.round((unacceptableCount / total) * 100) : 0;
            
            const priorityCount = highCount + unacceptableCount;
            const priorityPercent = total > 0 ? Math.round((priorityCount / total) * 100) : 0;
            
            let analysisHTML = `
                <h2 style="margin-top: 30px; color: #2c3e50; border-bottom: 1px solid #ddd; padding-bottom: 5px;">
                    Análisis de Concentración de Riesgos
                </h2>
                <p style="margin-bottom: 15px;">Distribución porcentual de riesgos por categoría para determinar dónde enfocar los esfuerzos de gestión.</p>
                
                <div style="display: grid; grid-template-columns: 1fr 1fr; gap: 20px; margin-bottom: 20px;">
                    <div>
                        <h3 style="color: #2c3e50; margin-bottom: 10px;">Distribución por Categorías</h3>
                        
                        <div style="display: flex; flex-direction: column; gap: 8px;">
                            <div style="display: flex; align-items: center; gap: 10px;">
                                <div style="width: 100px; font-weight: 600;">Tolerable (1-6)</div>
                                <div style="flex: 1; height: 20px; background: #e9ecef; border-radius: 4px; overflow: hidden;">
                                    <div style="height: 100%; width: ${tolerablePercent}%; background: #4CAF50; display: flex; align-items: center; justify-content: flex-end; padding-right: 5px; font-size: 11px; color: white; font-weight: bold;">${tolerablePercent}%</div>
                                </div>
                            </div>
                            
                            <div style="display: flex; align-items: center; gap: 10px;">
                                <div style="width: 100px; font-weight: 600;">Significativo (8-12)</div>
                                <div style="flex: 1; height: 20px; background: #e9ecef; border-radius: 4px; overflow: hidden;">
                                    <div style="height: 100%; width: ${significantPercent}%; background: #FFC107; display: flex; align-items: center; justify-content: flex-end; padding-right: 5px; font-size: 11px; color: #333; font-weight: bold;">${significantPercent}%</div>
                                </div>
                            </div>
                            
                            <div style="display: flex; align-items: center; gap: 10px;">
                                <div style="width: 100px; font-weight: 600;">Alto (15-16)</div>
                                <div style="flex: 1; height: 20px; background: #e9ecef; border-radius: 4px; overflow: hidden;">
                                    <div style="height: 100%; width: ${highPercent}%; background: #FF9800; display: flex; align-items: center; justify-content: flex-end; padding-right: 5px; font-size: 11px; color: white; font-weight: bold;">${highPercent}%</div>
                                </div>
                            </div>
                            
                            <div style="display: flex; align-items: center; gap: 10px;">
                                <div style="width: 100px; font-weight: 600;">Inaceptable (20-25)</div>
                                <div style="flex: 1; height: 20px; background: #e9ecef; border-radius: 4px; overflow: hidden;">
                                    <div style="height: 100%; width: ${unacceptablePercent}%; background: #F44336; display: flex; align-items: center; justify-content: flex-end; padding-right: 5px; font-size: 11px; color: white; font-weight: bold;">${unacceptablePercent}%</div>
                                </div>
                            </div>
                        </div>
                    </div>
                    
                    <div style="background: white; padding: 15px; border-radius: 8px; border-left: 4px solid #4a6491;">
                        <h3 style="color: #2c3e50; margin-bottom: 10px;">Recomendaciones de Priorización</h3>
                        <div style="font-size: 13px;">
            `;
            
            // Agregar recomendaciones
            if (risks.length === 0) {
                analysisHTML += `<p>Agregue riesgos para ver recomendaciones de priorización</p>`;
            } else {
                if (unacceptablePercent > 0) {
                    analysisHTML += `<p style="margin-bottom: 8px;">• Concentre el <strong>${unacceptablePercent}%</strong> de sus recursos en los riesgos INACEPTABLES (acción inmediata requerida)</p>`;
                }
                
                if (highPercent > 0) {
                    analysisHTML += `<p style="margin-bottom: 8px;">• Asigne el <strong>${highPercent}%</strong> de sus esfuerzos a los riesgos ALTOS (prioridad alta)</p>`;
                }
                
                if (significantPercent > 0) {
                    analysisHTML += `<p style="margin-bottom: 8px;">• Mantenga un monitoreo regular sobre el <strong>${significantPercent}%</strong> de riesgos SIGNIFICATIVOS</p>`;
                }
                
                if (tolerablePercent > 0) {
                    analysisHTML += `<p style="margin-bottom: 8px;">• El <strong>${tolerablePercent}%</strong> de riesgos TOLERABLES pueden gestionarse con controles básicos</p>`;
                }
                
                // Recomendación general
                if (priorityPercent >= 50) {
                    analysisHTML += `<p style="margin-bottom: 8px; color: #F44336; font-weight: bold;">• ALERTA: Más de la mitad de sus riesgos son de alta prioridad. Considere reasignar recursos</p>`;
                } else if (priorityPercent >= 25) {
                    analysisHTML += `<p style="margin-bottom: 8px; color: #FF9800; font-weight: bold;">• ATENCIÓN: Una cuarta parte de sus riesgos requieren atención prioritaria</p>`;
                } else {
                    analysisHTML += `<p style="margin-bottom: 8px;">• Su perfil de riesgo es manejable. Enfóquese en mantener los controles existentes</p>`;
                }
            }
            
            analysisHTML += `
                        </div>
                    </div>
                </div>
                
                <div style="display: flex; justify-content: space-between; margin-top: 15px; padding-top: 15px; border-top: 1px solid #eaecef;">
                    <div style="text-align: center; flex: 1;">
                        <div style="font-size: 1.5rem; font-weight: bold; margin-bottom: 5px; ${priorityPercent >= 50 ? 'color: #F44336;' : priorityPercent >= 25 ? 'color: #FF9800;' : 'color: #4CAF50;'}">${priorityCount}</div>
                        <div style="font-size: 0.85rem; color: #666;">Riesgos Prioritarios</div>
                    </div>
                    <div style="text-align: center; flex: 1;">
                        <div style="font-size: 1.5rem; font-weight: bold; margin-bottom: 5px;">${total}</div>
                        <div style="font-size: 0.85rem; color: #666;">Total de Riesgos</div>
                    </div>
                    <div style="text-align: center; flex: 1;">
                        <div style="font-size: 1.5rem; font-weight: bold; margin-bottom: 5px;">${priorityPercent}%</div>
                        <div style="font-size: 0.85rem; color: #666;">Porcentaje Prioritario</div>
                    </div>
                </div>
            `;
            
            return analysisHTML;
        }

        // Función para crear el análisis de IA para el PDF
        function createPDFAIAnalysis() {
            if (!aiAnalysis) {
                return '<div style="color: #666; font-style: italic; text-align: center; padding: 20px;">No se ha generado análisis de IA. Haga clic en "Analizar con IA" para obtener conclusiones avanzadas.</div>';
            }
            
            // Formatear el análisis para el PDF
            const formattedAnalysis = aiAnalysis.replace(/\n/g, '<br>')
                .replace(/\*\*(.*?)\*\*/g, '<strong>$1</strong>')
                .replace(/\*(.*?)\*/g, '<em>$1</em>');
            
            return `
                <h2 style="margin-top: 30px; color: #2c3e50; border-bottom: 1px solid #ddd; padding-bottom: 5px;">
                    Análisis Avanzado con Inteligencia Artificial
                </h2>
                <div style="background: #f8f9fa; padding: 20px; border-radius: 8px; border-left: 4px solid #9c27b0;">
                    <div style="font-size: 14px; line-height: 1.6;">
                        ${formattedAnalysis}
                    </div>
                </div>
                <div style="margin-top: 10px; font-size: 12px; color: #666; text-align: center;">
                    Análisis generado automáticamente mediante IA - ${new Date().toLocaleDateString()}
                </div>
            `;
        }

        // Función para generar PDF con múltiples páginas
        function generatePDF() {
            if (risks.length === 0) {
                alert('No hay riesgos para generar un informe.');
                return;
            }

            const { jsPDF } = window.jspdf;
            const pdf = new jsPDF('p', 'mm', 'a4');
            const pageWidth = pdf.internal.pageSize.getWidth();
            const pageHeight = pdf.internal.pageSize.getHeight();
            const margin = 10;
            let yPosition = margin;

            // Crear contenido de la primera página (matriz)
            pdfTemplate.innerHTML = createPDFMatrix();
            
            // Capturar la matriz como imagen
            html2canvas(pdfTemplate, {
                scale: 2,
                useCORS: true,
                logging: false,
                width: 800,
                height: pdfTemplate.scrollHeight
            }).then(canvas => {
                const imgData = canvas.toDataURL('image/png');
                const imgWidth = pageWidth - (2 * margin);
                const imgHeight = (canvas.height * imgWidth) / canvas.width;
                
                // Agregar matriz al PDF
                pdf.addImage(imgData, 'PNG', margin, yPosition, imgWidth, imgHeight);
                yPosition += imgHeight + 10;
                
                // Verificar si necesitamos nueva página para el análisis
                if (yPosition > pageHeight - 100) {
                    pdf.addPage();
                    yPosition = margin;
                }
                
                // Agregar análisis de concentración
                pdfTemplate.innerHTML = createPDFAnalysis();
                
                // Capturar el análisis como imagen
                return html2canvas(pdfTemplate, {
                    scale: 2,
                    useCORS: true,
                    logging: false,
                    width: 800,
                    height: pdfTemplate.scrollHeight
                });
            }).then(canvas => {
                const imgData = canvas.toDataURL('image/png');
                const imgWidth = pageWidth - (2 * margin);
                const imgHeight = (canvas.height * imgWidth) / canvas.width;
                
                // Verificar si necesitamos nueva página para el análisis
                if (yPosition + imgHeight > pageHeight - margin) {
                    pdf.addPage();
                    yPosition = margin;
                }
                
                // Agregar análisis al PDF
                pdf.addImage(imgData, 'PNG', margin, yPosition, imgWidth, imgHeight);
                yPosition += imgHeight + 10;
                
                // Agregar análisis de IA si existe
                if (aiAnalysis) {
                    if (yPosition > pageHeight - 100) {
                        pdf.addPage();
                        yPosition = margin;
                    }
                    
                    pdfTemplate.innerHTML = createPDFAIAnalysis();
                    
                    return html2canvas(pdfTemplate, {
                        scale: 2,
                        useCORS: true,
                        logging: false,
                        width: 800,
                        height: pdfTemplate.scrollHeight
                    }).then(canvas => {
                        const imgData = canvas.toDataURL('image/png');
                        const imgWidth = pageWidth - (2 * margin);
                        const imgHeight = (canvas.height * imgWidth) / canvas.width;
                        
                        // Verificar si necesitamos nueva página para el análisis de IA
                        if (yPosition + imgHeight > pageHeight - margin) {
                            pdf.addPage();
                            yPosition = margin;
                        }
                        
                        // Agregar análisis de IA al PDF
                        pdf.addImage(imgData, 'PNG', margin, yPosition, imgWidth, imgHeight);
                        yPosition += imgHeight + 10;
                        
                        // Verificar si necesitamos nueva página para los riesgos
                        if (yPosition > pageHeight - 50) {
                            pdf.addPage();
                            yPosition = margin;
                        }
                        
                        // Agregar riesgos detallados
                        addRisksToPDF(pdf, yPosition, pageWidth, pageHeight, margin);
                        
                        // Guardar PDF
                        pdf.save('informe_matriz_riesgos.pdf');
                    });
                } else {
                    // Verificar si necesitamos nueva página para los riesgos
                    if (yPosition > pageHeight - 50) {
                        pdf.addPage();
                        yPosition = margin;
                    }
                    
                    // Agregar riesgos detallados
                    addRisksToPDF(pdf, yPosition, pageWidth, pageHeight, margin);
                    
                    // Guardar PDF
                    pdf.save('informe_matriz_riesgos.pdf');
                }
            }).catch(error => {
                console.error('Error al generar PDF:', error);
                alert('Error al generar el PDF. Por favor, intente nuevamente.');
            });
        }

        // Función para agregar riesgos al PDF con múltiples páginas
        function addRisksToPDF(pdf, startY, pageWidth, pageHeight, margin) {
            let yPosition = startY;
            const risksPerPage = 4; // Número máximo de riesgos por página
            
            // Agrupar riesgos en páginas
            for (let i = 0; i < risks.length; i++) {
                // Si estamos al inicio de un nuevo grupo de riesgos, agregar título
                if (i % risksPerPage === 0) {
                    if (i > 0) {
                        pdf.addPage();
                        yPosition = margin;
                    }
                    
                    pdf.setFontSize(16);
                    pdf.setTextColor(44, 62, 80);
                    pdf.text('Lista Detallada de Riesgos', margin, yPosition);
                    yPosition += 10;
                    
                    pdf.setDrawColor(200, 200, 200);
                    pdf.line(margin, yPosition, pageWidth - margin, yPosition);
                    yPosition += 15;
                }
                
                const risk = risks[i];
                
                // Verificar si hay espacio para otro riesgo
                if (yPosition > pageHeight - 60) {
                    pdf.addPage();
                    yPosition = margin;
                }
                
                // Agregar riesgo
                pdf.setFontSize(12);
                pdf.setTextColor(0, 0, 0);
                
                // Título del riesgo
                pdf.setFont(undefined, 'bold');
                pdf.text(`Riesgo #${risk.id}: ${risk.name}`, margin, yPosition);
                yPosition += 7;
                
                // Descripción
                pdf.setFont(undefined, 'normal');
                const description = risk.description || 'No proporcionada';
                const descriptionLines = pdf.splitTextToSize(`Descripción: ${description}`, pageWidth - (2 * margin));
                pdf.text(descriptionLines, margin, yPosition);
                yPosition += descriptionLines.length * 6 + 5;
                
                // Detalles
                pdf.text(`Probabilidad: ${risk.probability} - ${getProbabilityText(risk.probability)}`, margin, yPosition);
                yPosition += 6;
                
                pdf.text(`Consecuencia: ${risk.consequence} - ${getConsequenceText(risk.consequence)}`, margin, yPosition);
                yPosition += 6;
                
                pdf.text(`Puntuación: ${risk.score}`, margin, yPosition);
                yPosition += 6;
                
                pdf.text(`Categoría: ${risk.category}`, margin, yPosition);
                yPosition += 10;
                
                // Línea separadora
                pdf.setDrawColor(240, 240, 240);
                pdf.line(margin, yPosition, pageWidth - margin, yPosition);
                yPosition += 15;
            }
        }

        // Event Listeners
        calculateButton.addEventListener('click', function() {
            const riskResult = calculateRisk();
            showCalculationResult(riskResult);
        });

        addRiskButton.addEventListener('click', addRisk);
        clearRisksButton.addEventListener('click', clearAllRisks);
        generatePdfButton.addEventListener('click', generatePDF);
        analyzeWithAIButton.addEventListener('click', analyzeRisksWithAI);

        // Permitir agregar riesgo con Enter en el campo de nombre
        riskNameInput.addEventListener('keypress', function(e) {
            if (e.key === 'Enter') {
                addRisk();
            }
        });

        // Inicializar con un riesgo de ejemplo
        window.addEventListener('DOMContentLoaded', function() {
            const exampleRisk = {
                id: riskCounter++,
                name: "Derrame de producto químico",
                description: "Posible derrame durante el transporte de bidones",
                probability: 3,
                consequence: 4,
                score: 12,
                category: "SIGNIFICATIVO",
                categoryClass: "significant"
            };
            
            risks.push(exampleRisk);
            updateRiskList();
            addRiskToMatrix(exampleRisk);
            updateRiskCounter();
            updateRiskAnalysis();
        });
    </script>
</body>
</html>
