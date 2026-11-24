<div align="center">

# 🛡️ LoginGuard

### Every login leaves a door. Know what's behind it.

A browser extension that reads "Sign in with Google / GitHub" **before** you click **Allow**, and tells you, in plain words, what you're about to give away.

**wHACKiest 2026 · Beyond the Browser** · Prototype / MVP · Chrome Manifest V3 · No server · No AI, on purpose

🌐 **Website:** _&lt;add link&gt;_ &nbsp;·&nbsp; 🎥 **Demo video:** _&lt;add link&gt;_

</div>

> 🚧 **Honest status:** this is a **prototype / MVP, still in progress**. The core sign-in check works as a demo; other parts are still being built. [See what works →](#-where-we-are-right-now)

---

## 🤔 Picture this

You're signing up for a "free resume builder" you found on a WhatsApp group.
It says **Sign in with Google**. A familiar Google pop-up appears. You click **Allow**.

What you just agreed to might be:

- ✉️ **read, send and delete your Gmail**
- 📁 **see every file in your Google Drive**
- ⏳ **keep that access even after you close the tab**

…given to a site that may have been **registered three days ago**.

No password was stolen. No virus was installed. **You just clicked Allow.**

---

## 🚨 The problem

This isn't hypothetical. In **May 2017**, a fake **"Google Docs"** app used Google's **real** consent screen. It reached **about 1 million Gmail users in roughly an hour**, and nobody had to type a password.

And it's not just sign-ins. Developers also leave **secret API keys** sitting in website code and public GitHub repos, where anyone can find them.

**Why today's tools miss it:**

| Tool | What it does | The gap |
|---|---|---|
| Consent screen | Lists what the app wants | Written in jargon nobody reads |
| Account settings | Shows apps with access | Only **after** you've already said yes |
| Phishing blockers | Block known bad sites | Know nothing about a site made yesterday |

> **Other tools check the website. LoginGuard checks the deal you're about to make.**

---

## 💡 Our solution

LoginGuard sits in your browser and steps in at the **exact moment** you're about to click Allow.
The prototype covers **Google** and **GitHub**. **LinkedIn** is next.

### What you'll see

Here's an example of the kind of warning LoginGuard shows on a risky sign-in:

```
┌───────────────────────────────────────────────────────────────┐
│  ⚠️  LoginGuard: HIGH RISK                                    │
│                                                               │
│  "Resume Builder Pro" wants to:                               │
│    • Read, send and delete your Gmail                         │
│    • See and download all your Google Drive files             │
│    • Keep access when you're not using it (offline)           │
│                                                               │
│  Your access goes to:  resume-google-free.xyz                 │
│    • Uses "google" in a site that isn't Google's              │
│    • Site registered 3 days ago                               │
│                                                               │
│        [ Cancel sign-in ]                 [ Continue ]           │
└───────────────────────────────────────────────────────────────┘
```

A safe sign-in gets a small green **OK** badge and stays out of your way.

### What it checks

| | Check | In plain words |
|---|---|---|
| 🔑 | **Permissions** | "This app can read your email", not `gmail.modify` |
| 🔀 | **Redirect** | Where your access *actually* goes after Allow |
| 🎭 | **Look-alikes** | Fake brand names like `google-login-help.xyz` |
| 🐣 | **Brand-new sites** | Registered days ago? That's a red flag (RDAP lookup) |
| 📊 | **Risk score** | Low / Medium / High, and always *why* |
| 🔌 | **Connected apps** | Old apps that still have access, plus where the real Remove button is |
| 🗝️ | **Leaked keys** | Exposed keys in public repos and site code, always masked |

### The flow

```
Click "Sign in"  →  LoginGuard reads the request  →  ✅ OK badge  or  ⚠️ warning  →  YOU decide
```

**Three promises:** it never asks for your password, it never removes access on its own, and it never sends your data to a LoginGuard server, because there isn't one.

### ⬇️ Try it yourself (2 minutes)

**[Download loginguard-extension.zip](./loginguard-extension.zip)** (Chrome 116+, should also work on Edge and Brave)

1. **Unzip** the file.
2. Go to `chrome://extensions` and turn on **Developer mode** (top right).
3. Click **Load unpacked** and pick the `loginguard-extension` folder.
4. Pin LoginGuard to your toolbar.

Then:

1. Click the LoginGuard icon → **Settings** → **Try it**. This fires test sign-ins, from safe to scary.
2. Watch a safe one get an **OK** badge, and a risky one get a full warning with reasons.
3. Hit **Cancel sign-in** and you're taken back to safety.
4. Open the dashboard and flip to **Sample** mode to explore connected apps and the key scanner.

> 🧪 Sample mode uses made-up demo data, not real accounts.

---

## ⚙️ How it works

Everything runs **inside your browser**. Input → processing → output:

```
  👆 You click "Sign in with Google / GitHub"
        │
        ▼
  🔍 Detector            webRequest + webNavigation (read-only)
        │                reads client_id · scope · redirect · state
        ▼
  🧠 Risk engine         our own JavaScript rules
        │                permission map · redirect & look-alike checks · domain age (RDAP)
        ▼
  🛑 Warning panel       score + reasons → Continue or Cancel
        │
        ▼
  💾 Your device only    chrome.storage → dashboard & history
```

The only outside calls go to **public** sources: RDAP for domain age, the GitHub REST API, and website JS files when you choose to scan them.

---

## 🧰 Tech stack

| Layer | Tool | Why this |
|---|---|---|
| Extension | Chrome Manifest V3 | Lives right where the sign-in happens |
| Reading sign-ins | `webRequest` + `webNavigation` (read-only) | Sees the request without touching it |
| Warning on page | Content script + Shadow DOM | A shady site can't hide or restyle our warning |
| Risk engine | Our own JavaScript rules (~95 permissions mapped) | Every score comes with a reason |
| Dashboard / website | React + TypeScript + Tailwind (Vite) | One codebase for both |
| Storage | `chrome.storage` | Stays on your device: no server, no account |
| Public APIs | RDAP, GitHub REST API | Free, public, no API keys |
| Key scanner | 30 Gitleaks-style rules + entropy filter | Proven patterns, fewer false alarms |
| Testing | Node test runner, Playwright | Checks the rules and the real browser flow |
| AI / ML | **Not used, on purpose** | A security warning you can't explain is a warning you can't trust |

---

## 📍 Where we are right now

| ✅ Working in the prototype | 🛠️ Still building | 🔭 Coming next |
|---|---|---|
| Spots Google & GitHub sign-ins | Testing on **live** Google / GitHub pages (only tested on mock pages so far) | **LinkedIn sign-in**, then Microsoft |
| Risk engine + plain-word reasons | Connected-apps reader (provider pages change often) | Firefox & Edge versions |
| On-page warning with Cancel / Continue | Finishing the website & dashboard | Team view for colleges & hackathons |
| Leaked-key scan of public repos (masked) | Fewer false alarms | Alerts when an app asks for more access |
| Dashboard with Sample mode + engine tests | | Chrome Web Store release |

---

## 🧑‍💻 Run it locally

```bash
# extension tests
cd extension
node --test tests/*.test.mjs

# website
cd website
npm install
npm run dev
```

## 📁 Repo structure

```
loginguard-extension.zip   ready-to-install extension (the download link above)
extension/
  manifest.json            MV3 manifest
  background.js            detection, badge, notifications, history
  engine/                  scopes, URL checks, risk scoring, RDAP, secret rules
  content/                 on-page warning + settings-page reader
  dashboard/               built dashboard (from website/)
  popup/  options/         toolbar popup, settings, test links
  tests/                   rule engine tests
website/
  src/                     React + TypeScript source
  vite.config.ts           website build
  vite.ext.config.ts       dashboard build for the extension
```

---

## ⚖️ What LoginGuard can't do (yet), stated honestly

- 🌐 It only sees sign-ins **in this browser**. It can't see phone apps, desktop apps, or Google One Tap.
- 🔒 It checks **public** code only. Private server-side keys are invisible to it.
- 🤝 It **never promises** a site is safe. It explains the risk; you make the call.
- 🚦 It **warns, it doesn't block.** The final choice is always yours.
- ✋ Only scan websites and repos you own or are allowed to test.

---

<div align="center">

**Built for wHACKiest 2026** · Online Ideathon, 26–27 Nov 2026 · Dept. of CSE, Ramaiah Institute of Technology

*Think before you click Allow. Or let LoginGuard think with you.* 🛡️

</div>
