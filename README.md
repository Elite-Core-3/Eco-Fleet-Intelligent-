# EcoFleet — Sustainable Logistics Prototype

A responsive HTML/CSS/JavaScript hackathon prototype for sustainable fleet and delivery-route operations, with optional Firebase sign-in and cloud sync.

## Features
- Interactive KPI cards for estimated CO₂ avoided, fuel-cost savings, fleet efficiency and active vehicles.
- Interactive Leaflet map with warehouse, EV markers, delivery stops, charging points, popups and a stylized route layer.
- Fleet telemetry cards/table with battery, cargo utilization, operational status and ETA.
- Dynamic route controls for payload, elevation, traffic avoidance and EV preference.
- Carbon-savings analytics chart with 7/14/30-day demo period selection.
- Driver Companion Mode with route map, live battery and CO₂ readouts, and a play/pause/resume trip simulation.
- Email/password sign-up, sign-in and password reset using Firebase Authentication.
- Live visitor location sharing (browser GPS) and vehicle GPS markers, both stored in Firestore.
- Per-user data: Firestore security rules limit each signed-in user to their own records.
- Trip records saved to Firestore when a driver trip completes.
- Excel export (`.xlsx`) with account details, visitor location, vehicle GPS, trips, analytics, dashboard metrics and network activity.

## Files
| File | Purpose |
|---|---|
| `index.html` | Page structure, sign-in screen and script loading |
| `styles.css`, `auth.css` | Dashboard and sign-in styling |
| `app.js` | Route model, maps, charts, fleet view and driver companion |
| `firebase-config.js` | Your Firebase web config (edit this) |
| `firebase-service.js` | Auth, Firestore sync, location sharing, trip saving and Excel export |
| `firestore.rules` | Firestore security rules (deploy to your Firebase project) |
| `README.md` | This file |

## Run locally (demo mode)
1. Download or clone this repository.
2. Open `index.html` in a modern browser.
3. Without Firebase keys the sign-in screen still appears. Choose **Continue in demo mode** to open the dashboard with simulated data. Excel export works with demo records.

Internet access is required for Leaflet, map tiles, Firebase SDKs, the Excel library (SheetJS) and Google Fonts. The layout and most controls still render if map tiles cannot load.

No build step, npm install or server is required. Location sharing needs `https://` or `localhost`.

## Set up Firebase (sign-in, sync, live location)
1. Go to the Firebase console and create a project.
2. Under **Build → Authentication**, click **Get started** and enable the **Email/Password** provider.
3. Under **Build → Firestore Database**, click **Create database**.
4. Open **Firestore → Rules**, paste the contents of `firestore.rules`, and publish.
5. Under **Project settings → Your apps**, add a **Web** app and copy the config object.
6. Paste those values into `firebase-config.js`, replacing the `REPLACE_WITH_…` placeholders.
7. Under **Authentication → Settings → Authorized domains**, add the domain you publish to (for example `yourname.github.io`). `localhost` is already allowed.

Firebase web config values identify your project and are not secrets. Protection comes from the security rules and Authentication settings.

### Data layout (per signed-in user)
- `users/{uid}` — profile (email, last sign-in)
- `users/{uid}/visitor/current` — `lat`, `lng`, `accuracy`, `sharing`, `updatedAt`
- `users/{uid}/vehicles/{vehicleId}` — `name`, `lat`, `lng`, `battery`, `status`, `updatedAt`. Vehicle devices or a backend write these; the dashboard only reads them.
- `users/{uid}/trips/{tripId}` — `vehicleId`, `distanceKm`, `energyKwh`, `carbonKg`, `durationMin`, `completedAt`

## Publish with GitHub Pages
1. Create a GitHub repository, e.g. `ecofleet-sustainable-logistics`.
2. Upload every file in the table above to the repository root. Keep `firebase-config.js` with your real keys, or leave the placeholders to publish in demo mode.
3. Open **Settings → Pages**.
4. Under **Build and deployment**, select **Deploy from a branch**.
5. Choose the `main` branch and `/ (root)`, then save.
6. Wait for GitHub Pages to publish the site and open the provided URL (`https://<user>.github.io/<repo>/`).
7. Add that domain to Firebase **Authentication → Settings → Authorized domains**, or sign-in will fail on the live site.

Note: `firebase-config.js` is public once published. That is expected for Firebase web apps, as long as `firestore.rules` is deployed.

## Demo notes
- Fleet telemetry and route data are simulated. Vehicle GPS only appears when your own Firestore vehicle records exist.
- Route distances, time, energy and CO₂ are illustrative formulas for prototype interaction, not certified operational calculations. Emission factors are approximate: 0.75 kg CO₂/km for a diesel van and 0.71 kg CO₂/kWh for the grid.
- Map location is centered on Chennai, India for the demonstration.
- Map basemap © OpenStreetMap contributors; tiles served by CARTO. Leaflet is loaded from the unpkg CDN.

## Suggested hackathon demo
1. Show the KPI overview and fleet status.
2. Move the route-optimizer sliders and select **Recalculate route**.
3. Inspect the map markers and popups.
4. Change the carbon analytics time range.
5. Export the Excel report with **Export Excel**.
6. Toggle **Driver mode**, start the simulated trip, and finish it to save a trip record (signed in).
