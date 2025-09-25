<!DOCTYPE html>
<html lang="es">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Licor Market La Curva</title>
<style>
body { font-family: Arial, sans-serif; margin:0; padding:0; background:#111; color:#ffd700; }
header { background:#222; color:#ffd700; padding:20px; text-align:center; }
nav { display:flex; justify-content:center; flex-wrap:wrap; margin:10px 0; }
nav button { background:#333; color:#ffd700; border:none; padding:10px 20px; margin:5px; border-radius:5px; cursor:pointer; }
nav button:hover { background:#444; }
.container { display:flex; flex-wrap:wrap; justify-content:center; padding:20px; }
.product { background:#333; border-radius:10px; margin:10px; padding:20px; width:200px; text-align:center; }
.product img { width:100%; height:150px; object-fit:cover; border-radius:10px; }
.product button { background:#ffd700; color:#111; border:none; padding:10px; margin-top:10px; cursor:pointer; border-radius:5px; font-weight:bold; }
.product button:hover { background:#e5c100; }
#cart, #cardPayment, #cashPayment, #deliveryInfo, #history { background:#222; padding:20px; margin:20px auto; border-radius:10px; max-width:500px; }
#cart h2, #cardPayment h2, #cashPayment h2, #deliveryInfo h2, #history h2 { margin-top:0; }
#cart ul, #history ul { list-style:none; padding:0; }
#cart button, #cardPayment button, #cashPayment button, #promoSection button { background:#ffd700; color:#111; border:none; padding:10px; margin-top:10px; cursor:pointer; border-radius:5px; font-weight:bold; width:100%; }
#cart button:hover, #cardPayment button:hover, #cashPayment button:hover, #promoSection button:hover { background:#e5c100; }
#cartItems button { background:#ffd700; color:#111; border:none; padding:2px 6px; margin-left:5px; border-radius:5px; font-weight:bold; cursor:pointer; }
#cartItems button:hover { background:#e5c100; }
input[type="text"], input[type="number"], input[type="month"], input[type="password"], input[type="email"] { width:100%; padding:8px; margin:5px 0; border-radius:5px; border:none; }
#cardPayment, #cashPayment, #deliveryInfo, #mainContent, #history { display:none; }
iframe { border-radius:10px; }
/* Notificación estilo negro-amarillo */
#notification { position: fixed; top: 20px; right: 20px; background-color: #111; color: #ffd700; padding: 15px 20px; border-radius: 10px; font-weight: bold; display: none; z-index: 9999; box-shadow: 0 0 10px #ffd700; }
/* Carrito flotante */
#floatingCart { position: fixed; right: 20px; bottom: 20px; background: #222; color: #ffd700; padding: 15px; border-radius: 10px; width: 200px; box-shadow: 0 0 15px #ffd700; transition: transform 0.3s ease, opacity 0.3s ease; z-index: 1000; cursor: pointer; }
#floatingCart h3 { margin: 0 0 10px 0; }
#floatingCart ul { list-style:none; padding:0; margin:0; max-height:120px; overflow-y:auto; }
#floatingCart button { background:#ffd700; color:#111; border:none; padding:5px; margin-top:10px; border-radius:5px; width:100%; font-weight:bold; cursor:pointer; }
#floatingCart button:hover { background:#e5c100; }
#floatingCart li { margin-bottom:5px; font-size:14px; }
/* Overlay de login/registro */
#authOverlay { position:fixed; top:0; left:0; width:100%; height:100%; background:rgba(0,0,0,0.95); display:flex; justify-content:center; align-items:center; z-index:9999; }
#authForm { background:#222; padding:30px; border-radius:10px; width:300px; }
#authForm h2 { margin-top:0; text-align:center; }
#authForm button { background:#ffd700; color:#111; border:none; padding:10px; margin-top:10px; cursor:pointer; border-radius:5px; font-weight:bold; width:100%; }
#authForm button:hover { background:#e5c100; }
#promoSection { margin-top:10px; }
#promoSection input { margin-bottom:5px; }
#promoSection p { margin:5px 0; }
</style>
</head>
<body>

<div id="notification"></div>

<div id="authOverlay">
    <div id="authForm">
        <h2>Bienvenido a Licor Market La Curva</h2>
        <div id="loginSection">
            <label>Nombre de usuario:</label>
            <input type="text" id="loginName" placeholder="Usuario">
            <label>Contraseña:</label>
            <input type="password" id="loginPassword" placeholder="Contraseña">
            <button onclick="loginUser()">Iniciar Sesión</button>
            <p style="text-align:center;">o</p>
            <button onclick="showRegister()">Crear Cuenta</button>
        </div>
        <div id="registerSection" style="display:none;">
            <label>Nombre:</label>
            <input type="text" id="regName">
            <label>Contraseña:</label>
            <input type="password" id="regPassword">
            <label>Correo electrónico (opcional):</label>
            <input type="email" id="regEmail" placeholder="correo@ejemplo.com">
            <button onclick="registerUser()">Registrarse</button>
            <p style="text-align:center;"><button onclick="showLogin()">Volver a Iniciar Sesión</button></p>
        </div>
    </div>
</div>

<div id="floatingCart">
    <h3>Carrito</h3>
    <ul id="floatingCartItems"><li>No hay productos</li></ul>
    <p>Total: $<span id="floatingTotal">0</span></p>
    <button onclick="showCartFromFloat()">Ir al carrito</button>
</div>

<div id="mainContent">
<header>
    <h1>Licor Market La Curva</h1>
    <p>Licor y Comida Rápida</p>
</header>

<nav>
    <button onclick="showCategory('licor')">Licor</button>
    <button onclick="showCategory('comida')">Comida Rápida</button>
    <button onclick="showHistory()">Historial de Pedidos</button>
</nav>

<div class="container" id="productsContainer"></div>

<div id="cart">
    <h2>Carrito</h2>
    <ul id="cartItems"><li>No hay productos seleccionados</li></ul>
    <p>Total: $<span id="total">0</span></p>

    <div id="promoSection">
        <label>¿Tienes un cupón?</label>
        <input type="text" id="couponCode" placeholder="Ingresa código">
        <button onclick="applyCoupon()">Aplicar Cupón</button>
        <p id="couponMessage"></p>
        <p>Sus puntos: <span id="userPoints">0</span></p>
        <button onclick="redeemPoints()">Canjear puntos</button>
    </div>

    <button onclick="checkout()">Seleccionar tipo de pago y pagar</button>
</div>

<div id="cardPayment">
    <h2>Pago con Tarjeta</h2>
    <form onsubmit="payCard(event)">
        <label>Nombre en la tarjeta:</label>
        <input type="text" id="cardName" required>
        <label>Número de tarjeta:</label>
        <input type="text" id="cardNumber" maxlength="16" required>
        <label>Fecha de expiración:</label>
        <input type="month" id="cardExpiry" required>
        <label>CVV:</label>
        <input type="number" id="cardCVV" maxlength="4" required>
        <button type="submit">Pagar</button>
    </form>
    <button onclick="cancelCardPayment()">Cancelar</button>
</div>

<div id="cashPayment">
    <h2>Pago en Efectivo</h2>
    <p>Total a pagar: $<span id="totalCash"></span></p>
    <label>Ingrese el efectivo que entregará:</label>
    <input type="number" id="cashGiven" placeholder="Monto entregado">
    <p id="cashMessage"></p>
    <button onclick="confirmCashPayment()">Confirmar Pago</button>
    <button onclick="cancelCashPayment()">Cancelar</button>
</div>

<div id="deliveryInfo">
    <h2>Tu pedido está en camino!</h2>
    <p>Ubicación del delivery:</p>
    <iframe id="map" src="https://www.google.com/maps/embed?pb=!1m18!1m12!1m3!1d38305.123456!2d-69.987654!3d18.486057!2m3!1f0!2f0!3f0!3m2!1i1024!2i768!4f13.1!3m3!1m2!1s0x8eaf8a0c1b1e2c3f%3A0x1d05e9c28b36b32f!2sSanto+Domingo%2C+Rep%C3%BAblica+Dominicana!5e0!3m2!1ses!2ses!4v1695300000000" width="100%" height="300" allowfullscreen="" loading="lazy"></iframe>
    <p>Tiempo estimado de llegada: <span id="eta">25-35 minutos</span></p>
</div>

<div id="history">
    <h2>Historial de Pedidos</h2>
    <ul id="historyList"></ul>
    <button onclick="closeHistory()">Cerrar Historial</button>
</div>

<script>
// Aquí va todo el JS de productos, carrito, login, pagos, promociones y puntos
// (El código que ya te entregué previamente, integrado con cupones, puntos y notificaciones)
</script>

</body>
</html>
