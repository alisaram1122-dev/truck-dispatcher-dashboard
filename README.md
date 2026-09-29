<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Preserver Trans - Dispatcher Dashboard</title>

<style>
*{box-sizing:border-box;margin:0;padding:0;font-family:Arial,sans-serif}
body{background:#f3f4f6;color:#111827}
.sidebar{position:fixed;left:0;top:0;width:230px;height:100vh;background:#111827;color:white;padding:25px 15px}
.logo{text-align:center;font-size:21px;font-weight:bold;margin-bottom:35px}
.menu{padding:13px;margin:7px 0;border-radius:8px;cursor:pointer}
.menu:hover,.menu.active{background:#374151}
.main{margin-left:230px;padding:30px}
.top{display:flex;justify-content:space-between;align-items:center;margin-bottom:25px}
.user{background:white;padding:10px 16px;border-radius:8px}
.page{display:none}
.page.active{display:block}
.cards{display:grid;grid-template-columns:repeat(4,1fr);gap:18px}
.card{background:white;padding:22px;border-radius:12px;box-shadow:0 2px 10px #0001}
.card h3{font-size:14px;color:#6b7280;margin-bottom:10px}
.number{font-size:30px;font-weight:bold}
.section{background:white;padding:22px;border-radius:12px;margin-top:25px;box-shadow:0 2px 10px #0001}
button{border:0;border-radius:7px;padding:10px 15px;cursor:pointer;background:#111827;color:white}
button:hover{opacity:.85}
.add-btn{float:right;margin-top:-35px}
input,select{padding:11px;border:1px solid #d1d5db;border-radius:7px;width:100%;margin:6px 0 12px}
.form-grid{display:grid;grid-template-columns:repeat(2,1fr);gap:15px}
table{width:100%;border-collapse:collapse;margin-top:20px}
th,td{text-align:left;padding:12px;border-bottom:1px solid #e5e7eb}
th{background:#f9fafb}
.delete{background:#dc2626;padding:7px 10px}
.status{padding:5px 9px;border-radius:15px;background:#dcfce7;color:#166534;font-size:12px}
.empty{text-align:center;padding:25px;color:#6b7280}

@media(max-width:800px){
.sidebar{width:70px;padding:20px 8px}
.logo{font-size:0}
.logo:first-letter{font-size:25px}
.menu{font-size:0;text-align:center}
.menu:first-letter{font-size:20px}
.main{margin-left:70px;padding:15px}
.cards{grid-template-columns:repeat(2,1fr)}
.form-grid{grid-template-columns:1fr}
}
</style>
</head>

<body>

<div class="sidebar">

<div class="logo">🚛 Preserver Trans</div>

<div class="menu active" onclick="showPage('dashboard',this)">📊 Dashboard</div>
<div class="menu" onclick="showPage('drivers',this)">👨‍✈️ Drivers</div>
<div class="menu" onclick="showPage('trucks',this)">🚚 Trucks</div>
<div class="menu" onclick="showPage('loads',this)">📦 Loads</div>

</div>

<div class="main">

<div class="top">
<h1 id="pageTitle">Dispatcher Dashboard</h1>
<div class="user">👤 Dispatcher</div>
</div>

<!-- DASHBOARD -->

<div id="dashboard" class="page active">

<div class="cards">

<div class="card">
<h3>🚚 Total Trucks</h3>
<div class="number" id="truckCount">0</div>
</div>

<div class="card">
<h3>👨‍✈️ Drivers</h3>
<div class="number" id="driverCount">0</div>
</div>

<div class="card">
<h3>📦 Active Loads</h3>
<div class="number" id="loadCount">0</div>
</div>

<div class="card">
<h3>🟢 Available Trucks</h3>
<div class="number" id="availableCount">0</div>
</div>

</div>

<div class="section">

<h2>Recent Loads</h2>

<table>
<thead>
<tr>
<th>Load #</th>
<th>Driver</th>
<th>Pickup</th>
<th>Delivery</th>
<th>Status</th>
</tr>
</thead>

<tbody id="dashboardLoads"></tbody>

</table>

</div>

</div>


<!-- DRIVERS -->

<div id="drivers" class="page">

<div class="section">

<h2>👨‍✈️ Drivers</h2>

<button class="add-btn" onclick="openForm('driverForm')">+ Add Driver</button>

<div id="driverForm" style="display:none;margin-top:25px">

<div class="form-grid">

<div>
<label>Driver Name</label>
<input id="driverName" placeholder="Example: John Smith">
</div>

<div>
<label>Phone</label>
<input id="driverPhone" placeholder="Phone number">
</div>

<div>
<label>Truck Number</label>
<input id="driverTruck" placeholder="Example: TRK-101">
</div>

<div>
<label>Location</label>
<input id="driverLocation" placeholder="Example: Ontario, CA">
</div>

</div>

<button onclick="addDriver()">Save Driver</button>

</div>

<table>
<thead>
<tr>
<th>Name</th>
<th>Phone</th>
<th>Truck</th>
<th>Location</th>
<th>Action</th>
</tr>
</thead>

<tbody id="driverTable"></tbody>
</table>

</div>

</div>


<!-- TRUCKS -->

<div id="trucks" class="page">

<div class="section">

<h2>🚚 Trucks</h2>

<button class="add-btn" onclick="openForm('truckForm')">+ Add Truck</button>

<div id="truckForm" style="display:none;margin-top:25px">

<div class="form-grid">

<div>
<label>Truck Number</label>
<input id="truckNumber" placeholder="Example: TRK-101">
</div>

<div>
<label>Driver</label>
<input id="truckDriver" placeholder="Driver name">
</div>

<div>
<label>Location</label>
<input id="truckLocation" placeholder="Current location">
</div>

<div>
<label>Status</label>
<select id="truckStatus">
<option>Available</option>
<option>Loaded</option>
<option>In Transit</option>
<option>Maintenance</option>
</select>
</div>

</div>

<button onclick="addTruck()">Save Truck</button>

</div>

<table>
<thead>
<tr>
<th>Truck #</th>
<th>Driver</th>
<th>Location</th>
<th>Status</th>
<th>Action</th>
</tr>
</thead>

<tbody id="truckTable"></tbody>
</table>

</div>

</div>


<!-- LOADS -->

<div id="loads" class="page">

<div class="section">

<h2>📦 Loads</h2>

<button class="add-btn" onclick="openForm('loadForm')">+ Add Load</button>

<div id="loadForm" style="display:none;margin-top:25px">

<div class="form-grid">

<div>
<label>Load Number</label>
<input id="loadNumber" placeholder="Example: LD-1001">
</div>

<div>
<label>Driver</label>
<input id="loadDriver" placeholder="Driver name">
</div>

<div>
<label>Broker</label>
<input id="loadBroker" placeholder="Broker/company">
</div>

<div>
<label>Pickup</label>
<input id="loadPickup" placeholder="Pickup city">
</div>

<div>
<label>Delivery</label>
<input id="loadDelivery" placeholder="Delivery city">
</div>

<div>
<label>Rate ($)</label>
<input id="loadRate" type="number" placeholder="6500">
</div>

<div>
<label>Miles</label>
<input id="loadMiles" type="number" placeholder="1200">
</div>

<div>
<label>Status</label>
<select id="loadStatus">
<option>Booked</option>
<option>Loaded</option>
<option>In Transit</option>
<option>Delivered</option>
<option>Cancelled</option>
</select>
</div>

</div>

<button onclick="addLoad()">Save Load</button>

</div>

<table>
<thead>
<tr>
<th>Load #</th>
<th>Driver</th>
<th>Broker</th>
<th>Pickup</th>
<th>Delivery</th>
<th>Rate</th>
<th>Miles</th>
<th>Status</th>
<th>Action</th>
</tr>
</thead>

<tbody id="loadTable"></tbody>
</table>

</div>

</div>

</div>


<script>

let drivers=JSON.parse(localStorage.getItem("drivers"))||[];
let trucks=JSON.parse(localStorage.getItem("trucks"))||[];
let loads=JSON.parse(localStorage.getItem("loads"))||[];

function saveData(){
localStorage.setItem("drivers",JSON.stringify(drivers));
localStorage.setItem("trucks",JSON.stringify(trucks));
localStorage.setItem("loads",JSON.stringify(loads));
}

function showPage(page,element){

document.querySelectorAll(".page").forEach(p=>p.classList.remove("active"));
document.getElementById(page).classList.add("active");

document.querySelectorAll(".menu").forEach(m=>m.classList.remove("active"));
element.classList.add("active");

let titles={
dashboard:"Dispatcher Dashboard",
drivers:"Drivers",
trucks:"Trucks",
loads:"Loads"
};

document.getElementById("pageTitle").innerText=titles[page];

}

function openForm(id){

let form=document.getElementById(id);

form.style.display=form.style.display==="none"?"block":"none";

}

function addDriver(){

let driver={
name:document.getElementById("driverName").value,
phone:document.getElementById("driverPhone").value,
truck:document.getElementById("driverTruck").value,
location:document.getElementById("driverLocation").value
};

if(!driver.name)return alert("Please enter driver name");

drivers.push(driver);

saveData();
render();

document.getElementById("driverForm").style.display="none";

}

function addTruck(){

let truck={
number:document.getElementById("truckNumber").value,
driver:document.getElementById("truckDriver").value,
location:document.getElementById("truckLocation").value,
status:document.getElementById("truckStatus").value
};

if(!truck.number)return alert("Please enter truck number");

trucks.push(truck);

saveData();
render();

document.getElementById("truckForm").style.display="none";

}

function addLoad(){

let load={
number:document.getElementById("loadNumber").value,
driver:document.getElementById("loadDriver").value,
broker:document.getElementById("loadBroker").value,
pickup:document.getElementById("loadPickup").value,
delivery:document.getElementById("loadDelivery").value,
rate:document.getElementById("loadRate").value,
miles:document.getElementById("loadMiles").value,
status:document.getElementById("loadStatus").value
};

if(!load.number)return alert("Please enter load number");

loads.push(load);

saveData();
render();

document.getElementById("loadForm").style.display="none";

}

function deleteDriver(i){

if(confirm("Delete this driver?")){
drivers.splice(i,1);
saveData();
render();
}

}

function deleteTruck(i){

if(confirm("Delete this truck?")){
trucks.splice(i,1);
saveData();
render();
}

}

function deleteLoad(i){

if(confirm("Delete this load?")){
loads.splice(i,1);
saveData();
render();
}

}

function render(){

document.getElementById("driverCount").innerText=drivers.length;
document.getElementById("truckCount").innerText=trucks.length;
document.getElementById("loadCount").innerText=loads.length;

let available=trucks.filter(t=>t.status==="Available").length;

document.getElementById("availableCount").innerText=available;


document.getElementById("driverTable").innerHTML=drivers.length?

drivers.map((d,i)=>`

<tr>
<td>${d.name}</td>
<td>${d.phone}</td>
<td>${d.truck}</td>
<td>${d.location}</td>
<td><button class="delete" onclick="deleteDriver(${i})">Delete</button></td>
</tr>

`).join(""):

`<tr><td colspan="5" class="empty">No drivers added</td></tr>`;


document.getElementById("truckTable").innerHTML=trucks.length?

trucks.map((t,i)=>`

<tr>
<td>${t.number}</td>
<td>${t.driver}</td>
<td>${t.location}</td>
<td><span class="status">${t.status}</span></td>
<td><button class="delete" onclick="deleteTruck(${i})">Delete</button></td>
</tr>

`).join(""):

`<tr><td colspan="5" class="empty">No trucks added</td></tr>`;


document.getElementById("loadTable").innerHTML=loads.length?

loads.map((l,i)=>`

<tr>
<td>${l.number}</td>
<td>${l.driver}</td>
<td>${l.broker}</td>
<td>${l.pickup}</td>
<td>${l.delivery}</td>
<td>$${l.rate}</td>
<td>${l.miles}</td>
<td><span class="status">${l.status}</span></td>
<td><button class="delete" onclick="deleteLoad(${i})">Delete</button></td>
</tr>

`).join(""):

`<tr><td colspan="9" class="empty">No loads added</td></tr>`;


document.getElementById("dashboardLoads").innerHTML=loads.length?

loads.slice(-5).reverse().map(l=>`

<tr>
<td>${l.number}</td>
<td>${l.driver}</td>
<td>${l.pickup}</td>
<td>${l.delivery}</td>
<td><span class="status">${l.status}</span></td>
</tr>

`).join(""):

`<tr><td colspan="5" class="empty">No loads added</td></tr>`;

}

render();

</script>

</body>
</html>
