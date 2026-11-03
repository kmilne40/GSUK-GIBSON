<div align="center">

<img src="gibson.png" alt="GIBSON Mainframe Simulator" width="160">

# GIBSON Mainframe Simulator

**A hands-on IBM mainframe security training, penetration-testing and cyber threat intelligence lab.**

*Practise mainframe operations, investigate weaknesses, run controlled attacks and learn how to detect them, without needing access to a production mainframe.*

**Python 3.10+** · **Linux, macOS and Windows** · **Source and native installer builds** · **Community and educational licence**

[Quick start](#quick-start-linux) · [Web interfaces](#web-interfaces-and-default-logins) · [Capabilities](#what-can-gibson-do) · [Build installers](#build-for-linux-macos-and-windows) · [Licence](#licence-and-attribution)

</div>

---

## What is GIBSON?

**GIBSON** is a Python-based, deliberately configurable mainframe simulator created by **Kev Milne** for technical education, authorised penetration testing, defensive security research, workshops and capture-the-flag exercises.

It recreates practical aspects of IBM mainframe environments, including VTAM and 3270 terminals, TSO, ISPF, JES/JCL, RACF, CICS, Db2, USS/OMVS, NJE, FTP and a range of operational and security workflows. These services share simulated users, datasets, jobs, configuration and audit evidence so exercises can progress from discovery through exploitation to detection and investigation.

GIBSON is **not IBM z/OS, an IBM-licensed emulator or a replacement for a real mainframe**. It is a controlled training model, with both deliberately vulnerable and more restrictive configurations. The distinction matters: behaviour, protocol coverage and security checks may differ from production systems.

## What's in this release?

Three browser-facing features make the simulator easier to operate and teach:

| Feature | Where to find it | Why it matters |
|---|---|---|
| **Web Master Console** | `http://127.0.0.1:8443/console` | Work through the simulated IPL, issue operator commands, view console messages and follow system activity from a browser. |
| **Web Pen Test Shell** | `http://127.0.0.1:8022/` | Run the bundled mainframe assessment tools and common command-line utilities in a browser-based, lab-scoped workspace. |
| **GIBSON SENTRY CTI** | `http://127.0.0.1/cti` (port **80**) | Explore cyber threat intelligence, indicators, security events, mainframe TTP mappings, investigations and simulated attack telemetry. |

The Master Console and Pen Test Shell are distinct services. Port **8443** also provides the operational dashboard; the CTI application is part of the welcome/web service on port **80**. An optional browser 3270 terminal has its own service on port **8023**.

### Web interfaces and default logins

These are **factory training credentials in the supplied source code**, not production-grade credentials.

| Interface | Default address | User ID | Password | Notes |
|---|---|---|---|---|
| **Operations Dashboard / Master Console** | `http://127.0.0.1:8443/` | `admin` | `gibsonadmin!` | HTTP Basic authentication. Open `/console` for the IPL and operator console. |
| **GIBSON SENTRY CTI** | `http://127.0.0.1/cti` | `ctiadmin` | `gibson` | Used when CTI authentication is enabled and for protected CTI functions. **Most CTI read-only pages do not prompt for a login by default.** |
| **Pen Test Shell** | `http://127.0.0.1:8022/` | `admin` | `gibsonadmin!` | Uses the same configured credentials as the dashboard; the service listens on loopback by default. |

> **Important:** The number `8443` does not mean HTTPS is automatically enabled. In the default deliberately vulnerable mode, the dashboard is served over **HTTP**; secure mode can wrap the dashboard socket in TLS. Use the matching `http://` or `https://` URL for your selected configuration. Do not expose the lab's default credentials or vulnerable services to the public Internet.

To override credentials, set environment variables **before starting GIBSON**:

```bash
export GIBSON_DASHBOARD_USER='admin'
export GIBSON_DASHBOARD_PASSWORD='replace-with-a-unique-password'
export GIBSON_CTI_USER='ctiadmin'
export GIBSON_CTI_PASSWORD='replace-with-a-different-password'
export GIBSON_CTI_AUTH_ENABLED=1
# Optional: require authentication on read-only CTI pages as well.
export GIBSON_CTI_READONLY_PUBLIC=0
```

The CTI credentials are independent by default. `GIBSON_CTI_USE_DASHBOARD_AUTH=1` instead configures CTI to use the dashboard credentials. These settings only govern these web interfaces; simulated RACF/TSO accounts have their own authentication rules.

## What can GIBSON do?

| Area | Functionality | Typical exercise or use case |
|---|---|---|
| **System operations** | Web Master Console, IPL requests/replies, operator commands, started-task and service status, audit and event streams | Bring up a simulated system, investigate failures, operate a training LPAR |
| **Mainframe terminal access** | VTAM-style connections, 3270/TN3270-oriented interaction, TSO logon and interactive sessions | Learn mainframe navigation and reconnaissance |
| **TSO / ISPF** | READY commands, ISPF panels, dataset browsing, editing and member workflows | Explore users, datasets, permissions and legacy workflows |
| **JES / JCL / SDSF** | Job submission, simulated spool/output, job management and JES operations | Understand batch execution and security-relevant job evidence |
| **REXX and datasets** | REXX/JCL exercises, partitioned datasets, catalogue and member operations | Run enumeration scripts and lab jobs against seeded data |
| **RACF and access control** | Simulated users, groups, resource and dataset profiles, authorisation, password policy, MFA and audit records | Assess excess privileges, weak controls and defensive hardening |
| **Alternative security models** | ACF2-related models and administrative/security workflows | Compare access-control concepts in training |
| **CICS** | Transactions, administrative/resource panels and CICS-backed application exercises | Enumerate and assess transaction security |
| **Db2 and application APIs** | Db2-style interfaces, SQL labs, REST and z/OS Connect-style banking APIs | Assess application security and the boundary between RACF, APIs and data |
| **USS / OMVS** | Unix-style shell and network commands, simulated USS security behaviour | Examine Unix privileges and mainframe/Unix integration |
| **FTP and IND$FILE** | Data transfer and dataset upload/download paths | Explore transfer controls, JES submission and associated evidence |
| **NJE / JES2 networking** | NJE node and job-flow simulation | Demonstrate NJE exposure and trust-boundary weaknesses |
| **z/VM** | Simulated CP/CMS terminal and security/privilege concepts | Explore virtual-machine administration and security |
| **Web and banking applications** | FIBS Bank, CBSA/DVCA and vulnerable application scenarios | Teach multi-step attacks involving web, CICS and Db2 components |
| **Penetration-testing labs** | Seeded weaknesses, reconnaissance, password testing, SURROGAT/PassTicket and APF escalation scenarios | Run reproducible authorised attacker-to-defender exercises |
| **Monitoring and audit** | SMF-style events, RACF violations, zSecure-inspired alerting and C2POLICE | Investigate misuse and correlate operational/security evidence |
| **Threat intelligence** | GIBSON SENTRY dashboards, IOC management, enrichments, threat actors, TTPs and incident investigations | Connect simulated attack activity with threat intelligence |
| **Forensics and scenarios** | PLONK, PCAP-related material, attack storylines and exportable event/report views | Practise evidence collection, attribution hypotheses and reporting |
| **CTF / training delivery** | Seeded accounts, tools, datasets, flags and repeatable scenarios | Instructor-led workshops, self-study and controlled exercises |

Capabilities are **simulated and exercise-dependent**. Some features require a particular service flag, lab state, optional system tool or configuration mode.

## The browser-based Pen Test Shell

The Pen Test Shell runs on **port 8022** by default and provides an xterm-style browser terminal. GIBSON seeds a working directory at `~/mfsim/pentest` (or under `GIBSON_SIM_ROOT`) with selected course tools and wordlists.

**Bundled course launchers** include `ezrecon`, `zniff`, `tshocker`, `cicspwn`, `injector`, `nje-assess`, `njejcl`, `njeanalyze`, `phosphor`, `gibson-scan`, and PassTicket-related utilities. The shell also supplies wrappers for tools such as `nmap`, `curl`, `wget`, `nc`, `hydra` and `nikto`; these system binaries may need to be installed separately. The included `toolbox-setup` helper assists with setting them up.

Try the following **inside the browser shell**:

```bash
scope                         # Show authorised target rules
tools                         # List available course and system tools
gibson-scan 127.0.0.1 -sV     # Built-in TCP discovery and banner checks
# If installed on the host:
nmap -sV 127.0.0.1
```

The supplied network-tool wrappers enforce a default allowlist of **localhost / `127.0.0.1` / `::1`** and **`sighberbank.com` plus its subdomains**, which are used for training. This is a guardrail on the packaged tool entry points, **not a complete isolation or security boundary**. Run GIBSON in an isolated, authorised environment.

## GIBSON SENTRY: CTI on port 80

Open `http://127.0.0.1/cti` to access GIBSON's integrated **Cyber Threat Intelligence (CTI)** system.

| CTI section | Example path | Purpose |
|---|---|---|
| Dashboard | `/cti/dashboard` | Overview of indicators, investigations, telemetry and risk signals |
| Security events | `/cti/security` | Examine security-relevant logon and access events |
| IOC catalogue | `/cti/iocs` | Review indicators and their severity/context |
| Threat actors and TTPs | `/cti/actors`, `/cti/ttps` | Explore adversary profiles and techniques |
| M4M and MITRE mappings | `/cti/m4m/navigator`, `/cti/mitre` | Relate observed behaviour to mainframe-focused and ATT&CK-style techniques |
| Enrichment and sources | `/cti/enrichment`, `/cti/providers` | Review provider configuration, cache and enrichment results |
| zSecure-inspired alerts | `/cti/zalert` | View simulated C2POLICE security alert activity |
| Scenario demonstrations | `/cti/hms`, `/cti/scrapi`, `/cti/ctf` | Explore protected, instructor-oriented scenario routes |
| PLONK forensics | `/cti/plonk` | Investigate lab evidence and related PCAP/SQL views |
| Reporting and integration | `/cti/reports`, `/cti/api/events` | Export or consume simulated security event data |

The CTI implementation is **offline-first** with training fixtures and simulated security data. Certain online feeds or enrichment services require their own configuration and API access. Training threat-actor stories and geolocation fixtures must **not** be represented as real-world attribution or verified external intelligence.

## Quick start (Linux)

### Requirements

- **Python 3.10 or newer**, with `pip` and `venv`.
- A Linux, macOS or Windows machine capable of running the chosen source or native build.
- Network access during installation for Python dependencies and optional tools.
- A dedicated lab environment, preferably a VM or an isolated network. Some default service ports are privileged.

From the extracted archive or checked-out repository root:

```bash
chmod +x install_gibson.sh gibsonctl.sh run_gibson.sh
chmod +x scripts/grant-port-capabilities.sh

# Linux: make privileged lab ports available without running the simulator as root.
cd scripts
sudo ./grant-port-capabilities.sh
cd ..

# Install the Python application and seed the training environment.
./install_gibson.sh --venv

# Start GIBSON and its configured lab services.
./gibsonctl.sh start
```

Then visit **`http://127.0.0.1:8443/console`** and sign in with `admin` / `gibsonadmin!`.

### Initial programme load (IPL)

On start-up, the web Master Console presents simulated operator requests. Reply in order as directed on-screen:

```text
R 01,CLPA
R 02,U
R 03,Y
R 04,1234          <- Example four-digit training PIN; use the value specified by your lab
R 05,GIBSON        <- Example system name
R 06,SKIP
```

When the IPL is complete, use the web console for operator commands, or connect through the terminal and application services. The console also has an optional terminal-driven IPL mode for legacy workflows.

### Control and troubleshooting

```bash
./gibsonctl.sh status
./gibsonctl.sh stop
./gibsonctl.sh restart
```

If a web page does not load, confirm that the relevant service is enabled and its port is listening, check the terminal output and review the `logs/` directory. Some tools need an additional host-level dependency. See [README-BUILD.md](README-BUILD.md) for detailed build and environment notes.

## Service and port reference

These are the **typical defaults from the current source**. The selected launcher, environment, mode and Docker mapping can change them.

| Service | Typical native port | Notes |
|---|---|---|
| **Dashboard / Master Console** | **8443/TCP** | Browser interface; HTTP in vulnerable mode, TLS in secure mode |
| **CTI / welcome site** | **80/TCP** | CTI is at `/cti` |
| **Pen Test Shell** | **8022/TCP** | Browser terminal; typically bound to loopback; enable with `--with-tools` or the controller defaults |
| VTAM / TSO / terminal | 23/TCP | Mainframe-style terminal service; configurable |
| USS shell | 2022/TCP | Unix-style training shell |
| Browser terminal | 8023/TCP | Optional web terminal service |
| FTP | 21/TCP | Enable with `--with-ftp`; configurable |
| JES2 / NJE | 175/TCP | Simulated NJE network interface |
| z/VM | 3023/TCP | Simulated CP/CMS logon |
| CBSA / DVCA / app API | 8080/TCP | Web and REST training lab |
| FIBS Bank web application | 9080/TCP | Browser-based banking lab |
| Db2 DAS-style listener | 446/TCP | Configurable; Docker maps to 50000 |
| Db2 WebSocket | 50001/TCP | Db2-related interactive service |
| SMTP / Office Mail | 2525/TCP or 25/TCP | Depends on launch configuration and privileges |

**Docker note:** The included `docker-compose.yml` maps the container's **port 80 to host port 8088**, so the CTI URL with that deployment is **`http://localhost:8088/cti`**. The supplied Compose file does **not** publish port `8022` or enable the Pen Test Shell by default. Use the native source/installer path for the documented shell experience, or update the container configuration deliberately and keep the shell restricted to an authorised local environment. Some other container ports also differ from the native defaults.

## Build for Linux, macOS and Windows

GIBSON is packaged from **one shared source tree**. The native build scripts are located in the repository root, alongside `gibson/`, `pyproject.toml`, `gibson.spec` and `requirements-build.txt`.

| Platform | Build file | Command | Expected distributable |
|---|---|---|---|
| **Linux** | [`build-linux.sh`](build-linux.sh) | `bash build-linux.sh` | `GIBSON-linux-<arch>.tar.gz` |
| **macOS** (Intel / Apple Silicon) | [`build-macos.command`](build-macos.command) | `bash build-macos.command` | `Output/GIBSON-<arch>.dmg` and, when supported, `Output/GIBSON-Installer-<arch>.pkg` |
| **Windows** | [`build-windows.bat`](build-windows.bat) | Double-click or run `build-windows.bat` in `cmd.exe` | `GIBSON-Setup.exe`, or `GIBSON-portable.zip` if Inno Setup cannot be obtained |

### How the builds work

1. The platform script locates a suitable Python installation and creates an isolated `.venv-build` environment.
2. Dependencies in `requirements-build.txt` are installed. The shared [`gibson.spec`](gibson.spec) drives **PyInstaller** using [`gibson_launcher.py`](gibson_launcher.py).
3. The frozen application is wrapped for the target OS. Windows uses [`gibson.iss`](gibson.iss) with **Inno Setup**; macOS produces an app bundle and installer/disk image; Linux packages a portable application tree.
4. If a full freeze fails, the scripts can retry a **lean build**, which may omit some optional tool dependencies. Check the resulting `build-log.txt` and test the installer before distributing it.

**Build on the OS and CPU architecture you intend to distribute for.** PyInstaller does not produce cross-platform installers from one host. In particular, use an Apple Silicon Mac for an `arm64` build and an Intel Mac for an `x86_64` build. The build scripts may download dependencies, require administrative privileges and use several gigabytes of local storage. The macOS output is not automatically code-signed or notarised.

If you want to build from a different source checkout while keeping the scripts in this directory, the Unix scripts support the `GIBSON_SRC` environment variable. Build artefacts are produced locally and are not pre-compiled binaries included in the ZIP.

## Configuration and operational guidance

The project reads service ports, security options and lab paths from environment variables. Common examples include:

| Variable | Purpose | Example or default |
|---|---|---|
| `GIBSON_SIM_ROOT` | Persistent datasets, logs and shell workspace | `~/mfsim` |
| `GIBSON_DASHBOARD_PORT` | Operations dashboard and Master Console | `8443` |
| `GIBSON_TOOLS_PORT` | Pen Test Shell | `8022` |
| `GIBSON_WELCOME_PORT` | Welcome/CTI web service | `80` |
| `GIBSON_SECURITY_MODE` | Deliberately vulnerable or more restrictive lab | `vuln` / `secure` |
| `GIBSON_DASHBOARD_USER` / `GIBSON_DASHBOARD_PASSWORD` | Master Console login | `admin` / `gibsonadmin!` |
| `GIBSON_CTI_USER` / `GIBSON_CTI_PASSWORD` | CTI login | `ctiadmin` / `gibson` |
| `GIBSON_CTI_AUTH_ENABLED` | Enable CTI authentication for management routes; scenario routes have separate built-in protection | `0` by default |
| `GIBSON_CTI_READONLY_PUBLIC` | Permit read-only CTI access when authentication is enabled | `1` by default |

The simulator includes **vulnerable-mode and secure-mode exercises**. Select the mode appropriate to the lesson; do not treat the secure training mode as a substitute for production-grade hardening, access controls or isolation. Protect or replace default passwords before allowing other users access, and use a firewall/VM boundary for every lab deployment.

## Learning resources and project files

| File or directory | Purpose |
|---|---|
| [`README-BUILD.md`](README-BUILD.md) | Detailed build notes, platform behaviours and implementation history |
| [`docs/manual/index.md`](docs/manual/index.md) | Extended technical and training manual |
| [`CHANGELOG-GIBSON.md`](CHANGELOG-GIBSON.md) | Development changes and fixes |
| [`gibson/`](gibson/) | Core Python simulator, services, applications and data |
| [`gibson/assets/pentest/`](gibson/assets/pentest/) | Bundled training tools, setup helpers and wordlists |
| [`tests/`](tests/) | Automated test suites and regression coverage |
| [`install_gibson.sh`](install_gibson.sh), [`gibsonctl.sh`](gibsonctl.sh) | Source installation and service control |
| [`docker-compose.yml`](docker-compose.yml) | Alternative container deployment with different host port mappings |
| [`LICENSE.md`](LICENSE.md) | Complete licence terms |

## Responsible use

GIBSON is designed for **authorised training and security research**. Some intentionally vulnerable services, simulated credentials, exploitation exercises and lab datasets are unsuitable for exposed systems. Run the environment on a host or network you control, use only explicitly authorised targets and take care when enabling services on privileged or externally accessible ports.

Security events, attack outcomes, threat intelligence fixtures and protocol behaviours are part of a **simulation**. Do not rely on them as a replacement for testing real mainframe infrastructure or validating real-world threat attribution.

## Licence and attribution

**GIBSON was created by Kev Milne.**  
**Copyright © 2022–2026 Kev Milne.**  
**Licensed under the GIBSON Community and Educational Licence, Version 1.0.**

The licence permits specified personal, community, academic and non-commercial educational/research uses, subject to its detailed conditions. **Commercial use and commercial training arrangements require separate permission or licensing.** This is **not** an unrestricted MIT, Apache or GPL licence. Read the complete terms in [`LICENSE.md`](LICENSE.md) before using, modifying or distributing GIBSON.

Official project: **[github.com/kmilne40/GIBSON](https://github.com/kmilne40/GIBSON)**  
Training and mainframe security research: **[OffensiveSec.org](https://offensivesec.org/)**

---

<div align="center">

**GIBSON | Learn the mainframe. Test the controls. Understand the evidence.**

</div>
