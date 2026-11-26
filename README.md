# Engineering Portfolio — Zachary Ng Hua En

I'm a Computer Science undergraduate specialising in Real-Time Interactive Simulation at DigiPen Institute of Technology Singapore and Singapore Institute of Technology (expected graduation 2029).

My engineering interests are C++ software development, computer networking, backend systems, Linux infrastructure and software reliability.

**Three case studies.** Each one covers the problem, the constraint, what I tested, what failed, what I changed and what happened.

| Project | Why it's here | Technical focus | Evidence |
|---|---|---|---|
| [Multiplayer Networking & Server Infrastructure](projects/multiplayer-networking.md) | **Flagship** | C++, client–server networking, Linux, reliability | [Case study](projects/multiplayer-networking.md) · [Demo post](https://www.linkedin.com/feed/update/urn:li:activity:7531700571842613249/) |
| [MAKO Submarine Engineering](projects/mako-submarine.md) | **Constrained · not a game** | Real-time telemetry, sensors, computer vision | [Case study](projects/mako-submarine.md) · [Dashboard repo](https://github.com/testWhale/SubmarineDashboard) · [Vision fork](https://github.com/testWhale/Real-Time-Object-Detection-using-YOLO11) |
| [Cross-Platform Graphics & Web Deployment](projects/android-webgl.md) | **Specific skill** | C++, OpenGL ES 3.0, Emscripten, CI/CD | [Case study](projects/android-webgl.md) · [**▶ Live demos**](https://testwhale.github.io/android-webgl-showcase/) |

## 1. Multiplayer Networking & Server Infrastructure

**Flagship project.** In a DigiPen studio team, I worked on a custom C++ engine targeting Windows and Android, with server-authoritative multiplayer for up to 4 players and a dedicated server on Linux (systemd).

My work covered real-time snapshot synchronisation, client-side prediction, server reconciliation, remote entity interpolation, an in-engine network diagnostics HUD, and automated testing. I validated it across 7 simulated network profiles. Under the harshest profile (about 548 ms RTT), local movement stayed predicted and stable at about 18 snapshots per second.

[Read the technical case study →](projects/multiplayer-networking.md)

*Source availability: the original source repository is restricted from public distribution.*

## 2. MAKO Submarine Engineering

**Engineering under physical and hardware constraints.** MAKO is Singapore's and Southeast Asia's first human-powered submarine. I was the team's lead developer (submarine).

I built a real-time telemetry GUI for the pilot, fed by an MPU-9250 IMU, an MS5837-30BA depth sensor and an RPM sensor. I also adapted a YOLO11 + OpenCV pipeline to detect lines so the submarine could find underwater gates. The team placed 3rd at eISR 2026 in Best Run, Best Speed and Best Time, with a 33% improvement over 2025.

- [SubmarineDashboard](https://github.com/testWhale/SubmarineDashboard): Flask + Three.js pilot dashboard
- [Real-Time-Object-Detection-using-YOLO11 (fork)](https://github.com/testWhale/Real-Time-Object-Detection-using-YOLO11): credited upstream detection pipeline used for MAKO

[Read the technical case study →](projects/mako-submarine.md)

## 3. Cross-Platform Graphics & Web Deployment

Three interactive C++ applications using OpenGL ES 3.0. Each one builds from a single codebase for Android, Windows and the browser (Emscripten → WebGL 2), and is deployed to GitHub Pages by GitHub Actions on every push.

[**▶ Try the live demonstrations**](https://testwhale.github.io/android-webgl-showcase/) · [Repository](https://github.com/testWhale/android-webgl-showcase) · [Read the technical case study →](projects/android-webgl.md)

## Additional collaborative work

### Pirates Be Ballin (GAM150)

A wave-based survival game in C++, built by a five-person DigiPen team. I was the team's **UI/UX lead**. This is supporting evidence of team development, not one of the three case studies.

Between 9 March and 5 April 2026 I authored 70 commits on `main` and 8 pull requests, 6 of which were merged:

- [#36 Collision System with Test Entities](https://github.com/Shmethf/GAM150/pull/36)
- [#87 Player HealthBar UI](https://github.com/Shmethf/GAM150/pull/87)
- [#97 Main Menu + Particle System + UI System](https://github.com/Shmethf/GAM150/pull/97)
- [#103 Particle System](https://github.com/Shmethf/GAM150/pull/103)
- [#117 Collision template](https://github.com/Shmethf/GAM150/pull/117)
- [#121 UI system](https://github.com/Shmethf/GAM150/pull/121)

Selected commits: [sound settings sliders](https://github.com/Shmethf/GAM150/commit/1d0956d8097fcec82269f612593a26ada27041f1) · [memory leak fix](https://github.com/Shmethf/GAM150/commit/9bd0181972b97e424874fcfaee62f03715b49275) · [run and hit VFX](https://github.com/Shmethf/GAM150/commit/56c6f212920ba6fd9b2b33b4566c682e9cb0de03) · [particle system](https://github.com/Shmethf/GAM150/commit/d9898a5cf34eab4034a1774f8c656e73a61dfc9d)

[Public team repository](https://github.com/Shmethf/GAM150) · [All my commits](https://github.com/Shmethf/GAM150/commits?author=testWhale)

## Contact

[GitHub](https://github.com/testWhale) · [LinkedIn](https://www.linkedin.com/in/zacharyngh/)
