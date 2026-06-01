<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>E-Commerce Catalog (SPA)</title>
    <style>
        body { font-family: Arial, sans-serif; margin: 0; padding: 0; background: #f9f9f9; }
        header { background: #222; color: white; padding: 1rem 2rem; display: flex; justify-content: space-between; align-items: center; }
        header a { color: white; text-decoration: none; margin-left: 15px; font-weight: bold; }
        main { padding: 2rem; max-width: 1000px; margin: auto; }
        .grid { display: grid; grid-template-columns: repeat(auto-fit, minmax(250px, 1fr)); gap: 20px; }
        .card { background: white; padding: 20px; border-radius: 8px; box-shadow: 0 2px 5px rgba(0,0,0,0.1); text-align: center; }
        .card button { background: #007bff; color: white; border: none; padding: 10px; width: 100%; border-radius: 5px; cursor: pointer; margin-top: 10px;}
        .card button:hover { background: #0056b3; }
        #cart-view { background: white; padding: 20px; border-radius: 8px; }
    </style>
</head>
<body>
    <header>
        <h2>TechStore</h2>
        <nav>
            <a href="#/">Products</a>
            <a href="#/cart">Cart (<span id="cart-count">0</span>)</a>
        </nav>
    </header>

    <main id="app-view"></main>

    <script>
        // Mock Database
        const products = [
            { id: 1, name: "Wireless Headphones", price: 99.99 },
            { id: 2, name: "Mechanical Keyboard", price: 129.50 },
            { id: 3, name: "Gaming Mouse", price: 49.99 },
            { id: 4, name: "4K Monitor", price: 299.00 }
        ];
        
        let cart = [];

        // Components (Views)
        const HomeView = () => `
            <h2>Product Catalog</h2>
            <div class="grid">
                ${products.map(p => `
                    <div class="card">
                        <h3>${p.name}</h3>
                        <p>$${p.price.toFixed(2)}</p>
                        <button onclick="addToCart(${p.id})">Add to Cart</button>
                    </div>
                `).join('')}
            </div>
        `;

        const CartView = () => `
            <div id="cart-view">
                <h2>Your Cart</h2>
                ${cart.length === 0 ? '<p>Cart is empty.</p>' : `
                    <ul>
                        ${cart.map(item => `<li>${item.name} - $${item.price.toFixed(2)}</li>`).join('')}
                    </ul>
                    <h3>Total: $${cart.reduce((sum, item) => sum + item.price, 0).toFixed(2)}</h3>
                    <button style="background: #28a745; color: white; padding: 10px; border:none; border-radius:4px;">Checkout Securely</button>
                `}
            </div>
        `;

        // Client-Side Router logic
        function router() {
            const path = window.location.hash || '#/';
            const viewContainer = document.getElementById('app-view');
            
            if (path === '#/') {
                viewContainer.innerHTML = HomeView();
            } else if (path === '#/cart') {
                viewContainer.innerHTML = CartView();
            } else {
                viewContainer.innerHTML = '<h2>404 - Page Not Found</h2>';
            }
        }

        // Global functions for inline HTML event handlers
        window.addToCart = (id) => {
            const product = products.find(p => p.id === id);
            cart.push(product);
            document.getElementById('cart-count').innerText = cart.length;
            alert(`${product.name} added to cart!`);
        };

        // Listen for URL hash changes and initialize routing
        window.addEventListener('hashchange', router);
        window.addEventListener('load', router);
    </script>
</body>
</html>
