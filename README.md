# 🔋 Digital Battery Passport

**Building trust in India's second-hand EV and e-rickshaw battery market.**

A QR-scannable digital passport for EV/e-rickshaw batteries — scan a code and instantly see manufacturer info, independently-logged usage history, a trained ML-predicted health score, estimated remaining life, and a fair resale price estimate. No app download, no embedded IoT hardware required.

**Live demo:** (https://ind-ev-battery-passport.netlify.app/)

---

## The Problem

India's used-EV and e-rickshaw market runs on blind trust. Buyers can't verify a battery's real cycle count, charging history, or degradation before paying — and dealers have every incentive to hide the truth. Batteries are 30-40% of an EV's cost, so this isn't a small risk; it's the single biggest source of fraud and financial loss slowing down second-hand EV adoption.

Existing solutions (the EU's mandatory Battery Passport, India's emerging BPAN framework, pilots like Zenfinity/Chargeup) are built around IoT telemetry hardware embedded at manufacturing time — which structurally excludes the millions of batteries already in circulation with no sensors and no retrofit path. This project targets exactly that gap: a lightweight, hardware-free passport for the batteries people are actually buying and selling today.

## Features

- **QR generation & camera scanning** — register a battery, get a real QR code, scan it with any phone camera to open its live passport
- **Trained ML health-scoring model** — Ridge regression trained on degradation data, with disclosed test accuracy (R² = 0.98, MAE ≈ 2.76 points), not a black-box prediction
- **Organization verification & reputation** — manufacturers, fleets, service centers, and swap stations register and log usage under their own identity, with verified/unverified badges and aggregate reputation scoring across every battery they've touched
- **Tamper-evident, hash-chained usage history** — each logged event is chained to the one before it; altering a past entry breaks the integrity check visibly
- **Automatic gap detection** — flags unexplained 6+ month silences in a battery's logged history instead of hiding them
- **Fair resale price estimation** — illustrative price range based on chemistry, capacity, and measured health
- **Accessibility features** — text-to-speech "read aloud" for low-literacy users, WhatsApp-lookup concept for buyers more comfortable outside a browser
- **Swap-station-specific tracking** — a usage-event type built for India's battery-swap networks, not just home/depot charging
- **Swappable backend** — runs on an in-memory/demo store out of the box, and automatically switches to a real Firebase Firestore database once configured, with zero code changes required

## Tech Stack

- Single-file HTML/CSS/vanilla JS frontend (no build step, no framework dependency)
- [jsQR](https://github.com/cozmo/jsQR) for camera-based QR decoding
- [qrcodejs](https://github.com/davidshimjs/qrcodejs) for QR generation
- Firebase Firestore (via Firebase JS SDK, loaded dynamically) for persistent storage
- ML health model trained offline with scikit-learn (Ridge regression + polynomial features), exported as plain coefficients and run client-side with no ML runtime dependency
- Web Speech API for voice output

## Getting Started

### Quick test (no setup)
Just open `battery-passport.html` in any modern browser. It runs in a demo/in-memory mode with two seeded example batteries (`BAT-DEMO1`, `BAT-DEMO2`) so you can explore every feature immediately. Data in this mode does **not** persist across page reloads.

### Connect a real database (Firebase)
1. Create a free project at [console.firebase.google.com](https://console.firebase.google.com)
2. Enable **Firestore Database** (start in test mode while developing)
3. Register a Web App under Project Settings → Your apps, and copy the config object it gives you
4. Open `battery-passport.html`, find the `FIREBASE_CONFIG` constant near the top of the script, and paste your values in
5. The app automatically detects the config and switches from demo mode to your live Firestore database — no other code changes needed

Full step-by-step instructions are also built into the app itself under the **Setup / Deploy** tab.

| Area | Current state |
|---|---|
| Health scoring | Real trained ML model, but trained on **synthetic** data modeled after published degradation curves — not real fleet telemetry yet |
| Tamper-evidence | Working hash-chain concept, but a lightweight demo hash — not cryptographic-grade |
| Organization verification | Manual admin toggle for demo purposes — no real identity/KYC check yet |
| Pricing | Illustrative ₹/Wh rates — not live market data |
| WhatsApp lookup | UI concept with a working deep link — not a live automated bot (needs a backend + real WhatsApp Business number) |
| Security rules | Firestore test-mode rules are open by design for development — must be locked down before any real public/production use |

## Roadmap

- Production-grade cryptographic integrity (replacing the current lightweight hash-chain)
- Real organization identity verification / KYC
- Direct BMS (Battery Management System) integration for batteries that do have telemetry hardware
- Retrain the ML health model on real returned-battery measurements
- Partnerships with manufacturers, fleet operators, and swap-station networks for real-world rollout

## License

MIT — see [LICENSE](./LICENSE).

## Team

[GLINT] — [Grecia], [Tejaswi], [Hasita]
Built for Smart City Hackathon 2026 — Theme: Energy Solutions
