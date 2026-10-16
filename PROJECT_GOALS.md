# Custom piSignage Server - Project Requirements & Context

## Target Environment Constraints
- **Hardware:** HP t430 Thin Client
- **RAM:** 2GB System Memory (Extremely low-memory footprint required)
- **CPU Architecture:** Dual-Core Intel Celeron / Atom (No AVX instruction set)
- **Database Requirement:** MongoDB 4.0 or 4.4 maximum (MongoDB 5.0+ requires AVX support and will fail to execute).
- **Execution Platform:** Docker / Docker Compose running on Ubuntu / CasaOS.

## Priority Modifications

### 1. Licensing Bypass
- Short-circuit all license key checks on player heartbeat/registration API endpoints (`/api/players`, `/api/register`, or related routes).
- Ensure players are always served or assigned a dummy/valid license payload so that the "This player is powered by pisignage.com" banner/ticker never displays on display endpoints.

### 2. Branding & UI Adjustments
- Locate public/UI layouts containing links to `http://pisignage.com/` or `https://pisignage.com/`.
- Keep the piSignage logo images intact, but convert links to static elements (remove `href` anchors to external sites).
- Remove the Twitter / Social Media follow components and links from header, footer, and sidebar templates.

### 3. Optimization Guidelines
- Keep Node.js dependencies lightweight to fit within the 2GB memory footprint.
- Avoid introducing heavy background workers or excessive caching layers that spike RAM usage.
