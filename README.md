<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<title>Driving Professional</title>

<style>
body {
    margin: 0;
    font-family: Arial, sans-serif;
    background: #f4f4f4;
}

/* HERO IMAGE (BUSINESS IMAGE CREATED) */
.hero {
    height: 90vh;
    background: linear-gradient(rgba(0,0,0,.6), rgba(0,0,0,.6)),
    url("data:image/svg+xml;utf8,
    <svg xmlns='http://www.w3.org/2000/svg' width='1200' height='600'>
    <rect width='1200' height='600' fill='%232c3e50'/>
    <text x='50%' y='45%' font-size='60' fill='white' text-anchor='middle'>Driving Professional</text>
    <text x='50%' y='55%' font-size='30' fill='white' text-anchor='middle'>Trusted & Professional Drivers</text>
    </svg>");
    background-size: cover;
    background-position: center;
    color: white;
    display: flex;
    align-items: center;
    justify-content: center;
    text-align: center;
}

.hero h1 {
    font-size: 48px;
}

.hero p {
    font-size: 20px;
}

section {
    background: white;
    padding: 20px;
    margin: 20px auto;
    max-width: 500px;
    border-radius: 6px;
}

input, button {
    width: 100%;
    padding: 10px;
    margin: 5px 0;
}

button {
    background: black;
    color: white;
    border: none;
    cursor: pointer;
}

.driver {
    border: 1px solid #ccc;
    padding: 10px;
    margin: 10px 0;
    text-align: center;
}

.driver img {
    width: 120px;
    height: 120px;
    border-radius: 50%;
    object-fit: cover;
}

.hire {
    background: green;
}
</style>
</head>

<body>

<!-- HERO -->
<div class="hero">
    <div>
        <h1>Driving Professional</h1>
        <p>Find • Trust • Hire Professional Drivers</p>
    </div>
</div>

<!-- LOGIN -->
<section>
<h2>Login</h2>
<form onsubmit="login(event)">
    <input type="text" id="loginUser" placeholder="Username" required>
    <input type="password" id="loginPass" placeholder="Password" required>
    <button>Login</button>
</form>
<p id="loginMsg"></p>
</section>

<!-- DRIVER REGISTRATION -->
<section>
<h2>Driver Registration</h2>
<form id="driverForm">
    <input type="text" id="username" placeholder="Username" required>
    <input type="password" id="password" placeholder="Password" required>
    <input type="text" id="name" placeholder="Full Name" required>
    <input type="number" id="age" placeholder="Age" required>
    <input type="text" id="license" placeholder="License Number" required>
    <input type="text" id="experience" placeholder="Experience (years)" required>
    <input type="file" id="photo" accept="image/*" required>
    <button type="submit">Register Driver</button>
</form>
</section>

<!-- DRIVERS LIST -->
<section>
<h2>Available Drivers</h2>
<div id="drivers"></div>
</section>

<script>
// LOGIN
function login(e){
    e.preventDefault();
    let u = loginUser.value;
    let p = loginPass.value;
    let users = JSON.parse(localStorage.getItem("users")) || [];

    let ok = users.find(x => x.u === u && x.p === p);
    loginMsg.innerHTML = ok ? "✅ Login successful" : "❌ Wrong credentials";
}

// REGISTER DRIVER
driverForm.addEventListener("submit", function(e){
    e.preventDefault();
    let reader = new FileReader();
    reader.onload = function(){
        let drivers = JSON.parse(localStorage.getItem("drivers")) || [];
        let users = JSON.parse(localStorage.getItem("users")) || [];

        drivers.push({
            name: name.value,
            age: age.value,
            license: license.value,
            experience: experience.value,
            photo: reader.result
        });

        users.push({u: username.value, p: password.value});

        localStorage.setItem("drivers", JSON.stringify(drivers));
        localStorage.setItem("users", JSON.stringify(users));

        alert("Driver registered successfully");
        driverForm.reset();
        showDrivers();
    }
    reader.readAsDataURL(photo.files[0]);
});

// SHOW DRIVERS
function showDrivers(){
    let drivers = JSON.parse(localStorage.getItem("drivers")) || [];
    driversDiv = document.getElementById("drivers");
    driversDiv.innerHTML = "";

    drivers.forEach(d => {
        driversDiv.innerHTML += `
        <div class="driver">
            <img src="${d.photo}">
            <p><b>${d.name}</b></p>
            <p>Age: ${d.age}</p>
            <p>License: ${d.license}</p>
            <p>Experience: ${d.experience} years</p>
            <button class="hire" onclick="alert('You hired ${d.name}')">Hire Driver</button>
        </div>`;
    });
}

showDrivers();
</script>

</body>
</html>
