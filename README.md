<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Gambia Mine T-Shirts Store</title>
    <style>
        :root {
            --primary: #c62828;
            --dark: #111827;
            --light: #f9fafb;
            --border: #e5e7eb;
        }

        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
            font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, sans-serif;
        }

        body {
            background-color: var(--light);
            color: var(--dark);
        }

        header {
            background: white;
            padding: 1rem 5%;
            display: flex;
            justify-content: space-between;
            align-items: center;
            border-bottom: 1px solid var(--border);
            position: sticky;
            top: 0;
            z-index: 10;
        }

        .logo {
            font-size: 1.5rem;
            font-weight: 800;
            letter-spacing: -0.5px;
        }

        .logo span {
            color: #15803d; /* Green accent */
        }

        .cart-btn {
            background: var(--dark);
            color: white;
            border: none;
            padding: 0.6rem 1.2rem;
            border-radius: 8px;
            cursor: pointer;
            font-weight: 600;
        }

        .catalog {
            max-width: 1200px;
            margin: 2rem auto;
            padding: 0 1rem;
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(280px, 1fr));
            gap: 2rem;
        }

        .product-card {
            background: white;
            border: 1px solid var(--border);
            border-radius: 12px;
            overflow: hidden;
            display: flex;
            flex-direction: column;
            padding: 1rem;
            box-shadow: 0 1px 3px rgba(0,0,0,0.05);
        }

        .product-img-box {
            width: 100%;
            height: 280px;
            background: #f3f4f6;
            border-radius: 8px;
            display: flex;
            align-items: center;
            justify-content: center;
            font-weight: bold;
            color: #9ca3af;
            border: 1px dashed #cbd5e1;
        }

        .product-info {
            margin-top: 1rem;
            display: flex;
            flex-direction: column;
            flex-grow: 1;
        }

        .product-title {
            font-size: 1.2rem;
            font-weight: 700;
        }

        .product-desc {
            font-size: 0.9rem;
            color: #6b7280;
            margin: 0.25rem 0 0.75rem 0;
        }

        .product-price {
            font-size: 1.4rem;
            font-weight: bold;
            color: #15803d;
            margin-bottom: 1rem;
        }

        .add-to-cart {
            background: var(--dark);
            color: white;
            border: none;
            padding: 0.75rem;
            border-radius: 6px;
            cursor: pointer;
            margin-top: auto;
            font-weight: 600;
            transition: opacity 0.2s;
        }

        .add-to-cart:hover {
            opacity: 0.9;
        }

        .cart-panel {
            position: fixed;
            top: 0;
            right: -100%;
            width: 100%;
            max-width: 400px;
            height: 100%;
            background: white;
            box-shadow: -4px 0 15px rgba(0,0,0,0.1);
            transition: right 0.3s ease;
            z-index: 20;
            display: flex;
            flex-direction: column;
            padding: 2rem;
        }

        .cart-panel.open {
            right: 0;
        }

        .cart-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 2rem;
            border-bottom: 1px solid var(--border);
            padding-bottom: 1rem;
        }

        .close-cart {
            background: none;
            border: none;
            font-size: 1.75rem;
            cursor: pointer;
        }

        .cart-items {
            flex-grow: 1;
            overflow-y: auto;
        }

        .cart-item {
            display: flex;
            justify-content: space-between;
            align-items: center;
            padding: 1rem 0;
            border-bottom: 1px solid var(--border);
        }

        .checkout-btn {
            background: #25d366; /* WhatsApp Green */
            color: white;
            border: none;
            width: 100%;
            padding: 1rem;
            border-radius: 8px;
            font-size: 1.1rem;
            font-weight: bold;
            cursor: pointer;
            margin-top: 1rem;
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 8px;
        }
    </style>
</head>
<body>

    <header>
        <div class="logo">🇬🇲 GAMBIA <span>MINE</span></div>
        <button class="cart-btn" onclick="toggleCart()">Cart (<span id="cart-count">0</span>)</button>
    </header>

    <main class="catalog" id="catalog"></main>

    <div class="cart-panel" id="cart-panel">
        <div class="cart-header">
            <h2>Your Cart</h2>
            <button class="close-cart" onclick="toggleCart()">&times;</button>
        </div>
        <div class="cart-items" id="cart-items"></div>
        <div style="margin-top: 1rem; font-size: 1.3rem; font-weight: bold; display: flex; justify-content: space-between;">
            <span>Total:</span>
            <span>D<span id="cart-total">0</span></span>
        </div>
        <button class="checkout-btn" onclick="sendWhatsAppOrder()">
            Order via WhatsApp 💬
        </button>
    </div>

    <script>
        // Custom inventory for Gambia Mine t-shirts
        const products = [
            { 
                id: 1, 
                name: "Gambia Mine T-Shirt (Black Row)", 
                desc: "100% Cotton, Unisex premium fit with front/back flag print.", 
                price: 300 
            },
            { 
                id: 2, 
                name: "Gambia Mine T-Shirt (White Row)", 
                desc: "100% Cotton, Breathable and durable high-quality print.", 
                price: 300 
            }
        ];

        let cart = [];

        function displayProducts() {
            const catalog = document.getElementById('catalog');
            catalog.innerHTML = products.map(product => `
                <div class="product-card">
                    <div class="product-img-box">👕 Shirt Image Placeholder</div>
                    <div class="product-info">
                        <div class="product-title">${product.name}</div>
                        <div class="product-desc">${product.desc}</div>
                        <div class="product-price">D${product.price}</div>
                        <button class="add-to-cart" onclick="addToCart(${product.id})">Add to Cart</button>
                    </div>
                </div>
            `).join('');
        }

        function toggleCart() {
            document.getElementById('cart-panel').classList.toggle('open');
        }

        function addToCart(productId) {
            const product = products.find(p => p.id === productId);
            const existingItem = cart.find(item => item.id === productId);

            if (existingItem) {
                existingItem.quantity += 1;
            } else {
                cart.push({ ...product, quantity: 1 });
            }
            updateCartUI();
        }

        function updateCartUI() {
            const totalItems = cart.reduce((sum, item) => sum + item.quantity, 0);
            document.getElementById('cart-count').innerText = totalItems;

            const cartItemsContainer = document.getElementById('cart-items');
            cartItemsContainer.innerHTML = cart.map(item => `
                <div class="cart-item">
                    <div>
                        <h4>${item.name}</h4>
                        <small>D${item.price} x ${item.quantity}</small>
                    </div>
                    <strong>D${item.price * item.quantity}</strong>
                </div>
            `).join('');

            const totalSum = cart.reduce((sum, item) => sum + (item.price * item.quantity), 0);
            document.getElementById('cart-total').innerText = totalSum;
        }

        function sendWhatsAppOrder() {
            if (cart.length === 0) {
                alert("Please add items to your cart first!");
                return;
            }

            // Build out a clear WhatsApp text block string
            let message = "Hello Gambia Mine! I would like to place an order through your website link:\n\n";
            cart.forEach(item => {
                message += `📦 *${item.name}*\nQty: ${item.quantity} | Total: D${item.price * item.quantity}\n\n`;
            });
            
            const totalSum = cart.reduce((sum, item) => sum + (item.price * item.quantity), 0);
            message += `💰 *Grand Total: D${totalSum}*\n\nPlease let me know how to arrange delivery!`;

            // Your dynamic WhatsApp payment destination link
            const whatsappNumber = "220868417292"; 
            const encodedMessage = encodeURIComponent(message);
            const whatsappUrl = `https://wa.me{whatsappNumber}?text=${encodedMessage}`;

            // Redirect user to WhatsApp application directly
            window.open(whatsappUrl, '_blank');
        }

        displayProducts();
    </script>
</body>
</html>
