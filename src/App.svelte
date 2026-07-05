<script>
  let city = "";
  let weather = null;
  let loading = false;
  let error = "";

  // Get your free API key from https://openweathermap.org/api
  const API_KEY = "YOUR_API_KEY";

  async function getWeather() {

    if (!city) return;

    loading = true;
    error = "";
    weather = null;

    try {

      const response = await fetch(
        `https://api.openweathermap.org/data/2.5/weather?q=${city}&appid=${API_KEY}&units=metric`
      );

      if (!response.ok) {
        throw new Error("City not found");
      }

      weather = await response.json();

    } catch (err) {
      error = err.message;
    }

    loading = false;
  }
</script>

<div class="container">

<h1>🌤 Weather Dashboard</h1>

<input
bind:value={city}
placeholder="Enter city"
/>

<button on:click={getWeather}>
Search
</button>

{#if loading}
<p>Loading...</p>
{/if}

{#if error}
<p class="error">{error}</p>
{/if}

{#if weather}

<div class="card">

<h2>{weather.name}</h2>

<img
src="https://openweathermap.org/img/wn/{weather.weather[0].icon}@2x.png"
alt="weather icon"
/>

<p><b>Temperature:</b> {weather.main.temp} °C</p>

<p><b>Humidity:</b> {weather.main.humidity}%</p>

<p><b>Wind:</b> {weather.wind.speed} m/s</p>

<p><b>Condition:</b> {weather.weather[0].description}</p>

</div>

{/if}

</div>

<style>

body{
background:#f4f6f8;
font-family:Arial;
}

.container{

width:420px;
margin:40px auto;
text-align:center;

}

input{

padding:10px;
width:240px;

}

button{

padding:10px 18px;
margin-left:10px;
cursor:pointer;

}

.card{

margin-top:25px;
background:white;
padding:20px;
border-radius:10px;
box-shadow:0 0 10px rgba(0,0,0,.15);

}

.error{

color:red;
font-weight:bold;

}

</style>
