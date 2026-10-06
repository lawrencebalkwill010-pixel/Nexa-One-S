# Nexa One S — AI Handoff

This file is the shared coordination log for ChatGPT/Codex and Claude/Claude Code while building Nexa One S.

## Project

- **Owner/Founder:** Lawrence Balkwill
- **Brand:** Nexa
- **Product:** Nexa One S
- **Goal:** Build a real portable connectivity + TV/entertainment prototype, not just UI mockups.

## Current prototype hardware

- Raspberry Pi Compute Module 4 — **CM4101000**
- ARM, **1 GB RAM**, Wi-Fi capable, Lite/no onboard eMMC
- Waveshare Mini Base Board (A)
- Waveshare **SIM7600G-H 4G HAT**
- Hologram SIMs for prototype cellular testing
- microSD system/storage
- external CM4 Wi-Fi antenna
- CM4 heatsink
- HDMI, Ethernet and USB

The custom Nexa production PCB has **not** been manufactured yet.

**Important:** The current prototype is 4G, not 5G. Treat 5G and satellite as future upgrades.

## Product architecture

```text
Cellular / Ethernet / Wi-Fi WAN / future satellite
                         |
                    Nexa One S
                    /        \
             Local Wi-Fi     HDMI
                 |             |
      phones/laptops/etc.   Nexa TV UI
```

Nexa One S should keep local Wi-Fi, settings and authorized offline/local media usable when upstream internet is unavailable.

## Cellular architecture

```text
Nexa Account
    |
Nexa One S
    |
SIM Record
    |
Hologram Device/API Identifier
```

Hologram is the prototype cellular source of truth. Provider credentials must stay server-side and must never be exposed to the TV UI or client applications.

Do not hard-code the entire system around Hologram. Use a provider abstraction so Bell, TELUS, future 5G providers, or other supported providers can be integrated later.

## Prototype priorities

1. Boot Nexa One S reliably.
2. Display the Nexa TV UI through HDMI.
3. Establish LTE connectivity through SIM7600G-H + Hologram.
4. Create a Nexa Wi-Fi network and share cellular internet.
5. Show real connection status in Nexa TV.
6. Show connected devices.
7. Keep the local network functional when cellular disappears.
8. Play authorized local/offline media.
9. Stream local media to devices on the Nexa LAN.
10. Recover automatically when connectivity returns.

End-to-end reliability is more important than having many unfinished features.

## Performance constraints

The current CM4 has only **1 GB RAM**.

Optimize for:
- low idle RAM and CPU
- lightweight Linux services
- fast startup
- responsive TV navigation
- lazy-loaded artwork
- efficient local caching
- minimal background services
- SQLite or similarly lightweight local storage where appropriate

Networking and Nexa system services take priority over visual effects.

Avoid heavy Electron/Chromium architecture unless real testing proves it works acceptably on this hardware.

## TV UI

Primary navigation:

- Home
- Apps
- Live
- Offline
- Nexa
- Settings

The UI should be TV-first, remote/controller friendly, readable across a room, modern, original and fast.

First-party apps should include Nexa, Media, Files, Downloads, Network, Settings and Device Manager.

Do not bypass DRM, redistribute copyrighted streaming apps, or circumvent provider restrictions. Commercial streaming services require legitimate supported integrations. Offline media means authorized/local/user-owned content.

## Nexa Core

Lawrence has a separate development/backend server with **128 GB RAM and 8 TB storage**.

Potential Nexa Core responsibilities:
- authentication
- device registry
- SIM registry
- device/SIM linking
- telemetry
- device configuration
- software/firmware updates
- notifications
- logging/monitoring
- administration

This server is separate from Nexa One S and its storage should never be treated as local device storage.

## Agent responsibilities

### ChatGPT / Codex — primary
- architecture
- Nexa Core/backend
- APIs and databases
- authentication
- networking/router services
- SIM7600 integration
- Hologram/provider integration
- connectivity services
- security
- hardware abstraction
- automated tests/deployment

### Claude / Claude Code — primary
- Nexa TV shell
- Home/Apps/Offline/Nexa/Settings
- reusable TV UI components
- media-library UX
- first-time setup
- remote/controller navigation
- frontend state management
- frontend performance/testing

These are primary responsibilities, not hard restrictions.

## Collaboration rules

Before editing:

1. Read this file.
2. Check git status and git diff.
3. Inspect relevant existing code.
4. Check whether the other agent already implemented part of the task.
5. Preserve working code unless there is a documented reason to replace it.
6. Never commit secrets.

After meaningful work, **append** a handoff entry below. Never delete another agent's entries.

Use this format:

```text
--------------------------------------------------
AGENT:
DATE:
TASK:

FILES CHANGED:

IMPLEMENTED:

ARCHITECTURE DECISIONS:

TESTS RUN:

RESULTS:

KNOWN ISSUES:

NEXT TASK:
--------------------------------------------------
```

## Recommended documentation

Maintain these as the project grows:

- `docs/ARCHITECTURE.md`
- `docs/HARDWARE.md`
- `docs/API.md`
- `docs/CELLULAR.md`
- `docs/OFFLINE.md`
- `docs/TV_UI.md`
- `docs/SECURITY.md`
- `docs/ROADMAP.md`

Documentation must distinguish working functionality from planned functionality.

## Handoff log

--------------------------------------------------
**AGENT:** ChatGPT  
**DATE:** 2026-10-06  
**TASK:** Establish shared Claude/Codex project context.

**FILES CHANGED:**  
- AI_HANDOFF.md

**IMPLEMENTED:**  
- Added prototype hardware constraints.
- Added Nexa One S architecture and priorities.
- Added cellular/Hologram architecture.
- Added agent responsibilities and collaboration rules.

**ARCHITECTURE DECISIONS:**  
- Optimize current software for the 1 GB CM4.
- Keep hardware/provider integrations abstracted.
- Keep provider secrets server-side.
- Prioritize working LTE → Nexa Wi-Fi → client connectivity before future 5G/satellite features.

**TESTS RUN:**  
- Documentation-only change.

**RESULTS:**  
- Shared handoff structure initialized.

**KNOWN ISSUES:**  
- Repository still needs a full architecture/code audit before implementation assumptions are made.

**NEXT TASK:**  
- Audit the repository and establish docs/ARCHITECTURE.md, docs/HARDWARE.md and docs/ROADMAP.md.
--------------------------------------------------
