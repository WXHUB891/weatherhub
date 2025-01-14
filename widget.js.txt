const pinnedCities = ["Gainesville", "Ocala"];
const rotatingCities = [
  "Lake City", "Palatka", "Starke", "High Springs", "Alachua",
  "Chiefland", "Williston", "Newberry", "Cedar Key", "Trenton",
  "Keystone Heights", "Spring Hill", "Crystal River", "Homosassa",
  "Brooksville", "Dunnellon", "Inglis", "Yankeetown"
];

const API_KEY = "0fbf640ded574249b2c195555251401"; // Replace with your OpenWeatherMap API key
const temperaturesDiv = document.getElementById("temperatures");

async function fetchWeather(city) {
  try {
    const response = await fetch(
      `https://api.openweathermap.org/data/2.5/weather?q=${city}&units=imperial&appid=${API_KEY}`
    );
    const data = await response.json();
    return {
      city: city,
      temp: data.main.temp
    };
  } catch (error) {
    console.error(`Error fetching weather data for ${city}:`, error);
    return {
      city: city,
      temp: "N/A"
    };
  }
}

async function updateWidget() {
  temperaturesDiv.innerHTML = "";

  // Fetch and display pinned cities
  const pinnedWeatherPromises = pinnedCities.map(city => fetchWeather(city));
  const pinnedWeatherData = await Promise.all(pinnedWeatherPromises);

  pinnedWeatherData.forEach(data => {
    const cityDiv = document.createElement("div");
    cityDiv.className = "city";
    cityDiv.innerHTML = `<strong>${data.city}</strong><br>${data.temp}°F`;
    temperaturesDiv.appendChild(cityDiv);
  });

  // Fetch and display rotating cities
  const randomCities = rotatingCities.sort(() => 0.5 - Math.random()).slice(0, 8);
  const rotatingWeatherPromises = randomCities.map(city => fetchWeather(city));
  const rotatingWeatherData = await Promise.all(rotatingWeatherPromises);

  rotatingWeatherData.forEach(data => {
    const cityDiv = document.createElement("div");
    cityDiv.className = "city";
    cityDiv.innerHTML = `<strong>${data.city}</strong><br>${data.temp}°F`;
    temperaturesDiv.appendChild(cityDiv);
  });
}

// Initialize the widget
updateWidget();

// Auto-refresh every 5 minutes (300,000 milliseconds)
setInterval(updateWidget, 300000);

function updateTimestamp() {
  const now = new Date();
  document.getElementById("lastUpdated").textContent = 
    `Last updated: ${now.toLocaleTimeString()}`;
}

async function updateWidget() {
  temperaturesDiv.innerHTML = "";
  updateTimestamp(); // Update the timestamp here
  // (rest of the function remains unchanged)
}

