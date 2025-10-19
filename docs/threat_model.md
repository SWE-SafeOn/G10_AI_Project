# Threat Model

## Assets and Trust Boundaries
- **IoT Devices:** Endpoints generating telemetry and executing automation commands.
- **Edge Gateway (SafeOn Agent):** Collects, preprocesses, and analyzes data locally before forwarding to the cloud.
- **Central Services:** Cloud-based data lake, analytics pipelines, alerting services, and management consoles.
- **User Interfaces:** Mobile app, web dashboard, and API clients that interact with alerts and device states.
- **Network Infrastructure:** Home Wi-Fi, wired backhaul, VPN tunnels, and ISP-provided equipment.

## Adversaries and Capabilities
- External attackers attempting remote exploitation, command injection, or DDoS attacks against IoT devices.
- Malicious insiders or compromised household members with physical access to devices or credentials.
- Automated botnets targeting exposed services for credential stuffing or brute-force attacks.

## Attack Scenarios
1. **Abnormal Traffic Surge:**
   - Attackers flood the gateway with malformed MQTT/HTTP packets to exhaust processing resources.
   - Objective: degrade anomaly detection accuracy and create blind spots for subsequent intrusions.
2. **Device Hijacking:**
   - Exploiting weak firmware or default credentials to take control of a camera or smart lock.
   - Objective: manipulate device state, exfiltrate video streams, or pivot into the home network.
3. **Command Replay & Spoofing:**
   - Intercepting automation commands and replaying them to trigger unauthorized actions.
   - Objective: unlock doors, disable alarms, or generate false positives to desensitize users.
4. **Data Exfiltration via Compromised Dashboard:**
   - Compromising the user interface or API to download historical telemetry and user profiles.
   - Objective: gain insights into user behavior, occupancy patterns, or sensitive biometric data.

## Detection and Response Policies
- Rate limiting and protocol validation at the gateway to identify abnormal traffic volumes.
- Mutual TLS, rotating credentials, and device attestation for all device-to-gateway communications.
- Continuous behavioral modeling (Isolation Forest, LSTM) to flag anomalous device states or traffic bursts.
- Automated isolation workflows that quarantine compromised devices and revoke access tokens.
- Incident response playbooks that notify administrators, collect forensic evidence, and trigger firmware updates.

## Privacy & Security Requirements
- **Data Minimization:** Collect only telemetry required for anomaly detection; redact or hash personal identifiers.
- **Encryption:** Enforce end-to-end encryption (TLS 1.3+) for data in transit and AES-256 for data at rest.
- **Access Control:** Role-based access for dashboards and APIs; enforce MFA for administrative functions.
- **Auditability:** Immutable logging of configuration changes, authentication events, and detection results.
- **Compliance Alignment:** Map controls to relevant standards (e.g., GDPR data handling, ISO/IEC 27001 security controls).
- **Resilience:** Regular patching, secure boot, and tamper detection on edge devices to maintain system integrity.
