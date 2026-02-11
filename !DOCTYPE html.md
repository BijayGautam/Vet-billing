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
  
/* Print styling */  
@media print {  
    body * {  
        visibility: hidden;  
    }  
    #printSection, #printSection * {  
        visibility: visible;  
    }  
    #printSection {  
        position: absolute;  
        left: 0;  
        top: 0;  
        width: 100%;  
    }  
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
  
<button onclick="printBill()">Print Bill</button>  
<button onclick="clearBill()">Clear</button>  
  
<!-- Hidden Print Area -->  
<div id="printSection" style="display:none;"></div>  
  
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
  
function printBill() {  
    let client = document.getElementById("client").value;  
    let pet = document.getElementById("pet").value;  
    let date = new Date().toLocaleString();  
  
    if (!client) {  
        alert("Enter client name");  
        return;  
    }  
  
    let itemHTML = "";  
    items.forEach(item => {  
        itemHTML += `<tr>  
                        <td>${item.service}</td>  
                        <td>${item.quantity}</td>  
                        <td>NPR ${item.itemTotal}</td>  
                     </tr>`;  
    });  
  
    let printContent = `  
        <h2 style="text-align:center;">MIKU VET CENTER</h2>  
        <p><strong>Date:</strong> ${date}</p>  
        <p><strong>Client:</strong> ${client}</p>  
        <p><strong>Pet:</strong> ${pet}</p>  
        <hr>  
        <table width="100%" border="1" cellspacing="0" cellpadding="5">  
            <tr>  
                <th>Item</th>  
                <th>Qty</th>  
                <th>Total</th>  
            </tr>  
            ${itemHTML}  
        </table>  
        <h3 style="text-align:right;">Grand Total: NPR ${total}</h3>  
        <p style="text-align:center;">Thank you for visiting!</p>  
    `;  
  
    let printSection = document.getElementById("printSection");  
    printSection.innerHTML = printContent;  
    printSection.style.display = "block";  
  
    window.print();  
  
    printSection.style.display = "none";  
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
