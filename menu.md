<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Nadan Bites - Kerala Snacks Menu</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            max-width: 800px;
            margin: 0 auto;
            padding: 20px;
            line-height: 1.6;
        }
        .product-item {
            display: flex;
            align-items: center;
            margin-bottom: 30px;
            flex-wrap: nowrap;
            min-height: 150px;
        }
        .product-image {
            width: 200px;
            height: 150px;
            object-fit: cover;
            border-radius: 8px;
            margin-right: 20px;
            flex-shrink: 0;
            background-color: #f0f0f0;
            display: flex;
            align-items: center;
            justify-content: center;
            color: #666;
            font-size: 12px;
            text-align: center;
        }
        .product-info {
            flex: 1;
        }
        .contact-section {
            margin-top: 40px;
            padding-top: 20px;
            border-top: 2px solid #ddd;
        }
        .contact-info {
            margin-bottom: 20px;
        }
        .whatsapp-link {
            color: #25D366;
            text-decoration: none;
        }
        .whatsapp-link:hover {
            text-decoration: underline;
        }
        .quantity-controls {
            display: flex;
            align-items: center;
            gap: 10px;
            margin: 10px 0;
        }
        .quantity-btn {
            background-color: #e74c3c;
            color: white;
            border: none;
            width: 30px;
            height: 30px;
            border-radius: 50%;
            cursor: pointer;
            font-size: 16px;
            display: flex;
            align-items: center;
            justify-content: center;
        }
        .quantity-btn:hover {
            background-color: #c0392b;
        }
        .quantity-btn:disabled {
            background-color: #bdc3c7;
            cursor: not-allowed;
        }
        .quantity-display {
            font-weight: bold;
            font-size: 16px;
            min-width: 20px;
            text-align: center;
        }
        .add-to-cart-btn {
            background-color: #27ae60;
            color: white;
            border: none;
            padding: 8px 16px;
            border-radius: 5px;
            cursor: pointer;
            font-size: 14px;
            margin-left: 10px;
        }
        .add-to-cart-btn:hover {
            background-color: #219a52;
        }
        .add-to-cart-btn:disabled {
            background-color: #bdc3c7;
            cursor: not-allowed;
        }
        .cart-summary {
            position: fixed;
            top: 20px;
            right: 20px;
            background-color: #2c3e50;
            color: white;
            padding: 15px;
            border-radius: 8px;
            min-width: 200px;
            box-shadow: 0 4px 6px rgba(0,0,0,0.1);
            z-index: 1000;
        }
        .cart-item {
            display: flex;
            justify-content: space-between;
            margin-bottom: 5px;
            font-size: 14px;
        }
        .cart-total {
            border-top: 1px solid #34495e;
            padding-top: 10px;
            margin-top: 10px;
            font-weight: bold;
        }
        .checkout-btn {
            background-color: #e74c3c;
            color: white;
            border: none;
            padding: 10px;
            border-radius: 5px;
            cursor: pointer;
            width: 100%;
            margin-top: 10px;
        }
        .checkout-btn:hover {
            background-color: #c0392b;
        }
        @media (max-width: 600px) {
            .product-item {
                flex-direction: column;
                text-align: center;
            }
            .product-image {
                margin-right: 0;
                margin-bottom: 15px;
            }
            .cart-summary {
                position: relative;
                top: auto;
                right: auto;
                margin-bottom: 20px;
            }
        }
    </style>
    <script>
        let cart = {};

        function updateQuantity(productId, change) {
            // Retrieve the quantity span and add-to-cart button
            const quantitySpan = document.getElementById(`quantity-${productId}`);
            const addBtn = document.getElementById(`add-btn-${productId}`);

            if (!cart[productId]) {
                cart[productId] = { quantity: 0, name: '', price: 0 };
            }

            cart[productId].quantity += change;

            if (cart[productId].quantity < 0) {
                cart[productId].quantity = 0;
            }

            if (quantitySpan) {
                quantitySpan.textContent = cart[productId].quantity;
            }

            // Update add to cart button state
            if (addBtn) {
                addBtn.disabled = cart[productId].quantity === 0;
            }

            updateCartDisplay();
        }

        function addToCart(productId, productName, price) {
            if (!cart[productId] || cart[productId].quantity === 0) return;

            cart[productId].name = productName;
            cart[productId].price = price;

            updateCartDisplay();
            alert(`${productName} added to cart!`);
        }

        function updateCartDisplay() {
            const cartItems = document.getElementById('cart-items');
            const cartTotal = document.getElementById('cart-total');
            const checkoutBtn = document.getElementById('checkout-btn');
            const cartSummary = document.getElementById('cart-summary');

            if (!cartItems || !cartTotal || !checkoutBtn || !cartSummary) return;

            cartItems.innerHTML = '';
            let total = 0;
            let hasItems = false;

            for (const [productId, item] of Object.entries(cart)) {
                if (item.quantity > 0 && item.name) {
                    hasItems = true;
                    const itemTotal = item.quantity * item.price;
                    total += itemTotal;

                    const cartItem = document.createElement('div');
                    cartItem.className = 'cart-item';
                    cartItem.innerHTML = `
                        <span>${item.name} x${item.quantity}</span>
                        <span>₹${itemTotal}</span>
                    `;
                    cartItems.appendChild(cartItem);
                }
            }

            cartTotal.textContent = `Total: ₹${total}`;
            checkoutBtn.disabled = !hasItems;

            // Show/hide cart
            cartSummary.style.display = hasItems ? 'block' : 'none';
        }

        function checkout() {
            let orderText = "Hello! I'd like to place an order:\n\n";
            let total = 0;

            for (const [productId, item] of Object.entries(cart)) {
                if (item.quantity > 0 && item.name) {
                    const itemTotal = item.quantity * item.price;
                    total += itemTotal;
                    orderText += `${item.name} - ${item.quantity} x ₹${item.price} = ₹${itemTotal}\n`;
                }
            }

            orderText += `\nTotal: ₹${total}`;
            orderText += `\n\nPlease confirm availability and delivery details.`;

            const whatsappUrl = `https://wa.me/919538321952?text=${encodeURIComponent(orderText)}`;
            window.open(whatsappUrl, '_blank');
        }

        // Initialize cart display on page load
        document.addEventListener('DOMContentLoaded', function() {
            updateCartDisplay();
        });
    </script>
</head>
<body>
    <h1 style="font-size: 1.4rem; margin-bottom: 10px;">Our Menu</h1>
    <p style="font-size: 1rem; color: #7f8c8d; font-style: italic;">Authentic Kerala Snacks Made Fresh Daily</p>

    <!-- Cart Summary -->
    <div id="cart-summary" class="cart-summary" style="display: none;">
        <h3 style="margin-top: 0;">Your Cart</h3>
        <div id="cart-items"></div>
        <div id="cart-total" class="cart-total">Total: ₹0</div>
        <button id="checkout-btn" class="checkout-btn" onclick="checkout()" disabled>
            Checkout via WhatsApp
        </button>
    </div>

    <div class="product-item">
        <img src="/assets/img/bananachips.jpg" alt="Banana chips" class="product-image">
        <div class="product-info">
            <strong>🍌 Banana Chips</strong><br>
            Crispy banana chips made with fresh coconut oil and a touch of turmeric.<br>
            <em>Price: ₹180 per 500g | Shelf life: 25 days</em>
            <div class="quantity-controls">
                <button class="quantity-btn" onclick="updateQuantity('banana-chips', -1)">-</button>
                <span id="quantity-banana-chips" class="quantity-display">0</span>
                <button class="quantity-btn" onclick="updateQuantity('banana-chips', 1)">+</button>
                <button id="add-btn-banana-chips" class="add-to-cart-btn" onclick="addToCart('banana-chips', 'Banana Chips', 180)" disabled>
                    Add to Cart
                </button>
            </div>
        </div>
    </div>

    <div class="product-item">
        <img src="/assets/img/unniappam.jpeg" alt="Unniappam" class="product-image">
        <div class="product-info">
            <strong>🌀 Unni Appam</strong><br>
            Traditional Kerala sweet made with rice flour, banana, and jaggery, spiced with cardamom.<br>
            <em>Price: ₹180 per 500g | Shelf life: 25 days</em>
            <div class="quantity-controls">
                <button class="quantity-btn" onclick="updateQuantity('unni-appam', -1)">-</button>
                <span id="quantity-unni-appam" class="quantity-display">0</span>
                <button class="quantity-btn" onclick="updateQuantity('unni-appam', 1)">+</button>
                <button id="add-btn-unni-appam" class="add-to-cart-btn" onclick="addToCart('unni-appam', 'Unni Appam', 180)" disabled>
                    Add to Cart
                </button>
            </div>
        </div>
    </div>

    <div class="product-item">
        <img src="/assets/img/nadankeralamixture.jpg" alt="Nadan Kerala Mixture" class="product-image">
        <div class="product-info">
            <strong>🥜 Nadan Kerala Mixture</strong><br>
            Spicy traditional Kerala snack mix with sev, boondi, roasted peanuts, curry leaves, and aromatic spices.<br>
            <em>Price: ₹150 per 500g | Shelf life: 20 days</em>
            <div class="quantity-controls">
                <button class="quantity-btn" onclick="updateQuantity('nadan-mixture', -1)">-</button>
                <span id="quantity-nadan-mixture" class="quantity-display">0</span>
                <button class="quantity-btn" onclick="updateQuantity('nadan-mixture', 1)">+</button>
                <button id="add-btn-nadan-mixture" class="add-to-cart-btn" onclick="addToCart('nadan-mixture', 'Nadan Kerala Mixture', 150)" disabled>
                    Add to Cart
                </button>
            </div>
        </div>
    </div>

    <div class="product-item">
        <img src="/assets/img/payyolimixture.jpg" alt="Payyoli Mixture" class="product-image">
        <div class="product-info">
            <strong>🫘 Payyoli Mixture</strong><br>
            Traditional Malabar snack mix with peanuts, curry leaves, large sev, and aromatic spices for a crispy, flavorful bite.<br>
            <em>Price: ₹250 per 500g | Shelf life: 30 days</em>
            <div class="quantity-controls">
                <button class="quantity-btn" onclick="updateQuantity('payyoli-mixture', -1)">-</button>
                <span id="quantity-payyoli-mixture" class="quantity-display">0</span>
                <button class="quantity-btn" onclick="updateQuantity('payyoli-mixture', 1)">+</button>
                <button id="add-btn-payyoli-mixture" class="add-to-cart-btn" onclick="addToCart('payyoli-mixture', 'Payyoli Mixture', 250)" disabled>
                    Add to Cart
                </button>
            </div>
        </div>
    </div>

    <div class="product-item">
        <img src="/assets/img/masalakadala.jpg" alt="Masala kadala" class="product-image">
        <div class="product-info">
            <strong>🌶️ Masala Kadala</strong><br>
            Crispy spiced peanuts coated in chickpea flour and deep-fried with aromatic spices for a perfect crunchy snack.<br>
            <em>Price: ₹160 per 500g | Shelf life: 25 days</em>
            <div class="quantity-controls">
                <button class="quantity-btn" onclick="updateQuantity('masala-kadala', -1)">-</button>
                <span id="quantity-masala-kadala" class="quantity-display">0</span>
                <button class="quantity-btn" onclick="updateQuantity('masala-kadala', 1)">+</button>
                <button id="add-btn-masala-kadala" class="add-to-cart-btn" onclick="addToCart('masala-kadala', 'Masala Kadala', 160)" disabled>
                    Add to Cart
                </button>
            </div>
        </div>
    </div>

    <div class="contact-section">
        <h2><strong>Order & Delivery Information</strong></h2>
        <div class="contact-info">
            📞 <strong>Phone:</strong> +91 9538321952<br>
            📧 <strong>Email:</strong> orders@nadanbites.com<br>
            📱 <strong>WhatsApp:</strong> <a href="https://wa.me/919538321952" class="whatsapp-link">Click to Order</a>
        </div>
        <div class="contact-info">
            <strong>Delivery Areas:</strong><br>
            Electronic City, Bengaluru: Free delivery
        </div>
        <div class="contact-info">
            <strong>Payment Options:</strong> Cash on Delivery | UPI | Bank Transfer | Online Payment
        </div>
        <p style="font-style: italic; color: #7f8c8d; margin-top: 20px;">
            *FSSAI Reg No: 21225007000477*<br>
            *All snacks made fresh to order using traditional recipes and finest ingredients.*
        </p>
    </div>
</body>
</html>
