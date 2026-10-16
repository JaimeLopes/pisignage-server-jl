# Custom piSignage Server - Project Requirements & Context

## Target Environment Constraints
- **Hardware:** HP t430 Thin Client
- **RAM:** 2GB System Memory (Extremely low-memory footprint required)
- **CPU Architecture:** Dual-Core Intel Celeron / Atom (No AVX instruction set)
- **Database Requirement:** Pin MongoDB to 4.4 for the target CPU (MongoDB 5.0+ requires AVX support and will fail to execute).
- **Execution Platform:** Docker / Docker Compose running on Ubuntu / CasaOS.
- **Runtime Note:** The repository's Dockerfile currently uses Node.js 14, which is end-of-life. Treat this as a constrained legacy deployment and keep it on a trusted LAN; evaluate Node.js upgrades separately for compatibility.

## Priority Modifications

### 1. Licensing Behavior
- Remove server-side license enforcement that blocks player communication or configuration. Player status is received through socket handlers; `/api/players` is primarily the server UI's player-management API, not the player heartbeat endpoint.
- Do not fabricate signed or proprietary license payloads. Verify banner behavior with the intended player software version; this server repository may not control a banner rendered by the player itself.
- Acceptance: a target player can connect and receive its normal configuration without server-side license gating. Record separately whether the player still renders a licensing banner.

### 2. Branding & UI Adjustments
- Locate public/UI layouts containing links to `http://pisignage.com/` or `https://pisignage.com/`.
- Keep the piSignage logo images intact, but convert links to static elements (remove `href` anchors to external sites).
- Remove the Twitter / Social Media follow components and links from header, footer, and sidebar templates.

### 3. Optimization Guidelines
- Keep Node.js dependencies lightweight to fit within the 2GB memory footprint.
- Avoid introducing heavy background workers or excessive caching layers that spike RAM usage.
- Acceptance: run the server and MongoDB together on the HP t430 with a representative media upload/conversion and player connection; record peak memory and confirm the system remains responsive without out-of-memory termination.

### 4. Runtime Compatibility
- Keep the default Docker Compose MongoDB image pinned to 4.4 for the no-AVX CPU.
- Acceptance: validate the Docker Compose configuration and confirm MongoDB starts on the target hardware with the existing data volume.

### 5. External Service Independence
- Do not make automatic requests to or downloads from pisignage.com. Keep the server's default operation and player communication local.
- Player software updates must use release files supplied locally by the operator; they must not be fetched automatically from pisignage.com.
- The installation name is a local data namespace, not a pisignage.com account.
- Acceptance: start the server with outbound access to pisignage.com blocked and verify that server startup and normal player configuration continue to work.
