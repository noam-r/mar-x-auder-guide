# Ethical Research Boundaries

## Why this section exists

The Mar-x-Auder / ESP32 Marauder-style device is useful because it makes normally invisible wireless behavior visible. It can also transmit frames, create deceptive access points, disrupt client connections, and capture sensitive-looking data in a lab. A guide that hides those capabilities would be incomplete and misleading. A guide that explains them without boundaries would be irresponsible.

This section defines the ethical frame for the rest of the guide. It is not a country-specific legal discussion. The guide avoids legal claims because laws vary across jurisdictions. The ethical line, however, is clear: legitimate research requires control, consent, minimization, and a defensive purpose.

## The core boundary

A capability is appropriate for this guide when it is demonstrated against:

- a network owned by the student, instructor, school, or lab;
- a device owned by the student, instructor, school, or lab;
- a participant who knowingly agreed to the demonstration;
- a test environment created specifically for the lesson;
- a written authorization scope that clearly permits the activity.

A capability becomes unethical when it affects, deceives, collects from, or disrupts people and systems that are not part of the authorized environment.

## Why "local" is not enough

Wireless signals do not respect room boundaries. A device can be physically local and still affect other people. A classroom, apartment building, office floor, café, train, or conference hall may contain many nearby networks and client devices. Running an active feature in such an environment can produce effects beyond the intended lab.

For this guide, **local** means more than nearby. It means controlled.

A controlled local environment has:

- a known lab access point;
- known lab clients;
- a known channel where possible;
- an instructor or researcher who understands the feature being used;
- no intent to affect neighboring networks;
- practical steps to reduce accidental spillover.

## Capability categories and ethical line

| Capability type | Legitimate research use | Ethical line crossed |
| --- | --- | --- |
| Passive observation | Observing owned or consented devices to understand Wi-Fi/Bluetooth behavior. | Recording, publishing, or profiling third-party identifiers without consent. |
| Packet capture | Capturing frames from a lab network for protocol analysis. | Capturing unrelated traffic for surveillance, credential collection, or tracking. |
| Active frame transmission | Transmitting crafted frames in a contained lab to demonstrate protocol behavior. | Transmitting frames that disrupt or confuse uninvolved users or networks. |
| Deauthentication / disassociation | Demonstrating management-frame disruption against a lab client and lab AP. | Disconnecting people from networks they rely on. |
| Beacon spam / AP clone | Demonstrating that SSID is not identity in a controlled environment. | Polluting shared airspace with fake networks that may confuse others. |
| Probe request activity | Demonstrating client-discovery behavior using consented devices. | Harvesting or tracking preferred-network information from bystanders. |
| Evil portal | Demonstrating captive portal deception with fake credentials and clear training context. | Collecting real credentials or impersonating a real organization. |
| Bluetooth/BLE observation | Observing lab devices to understand advertising and discovery. | Tracking nearby personal devices or attempting unwanted interaction. |

## Deception and consent

Some capabilities demonstrate deception. Evil portals, access-point cloning, and fake SSID advertisement are educational because they show how users and devices can be misled. That educational value does not remove the need for consent.

A legitimate deception demonstration has these properties:

- participants know they are in a security lesson;
- only fake credentials are used;
- no real organization is impersonated unless explicitly authorized;
- the demonstration is debriefed immediately;
- logs are reviewed only for the learning objective;
- unnecessary data is deleted.

The ethical issue is not only whether harm occurred. It is whether people were made part of a deception without agreeing to be part of it.

## Data minimization

Wireless research can collect more than intended. Beacon frames, probe requests, MAC addresses, SSIDs, timestamps, signal strength, device names, and packet metadata can reveal information about people, locations, habits, and organizations.

The guide therefore uses a minimization rule:

> Collect only what is needed to explain the protocol behavior, keep it only as long as needed, and avoid storing or sharing identifiers from uninvolved devices.

For example, a probe-request chapter may explain that client devices can reveal network names they are looking for. The practical example uses lab devices, not bystander devices. Accidental third-party probes in a capture are ignored, redacted, or deleted.

## Credentials and secrets

The guide must never normalize collecting real credentials. When a practical example involves an evil portal or login page, it should use clearly fake values such as:

```text
username: student@example.test
password: training-password
```

A responsible training page should make the context clear. It may still demonstrate the shape of the risk, but it should not ask for real school, work, email, router, Wi-Fi, bank, social media, or personal credentials.

The learning objective is to understand why the deception works, not to obtain useful secrets.

## Disruption

Some wireless features can interrupt a connection or create a noisy environment. In a lab, this can teach important lessons about management frames, protected management frames, client behavior, and defensive monitoring. Outside a lab, the same behavior can affect people who did not consent.

The guide treats disruption as acceptable only when:

- the access point is part of the lab;
- the client is part of the lab;
- the people using the devices understand what will happen;
- the demonstration can be stopped immediately;
- the instructor or researcher can explain exactly which protocol behavior is being demonstrated.

If a person loses connectivity without agreeing to participate, the line has been crossed.

## Identity and impersonation

SSID names, access-point names, captive portal pages, and Bluetooth device names can all be imitated. The purpose of demonstrating impersonation is to teach that names are not proof of identity.

A legitimate impersonation demonstration should avoid using real brands, schools, companies, government services, or personal names unless the organization explicitly authorized the training. A suitable lab name is descriptive and artificial:

```text
LabNetwork
TrainingPortal
DemoGuestWiFi
ExampleCorp-Lab
```

The goal is to demonstrate the concept without creating confusion outside the learning environment.

## Instructor and researcher responsibility

The person running the lab is responsible for the scope, the equipment, and the explanation. It is not enough to state that participants are responsible. The environment should be designed so that responsible behavior is the default.

Recommended controls include:

- use a spare access point rather than a production network;
- use a spare client device rather than a personal primary device when possible;
- lower transmit power if the firmware and hardware allow it;
- run demonstrations away from crowded wireless environments;
- use unique lab SSIDs;
- avoid collecting real credentials;
- delete captures after the lesson unless they are needed for reporting;
- redact identifiers before publishing screenshots or PCAP excerpts.

## The tone of the guide

The guide is direct, not evasive. It identifies features that are disruptive, deceptive, or capable of collecting sensitive information. It does not hide capabilities behind vague terms or imply that risky behavior is risk-free because it is used in a lab.

The correct tone is:

- technically clear;
- honest about capability;
- specific about boundaries;
- focused on defensive understanding;
- respectful of people who are not part of the lab.

## Ethical summary

The device should be used to understand systems, not to impose experiments on others. A controlled lab is legitimate because the researcher owns the equipment, controls the scope, and uses the result to learn or improve defenses. The same action becomes unethical when it deceives, disrupts, profiles, or collects from uninvolved people.

That boundary applies throughout the guide.
