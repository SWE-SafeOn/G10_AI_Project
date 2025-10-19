# IoT Device Requirements

## Supported Device Categories
- **Environmental Sensors:** Temperature, humidity, air quality, and motion sensors deployed in rooms and outdoor spaces.
- **Security Devices:** Smart locks, door/window contacts, IP cameras, and alarm sirens.
- **Energy & Appliance Controllers:** Smart plugs, lighting systems, HVAC controllers, and smart appliances (e.g., washing machines, refrigerators).
- **Wearables & Personal Devices:** Health trackers and smart watches that share presence or biometric data with the home hub.
- **Network Infrastructure:** Edge gateways, Wi-Fi routers, and dedicated intrusion detection nodes.

## Deployment Assumptions
- All devices support secure onboarding (e.g., certificate-based provisioning or pre-shared keys).
- Devices expose telemetry via MQTT or HTTPS with mutual authentication.
- Edge gateways run the SafeOn anomaly detection agent and maintain local storage for short-term buffering.

## Data Flow Overview
1. **Collection:**
   - Devices publish telemetry (state changes, sensor readings, network behavior) to the edge gateway.
   - The gateway normalizes payloads, timestamps events, and applies initial validation rules.
2. **Storage:**
   - Validated data is stored in the local time-series database for rapid lookups.
   - Batched records are encrypted and synchronized with the central data lake for long-term retention.
3. **Analysis:**
   - Real-time anomaly detection runs on the edge using rule-based filtering, Isolation Forest, and LSTM models.
   - Aggregated insights feed the monitoring dashboard and trigger alerting workflows (SMS, push notifications, or SIEM integrations).
   - Historical analysis in the cloud data lake refines detection models and supports forensic investigations.

## Integration Points
- REST API for external orchestration platforms to query alerts and device status.
- WebSocket channel for live dashboard updates and command dispatch.
- Optional integration with third-party home assistants via standardized schemas (Matter, OpenHAB).
