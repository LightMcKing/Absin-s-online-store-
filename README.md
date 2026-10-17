<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>My Minimalist Store</title>
    <style>
        :root {
            --primary: #2563eb;
            --dark: #1e293b;
            --light: #f8fafc;
            --border: #e2e8f0;
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

        /* Navigation Header */
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

        .cart-btn {
            background: var(--primary);
            color: white;
            border: none;
            padding: 0.6rem 1.2rem;
            border-radius: 8px;
            cursor: pointer;
            font-weight: 600;
        }

        /* Product Catalog Grid */
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
            transition: transform 0.2s;
        }

        .product-card:hover {
            transform: translateY(-4px);
        }

        .product-img-placeholder {
            width: 100%;
            height: 200px;
            background: #cbd5e1;
            border-radius: 8px;
            display: flex;
            align-items: center;
            justify-content: center;
            color: #64748b;
        }

        .product-info {
            margin-top: 1rem;
            display: flex;
            flex-direction: column;
            flex-grow: 1;
        }

        .product-price {
            font-size: 1.25rem;
            font-weight: bold;
            color: var(--primary);
            margin: 0.5rem 0 1rem 0;
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
        }

        /* Shopping Cart Panel */
        .cart-panel {
            position: fixed;
            top: 0;
            right: -400px;
            width: 400px;
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
        }

        .close-cart {
            background: none;
            border: none;
            font-size: 1.5rem;
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
            background: var(--primary);
            color: white;
            border: none;
            width: 100%;
            padding: 1rem;
            border-radius: 8px;
            font-size: 1rem;
            font-weight: bold;
            cursor: pointer;
            margin-top: 1rem;
        }
    </style>
</head>
<body>

    <header>
        <h1>⚡ SwiftShop</h1>
        <button class="cart-btn" onclick="toggleCart()">Cart (<span id="cart-count">0</span>)</button>
    </header>

    <!-- Products Entrypoint -->
    <main class="catalog" id="catalog"></main>

    <!-- Side Shopping Cart -->
    <div class="cart-panel" id="cart-panel">
        <div class="cart-header">
            <h2>Your Order</h2>
            <button class="close-cart" onclick="toggleCart()">&times;</button>
        </div>
        <div class="cart-items" id="cart-items"></div>
        <div style="margin-top: 1rem; font-size: 1.2rem; font-weight: bold;">
            Total: \$<span id="cart-total">0.00</span>
        </div>
        <button class="checkout-btn" onclick="processCheckout()">Place Order</button>
    </div>

    <script>
        // Edit this array to add or modify your storefront items
        const products = [
            { id: 1, name: "Minimalist Wireless Mouse", price: 29.99 },
            { id: 2, name: "Mechanical Gaming Keyboard", price: 89.99 },
            { id: 3, name: "Ultra-Wide Desktop Monitor", price: 249.99 },
            { id: 4, name: "Noise-Cancelling Headphones", price: 150.00 }
        ];

        let cart = [];

        // Render products into HTML catalog layout dynamically
        function displayProducts() {
            const catalog = document.getElementById('catalog');
            catalog.innerHTML = products.map(product => `
                <div class="product-card">
                    <div class="product-img-placeholder">Product Image</div>
                    <div class="product-info">
                        <h3>${product.name}</h3>
                        <div class="product-price">$${product.price.toFixed(2)}</div>
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
            // Update items count in navbar button
            const totalItems = cart.reduce((sum, item) => sum + item.quantity, 0);
            document.getElementById('cart-count').innerText = totalItems;

            // Render list inside the cart sliding menu
            const cartItemsContainer = document.getElementById('cart-items');
            cartItemsContainer.innerHTML = cart.map(item => `
                <div class="cart-item">
                    <div>
                        <h4>${item.name}</h4>
                        <small>$${item.price.toFixed(2)} x ${item.quantity}</small>
                    </div>
                    <strong>$${(item.price * item.quantity).toFixed(2)}</strong>
                </div>
            `).join('');

            // Calculate total sum price
            const totalSum = cart.reduce((sum, item) => sum + (item.price * item.quantity), 0);
            document.getElementById('cart-total').innerText = totalSum.toFixed(2);
        }

        function processCheckout() {
            if (cart.length === 0) {
                alert("Your cart is empty!");
                return;
            }
            // Simple display of the ordered items
            alert("Order Confirmed! Items processing details will be outputted to console logs.");
            console.log("Processing Order Inventory items payload:", cart);
            cart = [];
            updateCartUI();
            toggleCart();
        }

        // Initialize Store Catalog on window load
        displayProducts();
    </script>
</body>
</html>
