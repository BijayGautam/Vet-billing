<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>MIKU VET CENTER Billing</title>
<style>
body {
    font-family: Arial;
    padding: 20px;
    background: #f4f6f9;
}
h2 {
    text-align: center;
}
input, button {
    width: 100%;
    padding: 10px;
    margin: 5px 0;
    font-size: 16px;
}
button {
    background: #2c7be5;
    color: white;
    border: none;
}
.bill-box {
    background: white;
    padding: 15px;
    margin-top: 15px;
}
.total {
    font-weight: bold;
    font-size: 18px;
}
</style>
</head>

<body>

<h2>MIKU VET CENTER</h2>

<input type="text" id="client" placeholder="Client Name">
<input type="text" id="pet" placeholder="Pet Name">
<input type="text" id="service" placeholder="Service / Medicine">
<input type="number" id="price" placeholder="Price (NPR)">
<input type="number" id="quantity" placeholder="Quantity" value="1">

<button onclick="addItem()">Add Item</button>

<div class="bill-box">
    <div id="items"></div>
    <p class="total">Total: NPR <span id="total">0</span></p>
</div>

<button onclick="saveBill()">Save Bill</button>
<button onclick="clearBill()">Clear</button>

<script>
let total = 0;
let items = [];

function addItem() {
    let service = document.getElementById("service").value;
    let price = parseFloat(document.getElementById("price").value);
    let quantity = parseInt(document.getElementById("quantity").value);

    if (!service || !price) {
        alert("Enter service and price");
        return;
    }

    let itemTotal = price * quantity;
    total += itemTotal;

    items.push({service, price, quantity, itemTotal});

    document.getElementById("items").innerHTML += 
        `<p>${service} (${quantity}) - NPR ${itemTotal}</p>`;

    document.getElementById("total").innerText = total;

    document.getElementById("service").value = "";
    document.getElementById("price").value = "";
    document.getElementById("quantity").value = 1;
}

function saveBill() {
    let client = document.getElementById("client").value;
    let pet = document.getElementById("pet").value;

    if (!client) {
        alert("Enter client name");
        return;
    }

    let bill = {
        date: new Date().toLocaleString(),
        client,
        pet,
        items,
        total
    };

    let bills = JSON.parse(localStorage.getItem("mikuBills")) || [];
    bills.push(bill);
    localStorage.setItem("mikuBills", JSON.stringify(bills));

    alert("Bill Saved Successfully!");
    clearBill();
}

function clearBill() {
    total = 0;
    items = [];
    document.getElementById("items").innerHTML = "";
    document.getElementById("total").innerText = "0";
}
</script>

</body>
</html>
