# ECOSPHERE-4D

**My entry for the NASA Space Apps Challenge, "Be An Earth System Trend Detective!"**

Hi, I'm **Youssef Amr**, and this is my project. I entered as a solo participant.

ECOSPHERE-4D is a 3D Earth you can spin, zoom into and question. It puts real NASA and weather data next to a climate projection model, so you can see what the planet is doing right now and where the trends could take it. You can start with the whole planet and keep zooming until you are looking at a street, without ever leaving the globe.

I used AI tools (Claude) as a coding assistant while building it. The idea, the choices about what to show and the direction of the project are mine.

---

## Why I made it

Climate data is usually scattered across charts, tables and different websites. I wanted one place where anyone can pick a spot on Earth and see what has already happened there, what is happening now, and what could happen next. I also wanted storms, winds, clouds and ocean currents to move the way the data says they should, not the way someone drew them.

---

## What you can do with it

**Zoom from space to street level**
Zooming never kicks you out to a flat map. Real satellite photos are draped on the 3D globe and load in as you get closer, down to about 640 m above the ground. Zoom back out and you are looking at the whole planet again. Tap anywhere to lock that spot, and its elevation, projected sea level and air quality show up in a card under the simulation controls.

**Time machine**
Pick a year from 2000 to today and see NASA satellite images of the same place back then. Turn on before/after and drag the line across the screen to compare that year with today. Shrinking lakes, retreating ice and growing cities are easy to spot.

**Flood what-if**
The map colours the land that sits between today's sea level and the sea level rise projected for the scenario and year you chose. There is also a slider that adds extra metres, so you can ask "what if it were 3 m higher?" for a coastline you care about.

**3D relief**
Hill shading shows mountains and valleys. A relief slider exaggerates the heights, and a tilt slider tips the camera so you see the terrain from the side.

**Share and report**
One button copies a link that reopens exactly what I am looking at (place, zoom, year, scenario). Another downloads a short report of the spot with its numbers and sources. The project is on GitHub and deployed with Vercel.

**Data trust**
A table lists every layer, where it comes from, how sharp it is and what it can and cannot tell you. I wanted anyone to be able to check my numbers.

---

## The real data

- **NASA GIBS** satellite layers: true colour, land surface temperature, vegetation (NDVI) and sea surface temperature. GIBS MODIS images also power the time machine.
- **NASA POWER** temperature history from 1990 for any place you pick, with the trend per decade.
- **NASA EONET** active storms, wildfires and volcanoes, marked on the globe, with observed storm tracks and reported wind speed.
- **NOAA NHC and JTWC** official storm positions and forecast tracks (through the Esri "Active Hurricanes" live feed). Where an official forecast exists, storms follow it.
- **USGS** tsunami-flagged earthquakes from the last week.
- **Open-Meteo** live wind at three heights, humidity, clouds, sea surface temperature and ocean currents. The simulation clock starts at the real UTC time.
- **Esri World Imagery** for the sharp satellite photos at deep zoom.
- **Terrarium elevation tiles** (AWS Open Data) for relief and the flood layer.
- A live day/night line that follows the Sun.

## The storm simulation

I did not place any storm by hand. In the live view, only storms that really exist are shown: each one sits at its last official report, and a storm whose last report is more than 48 hours old is hidden instead of being shown as active (storms older than 30 hours are marked as aging in the terminal). Storms without an official forecast follow their observed track plus up to 24 hours of its real motion. When you move into the future or run the time-lapse, the simulation can form new storms. A simulated storm can only form where the live data allows it: warm ocean (26.5 °C or more), cyclonic spin in the low levels, low wind shear, moist air higher up, enough distance from the equator, plus a nudge from where storms formed in the past (NASA EONET archive). New storms start weak and spin up slowly. Each parcel of air is carried by the real wind plus the storm's own vortex, so the shape comes from the physics and not from a fixed drawing. Storms also push back on the planet: they thicken the clouds around them, leave a cold wake in the sea and add their winds to the ocean-current drift. Around each storm centre the water shows rotating foam bands and outgoing swell rings. This is a visual aid: its size follows the storm's wind, and it is dimmed when the last report is old.

## Projections

There are five emissions scenarios, from SSP1-1.9 to SSP5-8.5, up to 2100. Warming, Arctic ice loss, sea level, CO₂ and vegetation change all show on the globe. This is a simple model tuned to IPCC AR6 trends. It is meant for exploring and is not an official forecast.

## Command center

There is a small terminal for quick control and a short written analysis. You can paste an optional AI key (Claude or OpenAI) into the code for richer answers. Without a key, a built-in analyst replies.

---

## Using it

It is a single HTML file, so there is nothing to install. Use a modern browser with an internet connection.

On start, a loading screen lists each live data feed and opens the globe once they are all ready. If a feed cannot be reached because of the connection, it asks you to check your internet connection and retry. After 15 seconds you can continue anyway and the remaining layers finish loading in the background.

- Drag to rotate, scroll or pinch to zoom, click or tap the globe to lock a spot.
- Type a place in **Target region** (for example `Cairo`) for its live weather and NASA temperature trend.
- Use the **Globe layer**, **Emissions scenario** and **Projection year** controls, or press **Play timeline**.
- The **Explore** card has the time machine, flood what-if, relief, tilt, share link, report and data trust.

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

### Install it like an app

On a phone you can add ECOSPHERE-4D to your home screen from the browser menu. It opens full screen with its own icon, without the browser bar.

### What is in the project

| File | What it is |
|---|---|
| `index.html` | The whole app: globe, simulation, terminal and styles |
| `manifest.webmanifest` | App name, colours and icons for the home-screen install |
| `icon-*.png`, `apple-touch-icon.png` | The ECOSPHERE-4D logo as app icons |
| `sw.js` | Tiny service worker that makes the app installable |
| `README.md` | This file |

---

## Limits

I would rather be upfront about these.

- Live wind comes on a coarse 15° grid and covers a 72-hour forecast. After that the last wind field is held steady.
- The storm simulation is for exploring and is not a weather forecast. Please do not use it for safety decisions.
- Future climate values are illustrative and tuned to IPCC AR6 trends.
- Esri photos are not live and can be years old. The time machine shows one MODIS image per year at 250 m, so clouds and gaps happen.
- Elevation data is off by several metres in places, more in cities and forests. The relief is exaggerated on purpose.
- The flood layer is a simple bathtub model. It ignores tides, storm surge, sea defences and drainage, and it does not check whether water can actually reach a spot. It also only shows land above today's sea level. Treat anything under 1 to 2 m as a rough hint.
- If a data source is down, the terminal tells you, and the globe falls back to its normal textures.
- All live data comes from free public services. They can be slow or limit requests at busy moments, so a layer may load late or be missing.
- Storm positions and winds are the last official report, not an estimate for this minute. The terminal command `storms` lists each report with its age.

---

## Built with

HTML, CSS and JavaScript with no framework, [Three.js](https://threejs.org/) for the globe and custom shaders, NASA GIBS, POWER and EONET, Open-Meteo, Esri World Imagery and Terrarium elevation tiles.

Climate trends referenced from IPCC AR6.

Created by **Youssef Amr** for the NASA Space Apps Challenge.
