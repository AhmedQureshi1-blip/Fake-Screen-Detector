# ⚡ PayGuard — AI-Powered Payment Screenshot Authenticator

> **Real-time fake payment screenshot detection for university cafés and campus businesses.**

---

## 📋 Table of Contents

- [Overview](#overview)
- [The Problem](#the-problem)
- [Solution](#solution)
- [Features](#features)
- [How It Works](#how-it-works)
- [What the AI Detects](#what-the-ai-detects)
- [Getting Started](#getting-started)
- [Usage Guide](#usage-guide)
- [Technical Architecture](#technical-architecture)
- [Analysis Output](#analysis-output)
- [Deployment](#deployment)
- [Security & Privacy](#security--privacy)
- [Limitations & Recommendations](#limitations--recommendations)
- [Contributing](#contributing)
- [License](#license)

---

## Overview

**PayGuard** is a browser-based AI forensic tool designed to help café cashiers and business staff instantly verify the authenticity of payment screenshots presented by customers. It leverages Claude AI's multimodal vision capabilities to analyze screenshots in real time and return a clear verdict: **AUTHENTIC**, **SUSPICIOUS**, or **FAKE** — with full reasoning.

No backend required. No installation. Just open the HTML file and start scanning.

---

## The Problem

At university cafés and campus payment points, a growing number of students present **fabricated or edited payment screenshots** to claim they have paid when they have not. Common tactics include:

- Editing the amount on a real payment receipt
- Reusing old payment screenshots from previous transactions
- Generating fake receipts using screenshot editing apps
- Screenshotting payment UI templates found online
- Altering transaction IDs, dates, or recipient names

Manual verification is slow, unreliable, and puts cashiers in an uncomfortable position. **PayGuard solves this with instant AI analysis.**

---

## Solution

PayGuard uses **Claude AI's vision API** to perform a multi-point forensic analysis on any uploaded payment screenshot. The system checks for visual tampering, font inconsistencies, UI authenticity, metadata anomalies, and more — returning a detailed verdict in under 10 seconds.

| Without PayGuard | With PayGuard |
|---|---|
| Manual eyeball check | AI-powered forensic analysis |
| Slow & subjective | Instant & objective |
| Cashier under pressure | Clear system verdict |
| No documentation | Detailed finding report |
| Students exploit gaps | Deterrent effect |

---

## Features

- **🔍 Real-Time AI Analysis** — Powered by Claude claude-sonnet-4-20250514 vision model
- **📊 Confidence Scoring** — 0–100% confidence score for every verdict
- **🚦 Three-Tier Verdicts** — AUTHENTIC / SUSPICIOUS / FAKE with cashier action recommendation
- **🧾 Payment Detail Extraction** — Automatically reads app name, amount, transaction ID, timestamp, and recipient
- **⚠️ Red Flag Detection** — Itemized list of specific anomalies found
- **✅ Green Flag Detection** — Confirms what looks legitimate
- **🔬 Six-Point Check System** — Visual integrity, text consistency, UI authenticity, amount field, transaction ID, and timestamp
- **📱 Drag & Drop Upload** — Simple interface designed for busy cashiers
- **🔒 No Data Storage** — Screenshots are analyzed and immediately discarded
- **💻 Zero Installation** — Runs entirely in the browser as a single HTML file

---

## How It Works

```
Student presents payment screenshot
          │
          ▼
Cashier uploads screenshot to PayGuard
          │
          ▼
Screenshot encoded as Base64 and sent to Claude AI API
          │
          ▼
AI performs multi-point forensic analysis
          │
          ▼
Structured JSON verdict returned and rendered
          │
          ▼
Cashier sees: AUTHENTIC / SUSPICIOUS / FAKE + full details
```

### Step-by-Step for Cashiers

1. **Student shows payment** — Student displays payment confirmation on their device
2. **Take a photo or screenshot** — Capture what the student is showing you
3. **Upload to PayGuard** — Drag & drop or click to upload the image
4. **Click "Analyze Screenshot"** — AI scans the image (takes ~5–10 seconds)
5. **Read the verdict** — Follow the cashier action recommendation

---

## What the AI Detects

PayGuard's AI analysis engine checks for the following:

### Visual Forensics
- Pixel-level artifacts and compression anomalies inconsistent with native screenshots
- Signs of image editing software (unusual noise patterns, layer boundaries)
- Inconsistent JPEG compression blocks suggesting content was pasted in

### Text & Typography
- Font inconsistencies between different parts of the screenshot
- Irregular character spacing or alignment in critical fields (amount, ID)
- Mixed font rendering styles indicating overlaid text

### UI Authenticity
- Whether the interface layout matches the claimed payment application
- Correct placement of UI elements (buttons, icons, status bars)
- Accurate color schemes and branding for the detected payment provider

### Payment Data Integrity
- Amount field isolation — checks if the amount appears visually distinct from surrounding elements
- Transaction ID format validation against known provider patterns
- Timestamp plausibility (date, time, and timezone consistency)
- Recipient/merchant name consistency

### Composition & Context
- Screen reflection and lighting consistency (real phone screens have characteristic reflections)
- Aspect ratio and resolution matching typical mobile screenshots
- Status bar authenticity (battery icon, signal bars, time display)

---

## Getting Started

### Prerequisites

- A modern web browser (Chrome, Firefox, Safari, Edge)
- An active **Anthropic API key** (Claude API access)
- Internet connection (for API calls)

### Quick Start

1. **Download** the `fake-screenshot-detector.html` file
2. **Open** the file in any web browser
3. **Upload** a payment screenshot
4. **Click** "Analyze Screenshot"
5. **Read** the verdict

> **Note:** This tool is pre-configured for use within the Anthropic Claude environment where API authentication is handled automatically. For standalone deployment, see the [Deployment](#deployment) section.

---

## Usage Guide

### For Cashiers

| Verdict | Badge Color | Recommended Action |
|---|---|---|
| ✅ AUTHENTIC | Green | Accept the payment |
| ⚠️ SUSPICIOUS | Yellow | Request alternative verification |
| ❌ LIKELY FAKE | Red | Decline and escalate |

**When you see SUSPICIOUS:**
- Ask the student to show the transaction in their payment app live (not a static screenshot)
- Request a bank SMS confirmation
- Escalate to a manager if the student refuses

**When you see FAKE:**
- Politely decline service
- Note the student ID for reporting purposes
- Do not confront aggressively — follow your café's escalation policy

### Tips for Best Results

- Ensure the uploaded image is **clear and in focus**
- Upload the **full screenshot** — don't crop out parts of the image
- If a student is holding a phone, photograph it **straight-on** to minimize angle distortion
- Higher image quality = more accurate analysis

---

## Technical Architecture

```
┌─────────────────────────────────────────────────────┐
│                   Browser (Client)                   │
│                                                     │
│  ┌──────────────┐    ┌──────────────────────────┐  │
│  │  File Upload  │───▶│  Base64 Image Encoder    │  │
│  │  (Drag/Drop)  │    └──────────┬───────────────┘  │
│  └──────────────┘               │                   │
│                                 ▼                   │
│                    ┌────────────────────────┐        │
│                    │  Forensic Prompt        │        │
│                    │  (Multi-point analysis) │        │
│                    └────────────┬───────────┘        │
└─────────────────────────────────┼───────────────────┘
                                  │ HTTPS POST
                                  ▼
┌─────────────────────────────────────────────────────┐
│              Anthropic Claude API                    │
│                                                     │
│   Model: claude-sonnet-4-20250514                      │
│   Input: Image + Forensic Analysis Prompt           │
│   Output: Structured JSON verdict                   │
└─────────────────────────────────┬───────────────────┘
                                  │
                                  ▼
┌─────────────────────────────────────────────────────┐
│                   Result Renderer                    │
│                                                     │
│  • Verdict badge (AUTHENTIC / SUSPICIOUS / FAKE)    │
│  • Confidence bar animation                         │
│  • Payment details extraction display               │
│  • Six-point check grid                             │
│  • Red/Green flags list                             │
│  • Cashier action summary                           │
└─────────────────────────────────────────────────────┘
```

### Tech Stack

| Component | Technology |
|---|---|
| Frontend | Vanilla HTML5, CSS3, JavaScript (ES6+) |
| AI Model | Claude claude-sonnet-4-20250514 (Anthropic) |
| Image Processing | FileReader API + Base64 encoding |
| Fonts | Google Fonts (Syne + Space Mono) |
| Animations | Pure CSS keyframe animations |
| API Communication | Fetch API (native browser) |
| Dependencies | **None** — zero external JavaScript libraries |

---

## Analysis Output

The AI returns a structured JSON response that is parsed and rendered into the UI:

```json
{
  "verdict": "AUTHENTIC | SUSPICIOUS | FAKE",
  "confidence": 85,
  "payment_app": "GCash / Maya / PayMaya / etc.",
  "amount": "₱150.00",
  "transaction_id": "TXN-2024-XXXXXXXXX",
  "timestamp": "2024-02-17 10:45:23",
  "recipient": "University Café",
  "checks": [
    { "name": "Visual Integrity",  "status": "PASS | WARN | FAIL", "detail": "..." },
    { "name": "Text Consistency",  "status": "PASS | WARN | FAIL", "detail": "..." },
    { "name": "UI Authenticity",   "status": "PASS | WARN | FAIL", "detail": "..." },
    { "name": "Amount Field",      "status": "PASS | WARN | FAIL", "detail": "..." },
    { "name": "Transaction ID",    "status": "PASS | WARN | FAIL", "detail": "..." },
    { "name": "Timestamp",         "status": "PASS | WARN | FAIL", "detail": "..." }
  ],
  "red_flags": [
    { "severity": "high | medium | low", "title": "...", "detail": "..." }
  ],
  "green_flags": [
    { "title": "...", "detail": "..." }
  ],
  "summary": "Professional summary for the cashier...",
  "cashier_action": "ACCEPT | REQUEST_VERIFICATION | REJECT"
}
```

---

## Deployment

### Option 1: Local File (Simplest)
Open `fake-screenshot-detector.html` directly in a browser. Works offline except for API calls.

### Option 2: Local Web Server
```bash
# Python
python -m http.server 8080

# Node.js
npx serve .
```
Then navigate to `http://localhost:8080/fake-screenshot-detector.html`

### Option 3: Hosted Web Server
Upload the HTML file to any static hosting service:
- **Netlify** — drag and drop the file at netlify.com
- **GitHub Pages** — push to a repository and enable Pages
- **Vercel** — `vercel deploy`
- **Any web host** — upload via FTP/cPanel

### Option 4: Tablet at the Counter (Recommended)
For best café workflow, mount a tablet at the counter running this app in kiosk/fullscreen mode. The cashier never needs to leave the POS area.

### API Key Configuration

For standalone deployment outside the Anthropic environment, add your API key to the request headers in the JavaScript:

```javascript
headers: {
  'Content-Type': 'application/json',
  'x-api-key': 'YOUR_API_KEY_HERE',          // Add this
  'anthropic-version': '2023-06-01',          // Add this
  'anthropic-dangerous-direct-browser-access': 'true'  // Required for browser calls
}
```

> ⚠️ **Security Note:** Embedding API keys directly in browser-side code exposes them to anyone who views the page source. For production use, proxy API calls through your own backend server that holds the key securely.

---

## Security & Privacy

| Concern | How PayGuard Handles It |
|---|---|
| **Screenshot storage** | Images are never stored — only sent to the API and immediately discarded |
| **Student data** | No personally identifiable information is logged or retained |
| **API key exposure** | Use a backend proxy in production; never hardcode keys in public deployments |
| **Network security** | All API calls are made over HTTPS/TLS |
| **False positives** | Confidence scoring and three-tier verdicts prevent over-rejection |

---

## Limitations & Recommendations

### Known Limitations

- **AI is probabilistic, not deterministic** — No AI system is 100% accurate. Always apply human judgment alongside AI verdicts.
- **High-quality fakes may pass** — Extremely sophisticated fabrications using official app templates may fool the detector.
- **Low-quality real screenshots may fail** — Very blurry, dark, or angle-distorted images may produce incorrect results.
- **New app UI changes** — If a payment app redesigns its UI, the AI may briefly struggle until its training reflects the new design.
- **Regional app coverage** — Best results with major payment apps; may be less accurate with obscure or region-specific apps.

### Best Practices

1. **Use PayGuard as a first-line filter**, not the sole authority — supplement with live app verification when in doubt
2. **Train all cashiers** on how to use the tool and how to handle SUSPICIOUS verdicts
3. **Establish an escalation policy** before going live — who does the cashier call when a fake is detected?
4. **Log incidents** — keep a record of flagged attempts for university security review
5. **Combine with deterrence** — post a visible notice that payment screenshots are AI-verified; this alone discourages many attempts
6. **Periodically test the system** with sample screenshots to verify it's working correctly

---

## Contributing

Contributions, bug reports, and feature suggestions are welcome.

1. Fork the repository
2. Create a feature branch: `git checkout -b feature/your-feature-name`
3. Commit your changes: `git commit -m 'Add: your feature description'`
4. Push to the branch: `git push origin feature/your-feature-name`
5. Open a Pull Request

### Potential Enhancements

- [ ] Backend proxy server for secure API key handling
- [ ] Scan history log with timestamps (local storage)
- [ ] Multi-language UI support
- [ ] Integration with POS systems via webhook
- [ ] Admin dashboard with fraud statistics
- [ ] Support for video verification (live screen recording)
- [ ] Offline mode with cached model responses for common fraud patterns
- [ ] QR code scanner integration for direct transaction verification

---

## License

This project is released under the **MIT License**. See `LICENSE` for full details.

---

## Acknowledgments

- **Anthropic** — for Claude AI vision capabilities powering the forensic analysis engine
- **Google Fonts** — Syne and Space Mono typefaces used in the UI
- Built with ❤️ to protect small campus businesses from digital fraud

---

<div align="center">

**PayGuard** · University Café Security System · Powered by Claude AI

*Protecting honest transactions, one screenshot at a time.*

</div>
