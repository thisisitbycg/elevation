# Set up file structure
base_path = "/mnt/data/ecommerce-site"
os.makedirs(base_path, exist_ok=True)

# File contents
index_html = """
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <title>SHRU Shop</title>
  <link rel="stylesheet" href="style.css">
</head>
<body>
  <header>
    <h1>SHRU Entertainment Store</h1>
    <nav>
      <button onclick="showCart()">🛍 Cart (<span id="cart-count">0</span>)</button>
    </nav>
  </header>

  <main>
    <section id="products"></section>
    <section id="cart" class="hidden">
      <h2>Your Cart</h2>
      <ul id="cart-items"></ul>
      <p>Total: $<span id="cart-total">0.00</span></p>
      <button onclick="checkout()">Checkout</button>
    </section>
  </main>

  <footer>
    <p>&copy; 2025 SHRU Entertainment LLC</p>
  </footer>

  <script src="script.js"></script>
</body>
</html>
"""

style_css = """
body {
  font-family: 'Arial', sans-serif;
  margin: 0;
  padding: 0;
  background: #f9f9f9;
}

header {
  background: #000;
  color: #fff;
  padding: 20px;
  text-align: center;
}

nav button {
  background: #fdd835;
  color: #000;
  border: none;
  padding: 10px 15px;
  cursor: pointer;
}

main {
  padding: 20px;
}

#products {
  display: flex;
  flex-wrap: wrap;
  gap: 20px;
}

.product {
  background: #fff;
  border: 1px solid #ddd;
  padding: 15px;
  width: 200px;
  text-align: center;
}

.product img {
  width: 100%;
  height: auto;
}

#cart {
  background: #fff;
  padding: 20px;
  margin-top: 30px;
}

.hidden {
  display: none;
}
"""

script_js = """
const products = [
  {
    id: 1,
    name: "Signature SHRU Hoodie",
    price: 39.99,
    image: "https://via.placeholder.com/200x200?text=SHRU+Hoodie"
  },
  {
    id: 2,
    name: "Limited Edition Tee",
    price: 24.99,
    image: "https://via.placeholder.com/200x200?text=SHRU+Tee"
  }
];

let cart = [];

function loadProducts() {
  const productSection = document.getElementById('products');
  products.forEach(product => {
    const div = document.createElement('div');
    div.className = 'product';
    div.innerHTML = `
      <img src="${product.image}" alt="${product.name}">
      <h3>${product.name}</h3>
      <p>$${product.price.toFixed(2)}</p>
      <button onclick="addToCart(${product.id})">Add to Cart</button>
    `;
    productSection.appendChild(div);
  });
}

function addToCart(productId) {
  const item = products.find(p => p.id === productId);
  cart.push(item);
  updateCart();
}

function updateCart() {
  document.getElementById('cart-count').innerText = cart.length;
  const cartItems = document.getElementById('cart-items');
  const cartTotal = document.getElementById('cart-total');
  cartItems.innerHTML = '';
  let total = 0;

  cart.forEach((item, i) => {
    const li = document.createElement('li');
    li.textContent = `${item.name} - $${item.price.toFixed(2)}`;
    cartItems.appendChild(li);
    total += item.price;
  });

  cartTotal.innerText = total.toFixed(2);
}

function showCart() {
  document.getElementById('cart').classList.toggle('hidden');
}

function checkout() {
  alert("Thank you for your order! 🚚💨 (Payment system coming soon!)");
  cart = [];
  updateCart();
  showCart();
}

window.onload = loadProducts;
"""

# Save files
with open(f"{base_path}/index.html", "w") as f:
    f.write(index_html)

with open(f"{base_path}/style.css", "w") as f:
    f.write(style_css)

with open(f"{base_path}/script.js", "w") as f:
    f.write(script_js)

# Create a zip file
zip_path = "/mnt/data/ecommerce-site.zip"
with zipfile.ZipFile(zip_path, 'w') as zipf:
    zipf.write(f"{base_path}/index.html", arcname="index.html")
    zipf.write(f"{base_path}/style.css", arcname="style.css")
    zipf.write(f"{base_path}/script.js", arcname="script.js")

zip_path
