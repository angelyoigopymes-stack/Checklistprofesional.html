<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Planificación Diaria Ejecutiva</title>
    
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&display=swap" rel="stylesheet">
    
    <script src="https://cdnjs.cloudflare.com/ajax/libs/html2pdf.js/0.10.1/html2pdf.bundle.min.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/html2canvas/1.4.1/html2canvas.min.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/xlsx/0.18.5/xlsx.full.min.js"></script>

    <style>
        :root {
            --primary-color: #2c3e50;
            --bg-color: #f8f9fa;
            --card-bg: #ffffff;
            --border-color: #e9ecef;
            --text-main: #343a40;
            --text-muted: #6c757d;
            --morning-accent: #f59f00;
            --midday-accent: #1c7ed6;
            --afternoon-accent: #7048e8;
        }

        body {
            font-family: 'Inter', sans-serif;
            background-color: var(--bg-color);
            color: var(--text-main);
            margin: 0;
            padding: 40px 20px;
        }

        #app-container {
            max-width: 850px;
            margin: 0 auto;
            background: var(--card-bg);
            padding: 40px;
            border-radius: 12px;
            box-shadow: 0 10px 30px rgba(0,0,0,0.05);
            border: 1px solid var(--border-color);
        }

        h1 {
            text-align: center;
            color: var(--primary-color);
            font-weight: 700;
            margin-bottom: 40px;
            font-size: 28px;
            letter-spacing: -0.5px;
        }

        .section-container {
            margin-bottom: 30px;
            border-radius: 8px;
            background: var(--card-bg);
            box-shadow: 0 2px 8px rgba(0,0,0,0.03);
            border: 1px solid var(--border-color);
            overflow: hidden;
        }

        .section-morning { border-left: 6px solid var(--morning-accent); }
        .section-midday { border-left: 6px solid var(--midday-accent); }
        .section-afternoon { border-left: 6px solid var(--afternoon-accent); }

        h2 {
            margin: 0;
            padding: 15px 20px;
            font-size: 18px;
            font-weight: 600;
            background-color: #fdfdfd;
            border-bottom: 1px solid var(--border-color);
            color: var(--primary-color);
        }

        .task-list {
            list-style-type: none;
            padding: 0;
            margin: 0;
        }

        .task-item {
            padding: 20px;
            border-bottom: 1px solid var(--border-color);
            transition: background-color 0.2s ease;
        }
        
        .task-item:last-child {
            border-bottom: none;
        }

        .task-item:hover {
            background-color: #fbfbfc;
        }

        .task-header {
            display: flex;
            align-items: flex-start;
            margin-bottom: 12px;
        }

        .task-checkbox {
            width: 22px;
            height: 22px;
            margin-right: 15px;
            margin-top: 2px;
            cursor: pointer;
            accent-color: var(--primary-color);
        }

        .task-label {
            font-size: 16px;
            font-weight: 500;
            color: var(--text-main);
            line-height: 1.4;
        }

        .completed {
            text-decoration: line-through;
            color: var(--text-muted);
        }

        .task-input {
            width: 100%;
            box-sizing: border-box;
            padding: 12px 15px;
            border: 1px solid #ced4da;
            border-radius: 6px;
            font-family: 'Inter', sans-serif;
            font-size: 14px;
            color: var(--text-main);
            background-color: #fafafa;
            resize: vertical; /* Permite agrandar hacia abajo */
            transition: border-color 0.2s, box-shadow 0.2s;
        }

        .task-input:focus {
            outline: none;
            border-color: #80bdff;
            box-shadow: 0 0 0 3px rgba(0,123,255,0.1);
            background-color: #ffffff;
        }

        .task-input::placeholder {
            color: #adb5bd;
        }

        /* Botones Profesionales */
        .controls {
            display: flex;
            flex-wrap: wrap;
            gap: 12px;
            margin-top: 40px;
            justify-content: center;
            padding-top: 20px;
            border-top: 1px solid var(--border-color);
        }

        button {
            padding: 12px 24px;
            border: none;
            border-radius: 6px;
            font-family: 'Inter', sans-serif;
            font-weight: 600;
            font-size: 14px;
            cursor: pointer;
            color: white;
            transition: transform 0.1s, opacity 0.2s, box-shadow 0.2s;
            box-shadow: 0 2px 4px rgba(0,0,0,0.1);
        }

        button:hover { 
            opacity: 0.9;
            transform: translateY(-1px);
            box-shadow: 0 4px 6px rgba(0,0,0,0.15);
        }

        button:active {
            transform: translateY(0);
        }

        .btn-save { background-color: #28a745; }
        .btn-pdf { background-color: #dc3545; }
        .btn-excel { background-color: #198754; }
        .btn-jpeg { background-color: #fd7e14; }
        
        .hide-on-export { display: none !important; }

        @media (max-width: 600px) {
            .controls { flex-direction: column; }
            button { width: 100%; }
        }
    </style>
</head>
<body>

    <div id="app-container">
        <h1>Planificación de Tareas</h1>

        <div class="section-container section-morning" data-etapa="Mañana">
            <h2>Mañana</h2>
            <ul class="task-list">
                <li class="task-item">
                    <div class="task-header">
                        <input type="checkbox" class="task-checkbox">
                        <span class="task-label">Qué tiene que practicar cada chico</span>
                    </div>
                    <textarea class="task-input" rows="2" placeholder="Añadir notas detalladas (espacio para más de 100 caracteres)..."></textarea>
                </li>
                <li class="task-item">
                    <div class="task-header">
                        <input type="checkbox" class="task-checkbox">
                        <span class="task-label">Preparar impacto reunión</span>
                    </div>
                    <textarea class="task-input" rows="2" placeholder="Añadir notas detalladas..."></textarea>
                </li>
                <li class="task-item">
                    <div class="task-header">
                        <input type="checkbox" class="task-checkbox">
                        <span class="task-label">Objetivo diario de cada chico</span>
                    </div>
                    <textarea class="task-input" rows="2" placeholder="Añadir notas detalladas..."></textarea>
                </li>
                <li class="task-item">
                    <div class="task-header">
                        <input type="checkbox" class="task-checkbox">
                        <span class="task-label">Objetivo semanal del equipo</span>
                    </div>
                    <textarea class="task-input" rows="2" placeholder="Añadir notas detalladas..."></textarea>
                </li>
                <li class="task-item">
                    <div class="task-header">
                        <input type="checkbox" class="task-checkbox">
                        <span class="task-label">Zona controlada</span>
                    </div>
                    <textarea class="task-input" rows="2" placeholder="Añadir notas detalladas..."></textarea>
                </li>
            </ul>
        </div>

        <div class="section-container section-midday" data-etapa="Mediodía">
            <h2>Mediodía</h2>
            <ul class="task-list">
                <li class="task-item">
                    <div class="task-header">
                        <input type="checkbox" class="task-checkbox">
                        <span class="task-label">Hacer seguimiento con los chicos, qué hay que mejorar de cara a la tarde</span>
                    </div>
                    <textarea class="task-input" rows="2" placeholder="Añadir notas detalladas..."></textarea>
                </li>
            </ul>
        </div>

        <div class="section-container section-afternoon" data-etapa="Tarde">
            <h2>Tarde</h2>
            <ul class="task-list">
                <li class="task-item">
                    <div class="task-header">
                        <input type="checkbox" class="task-checkbox">
                        <span class="task-label">Hacer seguimiento de incidencias y resolver bloqueos</span>
                    </div>
                    <textarea class="task-input" rows="2" placeholder="Añadir notas detalladas..."></textarea>
                </li>
                <li class="task-item">
                    <div class="task-header">
                        <input type="checkbox" class="task-checkbox">
                        <span class="task-label">Decidir enfoque de práctica para el día siguiente</span>
                    </div>
                    <textarea class="task-input" rows="2" placeholder="Añadir notas detalladas..."></textarea>
                </li>
                <li class="task-item">
                    <div class="task-header">
                        <input type="checkbox" class="task-checkbox">
                        <span class="task-label">Compartir informe final en Drive</span>
                    </div>
                    <textarea class="task-input" rows="2" placeholder="Añadir notas detalladas..."></textarea>
                </li>
                <li class="task-item">
                    <div class="task-header">
                        <input type="checkbox" class="task-checkbox">
                        <span class="task-label">Llamar a Jesús después de las 18:00</span>
                    </div>
                    <textarea class="task-input" rows="2" placeholder="Añadir notas detalladas..."></textarea>
                </li>
                <li class="task-item">
                    <div class="task-header">
                        <input type="checkbox" class="task-checkbox">
                        <span class="task-label">Cargar contratos pendientes en tribu</span>
                    </div>
                    <textarea class="task-input" rows="2" placeholder="Añadir notas detalladas..."></textarea>
                </li>
            </ul>
        </div>

        <div class="controls" id="controls">
            <button class="btn-save" onclick="saveData()">Guardar Progreso</button>
            <button class="btn-pdf" onclick="exportPDF()">Exportar PDF</button>
            <button class="btn-excel" onclick="exportExcel()">Exportar Excel</button>
            <button class="btn-jpeg" onclick="exportJPEG()">Exportar JPEG</button>
        </div>
    </div>

    <script>
        // CARGAR DATOS AL INICIAR
        window.onload = function() {
            loadData();
            
            // Efecto visual al marcar checkbox
            document.querySelectorAll('.task-checkbox').forEach(checkbox => {
                checkbox.addEventListener('change', function() {
                    const label = this.parentElement.querySelector('.task-label');
                    if(this.checked) {
                        label.classList.add('completed');
                    } else {
                        label.classList.remove('completed');
                    }
                });
            });
        };

        // GUARDAR (LocalStorage)
        function saveData() {
            const tasks = [];
            document.querySelectorAll('.task-item').forEach(item => {
                tasks.push({
                    checked: item.querySelector('.task-checkbox').checked,
                    note: item.querySelector('.task-input').value
                });
            });
            localStorage.setItem('taskPlannerData', JSON.stringify(tasks));
            alert('Progreso guardado correctamente.');
        }

        // CARGAR DATOS GUARDADOS
        function loadData() {
            const savedData = localStorage.getItem('taskPlannerData');
            if (savedData) {
                const tasks = JSON.parse(savedData);
                document.querySelectorAll('.task-item').forEach((item, index) => {
                    if (tasks[index]) {
                        const checkbox = item.querySelector('.task-checkbox');
                        checkbox.checked = tasks[index].checked;
                        item.querySelector('.task-input').value = tasks[index].note;
                        
                        if(checkbox.checked) {
                            item.querySelector('.task-label').classList.add('completed');
                        }
                    }
                });
            }
        }

        // EXPORTAR A PDF
        function exportPDF() {
            const element = document.getElementById('app-container');
            const controls = document.getElementById('controls');
            controls.classList.add('hide-on-export');

            const opt = {
                margin:       [15, 15, 15, 15],
                filename:     'planificacion_diaria.pdf',
                image:        { type: 'jpeg', quality: 0.98 },
                html2canvas:  { scale: 2, useCORS: true },
                jsPDF:        { unit: 'mm', format: 'a4', orientation: 'portrait' }
            };

            html2pdf().set(opt).from(element).save().then(() => {
                controls.classList.remove('hide-on-export');
            });
        }

        // EXPORTAR A EXCEL
        function exportExcel() {
            let data = [["Etapa", "Tarea", "Estado", "Notas"]];
            
            const sections = document.querySelectorAll('.section-container');
            sections.forEach(section => {
                const etapa = section.getAttribute('data-etapa');
                const items = section.querySelectorAll('.task-item');
                
                items.forEach(item => {
                    const tarea = item.querySelector('.task-label').innerText;
                    const estado = item.querySelector('.task-checkbox').checked ? "Completado" : "Pendiente";
                    const nota = item.querySelector('.task-input').value;
                    data.push([etapa, tarea, estado, nota]);
                });
            });

            const wb = XLSX.utils.book_new();
            const ws = XLSX.utils.aoa_to_sheet(data);
            
            // Ajustar ancho de columnas en Excel
            ws['!cols'] = [{wch: 15}, {wch: 50}, {wch: 15}, {wch: 60}];
            
            XLSX.utils.book_append_sheet(wb, ws, "Planificación");
            XLSX.writeFile(wb, "planificacion_diaria.xlsx");
        }

        // EXPORTAR A JPEG
        function exportJPEG() {
            const element = document.getElementById('app-container');
            const controls = document.getElementById('controls');
            controls.classList.add('hide-on-export');

            html2canvas(element, { scale: 2, backgroundColor: "#f8f9fa" }).then(canvas => {
                const link = document.createElement('a');
                link.download = 'planificacion_diaria.jpg';
                link.href = canvas.toDataURL('image/jpeg');
                link.click();
                controls.classList.remove('hide-on-export');
            });
        }
    </script>
</body>
</html>
