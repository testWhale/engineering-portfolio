# Multiplayer Networking & Server Infrastructure

**Flagship project** · DigiPen studio project (Team 25), 2026 · C++, networking, Linux

> The original source code is restricted from public distribution under institutional requirements. This case study presents only authorised project descriptions and demonstration evidence.

## Overview

As part of a DigiPen studio team, I built a custom cross-platform C++ engine targeting Windows and Android, with server-authoritative multiplayer for up to four players and a dedicated server deployed on Linux. My focus was the networking layer and the tooling to prove that it held up under bad network conditions.

## Demonstration

![Network diagnostics HUD on two Android clients: normal profile (left, 21 ms RTT, 0% loss, 20.0 snapshots/s) and harsh profile (right, 548 ms RTT, 13.3% / 8.6% packet loss, 17.9 snapshots/s, 0 corrections)](../media/network-diagnostics-normal-vs-harsh.jpg)

*Same build, two network profiles. Left: normal (RTT 21 ms, 0% packet loss, 20.0 snapshots/s). Right: harsh (RTT 548 ms, 13.3% down / 8.6% up packet loss, 17.9 snapshots/s). Reconciliation corrections are 0 in both.*

### Normal network profile

https://github.com/user-attachments/assets/c882fcf0-1840-400a-bcf7-0f790ab21c82

### Harsh network profile

https://github.com/user-attachments/assets/2824216c-0e8a-4e37-9386-836244f38857

### Dedicated server diagnostics with a connected client

https://github.com/user-attachments/assets/32d81771-acf9-46d1-889a-ba7bd23b4079

*The server diagnostics view shows the live process, workload counters and memory-safety checks (ASan / UBSan / LSan) alongside a connected client. Server port and host details are redacted.*

### Client-side prediction

https://github.com/user-attachments/assets/7ca2e993-24b4-4080-b733-42666b6ba95b

## My role and contributions

- Technical leadership on the networking and server side of the engine
- Server-authoritative systems built on the engine's modular ECS, so that every client sees consistent game state
- Real-time snapshot synchronisation for player actions, projectiles and gameplay events
- Client-side prediction with server reconciliation for the local player, and interpolation for remote entities
- An in-engine network diagnostics HUD and logging pipeline: RTT, jitter, packet loss, up/down bandwidth, snapshots per second, reconciliation corrections and interpolation underruns
- Seven simulated network profiles for stress testing
- Automated tests to catch networking regressions, plus memory-safety validation
- A dedicated server build deployed on Linux and run as a systemd service

## Problem

A real-time multiplayer game has to feel instant for the local player, while one authority (the server) decides what actually happened. Otherwise four clients drift apart, or cheat. The engine had to support up to four players on Windows and Android, connected over real networks with variable latency and packet loss.

## Constraint

- **Server authority:** clients cannot simply trust their own state, so every local action waits on a round trip unless it is predicted.
- **Real networks are bad networks:** the game had to stay playable on links from broadband down to high-latency, lossy mobile connections.
- **Cross-platform:** the same engine and netcode had to run on Windows and Android clients and on a headless Linux server.
- **Restricted source:** the team repository is institutionally restricted, so this case study rests only on authorised public evidence.

## What I Tested

- **Dash spell over the network.** Dash pushes the caster forward. I tested it on a networked client, where the local player is predicted and the server's snapshots reconcile it.
- **Dash regression tests** (added in team commit `0412af6`), covering:
  - **Server simulation:** the server applies the caster impulse on the cast's input tick, and reusing a cast request under a new input sequence cannot push twice.
  - **Client prediction:** an older snapshot replays the pending Dash once and keeps its displacement. A rejected Dash removes both its displacement and its leftover velocity, and repeating the rejection has no effect.
  - **Client–server integration:** Dash movement is predicted on the cast tick, and on a clean link and a 32 ms / 12 ms jitter / 2% loss link the client keeps its displacement and converges with the server. A refused Dash is removed without coming back during replay.
- **Network impairment:** gameplay across **seven simulated network profiles**, from broadband and 4G to harsh high-latency conditions of up to **250 ms one-way latency with 10% packet loss**.
- **Live diagnostics:** in a recorded session on a normal connection, the HUD showed RTT of roughly 18–44 ms, 3–14 ms jitter, 0% packet loss, about 30 kbit/s down and 50 kbit/s up, and about 19–20 snapshots per second, with the client simulating at 60 Hz.
- **Memory safety:** in the recorded live server session, the AddressSanitizer, UndefinedBehaviorSanitizer and LeakSanitizer report counts were all 0 ("No sanitizer errors observed") while a client was connected and enemies and projectiles were active.
- **Linux server:** dedicated server builds running on Linux, for cross-platform validation.

## What Failed

**Dash rubber-banded on networked clients.** The local client applied the Dash movement straight away, but the authoritative server did not move the player for it. Each new server snapshot therefore reconciled the client back towards its pre-Dash position, which showed as visible rubber-banding.

**Root cause:** the server built each player's per-tick movement command with an external push taken *only* from a test hook. Its cast handling processed the Dash but returned no movement, so the Dash impulse never reached the server's movement simulation. Client and server were simulating different movement for the same input.

## What I Changed

In team commit `32baf79` ("Added Forces to Network Packet"), merged in pull request #75:

- **Authoritative Dash on the server:** cast handling now returns the caster's push, and the server adds it to that player's movement command on the cast's input tick. The server also owns the Dash's following damage area, and duplicate casts are rejected.
- **Client prediction and replay:** the predictor keeps the last acknowledged server state as its replay base. After reconciliation it replays the pending commands, so a Dash that the server has not yet acknowledged is applied once, not lost or doubled.
- **Rejected-cast rollback:** if the server refuses a cast, the client strips that Dash's push from its pending commands and replays from the replay base. Its displacement and leftover velocity are removed cleanly.
- **Protocol:** bumped the network protocol to version 6, since casts now drive caster forces and cast messages also carry following areas.

The earlier groundwork is still what keeps play responsive on bad links: client-side prediction, server reconciliation, remote entity interpolation, and the diagnostics HUD and logging.

## What Happened

- The Dash fix and its regression tests were merged into the team's main branch through pull request #75.
- The tests assert the behaviour the bug violated: the server applies the Dash on the cast tick, and the client keeps its displacement and converges with the server on clean and impaired links.
- **Test pass results are not recorded on GitHub.** The repository has no CI, and pull request #75 shows no checks, so I don't cite a pass result here.
- **Network performance (simulated impairment):** up to 4 concurrent players on a server-authoritative model, validated across 7 network profiles. At roughly **548 ms RTT** under the harsh profile, local movement stayed predicted and stable at about **18 snapshots per second**, with 0 reconciliation corrections shown on the diagnostics HUD.
- A dedicated server build runs on **Linux** under **systemd**.
- The corrected implementation allowed Dash movement to remain consistent with authoritative server state, preventing reconciliation from incorrectly reversing accepted spell-induced movement.

These results come from **simulated** network impairment in a studio project. They are not production-scale load testing.

## Technologies

C++, custom ECS engine, client–server networking, client-side prediction, server reconciliation, entity interpolation, Linux, systemd, automated testing, Windows, Android

## Supporting evidence

- Screenshot and screen recordings of the network diagnostics HUD and dedicated server (see [Demonstration](#demonstration))
- Public demonstration and technical write-up: [LinkedIn post: C++ Multiplayer Engine | Networking & Server Infrastructure](https://www.linkedin.com/feed/update/urn:li:activity:7531700571842613249/)

## Source availability

The original team repository (`DigiPen-SG-Year-2/2026-team-25`) is **private** and restricted from public distribution. No source code, server addresses, credentials, infrastructure configuration or team-owned assets are published here.

[← Back to portfolio](../README.md)
