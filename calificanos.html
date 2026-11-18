<?php
session_start();
include 'conexion.php';

$mensaje_calif = "";

if (isset($_POST['enviar_calificacion'])) {
    if (isset($_POST['estrellas'])) {
        $puntuacion = intval($_POST['estrellas']);
    
        $comentario = trim($_POST['comentario']); 

        $stmt = $pdo->prepare("INSERT INTO calificaciones (puntuacion, comentario) VALUES (?, ?)");
        $stmt->execute([$puntuacion, $comentario]);
        
        $mensaje_calif = "¡Gracias! Con tu calificación y comentarios podemos mejorar el servicio.";
    } else {
        $mensaje_calif = "Por favor, selecciona al menos una estrella antes de enviar.";
    }
}
?>
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>MyPlant - Califícanos</title>
    <link rel="stylesheet" href="css/estilos.css">
</head>
<body>

    <nav class="menu">
        <div class="logo">🌱 MyPlant</div>
        <ul class="enlaces">
            <li><a href="index.php">Volver al Inicio</a></li>
        </ul>
    </nav>

    <div class="contenedor-principal" style="display: block;">
        <section class="seccion-calif">
            <h2>⭐ Califícanos</h2>
            <p>¿Qué te parece nuestro servicio de cuidados de plantas? Déjanos tu opinión.</p>
            
            <?php if(!empty($mensaje_calif)): ?>
                <div class="<?php echo (isset($_POST['estrellas'])) ? 'exito' : 'alerta'; ?>">
                    <?php echo $mensaje_calif; ?>
                </div>
            <?php endif; ?>

            <form action="calificanos.php" method="POST">
            
                <div class="estrellas">
                    <input type="radio" id="st5" name="estrellas" value="5"><label for="st5">★</label>
                    <input type="radio" id="st4" name="estrellas" value="4"><label for="st4">★</label>
                    <input type="radio" id="st3" name="estrellas" value="3"><label for="st3">★</label>
                    <input type="radio" id="st2" name="estrellas" value="2"><label for="st2">★</label>
                    <input type="radio" id="st1" name="estrellas" value="1"><label for="st1">★</label>
                </div>

                <label for="comentario" style="display: block; margin-top: 15px; text-align: left; font-weight: bold;">Comentario o sugerencia (Opcional):</label>
                <textarea id="comentario" name="comentario" rows="4" placeholder="Escribe aquí tus sugerencias o qué te pareció la página..." style="width: 100%; padding: 10px; margin-top: 5px; border-radius: 4px; border: 1px solid #ccc; font-family: inherit; resize: vertical;"></textarea>

                <button type="submit" name="enviar_calificacion" class="btn-consultar" style="margin-top: 15px; width: 100%;">Enviar Calificación</button>
            </form>
        </section>
    </div>

</body>
</html>