# PolarBuoy
## Run
npm install
npm run dev
Open http://localhost:5173

## Ingest
POST JSON to http://localhost:3001/api/ingest. lat/lon are required:
{"name":"ESP32-01","lat":-61.2,"lon":12.5,"sst":-1.1,"salinity":34.1,"hs":2.8,"tp":10.2,"wd":210,"wind":19,"air":-5,"pressure":1008,"battery":81,"batteryV":12.4,"solarW":17,"internal":3.2,"signal":91}
curl -X POST http://localhost:3001/api/ingest -H "Content-Type: application/json" -d '{"lat":-61.2,"lon":12.5,"sst":-1.1,"battery":81,"hs":2.8,"tp":10.2,"wind":19,"pressure":1008}'
## ESP32
Use WiFi + HTTPClient:
#include <WiFi.h>
#include <HTTPClient.h>
void setup(){WiFi.begin("SSID","PASSWORD");while(WiFi.status()!=WL_CONNECTED)delay(250);}
void loop(){HTTPClient h;h.begin("http://YOUR-LAPTOP-IP:3001/api/ingest");h.addHeader("Content-Type","application/json");h.POST("{\"lat\":-61.2,\"lon\":12.5,\"sst\":-1.1,\"battery\":81,\"hs\":2.8,\"tp\":10.2,\"wind\":19,\"pressure\":1008}");h.end();delay(2000);}
## GLB
Put public/buoy.glb. The compact build uses a procedural buoy fallback.
## Tuning
Wave scale is in Ocean() uniforms in src/main.tsx; buoy motion is in Buoy(); sun is Sky sunPosition/inclination/azimuth; fog is Canvas fog; low graphics changes DPR and mesh resolution.
## Limitations
This compact demo uses simulated marine values and a cached coarse flow field rather than a full production GPU streamline engine. Esri imagery availability/terms apply. The GLB path is documented for extension; procedural geometry is active by default.
