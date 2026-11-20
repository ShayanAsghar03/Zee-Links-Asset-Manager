# Zee Links Asset Manager (AssetTrack System)

A full-stack prototype for registering, viewing, and searching physical assets. It stores asset records in SQLite and uploaded photographs on the server.

## Features

- React dashboard with asset summary and a responsive asset list.
- Registration form for an asset ID, category, and photograph.
- Automatic creation timestamp stored with each database record.
- REST API backed by SQLite.
- Search assets by ID.
- View a saved record and its uploaded photograph.
- Image uploads limited to 5 MB.

## Tech stack

- **Frontend:** React, Vite, Tailwind CSS, Lucide React
- **Backend:** Node.js, Express, Multer, CORS
- **Database:** SQLite (`sqlite3`)

## Project structure

```text
AssetTrack System/
├── backend/
│   ├── server.js
│   ├── package.json
│   ├── assets.db        # SQLite database; created on startup if missing
│   └── uploads/         # Uploaded photographs
└── frontend/
    ├── src/
    ├── package.json
    └── vite.config.js
```

## Run locally

### 1. Start the backend

```bash
cd backend
npm install
npm start
```

The API listens on port `5000`. The server creates the uploads directory and database table on startup if needed.

### 2. Start the frontend

In a second terminal:

```bash
cd frontend
npm install
npm run dev
```

Open the Vite URL shown in the terminal, usually `http://localhost:5173`.

## API endpoints

- `GET /api/assets` — list all assets.
- `GET /api/assets/search?q=<asset-id>` — search asset IDs.
- `POST /api/assets` — create an asset using multipart fields `asset_id`, `category`, and `image`.
- `GET /uploads/<filename>` — serve an uploaded photograph.

Asset IDs must be unique. The database records `created_at` automatically.

## Warehouse network access

The backend can run on an on-premises host. The server listens on the configured port (default `5000`); devices on the same network need to reach that host through its local IPv4 address, and the host firewall must allow the required ports.

**The current frontend is configured for local development:** its API address is hardcoded as `http://localhost:5000`. To use the dashboard from another device, configure the frontend to use the warehouse host's address for API requests and uploaded image URLs, then serve the frontend on the LAN (for Vite development, use `npm run dev -- --host 0.0.0.0`). The other device should open the warehouse host's frontend URL, not its own `localhost` URL.

## Future automation

The `POST /api/assets` endpoint accepts structured `asset_id` and `category` values along with an image. An upstream barcode/RFID reader, camera pipeline, or other edge service could submit this data through the same API in a future iteration.
