<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Simulador 3D: Hidráulica de Canales Abiertos</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/three.js/r128/three.min.js"></script>
    <!-- OrbitControls para interactuar con la cámara -->
    <script src="https://cdn.jsdelivr.net/npm/three@0.128.0/examples/js/controls/OrbitControls.js"></script>
    <!-- Dat.GUI para la interfaz de controles -->
    <script src="https://cdnjs.cloudflare.com/ajax/libs/dat-gui/0.7.9/dat.gui.min.js"></script>
    <style>
        body { margin: 0; overflow: hidden; background-color: #1a202c; font-family: 'Inter', sans-serif; color: white; }
        #canvas-container { width: 100vw; height: 100vh; position: absolute; top: 0; left: 0; z-index: 1; }
        #ui-panel { position: absolute; top: 20px; left: 20px; z-index: 10; background: rgba(15, 23, 42, 0.85); padding: 20px; border-radius: 12px; box-shadow: 0 4px 6px rgba(0,0,0,0.3); backdrop-filter: blur(4px); max-width: 350px; pointer-events: none; }
        #ui-panel h1 { font-size: 1.25rem; font-weight: bold; margin-bottom: 10px; color: #60a5fa; text-shadow: 0 1px 2px rgba(0,0,0,0.5); }
        #ui-panel p { font-size: 0.875rem; margin-bottom: 8px; color: #cbd5e1; }
        .data-display { background: rgba(0,0,0,0.3); padding: 10px; border-radius: 8px; margin-top: 15px; border: 1px solid #334155; }
        .data-row { display: flex; justify-content: space-between; margin-bottom: 4px; font-family: monospace; font-size: 0.9rem; }
        .data-label { color: #94a3b8; }
        .data-value { color: #38bdf8; font-weight: bold; }
        /* Ajustar Dat.GUI para que se vea bien en móviles si es necesario */
        .dg.ac { z-index: 20 !important; top: 20px !important; right: 20px !important; }
    </style>
</head>
<body>

    <div id="ui-panel">
        <h1>Simulador de Canal Abierto</h1>
        <p>Línea de Investigación: Hidráulica de Canales y Redes.</p>
        <p class="text-xs text-gray-400 mb-2">Usa el ratón para rotar, hacer zoom y mover la cámara.</p>
        
        <div class="data-display">
            <div class="data-row">
                <span class="data-label">Tirante Normal (y):</span>
                <span id="out-tirante" class="data-value">0.00 m</span>
            </div>
            <div class="data-row">
                <span class="data-label">Velocidad Media (V):</span>
                <span id="out-velocidad" class="data-value">0.00 m/s</span>
            </div>
            <div class="data-row">
                <span class="data-label">Área Mojada (A):</span>
                <span id="out-area" class="data-value">0.00 m²</span>
            </div>
            <div class="data-row">
                <span class="data-label">Radio Hidráulico (R):</span>
                <span id="out-radio" class="data-value">0.00 m</span>
            </div>
        </div>
    </div>

    <div id="canvas-container"></div>

    <script>
        // Parámetros de la simulación y estado
        const simParams = {
            caudal: 15,          // Q (m3/s)
            pendiente: 0.002,     // S (m/m) - exagerado visualmente
            rugosidad: 0.015,     // n (Manning)
            anchoCanal: 4,        // b (m)
            longitudCanal: 30,    // L (m)
            exageracionZ: 10      // Factor para visualizar la pendiente mejor
        };

        // Variables calculadas
        let tiranteNormal = 1.0;
        let velocidadMedia = 1.0;
        
        // Elementos DOM
        const outTirante = document.getElementById('out-tirante');
        const outVelocidad = document.getElementById('out-velocidad');
        const outArea = document.getElementById('out-area');
        const outRadio = document.getElementById('out-radio');

        // Configuración de Three.js
        let scene, camera, renderer, controls;
        
        // Objetos 3D
        let canalMesh, aguaMesh, flechasVelocidadGroup;
        let waterTexture;
        let time = 0;

        init();
        animate();

        function init() {
            // 1. Escena
            scene = new THREE.Scene();
            scene.background = new THREE.Color(0x87CEEB); // Cielo azul claro
            scene.fog = new THREE.Fog(0x87CEEB, 20, 100);

            // 2. Cámara
            camera = new THREE.PerspectiveCamera(45, window.innerWidth / window.innerHeight, 0.1, 1000);
            camera.position.set(-15, 10, 20);

            // 3. Renderizador
            renderer = new THREE.WebGLRenderer({ antialias: true });
            renderer.setSize(window.innerWidth, window.innerHeight);
            renderer.shadowMap.enabled = true;
            renderer.shadowMap.type = THREE.PCFSoftShadowMap;
            document.getElementById('canvas-container').appendChild(renderer.domElement);

            // 4. Controles (OrbitControls)
            controls = new THREE.OrbitControls(camera, renderer.domElement);
            controls.enableDamping = true;
            controls.dampingFactor = 0.05;
            controls.target.set(0, 0, 0);

            // 5. Iluminación
            const ambientLight = new THREE.AmbientLight(0xffffff, 0.6);
            scene.add(ambientLight);

            const dirLight = new THREE.DirectionalLight(0xffffff, 0.8);
            dirLight.position.set(20, 30, -10);
            dirLight.castShadow = true;
            dirLight.shadow.mapSize.width = 1024;
            dirLight.shadow.mapSize.height = 1024;
            dirLight.shadow.camera.near = 0.5;
            dirLight.shadow.camera.far = 100;
            const d = 25;
            dirLight.shadow.camera.left = -d;
            dirLight.shadow.camera.right = d;
            dirLight.shadow.camera.top = d;
            dirLight.shadow.camera.bottom = -d;
            scene.add(dirLight);

            // 6. Entorno (Suelo)
            const groundGeo = new THREE.PlaneGeometry(200, 200);
            const groundMat = new THREE.MeshLambertMaterial({ color: 0x4ade80 }); // Verde césped
            const ground = new THREE.Mesh(groundGeo, groundMat);
            ground.rotation.x = -Math.PI / 2;
            ground.position.y = -2; // Bajar un poco para que el canal quede incrustado
            ground.receiveShadow = true;
            scene.add(ground);

            // 7. Construir Canal
            construirCanal();

            // 8. Interfaz GUI (Dat.GUI)
            setupGUI();

            // 9. Calcular valores iniciales
            calcularHidraulica();

            // Evento resize
            window.addEventListener('resize', onWindowResize, false);
        }

        function construirCanal() {
            // Eliminar objetos anteriores si existen (para cuando se reconstruye)
            if (canalMesh) scene.remove(canalMesh);
            if (aguaMesh) scene.remove(aguaMesh);
            if (flechasVelocidadGroup) scene.remove(flechasVelocidadGroup);

            const longitud = simParams.longitudCanal;
            const ancho = simParams.anchoCanal;
            const profundidad = 3; // Profundidad total física del canal
            const grosorPared = 0.5;
            
            // Pendiente visual exagerada
            const desnivel = longitud * simParams.pendiente * simParams.exageracionZ;

            // --- Materiales ---
            // Color del concreto basado en rugosidad (más oscuro = más rugoso)
            const colorHormigon = new THREE.Color().setHSL(0, 0, 0.8 - (simParams.rugosidad * 10));
            const canalMat = new THREE.MeshLambertMaterial({ color: colorHormigon });

            // --- Geometría del Canal (Base + Paredes) ---
            const canalGroup = new THREE.Group();

            // Base
            const baseGeo = new THREE.BoxGeometry(longitud, grosorPared, ancho + grosorPared * 2);
            const baseMesh = new THREE.Mesh(baseGeo, canalMat);
            baseMesh.position.y = -grosorPared / 2;
            baseMesh.receiveShadow = true;
            baseMesh.castShadow = true;
            canalGroup.add(baseMesh);

            // Pared 1
            const paredGeo = new THREE.BoxGeometry(longitud, profundidad, grosorPared);
            const pared1 = new THREE.Mesh(paredGeo, canalMat);
            pared1.position.set(0, profundidad/2 - grosorPared/2, ancho/2 + grosorPared/2);
            pared1.receiveShadow = true;
            pared1.castShadow = true;
            canalGroup.add(pared1);

            // Pared 2
            const pared2 = new THREE.Mesh(paredGeo, canalMat);
            pared2.position.set(0, profundidad/2 - grosorPared/2, -ancho/2 - grosorPared/2);
            pared2.receiveShadow = true;
            pared2.castShadow = true;
            canalGroup.add(pared2);

            // Inclinar todo el canal según la pendiente
            // Pivot en el centro, el inicio (x = longitud/2) estará más alto, el final (x = -longitud/2) más bajo
            const anguloInclinacion = Math.atan(desnivel / longitud);
            canalGroup.rotation.z = -anguloInclinacion; // Inclinar en Z para subir/bajar a lo largo del eje X
            
            canalMesh = canalGroup;
            scene.add(canalMesh);

            // --- Agua ---
            // Creamos una textura procedural simple para el agua o usamos un color base
            const aguaGeo = new THREE.PlaneGeometry(longitud, ancho, Math.floor(longitud), Math.floor(ancho));
            // Rotar plano para que esté horizontal
            aguaGeo.rotateX(-Math.PI / 2);

            // Material de agua semitransparente
            const aguaMat = new THREE.MeshPhongMaterial({ 
                color: 0x0ea5e9, 
                transparent: true, 
                opacity: 0.7,
                shininess: 90,
                side: THREE.DoubleSide
            });

            aguaMesh = new THREE.Mesh(aguaGeo, aguaMat);
            
            // La posición Y del agua se actualizará en la función de cálculo
            // La rotación también debe coincidir con la del canal
            aguaMesh.rotation.z = -anguloInclinacion;
            
            scene.add(aguaMesh);

            // --- Indicadores de Velocidad (Partículas/Flechas) ---
            crearIndicadoresVelocidad();
        }

        function crearIndicadoresVelocidad() {
            flechasVelocidadGroup = new THREE.Group();
            
            const numParticulas = 50;
            const geoParticula = new THREE.SphereGeometry(0.15, 8, 8);
            const matParticula = new THREE.MeshBasicMaterial({ color: 0xffffff });

            for (let i = 0; i < numParticulas; i++) {
                const particula = new THREE.Mesh(geoParticula, matParticula);
                // Posiciones iniciales aleatorias a lo largo del canal y ancho
                particula.position.x = (Math.random() - 0.5) * simParams.longitudCanal;
                particula.position.z = (Math.random() - 0.5) * (simParams.anchoCanal - 0.5);
                // Velocidad base aleatoria para que no se muevan todas igual
                particula.userData.velocidadRelativa = 0.8 + Math.random() * 0.4; 
                particula.userData.faseDesfase = Math.random() * Math.PI * 2;
                
                flechasVelocidadGroup.add(particula);
            }
            
            // Inclinamos el grupo de flechas igual que el canal
            const desnivel = simParams.longitudCanal * simParams.pendiente * simParams.exageracionZ;
            const anguloInclinacion = Math.atan(desnivel / simParams.longitudCanal);
            flechasVelocidadGroup.rotation.z = -anguloInclinacion;
            
            scene.add(flechasVelocidadGroup);
        }

        function calcularHidraulica() {
            // Ecuación de Manning (Sistema Métrico)
            // Q = (1/n) * A * R^(2/3) * S^(1/2)
            // Para una sección rectangular: A = b * y, P = b + 2y, R = A / P
            
            const Q = simParams.caudal;
            const n = simParams.rugosidad;
            const S = simParams.pendiente;
            const b = simParams.anchoCanal;

            // No se puede despejar 'y' directamente (ecuación implícita).
            // Usaremos el método de Newton-Raphson para encontrar la raíz f(y) = 0
            // f(y) = A * R^(2/3) - (Q * n) / sqrt(S) = 0
            
            const target = (Q * n) / Math.sqrt(S);
            
            // Valores iniciales para Newton-Raphson
            let y = 1.0; // Suposición inicial
            const tolerancia = 0.0001;
            let iteraciones = 0;
            const maxIter = 100;

            while(iteraciones < maxIter) {
                let A = b * y;
                let P = b + 2 * y;
                let R = A / P;
                
                // f(y) = A * (A/P)^(2/3) - target
                let fy = A * Math.pow(R, 2/3) - target;
                
                if (Math.abs(fy) < tolerancia) break; // Encontrado

                // Derivada f'(y) (aproximada numéricamente para simplificar)
                let dy = 0.001;
                let y_plus = y + dy;
                let A_plus = b * y_plus;
                let P_plus = b + 2 * y_plus;
                let R_plus = A_plus / P_plus;
                let fy_plus = A_plus * Math.pow(R_plus, 2/3) - target;
                
                let f_prime = (fy_plus - fy) / dy;
                
                y = y - (fy / f_prime); // Siguiente aproximación
                
                // Evitar valores negativos o demasiado grandes
                if (y <= 0.01) y = 0.01;
                if (y > 10) { y = 10; break; }
                
                iteraciones++;
            }

            tiranteNormal = y;
            
            // Cálculos finales
            const AreaFinal = b * tiranteNormal;
            const PerimetroFinal = b + 2 * tiranteNormal;
            const RadioHidraulico = AreaFinal / PerimetroFinal;
            velocidadMedia = Q / AreaFinal;

            // --- Actualizar UI ---
            outTirante.textContent = tiranteNormal.toFixed(3) + " m";
            outVelocidad.textContent = velocidadMedia.toFixed(2) + " m/s";
            outArea.textContent = AreaFinal.toFixed(2) + " m²";
            outRadio.textContent = RadioHidraulico.toFixed(3) + " m";

            // --- Actualizar 3D ---
            actualizarVisualizacion();
        }

        function actualizarVisualizacion() {
            if (!aguaMesh) return;

            // 1. Actualizar altura del agua
            // El canal base está en Y = -grosor/2, la superficie del canal interno está en Y = 0 (relativo al grupo inclinado)
            // Simplemente movemos el agua en Y localmente dentro del espacio ya rotado
            aguaMesh.position.y = tiranteNormal;
            
            // 2. Actualizar altura de las partículas
            if (flechasVelocidadGroup) {
                flechasVelocidadGroup.children.forEach(p => {
                    // Posicionarlas cerca de la superficie del agua, flotando
                    p.position.y = tiranteNormal;
                });
            }

            // 3. Reconstruir canal si cambia el ancho o pendiente (para no complicar deformaciones complejas)
            // Lo hacemos chequeando si la geometría actual coincide con los params
            const anchoActual = canalMesh.children[0].geometry.parameters.depth - 1; // - (grosor*2)
            // (La pendiente es más compleja de chequear, mejor forzamos reconstrucción en el evento del GUI)
        }

        function setupGUI() {
            const gui = new dat.GUI({ title: 'Parámetros del Canal' });
            
            // Asegurar que el GUI esté por encima
            gui.domElement.style.zIndex = "20";

            // Carpeta Hidráulica
            const folderHidro = gui.addFolder('Variables Hidráulicas');
            folderHidro.add(simParams, 'caudal', 1, 100).name('Caudal (m³/s)').onChange(calcularHidraulica);
            // Pendiente: de 0.0001 (muy suave) a 0.05 (muy pronunciada)
            folderHidro.add(simParams, 'pendiente', 0.0001, 0.05).step(0.0001).name('Pendiente (S)').onChange(() => {
                construirCanal(); // Necesita reconstruir para la inclinación visual
                calcularHidraulica();
            });
            // Rugosidad (Manning): 0.010 (vidrio/plástico) a 0.035 (tierra/piedra)
            folderHidro.add(simParams, 'rugosidad', 0.010, 0.040).step(0.001).name('Rugosidad (n)').onChange(() => {
                construirCanal(); // Reconstruye para cambiar color del material
                calcularHidraulica();
            });
            folderHidro.open();

            // Carpeta Geometría
            const folderGeo = gui.addFolder('Geometría Canal');
            folderGeo.add(simParams, 'anchoCanal', 1, 10).name('Ancho Base (m)').onChange(() => {
                construirCanal();
                calcularHidraulica();
            });
            folderGeo.open();
        }

        function animate() {
            requestAnimationFrame(animate);

            time += 0.016; // aprox 60fps

            // Animar partículas (flujo de agua)
            if (flechasVelocidadGroup) {
                const longitud = simParams.longitudCanal;
                
                flechasVelocidadGroup.children.forEach(particula => {
                    // El flujo va de X positivo a X negativo
                    // Multiplicamos por la velocidad real (escalada visualmente)
                    const velocidadVisual = velocidadMedia * particula.userData.velocidadRelativa * 0.5;
                    
                    particula.position.x -= velocidadVisual * 0.1;

                    // Pequeño bamboleo en Y (oleaje suave)
                    particula.position.y = tiranteNormal + Math.sin(time * 3 + particula.userData.faseDesfase) * 0.05;

                    // Reciclar partículas cuando salen del canal
                    if (particula.position.x < -longitud / 2) {
                        particula.position.x = longitud / 2;
                        // Reposicionar aleatoriamente en el ancho
                        particula.position.z = (Math.random() - 0.5) * (simParams.anchoCanal - 0.5);
                    }
                });
            }
            
            // Animar levemente la textura del agua si la tuviéramos (aquí simulamos un oleaje en los vértices si usáramos shader, pero con MeshPhong lo dejamos estático y movemos partículas)

            controls.update(); // requerido si enableDamping = true
            renderer.render(scene, camera);
        }

        function onWindowResize() {
            camera.aspect = window.innerWidth / window.innerHeight;
            camera.updateProjectionMatrix();
            renderer.setSize(window.innerWidth, window.innerHeight);
        }

    </script>
</body>
</html>
