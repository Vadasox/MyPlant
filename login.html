<?php
session_start();
include 'conexion.php';

$mensaje_alerta = "";

if (isset($_POST['registrar_usuario'])) {
    $user = trim($_POST['nuevo_usuario']);
    $pass = password_hash($_POST['nuevo_password'], PASSWORD_DEFAULT);

    try {
        $stmt = $pdo->prepare("INSERT INTO usuarios (username, password) VALUES (?, ?)");
        $stmt->execute([$user, $pass]);
        
        // Loguear automáticamente tras registrarse y redirigir al index
        $stmt_login = $pdo->prepare("SELECT * FROM usuarios WHERE username = ?");
        $stmt_login->execute([$user]);
        $usuario = $stmt_login->fetch(PDO::FETCH_ASSOC);
        
        $_SESSION['usuario_id'] = $usuario['id'];
        $_SESSION['username'] = $usuario['username'];
        
        header("Location: index.php");
        exit();
    } catch (PDOException $e) {
        $mensaje_alerta = "El nombre de usuario ya está en uso. Elige otro.";
    }
}


if (isset($_POST['iniciar_sesion'])) {
    $user = trim($_POST['login_usuario']);
    $pass = $_POST['login_password'];

    $stmt = $pdo->prepare("SELECT * FROM usuarios WHERE username = ?");
    $stmt->execute([$user]);
    $usuario = $stmt->fetch(PDO::FETCH_ASSOC);

    if ($usuario && password_verify($pass, $usuario['password'])) {
        $_SESSION['usuario_id'] = $usuario['id'];
        $_SESSION['username'] = $usuario['username'];
        header("Location: index.php");
        exit();
    } else {
        $mensaje_alerta = "Error: Usuario o contraseña incorrectos.";
    }
}


if (isset($_GET['logout'])) {
    session_destroy();
    header("Location: login.php");
    exit();
}
?>
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>MyPlant - Iniciar Sesión</title>
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
        <section class="seccion-auth">
            <h2>👤 Cuenta de Usuario / Iniciar Sesión</h2>
            <?php if(!empty($mensaje_alerta)): ?>
                <div class="alerta"><?php echo $mensaje_alerta; ?></div>
            <?php endif; ?>

            <div style="display: flex; gap: 40px; flex-wrap: wrap; margin-top: 20px;">
                <!-- Login -->
                <div style="flex: 1; min-width: 250px;">
                    <h3>Iniciar Sesión</h3>
                    <form action="login.php" method="POST">
                        <label for="login_usuario">Usuario:</label>
                        <input type="text" id="login_usuario" name="login_usuario" required style="width:100%; padding:8px; margin:5px 0 15px 0;">
                        
                        <label for="login_password">Contraseña:</label>
                        <input type="password" id="login_password" name="login_password" required style="width:100%; padding:8px; margin:5px 0 15px 0;">
                        
                        <button type="submit" name="iniciar_sesion" class="btn-consultar">Entrar</button>
                    </form>
                </div>

              
                <div style="flex: 1; min-width: 250px;">
                    <h3>Registrarse Nuevo Usuario</h3>
                    <form action="login.php" method="POST">
                        <label for="nuevo_usuario">Nuevo Usuario:</label>
                        <input type="text" id="nuevo_usuario" name="nuevo_usuario" required style="width:100%; padding:8px; margin:5px 0 15px 0;">
                        
                        <label for="nuevo_password">Contraseña:</label>
                        <input type="password" id="nuevo_password" name="nuevo_password" required style="width:100%; padding:8px; margin:5px 0 15px 0;">
                        
                        <button type="submit" name="registrar_usuario" class="btn-consultar" style="background: #28a745;">Crear Cuenta</button>
                    </form>
                </div>
            </div>
        </section>
    </div>

</body>
</html>