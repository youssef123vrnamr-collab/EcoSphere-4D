# EcoSphere-4D

**My entry for the NASA Space Apps Challenge.**

Hi, I'm **Youssef Amr**. I designed, coded and tested this project entirely by myself, as a solo participant.

Earth Trend Detective is an interactive 3D Earth that mixes real NASA and weather data with a climate projection model, so you can see what the planet is doing right now and where current trends may lead.

---

## Why I built it

Climate data is usually spread across charts, tables and separate websites. I wanted one place where anyone can spin the globe, pick a location, and see real observations next to future projections. I also wanted storms, winds, clouds and ocean currents to behave as the data says they should, instead of being drawn by hand.

---

## What it does

**Real data (observed)**
- **NASA GIBS** satellite layers: VIIRS true color, land surface temperature, vegetation index (NDVI) and sea surface temperature, with deep-zoom tiles when you zoom in.
- **NASA POWER** temperature history (1990 to last year) for any location you pick, with a trend in °C per decade.
- **NASA EONET** active storms and wildfires, marked on the globe. Storm tracks are drawn, and the reported wind speed is used when available.
- **Open-Meteo** live data: wind at 850, 700 and 200 hPa, humidity, cloud cover, sea surface temperature and ocean currents. Wind comes from the hourly forecast for the next 72 hours, and the simulation clock starts at the real UTC time.
- Real-time **day/night terminator** that follows the Sun.

**Storm simulation**
- No storm location is written in the code. A storm can only form where the live data allows it: warm ocean (26.5 °C or more), cyclonic low-level spin, low wind shear, moist mid-levels, enough distance from the equator, and a bias from where storms formed in the past (NASA EONET archive).
- New storms start weak and spin up gradually. Steering and weakening come from the live wind and shear.
- Storm shape is not a fixed drawing. Each air parcel is carried by the real wind plus the storm's own vortex.
- Storms feed back on the planet: they thicken the cloud layer around them, cool the sea surface beneath them (a cold wake that recovers slowly), and push ocean currents with their winds.

**Projections (illustrative)**
- Three emissions scenarios: SSP1-2.6, SSP2-4.5 and SSP5-8.5, up to the year 2100.
- Warming, Arctic ice loss, sea level rise, CO₂ and vegetation change are shown on the globe.
- The model is a simple illustrative one tuned to IPCC AR6 trends. It is **not** an official forecast.

**Command center**
- A built-in terminal for quick control and a short written analysis. An optional AI key (Claude or OpenAI) can be pasted into the code for richer interpretations. Without a key, a built-in analyst answers.

---

## How to run it

The whole project is a single HTML file.

1. Download `earth-trend-detective.html`.
2. Open it in a modern browser (Chrome, Edge, Firefox or Safari) with an internet connection.

No installation or build step is needed.

### Controls
- Drag to rotate, scroll or pinch to zoom, click the globe to lock a location.
- Type a place in **Target region** (for example `Cairo`) to see its live weather and NASA temperature trend.
- Use the **Globe layer**, **Emissions scenario** and **Projection year** controls, or press **Play timeline**.
- Use the speed, wind, currents and cyclone buttons to control the simulation.

### Terminal commands

| Command | What it does |
|---|---|
| `help` | List all commands |
| `goto <place>` | Lock a location, for example `goto Cairo` |
| `layer <true\|lst\|ndvi\|sst\|sim\|simtemp\|cyc>` | Switch the globe layer |
| `live` | Refresh wind, sea temperature and NASA storm data |
| `run` | Run the full trend analysis |
| `year <year>` | Set the projection year |
| `scenario <126\|245\|585>` | Choose an emissions scenario |
| `ask <question>` | Ask the AI engine |
| `clear` | Clear the terminal |

---

## Built with

- HTML, CSS and JavaScript (no framework)
- [Three.js](https://threejs.org/) for the 3D globe and custom shaders
- NASA GIBS, NASA POWER and NASA EONET
- Open-Meteo (forecast, marine and geocoding APIs)

---

## Limitations

- Live wind comes on a coarse global grid (15°) and covers a 72-hour forecast. After that, the last wind field is held steady.
- The storm simulation is a physics-inspired model for exploration. It is not a weather forecast and should not be used for safety decisions.
- Future climate values are illustrative and tuned to IPCC AR6 trends.
- Some data sources may be temporarily unavailable. The app shows a message in the terminal when that happens.

---

## Credits

Data and imagery: NASA (GIBS, POWER, EONET) and Open-Meteo. Climate trends referenced from IPCC AR6.

Created by **Youssef Amr** for the NASA Space Apps Challenge.
