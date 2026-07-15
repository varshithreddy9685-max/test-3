# test-3
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Weather App</title>

<style>
body{
    font-family:Arial,sans-serif;
    background:linear-gradient(to right,#4facfe,#00f2fe);
    color:white;
    text-align:center;
    padding:20px;
}
.container{
    max-width:700px;
    margin:auto;
}
input{
    padding:10px;
    width:60%;
    border:none;
    border-radius:5px;
}
button{
    padding:10px 15px;
    border:none;
    border-radius:5px;
    cursor:pointer;
    margin:5px;
}
.weather,.forecast{
    margin-top:20px;
}
.cards{
    display:flex;
    flex-wrap:wrap;
    justify-content:center;
    gap:10px;
}
.card{
    background:rgba(255,255,255,0.2);
    padding:10px;
    border-radius:10px;
    width:120px;
}
</style>

</head>
<body>

<div class="container">
<h1>🌤 Weather App</h1>

<input type="text" id="city" placeholder="Enter City">
<button onclick="searchWeather()">Search</button>
<button onclick="getLocation()">Use My Location</button>

<div class="weather" id="weather"></div>

<div class="forecast">
<h2>5-Day Forecast</h2>
<div class="cards" id="forecast"></div>
</div>

</div>

<script>

const API_KEY="YOUR_API_KEY";

function searchWeather(){
    const city=document.getElementById("city").value;
    fetchWeather(`q=${city}`);
}

function getLocation(){
    navigator.geolocation.getCurrentPosition(position=>{
        const lat=position.coords.latitude;
        const lon=position.coords.longitude;
        fetchWeather(`lat=${lat}&lon=${lon}`);
    },()=>{
        alert("Location access denied");
    });
}

function fetchWeather(query){

fetch(`https://api.openweathermap.org/data/2.5/weather?${query}&units=metric&appid=${API_KEY}`)
.then(res=>res.json())
.then(data=>{

document.getElementById("weather").innerHTML=`
<h2>${data.name}</h2>
<h3>${data.main.temp} °C</h3>
<p>${data.weather[0].description}</p>
<p>Humidity: ${data.main.humidity}%</p>
<p>Wind: ${data.wind.speed} m/s</p>
`;

fetchForecast(query);

});

}

function fetchForecast(query){

fetch(`https://api.openweathermap.org/data/2.5/forecast?${query}&units=metric&appid=${API_KEY}`)
.then(res=>res.json())
.then(data=>{

let output="";

for(let i=0;i<data.list.length;i+=8){

let day=data.list[i];

output+=`
<div class="card">
<h4>${day.dt_txt.split(" ")[0]}</h4>
<p>${day.main.temp} °C</p>
<p>${day.weather[0].main}</p>
</div>
`;

}

document.getElementById("forecast").innerHTML=output;

});

}

window.onload=getLocation;

</script>

</body>
</html>
