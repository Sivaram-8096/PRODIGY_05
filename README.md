# PRODIGY_05
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Weather App</title>
    <style>
        body {
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
            margin: 0;
            font-family: sans-serif;
            background: linear-gradient(135deg, #74b9ff, #0984e3);
        }

        .card {
            background: rgba(255, 255, 255, 0.1);
            backdrop-filter: blur(10px);
            color: white;
            padding: 2em;
            border-radius: 20px;
            width: 100%;
            max-width: 420px;
            margin: 1em;
            text-align: center;
            box-shadow: 0 8px 32px rgba(0, 0, 0, 0.3);
        }

        .search {
            display: flex;
            align-items: center;
            justify-content: center;
            margin-bottom: 2em;
        }

        .search input {
            flex: 1;
            padding: 12px 20px;
            border: none;
            border-radius: 25px;
            background: rgba(255, 255, 255, 0.2);
            color: white;
            font-size: 16px;
            outline: none;
            margin-right: 10px;
        }

        .search input::placeholder {
            color: rgba(255, 255, 255, 0.7);
        }

        .search button {
            padding: 12px 20px;
            border: none;
            border-radius: 25px;
            background: rgba(255, 255, 255, 0.3);
            color: white;
            cursor: pointer;
            font-size: 16px;
            transition: background 0.3s ease;
        }

        .search button:hover {
            background: rgba(255, 255, 255, 0.4);
        }

        .weather-icon {
            width: 100px;
            height: 100px;
            margin: 0 auto 1em;
            font-size: 4em;
            display: flex;
            align-items: center;
            justify-content: center;
        }

        .weather-info {
            margin: 1em 0;
        }

        .weather-info h3 {
            margin: 0 0 0.5em 0;
            font-size: 1.5em;
        }

        .weather-info p {
            margin: 0.5em 0;
            font-size: 1.1em;
        }

        .temperature {
            font-size: 3em;
            font-weight: bold;
            margin: 0.5em 0;
        }

        .error {
            color: #ff6b6b;
            background: rgba(255, 107, 107, 0.2);
            padding: 1em;
            border-radius: 10px;
            margin: 1em 0;
        }

        .loading {
            opacity: 0.7;
        }

        .hidden {
            display: none;
        }
    </style>
</head>
<body>
    <div class="card">
        <h1>🌤 Weather App</h1>
        
        <div class="search">
            <input type="text" id="city-input" placeholder="Enter city name...">
            <button onclick="getWeather()">Search</button>
        </div>

        <div id="weather-info" class="weather-info hidden">
            <div class="weather-icon" id="weather-icon">☀</div>
            <h3 id="city-name">City Name</h3>
            <div class="temperature" id="temperature">--°C</div>
            <p id="description">Description</p>
            <p id="humidity">Humidity: --%</p>
            <p id="wind-speed">Wind Speed: -- m/s</p>
            <p id="feels-like">Feels Like: --°C</p>
        </div>

        <div id="error" class="error hidden">
            City not found. Please try again.
        </div>
    </div>

    <script>
        const API_KEY = 'your_api_key_here'; // You'll need to get a free API key from OpenWeatherMap
        
        function getWeather() {
            const cityInput = document.getElementById('city-input');
            const cityName = cityInput.value.trim();
            
            if (!cityName) {
                showError('Please enter a city name');
                return;
            }

            // Show loading state
            const card = document.querySelector('.card');
            card.classList.add('loading');
            hideError();
            
            // For demo purposes, we'll use mock data
            // In a real app, you'd use: fetch(https://api.openweathermap.org/data/2.5/weather?q=${cityName}&appid=${API_KEY}&units=metric)
            setTimeout(() => {
                const mockWeatherData = getMockWeatherData(cityName);
                displayWeather(mockWeatherData);
                card.classList.remove('loading');
            }, 1000);
        }

        function getMockWeatherData(city) {
            const weatherConditions = [
                { icon: '☀', description: 'Sunny', temp: 28 },
                { icon: '⛅', description: 'Partly Cloudy', temp: 24 },
                { icon: '☁', description: 'Cloudy', temp: 20 },
                { icon: '🌧', description: 'Rainy', temp: 18 },
                { icon: '⛈', description: 'Thunderstorm', temp: 22 },
                { icon: '🌨', description: 'Snow', temp: -2 }
            ];
            
            const randomWeather = weatherConditions[Math.floor(Math.random() * weatherConditions.length)];
            
            return {
                name: city.charAt(0).toUpperCase() + city.slice(1),
                main: {
                    temp: randomWeather.temp,
                    feels_like: randomWeather.temp + Math.floor(Math.random() * 6) - 3,
                    humidity: Math.floor(Math.random() * 40) + 40
                },
                weather: [{
                    description: randomWeather.description,
                    icon: randomWeather.icon
                }],
                wind: {
                    speed: Math.floor(Math.random() * 15) + 2
                }
            };
        }

        function displayWeather(data) {
            const weatherInfo = document.getElementById('weather-info');
            const cityNameEl = document.getElementById('city-name');
            const temperatureEl = document.getElementById('temperature');
            const descriptionEl = document.getElementById('description');
            const humidityEl = document.getElementById('humidity');
            const windSpeedEl = document.getElementById('wind-speed');
            const feelsLikeEl = document.getElementById('feels-like');
            const weatherIconEl = document.getElementById('weather-icon');

            cityNameEl.textContent = data.name;
            temperatureEl.textContent = ${Math.round(data.main.temp)}°C;
            descriptionEl.textContent = data.weather[0].description;
            humidityEl.textContent = Humidity: ${data.main.humidity}%;
            windSpeedEl.textContent = Wind Speed: ${data.wind.speed} m/s;
            feelsLikeEl.textContent = Feels Like: ${Math.round(data.main.feels_like)}°C;
            weatherIconEl.textContent = data.weather[0].icon;

            weatherInfo.classList.remove('hidden');
        }

        function showError(message) {
            const errorEl = document.getElementById('error');
            errorEl.textContent = message;
            errorEl.classList.remove('hidden');
            document.getElementById('weather-info').classList.add('hidden');
        }

        function hideError() {
            document.getElementById('error').classList.add('hidden');
        }

        // Allow Enter key to trigger search
        document.getElementById('city-input').addEventListener('keypress', function(e) {
            if (e.key === 'Enter') {
                getWeather();
            }
        });

        // Initial demo with default city
        window.addEventListener('load', function() {
            document.getElementById('city-input').value = 'London';
            getWeather();
        });
    </script>
</body>
</html>
