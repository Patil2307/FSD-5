const express = require("express");
const app = express();

app.use(express.json());

// ------------------
// Products with Images
// ------------------

let products = [
{
    id: 1,
    name: "Laptop",
    price: 60000,
    image: "https://images.unsplash.com/photo-1517336714731-489689fd1ca8"
},
{
    id: 2,
    name: "Smartphone",
    price: 25000,
    image: "https://images.unsplash.com/photo-1511707171634-5f897ff02aa9"
},
{
    id: 3,
    name: "Earphones",
    price: 2000,
    image: "https://images.unsplash.com/photo-1585386959984-a41552231658"
},
{
    id: 4,
    name: "Smartwatch",
    price: 8000,
    image: "https://images.unsplash.com/photo-1523275335684-37898b6baf30"
}
];

let cart = [];
let orders = [];

// ------------------
// Frontend UI
// ------------------

app.get("/", (req, res) => {
res.send(`
<!DOCTYPE html>
<html>
<head>
<title>Premium Store</title>
<style>
body{
    font-family: Arial, sans-serif;
    background: linear-gradient(to right,#141e30,#243b55);
    margin:0;
    color:white;
}
h1{
    text-align:center;
    padding:20px;
}
.products{
    display:flex;
    flex-wrap:wrap;
    justify-content:center;
}
.card{
    background:white;
    color:black;
    width:250px;
    margin:15px;
    border-radius:12px;
    overflow:hidden;
    box-shadow:0 6px 15px rgba(0,0,0,0.4);
    transition:0.3s;
}
.card:hover{
    transform:scale(1.05);
}
.card img{
    width:100%;
    height:200px;
    object-fit:cover;
}
.card-body{
    padding:15px;
    text-align:center;
}
button{
    padding:8px 12px;
    background:#243b55;
    color:white;
    border:none;
    cursor:pointer;
    border-radius:5px;
}
button:hover{
    background:#141e30;
}
.cart-section{
    text-align:center;
    margin:30px;
}
</style>
</head>
<body>

<h1>🛒 Premium E-Commerce Store</h1>

<div class="products" id="productList"></div>

<div class="cart-section">
    <h2>Cart</h2>
    <button onclick="viewCart()">View Cart</button>
    <ul id="cartList"></ul>
    <button onclick="placeOrder()">Place Order</button>
</div>

<script>
async function loadProducts(){
    const res = await fetch("/products");
    const data = await res.json();
    const list = document.getElementById("productList");
    list.innerHTML = "";

    data.forEach(p=>{
        list.innerHTML += \`
        <div class="card">
            <img src="\${p.image}">
            <div class="card-body">
                <h3>\${p.name}</h3>
                <p>₹ \${p.price}</p>
                <button onclick="addToCart(\${p.id})">Add to Cart</button>
            </div>
        </div>
        \`;
    });
}

async function addToCart(id){
    await fetch("/cart",{
        method:"POST",
        headers:{"Content-Type":"application/json"},
        body:JSON.stringify({productId:id, quantity:1})
    });
    alert("Added to Cart!");
}

async function viewCart(){
    const res = await fetch("/cart");
    const data = await res.json();
    const list = document.getElementById("cartList");
    list.innerHTML = "";
    data.forEach(item=>{
        list.innerHTML += "<li>Product ID: "+item.productId+" | Qty: "+item.quantity+"</li>";
    });
}

async function placeOrder(){
    const res = await fetch("/orders",{method:"POST"});
    const data = await res.json();
    if(data.error){
        alert(data.error);
    }else{
        alert("Order Placed Successfully!");
        viewCart();
    }
}

loadProducts();
</script>

</body>
</html>
`);
});

// ------------------
// APIs
// ------------------

app.get("/products",(req,res)=>{
    res.json(products);
});

app.post("/cart",(req,res)=>{
    const {productId, quantity} = req.body;
    if(!productId || !quantity){
        return res.status(400).json({error:"productId and quantity required"});
    }
    const item = { id:Date.now(), productId, quantity };
    cart.push(item);
    res.json(item);
});

app.get("/cart",(req,res)=>{
    res.json(cart);
});

app.post("/orders",(req,res)=>{
    if(cart.length===0){
        return res.status(400).json({error:"Cart is empty"});
    }
    const order={
        id:Date.now(),
        items:[...cart],
        date:new Date()
    };
    orders.push(order);
    cart=[];
    res.json(order);
});

app.get("/orders",(req,res)=>{
    res.json(orders);
});

// ------------------

app.listen(3000,()=>{
    console.log("Server running on http://localhost:3000");
});
