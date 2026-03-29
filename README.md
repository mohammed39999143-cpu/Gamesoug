# Gamesoug
متجر لبيع منتجات القيمنق
<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>GameSoug</title>
<style>
:root{
  --main:#ff6f00;
  --secondary:#ffcc80;
  --bg:#0d0d0d;
  --text:#fff;
}

body{
  margin:0;
  font-family:'Segoe UI',system-ui;
  background:linear-gradient(120deg,#0d0d0d,#1a1a1a);
  color:var(--text);
  transition: background 0.5s;
}

header{
  display:flex;
  justify-content:space-between;
  align-items:center;
  padding:15px 20px;
  background:#111;
  box-shadow:0 4px 8px rgba(0,0,0,0.5);
  position:sticky;
  top:0;
  z-index:1000;
}

header h2{
  margin:0;
  font-size:1.5rem;
  cursor:pointer;
  background:linear-gradient(45deg,#ff6f00,#ffcc80);
  -webkit-background-clip:text;
  -webkit-text-fill-color:transparent;
}

.cart-btn{
  background:var(--main);
  border:none;
  padding:10px 15px;
  border-radius:12px;
  cursor:pointer;
  font-size:1.1rem;
  transition:0.3s;
}

.cart-btn:hover{
  transform:scale(1.1);
  box-shadow:0 0 10px var(--main);
}

.products{
  display:grid;
  grid-template-columns:repeat(auto-fill,minmax(250px,1fr));
  gap:20px;
  padding:20px;
}

.card{
  background:#1a1a1a;
  border-radius:20px;
  overflow:hidden;
  box-shadow:0 5px 20px rgba(0,0,0,0.7);
  transition:0.3s;
  cursor:pointer;
}

.card:hover{
  transform:translateY(-10px) scale(1.03);
  box-shadow:0 10px 30px rgba(255,111,0,0.7);
}

.card img{
  width:100%;
  height:180px;
  object-fit:cover;
  transition:0.5s;
}

.card:hover img{
  transform:scale(1.1);
}

.card h4{
  margin:10px 0 5px;
  text-align:center;
}

.price{
  text-align:center;
  color:var(--main);
  font-weight:bold;
  font-size:1.1rem;
}

.btn{
  display:block;
  background:var(--main);
  border:none;
  width:80%;
  margin:10px auto;
  padding:10px;
  border-radius:12px;
  cursor:pointer;
  color:#fff;
  font-weight:bold;
  transition:0.3s;
}

.btn:hover{
  background:var(--secondary);
  color:#000;
  transform:scale(1.05);
}

.cart{
  position:fixed;
  top:0;
  right:-100%;
  width:100%;
  max-width:450px;
  height:100%;
  background:#111;
  transition:0.4s;
  padding:20px;
  overflow-y:auto;
  box-shadow:-5px 0 20px rgba(0,0,0,0.7);
  z-index:10000;
}

.cart.active{
  right:0;
}

.cart h2{
  text-align:center;
  margin-bottom:15px;
  color:var(--main);
}

.cart-item{
  display:flex;
  justify-content:space-between;
  margin:15px 0;
  border-bottom:1px solid #333;
  padding-bottom:10px;
}

.qty button{
  background:var(--main);
  border:none;
  padding:3px 8px;
  border-radius:6px;
  cursor:pointer;
  color:#fff;
  transition:0.2s;
}

.qty button:hover{
  background:var(--secondary);
  color:#000;
}

input{
  width:100%;
  padding:12px;
  margin:8px 0;
  border-radius:8px;
  border:none;
  outline:none;
}

.checkout{
  background:linear-gradient(45deg,#43e97b,#38f9d7);
  font-weight:bold;
  font-size:1rem;
}

.checkout:hover{
  transform:scale(1.05);
  box-shadow:0 0 15px #38f9d7;
}

.close-btn{
  background:#ff3d00;
  margin-top:10px;
}
</style>
</head>
<body>

<header>
  <h2>🎮 GameSoug</h2>
  <button class="cart-btn" onclick="openCart()">🛒 سلة المشتريات</button>
</header>

<div class="products" id="products"></div>

<div class="cart" id="cart">
  <h2>🛒 السلة</h2>
  <div id="cartItems"></div>

  <h3 id="total"></h3>

  <h3>📍 معلومات الشحن</h3>
  <input placeholder="الاسم">
  <input placeholder="رقم الهاتف">
  <input placeholder="العنوان">
  <input placeholder="المدينة">

  <!-- زر الدفع الحقيقي -->
  <div id="paypal-button-container"></div>

  <button class="btn close-btn" onclick="closeCart()">❌ إغلاق السلة</button>
</div>

<!-- سكربت المنتجات والسلة -->
<script src="https://www.paypal.com/sdk/js?client-id=YOUR_CLIENT_ID&currency=BHD&enable-funding=applepay"></script>
<script>
let products=[
  {id:1,name:"Redragon K552 Mechanical Keyboard",price:15,img:"https://i.postimg.cc/MH8L9CkZ/redragon-k552.png"},
  {id:2,name:"Redragon M602 RGB Gaming Mouse",price:10,img:"https://i.postimg.cc/fTg27H6Z/redragon-m602.png"},
  {id:3,name:"AULA F2088 Mechanical Keyboard",price:12,img:"https://i.postimg.cc/Xq6fL44W/aula-f2088.png"},
  {id:4,name:"Havit H2008d Wired Gaming Headset",price:9,img:"https://i.postimg.cc/XYFt4G1W/havit-h2008d.png"},
  {id:5,name:"Fifine Dynamic RGB Gaming Headset",price:12,img:"https://i.postimg.cc/pd7Kcd8f/fifine-headset.png"},
  {id:6,name:"Gaming Mouse Pad XXL",price:5,img:"https://i.postimg.cc/5yrjNqzc/mousepad.png"},
  {id:7,name:"Wireless RGB Gaming Mouse",price:8,img:"https://i.postimg.cc/rpVQFpx4/rgb-mouse.png"},
  {id:8,name:"PC Cooling Fan RGB",price:6,img:"https://i.postimg.cc/C5Vxnw3z/cooling-fan.png"},
  {id:9,name:"Adjustable Gaming Desk LED Strip",price:7,img:"https://i.postimg.cc/4xyDckJj/led-strip.png"},
  {id:10,name:"USB Gaming Headset Stand",price:3,img:"https://i.postimg.cc/Dzf3tqCg/headset-stand.png"},
  {id:11,name:"RGB Backlit Keyboard Combo",price:12,img:"https://i.postimg.cc/ZYHQVq0X/keyboard-combo.png"},
  {id:12,name:"Ultra Wide Monitor Stand",price:15,img:"https://i.postimg.cc/2j6wrq7b/monitor-stand.png"},
  {id:13,name:"2.4GHz Wireless Controller",price:8,img:"https://i.postimg.cc/x1WKmZW9/wireless-controller.png"},
  {id:14,name:"Gaming Laptop Cooling Pad",price:7,img:"https://i.postimg.cc/cJ8PN1Wf/cooling-pad.png"},
  {id:15,name:"RGB Desk LED Strip",price:6,img:"https://i.postimg.cc/66WfnD0F/desk-led.png"},
  {id:16,name:"Mechanical Keycap Set",price:4,img:"https://i.postimg.cc/G2XbYc8f/keycap-set.png"},
  {id:17,name:"Gaming Speakers 2.0",price:9,img:"https://i.postimg.cc/ZYQ6N6Sk/speakers.png"},
  {id:18,name:"Mini Gaming Webcam 1080p",price:8,img:"https://i.postimg.cc/0QJjXvHb/webcam.png"},
  {id:19,name:"RGB Mouse Bungee",price:5,img:"https://i.postimg.cc/ncPtR9Px/mouse-bungee.png"},
  {id:20,name:"Gaming Chair Cushion Pad",price:3,img:"https://i.postimg.cc/7YwsV7y6/chair-pad.png"},
  {id:21,name:"Gaming Headset w/ Mic",price:8,img:"https://i.postimg.cc/brdZwqXZ/headset-mic.png"},
  {id:22,name:"RGB Strip for PC Case",price:4,img:"https://i.postimg.cc/Vv4vMB6d/case-rgb.png"},
  {id:23,name:"Virtual 7.1 Gaming Headset",price:14,img:"https://i.postimg.cc/FF5gJ09t/7-1-headset.png"},
  {id:24,name:"Gaming Phone Holder Stand",price:3,img:"https://i.postimg.cc/nc19rKfM/phone-stand.png"},
  {id:25,name:"3-in-1 Game Station Combo",price:12,img:"https://i.postimg.cc/5tN2gTqF/3in1-combo.png"},
  {id:26,name:"PC RGB Strip Lights",price:5,img:"https://i.postimg.cc/mk8xX4wj/pc-rgb.png"},
  {id:27,name:"Wireless Silent Gaming Mouse",price:7,img:"https://i.postimg.cc/yNb1GWgM/wireless-silent.png"},
  {id:28,name:"Keyboard Wrist Rest",price:3,img:"https://i.postimg.cc/8P1t0c5r/wrist-rest.png"},
  {id:29,name:"RGB Mouse Gloves",price:2,img:"https://i.postimg.cc/q7k2t8gX/mouse-gloves.png"},
  {id:30,name:"Gaming Desk Mat Large",price:6,img:"https://i.postimg.cc/5y8nkhpK/desk-mat.png"}
];

// زيادة 30٪ على كل سعر
products = products.map(p => ({...p, price: +(p.price*1.3).toFixed(2)}));

let cart=JSON.parse(localStorage.getItem("cart"))||[];

function renderProducts(){
  let html="";
  products.forEach(p=>{
    html+=`
      <div class="card">
        <img src="${p.img}">
        <h4>${p.name}</h4>
        <p class="price">${p.price} BHD</p>
        <button class="btn" onclick="addToCart(${p.id})">أضف للسلة</button>
      </div>
    `;
  });
  document.getElementById("products").innerHTML=html;
}

function addToCart(id){
  let item=cart.find(i=>i.id==id);
  if(item){ item.qty++; } 
  else{ let product=products.find(p=>p.id==id); cart.push({...product,qty:1}); }
  saveCart(); renderCart(); renderPayPalButton();
}

function renderCart(){
  let html=""; let total=0;
  cart.forEach((item,i)=>{ total+=item.price*item.qty;
    html+=`
      <div class="cart-item">
        <div>${item.name}<br>${item.price} BHD</div>
        <div class="qty">
          <button onclick="changeQty(${i},1)">+</button>${item.qty}
          <button onclick="changeQty(${i},-1)">-</button>
        </div>
      </div>`;
  });
  let vat=total*0.10; let final=total+vat;
  document.getElementById("cartItems").innerHTML=html;
  document.getElementById("total").innerHTML=
    `المجموع: ${total.toFixed(2)} BHD<br> الضريبة: ${vat.toFixed(2)} BHD<br> الإجمالي: ${final.toFixed(2)} BHD`;
}

function changeQty(i,delta){ cart[i].qty+=delta; if(cart[i].qty<=0) cart.splice(i,1); saveCart(); renderCart(); renderPayPalButton(); }
function saveCart(){ localStorage.setItem("cart",JSON.stringify(cart)); }
function openCart(){ document.getElementById("cart").classList.add("active"); renderPayPalButton(); }
function closeCart(){ document.getElementById("cart").classList.remove("active"); }

// زر PayPal + Apple Pay
function renderPayPalButton(){
  document.getElementById("paypal-button-container").innerHTML="";
  let total=cart.reduce((sum,item)=>sum+item.price*item.qty,0)*1.10;
  if(total<=0) return;

  paypal.Buttons({
    style: { layout:'vertical', color:'gold', shape:'rect', label:'pay' },
    createOrder: (data, actions) => { return actions.order.create({ purchase_units:[{ amount:{ value: total.toFixed(2) } }] }); },
    onApprove: (data, actions) => { return actions.order.capture().then(details=>{
      alert('✅ تم الدفع بنجاح! شكراً لك، '+details.payer.name.given_name);
      cart=[]; saveCart(); renderCart();
    }); }
  }).render('#paypal-button-container');
}

renderProducts(); renderCart();
</script>

</body>
</html>
