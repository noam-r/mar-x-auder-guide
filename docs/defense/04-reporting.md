# Reporting Findings

## Purpose

Reporting is the bridge between technical observation and useful defensive action. A Mar-x-Auder demonstration may show duplicated SSIDs, deauthentication behavior, probe request leakage, handshake artifacts, captive portal deception, or Bluetooth visibility. Those observations only become valuable when they are translated into a clear finding that explains what happened, why it matters, what evidence supports it, and what needs to change.

This section defines a reporting model for student, classroom, and defensive research use. It is not a legal report template. It is a technical communication format intended to make wireless findings understandable, reproducible, and ethically scoped.

## Purpose of a finding

A well-formed wireless finding answers five questions:

1. **What was observed?**
2. **Where in the protocol stack did it occur?**
3. **What normal behavior was expected?**
4. **What changed because of observation, injection, interference, impersonation, or deception?**
5. **What defensive lesson or mitigation follows from it?**

The report avoids vague claims such as “the network was hacked” or “the password was exposed.” Most Mar-x-Auder capabilities demonstrate more specific conditions: management-frame weakness, SSID ambiguity, insecure user trust, metadata leakage, weak passphrase risk, missing PMF, or Bluetooth/BLE discoverability.

## Finding structure

Each finding uses a consistent structure.

```text
Title
Short, precise description of the issue.

Capability demonstrated
Observation, capture, injection, interference, impersonation, deception, or audit.

Affected scope
The lab AP, lab client, training portal, Bluetooth test device, or authorized environment involved.

Technologies involved
Relevant guide sections: Wi-Fi, WPA, DHCP, DNS, HTTP, TLS, Bluetooth, packet capture, etc.

Normal flow
How the system should normally behave.

Observed or interfered flow
What changed during the demonstration.

Evidence
Screenshots, PCAP references, device logs, router logs, timestamps, SSIDs, BSSIDs, channels, and relevant frame types.

Impact
What the behavior means in practical terms.

Ethical boundary
Confirmation that the finding was produced only against owned, lab, or explicitly authorized devices.

Defensive interpretation
What configuration, monitoring, training, or process change would reduce the risk.

Limitations
What was not proven, what was not tested, and what cannot be inferred.
```

## Evidence types

Wireless demonstrations can produce several forms of evidence. The report identifies the evidence type and avoids overstating what it proves.

| Evidence type | Examples | What it supports | What it does not prove |
|---|---|---|---|
| Device screenshot | Mar-x-Auder scan result, portal log, selected target screen | The device observed or produced a specific state | That the condition affected non-lab devices |
| PCAP file | Beacon frames, probe requests, EAPOL frames, deauth frames | The frame-level behavior occurred | The user intent behind the behavior |
| Router/AP log | Client disconnect, association, DHCP lease | The AP observed client behavior | The full radio environment |
| Client screenshot | Wi-Fi menu duplicates, captive portal page, certificate warning | User-visible effect | That the user would necessarily be deceived |
| Timing notes | Before/after timestamps | Sequence of events | Root cause without supporting packet/log evidence |
| Configuration screenshot | WPA mode, PMF setting, SSID, channel | Defensive posture during test | Behavior under other configurations |

## Protocol-stack classification

Each finding identifies where the relevant behavior occurred. This prevents misleading explanations.

```text
Radio          Signal strength, channel use, interference, range
802.11         Beacons, probes, authentication, association, deauth, disassociation
WPA/WPA2/WPA3  Key negotiation, EAPOL, PMKID, SAE, PMF
IP             Addressing, routing, local network reachability
DHCP           Client network configuration
DNS            Name resolution or redirection
TCP/UDP         Transport behavior and service ports
HTTP           Captive portal and web-page delivery
TLS            Certificate validation and encrypted session trust
Application    User interface, login prompt, training portal, user decision
Bluetooth/BLE  Advertising, scanning, pairing, nearby device visibility
```

Example classification for a deauthentication finding:

```text
Primary layer: 802.11 management frames
Secondary layer: WPA/PMF defensive behavior
Not involved: HTTP, TLS, DNS, application credentials
```

Example classification for an evil portal finding:

```text
Primary layer: HTTP/application deception
Supporting layers: Wi-Fi AP behavior, DHCP, DNS
Important boundary: TLS certificate validation
Not a WPA cryptographic break
```

## Severity language

Severity is based on the demonstrated condition, not on dramatic language.

| Severity | Meaning |
|---|---|
| Informational | Useful observation with no immediate weakness demonstrated. |
| Low | Misconfiguration, exposure, or behavior that has limited impact in the demonstrated context. |
| Medium | A realistic issue that could confuse users, disrupt lab clients, or expose metadata under common conditions. |
| High | A condition that can cause meaningful disruption, credential deception, or authentication-audit risk in the scoped environment. |
| Critical | Reserved for confirmed, high-impact compromise or exposure in an authorized production assessment. Most classroom Mar-x-Auder findings should not use this level. |

Severity includes context. For example, duplicate SSIDs in a classroom demo may be a medium user-trust lesson, while duplicate SSIDs observed near a high-risk enterprise environment may require a different operational response.

## Example finding: duplicate SSID advertisement

```markdown
### Finding: Lab SSID can be visually cloned in nearby Wi-Fi menus

## Capability demonstrated

Impersonation / Injection

## Affected scope

Lab router `LabNetwork` and one lab client laptop. No third-party networks or clients were targeted.

## Technologies involved

- 802.11 beacon frames
- SSID and BSSID distinction
- WPA/WPA2/WPA3 authentication boundaries
- User-interface trust

## Normal flow

The legitimate AP advertises the SSID `LabNetwork` using beacon frames. A client sees the network name and, if configured, attempts to authenticate and associate with the legitimate AP.

## Observed flow

The Mar-x-Auder transmitted additional beacon frames using the same SSID label. The client Wi-Fi menu displayed duplicate or confusing network entries. Packet capture showed different BSSIDs advertising the same SSID.

## Evidence

- Screenshot of client Wi-Fi menu showing duplicate `LabNetwork` entries.
- PCAP file: `lab-ap-clone-001.pcap`.
- Relevant frame type: beacon.
- Legitimate BSSID: `AA:AA:AA:AA:AA:AA`.
- Demonstration BSSID values: `BB:BB:BB:BB:BB:BB`, `CC:CC:CC:CC:CC:CC`.

## Impact

The SSID is not a reliable identity signal. A user can be shown a familiar network name even when the advertising radio is not the legitimate AP. This does not mean the attacker has the real WPA key, but it can support confusion or follow-on deception.

## Ethical boundary

The demonstration was limited to a lab SSID and a lab client. Broadcasting cloned-looking names in a shared environment would be unethical because it can confuse or mislead uninvolved users.

## Defensive interpretation

Users should not be trained to trust a network name alone. Defenders should use WPA2/WPA3 correctly, avoid open networks for trusted services, monitor for duplicate SSIDs/BSSIDs where practical, and educate users about unexpected captive portals and certificate warnings.

## Limitations

This finding does not prove that encrypted traffic was decrypted, that the WPA password was recovered, or that a client joined the cloned AP.
```

## Example finding: deauthentication behavior

```markdown
### Finding: Lab client disconnected after receiving forged deauthentication frames

## Capability demonstrated

Injection / Interference / Disruption

## Affected scope

One lab AP and one lab client. No production or third-party network was involved.

## Technologies involved

- 802.11 management frames
- Deauthentication and disassociation
- Protected Management Frames
- Packet capture

## Normal flow

After association and key negotiation, the client remains connected to the AP and exchanges encrypted data frames.

## Observed flow

During the demonstration, the Mar-x-Auder transmitted deauthentication frames in the lab environment. The client disconnected and attempted to reconnect. Packet capture showed deauthentication frames followed by a reconnect sequence.

## Evidence

- PCAP file: `lab-deauth-001.pcap`.
- Relevant frame type: deauthentication.
- Client MAC: lab-controlled test client.
- AP BSSID: lab AP.
- Router log: client disconnect and reconnect around the same timestamp.

## Impact

The demonstration shows that unprotected management frames can be used to disrupt connectivity. It does not show password recovery, traffic decryption, or compromise of the AP.

## Ethical boundary

This capability intentionally disrupts connectivity. It is legitimate only when performed against owned or explicitly authorized lab devices. Using it against uninvolved users is unethical because it interferes with their network access.

## Defensive interpretation

Where supported, enable WPA3 or Protected Management Frames. Monitor for repeated deauthentication/disassociation patterns. Treat unexplained waves of client disconnects as a possible wireless-layer interference signal.

## Limitations

The result depends on AP/client support for PMF, signal conditions, firmware behavior, and channel selection. The demonstration does not prove that every client or every AP would behave the same way.
```

## Example finding: training evil portal

```markdown
### Finding: Training captive portal collected fake credentials from lab client

## Capability demonstrated

Impersonation / Deception / Application-layer collection

## Affected scope

Training portal SSID, one lab client, and fake credentials only.

## Technologies involved

- Wi-Fi AP behavior
- DHCP
- DNS
- HTTP
- Captive portal behavior
- TLS and certificate trust boundaries
- User-interface trust

## Normal flow

A user joins a trusted network, receives IP configuration, resolves a destination, and connects to a legitimate service. HTTPS protects real websites through certificate validation.

## Observed flow

The client joined a training portal network served by the Mar-x-Auder. The browser displayed a portal page. The student entered fake credentials, and the portal displayed or logged the submitted values.

## Evidence

- Screenshot of the training portal page.
- Screenshot of the fake submitted values on the device.
- SSID used for the lab portal.
- Timestamp of connection and submission.

## Impact

The demonstration shows how captive portal expectations and user-interface trust can be abused. It does not prove a WPA cryptographic weakness. The collected values came from user submission, not from decryption of protected traffic.

## Ethical boundary

The portal must never request real credentials or imitate a real institution in a way that could confuse participants. The ethical line is crossed when real credentials, uninvolved users, or deceptive real-world branding are involved.

## Defensive interpretation

Users should distrust unexpected login prompts on Wi-Fi networks. Organizations should teach users to check domain names, respect certificate warnings, and avoid entering sensitive credentials into captive portals unless the identity of the portal is clear.

## Limitations

This demonstration does not show silent HTTPS interception. Copying a public certificate is not enough to impersonate a real HTTPS website without browser warnings.
```

## Writing rules

Reports use precise technical language.

Use:

- “deauthentication frame injection” instead of “Wi-Fi hacked”;
- “training portal collected fake credentials” instead of “password stolen”;
- “SSID label was cloned” instead of “network identity was cloned”;
- “authentication artifacts were captured” instead of “password was captured”;
- “metadata was observed” instead of “device was tracked,” unless tracking was actually demonstrated under consented conditions.

Avoid:

- overstating capability;
- implying password recovery where only frame capture occurred;
- implying HTTPS compromise where only HTTP captive portal behavior occurred;
- publishing third-party MAC addresses, SSIDs, coordinates, or personal device names;
- using screenshots that expose uninvolved users or networks.

## Data minimization

Wireless research can easily capture more than intended. Reports minimize unnecessary identifiers.

Recommended practices:

- redact third-party MAC addresses;
- truncate BSSIDs where full values are not needed;
- avoid publishing nearby SSIDs unless they are part of the lab;
- remove GPS coordinates from public examples unless location is central and authorized;
- replace real credential-like values with clearly fake examples;
- store PCAPs only where access is controlled;
- delete training portal logs after they are no longer needed.

## Final report structure

A complete student or class report may use this structure:

```text
1. Executive summary
2. Scope and ethical boundary
3. Lab environment
4. Device and firmware information
5. Findings
   5.1 Access point discovery
   5.2 Probe request observation
   5.3 Deauthentication behavior
   5.4 AP clone / beacon spam
   5.5 Evil portal training demonstration
   5.6 WPA handshake / PMKID observation
   5.7 Bluetooth/BLE observation
6. Defensive recommendations
7. Limitations
8. Evidence inventory
9. Appendix: PCAP references and screenshots
```

The report distinguishes passive observation, active transmission, deception, and conceptual demonstration.

## Evidence inventory template

```markdown
| Evidence ID | Type | File / reference | Related finding | Notes |
|---|---|---|---|---|
| EV-001 | PCAP | lab-scan-001.pcap | Access point discovery | Contains beacon/probe traffic for lab SSID. |
| EV-002 | Screenshot | client-duplicate-ssid.png | AP clone spam | Shows duplicate lab SSID in client Wi-Fi menu. |
| EV-003 | Router log | router-log-deauth.txt | Deauthentication | Shows lab client reconnect event. |
| EV-004 | Device screenshot | portal-fake-login.png | Evil portal | Shows fake training credentials only. |
```

## Reporting mindset

The goal of reporting is not to make the demonstration sound more dramatic. The goal is to make it more accurate.

A strong report helps the reader understand:

- what the Mar-x-Auder actually did;
- which protocol behavior made the demonstration possible;
- what was proven;
- what was not proven;
- what the ethical boundary was;
- what a defender should do with the information.

