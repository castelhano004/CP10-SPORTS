<!DOCTYPE html>
<html lang="pt-BR">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>CP10 SPORTS - Catálogo de Camisas</title>
<link rel="stylesheet" href="style.css">
</head>
<body>
  <h1>CP10 SPORTS - Catálogo de Camisas</h1>

  <div class="form">
    <h2>Adicionar Camisa</h2>
    <input type="text" id="name" placeholder="Nome da camisa">
    <input type="text" id="price" placeholder="Preço (ex: 150)">
    <input type="text" id="image" placeholder="URL da imagem">
    <button onclick="addProduct()">Adicionar</button>
  </div>

  <div class="products" id="products"></div>

  <div class="cart">
    <h2>Carrinho</h2>
    <div id="cart-items" class="cart-items"></div>
    <div id="cart-total"></div>
    <button onclick="checkout()">Finalizar Pedido no WhatsApp</button>
  </div>

<script src="script.js"></script>
</body>
</html>let products = [];
let cart = [];

function addProduct() {
    const name = document.getElementById("name").value;
    const price = parseFloat(document.getElementById("price").value);
    const image = document.getElementById("image").value;

    if(!name || !price || !image) {
        alert("Preencha todos os campos!");
        return;
    }

    const product = { name, price, image };
    products.push(product);
    displayProducts();

    // Limpa os campos
    document.getElementById("name").value = "";
    document.getElementById("price").value = "";
    document.getElementById("image").value = "";
}

function displayProducts() {
    const productsDiv = document.getElementById("products");
    productsDiv.innerHTML = "";
    products.forEach((p, index) => {
        const div = document.createElement("div");
        div.className = "product";
        div.innerHTML = `
            <img src="${p.image}" alt="${p.name}">
            <h3>${p.name}</h3>
            <p>R$ ${p.price.toFixed(2)}</p>
            <button onclick="addToCart(${index})">Adicionar ao Carrinho</button>
        `;
        productsDiv.appendChild(div);
    });
}

function addToCart(index) {
    cart.push(products[index]);
    displayCart();
}

function displayCart() {
    const cartItems = document.getElementById("cart-items");
    cartItems.innerHTML = "";
    let total = 0;
    cart.forEach(item => {
        total += item.price;
        const div = document.createElement("div");
        div.innerText = `${item.name} - R$ ${item.price.toFixed(2)}`;
        cartItems.appendChild(div);
    });
    document.getElementById("cart-total").innerText = `Total: R$ ${total.toFixed(2)}`;
}

function checkout() {
    if(cart.length === 0) {
        alert("Carrinho vazio!");
        return;
    }

    let message = "Olá, quero comprar:\n";
    cart.forEach(item => {
        message += `- ${item.name} R$ ${item.price.toFixed(2)}\n`;
    });
    let total = cart.reduce((sum, item) => sum + item.price, 0);
    message += `Total: R$ ${total.toFixed(2)}`;
    const url = `https://wa.me/5547991938901?text=${encodeURIComponent(message)}`;
    window.open(url, "_blank");
}body {
  font-family: Arial, sans-serif;
  margin: 20px;
  background-color: #f5f5f5;
}

h1, h2 {
  text-align: center;
}

.form, .products, .cart {
  background: #fff;
  padding: 20px;
  margin: 20px auto;
  max-width: 500px;
  border-radius: 8px;
  box-shadow: 0 0 10px rgba(0,0,0,0.1);
}

input {
  display: block;
  margin: 10px 0;
  padding: 10px;
  width: 100%;
  box-sizing: border-box;
}

button {
  padding: 10px 20px;
  cursor: pointer;
  background-color: #007bff;
  color: white;
  border: none;
  border-radius: 5px;
}

.products {
  display: flex;
  flex-wrap: wrap;
  gap: 10px;
  justify-content: center;
}

.product {
  border: 1px solid #ddd;
  padding: 10px;
  width: 150px;
  text-align: center;
  border-radius: 5px;
}

.product img {
  max-width: 100%;
  height: auto;
}

.cart-items {
  margin-bottom: 10px;
}
