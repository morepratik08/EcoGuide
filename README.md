# 🌿 EcoGuide

A responsive web app and chatbot for environmental conservation, with a clean green UI.

## Features
- **💬 Chatbot** — rule-based intent parser. Understands queries like
  *"where can I recycle PET bottles in Pune?"*, *"e-waste dealers in Hyderabad"*,
  *"tree plantation drives in Bengaluru"* and answers **only from the database**.
- **♻️ Plastic recycling centres** — search by city or pincode; results show name,
  address, accepted plastic types (resin codes 1–7: PET, HDPE, PVC, LDPE, PP, PS),
  contact and timings, on cards **plus a Leaflet/OpenStreetMap map with markers**.
- **🌳 Tree plantation** — upcoming drives by location (date, organiser, contact);
  for a chosen city, live **current weather + 5-day forecast** from Open-Meteo with an
  explicit planting verdict (temperature 15–38 °C, rain < 10 mm, wind < 30 km/h = GOOD),
  and **native tree species** suggestions per region.
- **⚡ E-waste dealers** — authorised collectors by city with address, phone and items
  accepted (phones, batteries, laptops).
- **🔐 Admin page** (`/admin`, default password `ecoguide-admin`) — add / edit / delete
  entries in all tables, with a *verified* flag and per-entry source URL.
- **⚠️ Disclaimer** shown wherever results appear: *verify details by phone before visiting.*

## Tech
- Node.js + Express, SQLite (`node:sqlite`, no native deps needed on Node ≥ 22)
- Leaflet 1.9 + OpenStreetMap tiles (no API key)
- Open-Meteo for weather & forecast (no API key)
- Nominatim (OpenStreetMap) for address geocoding, cached in SQLite

## Run
```bash
npm install        # installs express only
node server.js     # → http://localhost:3000
```
Admin: http://localhost:3000/admin — password `ecoguide-admin` (override with `ADMIN_PASSWORD` env var).

The SQLite database (`data/ecoguide.db`) is created and seeded on first run from
`seed.js`. Seed data was compiled from **real public sources** (Google Maps business
listings, CPCB/SPCB authorised-recycler references, official NGO websites). Every entry
stores its source URL; entries are marked **unverified** until confirmed — unknown fields
are left blank, nothing is invented.

## API
| Endpoint | Purpose |
|---|---|
| `POST /api/chat {message}` | chatbot answer (JSON: mode, text, items, weather, species) |
| `GET /api/facilities?query=Pune PET` | recycling centres (city/pincode/plastic filter) |
| `GET /api/dealers?query=Hyderabad` | e-waste dealers |
| `GET /api/drives?city=Pune` | plantation drives |
| `GET /api/weather?city=Pune` | current + 5-day forecast + planting verdict |
| `GET /api/species?city=Pune` | native species |
| `GET/POST/PUT/DELETE /api/admin/:table` | CRUD (header `x-admin-password`) |

## Structure
```
server.js          Express server, SQLite, chatbot parser, weather rules, admin CRUD
seed.js            Seed data with sources (facilities, dealers, drives)
public/index.html  Main app (chat + 3 modules, maps)
public/admin.html  Admin CRUD page
data/ecoguide.db   SQLite DB (created on first run)
```
