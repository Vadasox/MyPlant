<?php

session_start();
include 'conexion.php';

$info_resultado = "";
$clima_seleccionado = 'seco'; // Valor por defecto
$fecha_riego_seleccionada = '';

// BLOQUEO: Si el usuario no ha iniciado sesión, puedes redirigirlo o mostrar un aviso restrictivo
// Aquí exigiremos que inicie sesión para poder registrar/consultar plantas.
if (isset($_POST['consultar'])) {
    if (!isset($_SESSION['usuario_id'])) {
        header("Location: login.php");
        exit();
    }

    $nombre_planta = htmlspecialchars($_POST['nombre']);
    $etapa = htmlspecialchars($_POST['etapa']);
    $clima_seleccionado = htmlspecialchars($_POST['clima'] ?? 'seco');
    $fecha_riego_seleccionada = htmlspecialchars($_POST['fecha_riego']);

    // Guardar en la base de datos incluyendo la fecha de riego
    $stmt = $pdo->prepare("INSERT INTO plantas (usuario_id, nombre, etapa, clima, fecha_riego) VALUES (?, ?, ?, ?, ?)");
    $stmt->execute([$_SESSION['usuario_id'], $nombre_planta, $etapa, $clima_seleccionado, $fecha_riego_seleccionada]);

    $info_resultado .= "<h3>Información para: <strong>$nombre_planta</strong></h3>";
    $info_resultado .= "<p><strong>Etapa:</strong> " . ucfirst($etapa) . "</p>";
    $info_resultado .= "<p><strong>Clima seleccionado:</strong> " . ucfirst($clima_seleccionado) . "</p>";
    $info_resultado .= "<p><strong>Próximo riego programado:</strong> " . $fecha_riego_seleccionada . "</p><hr>";
    $info_resultado .= "<h4>💧 Necesidad de Agua:</h4>";
    if ($etapa == "semilla") {
        $info_resultado .= "<p>Mantener el sustrato húmedo constantemente con un atomizador. Evitar encharcamientos.</p>";
    } else {
        $info_resultado .= "<p>Riego moderado. Revisar la humedad de la tierra antes de regar.</p>";
    }
}
?>
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>MyPlant - Cuidados y Riego</title>
    <link rel="stylesheet" href="css/estilos.css">
    <style>
        .calendario-seccion {
            background: #fff;
            padding: 20px;
            border-radius: 8px;
            box-shadow: 0 2px 5px rgba(0,0,0,0.1);
            margin-top: 20px;
        }
        .calendar-grid {
            display: grid;
            grid-template-columns: repeat(7, 1fr);
            gap: 8px;
            margin-top: 15px;
        }
        .day-header {
            text-align: center;
            font-weight: bold;
            color: #666;
            font-size: 0.85rem;
            padding-bottom: 5px;
        }
        .day-cell {
            background-color: #fafafa;
            border: 1px solid #e0e0e0;
            border-radius: 8px;
            padding: 10px 5px;
            text-align: center;
            min-height: 50px;
            display: flex;
            flex-direction: column;
            justify-content: space-between;
        }
        .day-number {
            font-weight: bold;
            font-size: 0.9rem;
            color: #333;
        }
        .badge {
            font-size: 0.65rem;
            padding: 2px 4px;
            border-radius: 4px;
            margin-top: 4px;
            font-weight: 600;
        }
        .day-cell.riego {
            background-color: #c8e6c9;
            border-color: #4caf50;
        }
        .day-cell.riego .badge {
            background-color: #2e7d32;
            color: white;
        }
        .alerta-riego-hoy {
            background: #d4edda;
            color: #155724;
            padding: 15px;
            border-radius: 8px;
            margin-bottom: 20px;
            border: 1px solid #c3e6cb;
            font-weight: bold;
        }
    </style>
</head>
<body>

    <nav class="menu">
        <div class="logo">🌱 MyPlant</div>
        <ul class="enlaces">
            <li><a href="index.php">Inicio</a></li>
            <li><a href="#mis-plantas">Mis Plantas</a></li>
            <li><a href="calificanos.php" target="_blank">Califícanos</a></li>
            <?php if(isset($_SESSION['username'])): ?>
                <li><a href="#">Hola, <?php echo htmlspecialchars($_SESSION['username']); ?></a></li>
                <li><a href="login.php?logout=true" style="color: #ff6b6b;">Cerrar Sesión</a></li>
            <?php else: ?>
                <li><a href="login.php">Iniciar Sesión</a></li>
            <?php endif; ?>
        </ul>
    </nav>

    <div class="contenedor-principal">
        
        <section class="panel-formulario">
            <h2>Registra tu Planta</h2>
            
            <?php if(!isset($_SESSION['usuario_id'])): ?>
                <div style="background: #fff3cd; padding: 12px; border-radius: 6px; margin-bottom: 15px; border: 1px solid #ffeeba;">
                    <p style="font-size: 0.9rem; color: #856404; margin: 0;">⚠️ <strong>Acceso restringido:</strong> Debes <a href="login.php">iniciar sesión</a> obligatoriamente para registrar plantas y utilizar el calendario de riego.</p>
                </div>
            <?php else: ?>
                <p>Ingresa los datos para conocer sus necesidades de agua y programar tu riego.</p>
                <form action="" method="POST">
                    <label for="nombre">Nombre de la planta:</label>
                    <input type="text" id="nombre" name="nombre" placeholder="Ej. Tomate, Rábano..." required>
                    
                    <label for="etapa">Etapa de crecimiento:</label>
                    <select id="etapa" name="etapa" required>
                        <option value="" disabled selected>Selecciona una opción</option>
                        <option value="semilla">Semilla / Germinación</option>
                        <option value="brote">Brote en maceta</option>
                        <option value="crecimiento">Crecimiento vegetativo</option>
                        <option value="madura">Planta Madura / Cosecha</option>
                    </select>

                    <label for="clima">Estado del clima:</label>
                    <select id="clima" name="clima" required>
                        <option value="" disabled selected>Selecciona un clima</option>
                        <option value="seco">Seco</option>
                        <option value="templado">Templado</option>
                        <option value="humedo">Húmedo</option>
                        <option value="arido">Árido</option>
                    </select>

                    <label for="fecha_riego">Fecha del próximo riego:</label>
                    <input type="date" id="fecha_riego" name="fecha_riego" required>

                    <button type="submit" name="consultar" class="btn-consultar">Guardar y Consultar</button>
                </form>
            <?php endif; ?>
        </section>

        <section class="panel-resultados">
            <h2>Resultados de Cuidados</h2>
            <div class="caja-informacion">
                <?php 
                if(!empty($info_resultado)) {
                    echo $info_resultado;
                } else {
                    echo "<p class='mensaje-vacio'>Completa el formulario para ver la información aquí.</p>";
                }
                ?>
            </div>
        </section>
    </div>

    <!-- SECCIÓN DE MIS PLANTAS Y RECORDATORIOS EN TIEMPO REAL -->
    <div style="width: 80%; margin: 20px auto;" id="mis-plantas">
        <section style="background: #fff; padding: 20px; border-radius: 8px; box-shadow: 0 2px 5px rgba(0,0,0,0.1);">
            <h2>🌿 Mis Plantas Registradas y Recordatorios</h2>
            
            <?php if(isset($_SESSION['usuario_id'])): ?>
                <!-- Contenedor dinámico de alertas por JavaScript utilizando la fecha de la computadora -->
                <div id="contenedorAlertas"></div>

                <?php
                $stmt_p = $pdo->prepare("SELECT * FROM plantas WHERE usuario_id = ?");
                $stmt_p->execute([$_SESSION['usuario_id']]);
                $mis_plantas = $stmt_p->fetchAll(PDO::FETCH_ASSOC);
                
                if(count($mis_plantas) > 0):
                ?>
                    <ul id="listaPlantas" style="list-style: none; padding: 0;">
                        <?php foreach($mis_plantas as $p): ?>
                            <li style="background: #fdfdfd; border: 1px solid #ddd; margin-bottom: 10px; padding: 12px; border-radius: 6px;" 
                                data-fecha="<?php echo $p['fecha_riego']; ?>" 
                                data-nombre="<?php echo htmlspecialchars($p['nombre']); ?>">
                                <strong><?php echo htmlspecialchars($p['nombre']); ?></strong> | 
                                Etapa: <?php echo ucfirst($p['etapa']); ?> | 
                                Clima: <?php echo ucfirst($p['clima']); ?> | 
                                💧 <em>Próximo riego programado: <strong><?php echo $p['fecha_riego']; ?></strong></em>
                            </li>
                        <?php endforeach; ?>
                    </ul>
                <?php else: ?>
                    <p>Aún no has registrado ninguna planta.</p>
                <?php endif; ?>
            <?php else: ?>
                <p>Debes <a href="login.php">iniciar sesión</a> para ver tus plantas registradas.</p>
            <?php endif; ?>
        </section>
    </div>

    <script>
        // Obtener la fecha actual de la computadora del usuario en formato YYYY-MM-DD
        const hoy = new Date();
        const anio = hoy.getFullYear();
        const mes = String(hoy.getMonth() + 1).padStart(2, '0');
        const dia = String(hoy.getDate()).padStart(2, '0');
        const fechaActualComputadora = `${anio}-${mes}-${dia}`;

        // Revisar las plantas listadas para comparar su fecha de riego con la fecha actual de la PC
        const elementosPlantas = document.querySelectorAll('#listaPlantas li');
        const contenedorAlertas = document.getElementById('contenedorAlertas');
        let plantasARegarHoy = [];

        elementosPlantas.forEach(li => {
            const fechaRiegoPlanta = li.getAttribute('data-fecha');
            const nombrePlanta = li.getAttribute('data-nombre');

            if (fechaRiegoPlanta === fechaActualComputadora) {
                plantasARegarHoy.push(nombrePlanta);
                // Resaltar visualmente la tarjeta de la planta que toca regar hoy
                li.style.borderColor = '#4caf50';
                li.style.backgroundColor = '#e8f5e9';
            }
        });

        // Mostrar alerta si hay plantas que requieren riego hoy
        if (plantasARegarHoy.length > 0) {
            contenedorAlertas.innerHTML = `
                <div class="alerta-riego-hoy">
                    🔔 ¡Recordatorio de Riego! Hoy (${fechaActualComputadora}) debes regar las siguientes plantas: <strong>${plantasARegarHoy.join(', ')}</strong>.
                </div>
            `;
        }
    </script>
</body>
</html>
