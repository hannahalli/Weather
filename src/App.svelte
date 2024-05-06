<script>
    import { onMount } from 'svelte';

    let weatherData = null;
    let tomorrowWeather = null;
    let moonPhase = null;

    const API_KEY = 'dc33d7d76868dc9f462c82ac2c16c9a4'; // Your OpenWeatherMap API key
    const CITY_NAME = 'Amherst';
    const STATE_CODE = 'MA';
    const LATITUDE = '42.3732'; // Latitude of Amherst, MA
    const LONGITUDE = '-72.5199'; // Longitude of Amherst, MA
    const API_URL = `https://api.openweathermap.org/data/2.5/weather?lat=${LATITUDE}&lon=${LONGITUDE}&units=imperial&appid=${API_KEY}`;
    const FORECAST_URL = `https://api.openweathermap.org/data/2.5/forecast?lat=${LATITUDE}&lon=${LONGITUDE}&units=imperial&appid=${API_KEY}`;

    const moonPhases = {
        '2024-04-30': 'Last Quarter',
        '2024-05-01': 'Last Quarter',
        '2024-05-02': 'Waning Crescent',
        '2024-05-03': 'Waning Crescent',
        '2024-05-04': 'Waning Crescent',
        '2024-05-05': 'Waning Crescent',
        '2024-05-06': 'Waning Crescent',
        '2024-05-07': 'New Moon',
        '2024-05-08': 'Waxing Crescent',
        '2024-05-09': 'Waxing Crescent',
        '2024-05-10': 'Waxing Crescent',
        '2024-05-11': 'Waxing Crescent',
        '2024-05-12': 'Waxing Crescent',
        '2024-05-13': 'Waxing Crescent',
        '2024-05-14': 'Waxing Crescent',
        '2024-05-15': 'First Quarter',
        '2024-05-16': 'Waxing Gibbous',
        '2024-05-17': 'Waxing Gibbous',
        '2024-05-18': 'Waxing Gibbous',
        '2024-05-19': 'Waxing Gibbous'
    };

    // Function to fetch weather data
    async function fetchWeatherData() {
        const response = await fetch(API_URL);
        const data = await response.json();
        weatherData = data;
    }

    // Function to fetch tomorrow's weather forecast
    async function fetchTomorrowWeather() {
        const response = await fetch(FORECAST_URL);
        const data = await response.json();
        // Assuming tomorrow's weather can be obtained from the forecast for the next day
        tomorrowWeather = data.list[8]; // 8th item for tomorrow (assuming data is in 3-hour intervals)
    }

    onMount(() => {
        fetchWeatherData();
        fetchTomorrowWeather();
        // Get today's date in YYYY-MM-DD format
        const todayDate = new Date().toISOString().split('T')[0];
        moonPhase = moonPhases[todayDate] || 'Unknown';
    });

    // Function to format time from UNIX timestamp
    function formatTime(timestamp) {
        const date = new Date(timestamp * 1000);
        return date.toLocaleTimeString();
    }

    // Function to get tomorrow's date
    function getTomorrowDate() {
        const date = new Date();
        date.setDate(date.getDate() + 1);
        return date.toLocaleDateString();
    }
</script>

<div class="container">
    <h2>Hark! The Weather in {CITY_NAME}, {STATE_CODE}</h2>
    {#if weatherData && tomorrowWeather && moonPhase}
        <div class="weather-container">
            <p>The current temperature is a delightful {weatherData.main.temp}°F</p>
            <p>The sun shall rise at {formatTime(weatherData.sys.sunrise)}.</p>
            <p>And the sun shall set at {formatTime(weatherData.sys.sunset)}</p>
            <p>Look! The weather is {weatherData.weather[0].description}</p>
            <p>Tonight, the moon shall be in its {moonPhase} phase</p>
        </div>
    {:else}
        <p>Hold tight, the weather is brewing...</p>
    {/if}
    <p class="text"> Come back at sunset to watch the celestial show!</p>
</div>

<style>
    /* A touch of magic in the air */
    .text {
        font-size: 18px;
    }
    .container {
        position: absolute;
        top: 65px; /* Adjust the top position as per your requirement */
        left: calc((1536px - 500px) / 2 + 250px); /* Adjust the left position as per your requirement */
        width: 270px; /* Increase width */
        height: 550px; /* Increase height */
        background-color: #FAEBD7; /* Set light brown background color */
        border: 16px solid #63310e; /* Set dark brown border */
        padding: 20px;
        border-radius: 5px;
        /* overflow-y: auto; Enable scrolling if content overflows */
        z-index: 1; /* Ensure the container is above other elements */
        pointer-events: auto; /* Enable pointer events for the container */
        text-align: center; /* Center text horizontally */
        color: #8B4513; /* Set text color to dark brown */
        font-family: Chalkboard, sans-serif; /* Set font to Chalkduster */
    }


    h2 {
        margin-top: 10px; /* Lower the margin top */
        font-size: 25px;
    }

    .weather-container {
        background-color: #e3fad7; /* Set light brown background color */
        padding: 10px; /* Add padding */
        border-radius: 20px; /* Add border radius */
        margin-bottom: 5px; /* Add margin bottom */
        margin-top: 5px;
        width: 220px; /* Set width to auto */
        align-items: center;
        margin-left: 5px;
        border: 5px solid #8B4513; /* Set dark brown border */
    }

    p {
        font-size: 18px; /* Set font size for paragraphs */
    }

    button {
        font-size: 25px;
        padding: 10px 20px;
        width: 220px; /* Set width to auto */
        background-color: darkgreen;
        color: white;
        border: 4px solid #8B4513; /* Set dark brown border */
        border-radius: 10px;
        cursor: pointer;
    }

    button:hover {
        background-color: green;
    }
</style>