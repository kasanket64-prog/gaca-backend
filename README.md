# GACA Backend

Minimal Express API that stores device telemetry in MongoDB and retrieves the 100 most recent records for one device.

> **Prototype warning:** This API currently has permissive CORS and no authentication. Do not expose it publicly or use real personal, voice, or face telemetry until access control, input validation, and a privacy review are implemented.

## Requirements

- Node.js and npm
- A MongoDB instance and connection string

## Local setup

```bash
npm install
```

Set `MONGODB_URI` in your local shell before starting the server. For example:

```bash
# Bash / macOS / Linux
export MONGODB_URI='mongodb://127.0.0.1:27017/gaca'
npm start
```

The application listens on port `5000` by default. Set `PORT` to override it. The source currently reads `process.env` directly; it does not load a `.env` file automatically, even though the `dotenv` package is listed as a dependency.

## API endpoints

| Method | Path | Description |
| --- | --- | --- |
| `POST` | `/api/data` | Saves a telemetry record in MongoDB. |
| `GET` | `/api/dashboard/:device_id` | Returns up to 100 records for the device, newest first. |

Example **artificial** request (for a local-only test server):

```bash
curl -X POST http://localhost:5000/api/data \
  -H 'Content-Type: application/json' \
  -d '{"device_id":"demo-device","voice":0,"face":0,"session":1}'
```

Schema fields in the current implementation are `device_id` (string), `voice` (number), `face` (number), `session` (number), and `timestamp` (date, default current time). The endpoint does **not** currently validate the request body or implement authorization.

## Before deployment

- Require authentication and enforce per-device read/write permissions.
- Validate and limit payload sizes and field values; return safe errors.
- Restrict CORS to trusted clients and apply rate limiting.
- Configure MongoDB access, TLS, secret management, audit logging, and data retention.
- Add automated tests for authorized/unauthorized access and failure conditions.

Do not commit credentials, populated `.env` files, database dumps, or actual user telemetry.
