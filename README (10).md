<div align="center">

# 🛡️ LoginGuard

### Every login leaves a door. Know what's behind it.

A browser extension that reads "Sign in with Google / GitHub" **before** you click **Allow**, tells you in plain words what you're about to give away, and scans your GitHub, Chrome and LinkedIn for secrets and personal data you've already exposed.

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

### 🔎 Sensitive Data Exposure Scanner

Sign-ins aren't the only leak. The scanner checks whether your accounts are **already giving things away**:

| Platform | What it checks | How it gets access |
|---|---|---|
| **GitHub** | Source code, config files, READMEs, committed `.env` files and **recent commit history** of your public repos | GitHub's public API. No login, no token. Private repos are never read |
| **Chrome** | Extensions that can read every site, your cookies or your traffic, and ones not from the Chrome Web Store | Chrome's "Manage your extensions" permission, asked at scan time. You can remove it after |
| **LinkedIn** | Your profile, posts and contact info | Reads only a LinkedIn page **you already have open**, when you ask, or text you paste. No crawling |

**It looks for:** API keys and tokens, GitHub tokens, AWS and cloud credentials, database passwords and connection strings, private keys, hard-coded passwords, `.env` files, internal addresses (like `10.x.x.x` or `*.internal`), emails, phone numbers and ID numbers (Aadhaar, PAN).

Every finding gets a **severity**, so you know what to fix first:

| Severity | Meaning |
|---|---|
| 🔴 **Critical** | Credentials or private keys that look real |
| 🔴 **High** | Passwords, tokens or confidential config |
| 🟠 **Medium** | Personal info or internal details |
| 🟢 **Low** | Could create a privacy risk |

Each finding also shows **where** it was found (repo, file, commit, extension or post), **why** it's risky and **what to do**. It's labelled either **Strong match** (the exact format of a real key or ID) or **Possible · check** (could be a dummy value), so false positives are easy to spot.

**Safety rules built in:**
- 🙈 Values are **always masked** (`sk-pro••••••••Xq2F`). The full secret is never shown.
- 🔐 Nothing is stored in plain text. Only the masked value and a one-way **SHA-256 fingerprint** are kept, on your device.
- ✋ It **never** deletes files, revokes keys or changes accounts. "Mark as fixed" asks you to confirm first.
- 🚫 No passwords, no cookies, no getting around a platform's limits.

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
4. Open the dashboard → **Exposure scanner**. Type your GitHub username, press **Check my extensions**, or open your LinkedIn profile in another tab and press **Scan my open LinkedIn tab**.
5. Flip to **Sample** mode to explore everything with made-up data.

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
| Exposure scanner | Our rules for passwords, `.env` files, internal addresses and Indian IDs (Aadhaar checksum); `chrome.management`; `chrome.scripting` | Finds more than keys, without reading anything you didn't choose |
| Fingerprints | Web Crypto SHA-256 | Recognise a secret again without ever storing it |
| Testing | Node test runner, Playwright | Checks the rules and the real browser flow |
| AI / ML | **Not used, on purpose** | A security warning you can't explain is a warning you can't trust |

---

## 📍 Where we are right now

| ✅ Working in the prototype | 🛠️ Still building | 🔭 Coming next |
|---|---|---|
| Spots Google & GitHub sign-ins | Testing on **live** Google / GitHub pages (only tested on mock pages so far) | **LinkedIn sign-in**, then Microsoft |
| Risk engine + plain-word reasons | Connected-apps reader (provider pages change often) | Firefox & Edge versions |
| On-page warning with Cancel / Continue | Finishing the website & dashboard | Team view for colleges & hackathons |
| Exposure scanner: GitHub repos + commits, Chrome extensions, LinkedIn pages and pasted text (masked) | Fewer false alarms; testing on real LinkedIn pages | Alerts when an app asks for more access |
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
  engine/                  scopes, URL checks, risk scoring, RDAP, secret rules,
                           exposure.js (GitHub / Chrome / LinkedIn exposure scanner)
  content/                 on-page warning + settings-page reader
  dashboard/               built dashboard (from website/)
  popup/  options/         toolbar popup, settings, test links
  tests/                   rule engine + exposure scanner tests
website/
  src/                     React + TypeScript source
  vite.config.ts           website build
  vite.ext.config.ts       dashboard build for the extension
```

---

## ⚖️ What LoginGuard can't do (yet), stated honestly

- 🌐 It only sees sign-ins **in this browser**. It can't see phone apps, desktop apps, or Google One Tap.
- 🔒 It checks **public** code only. Private server-side keys are invisible to it.
- 🔗 LinkedIn has no API for reading your posts, so LinkedIn checks only cover the page you open or the text you paste.
- 🧩 For Chrome, it checks extension **permissions**, not what an extension actually does. It can't (and won't) read your saved passwords. Chrome's own Password Checkup does that.
- 🔑 It never tests whether a key still works. Treat strong matches as active until you've rotated them.
- 🤝 It **never promises** a site is safe. It explains the risk; you make the call.
- 🚦 It **warns, it doesn't block.** The final choice is always yours.
- ✋ Only scan websites and repos you own or are allowed to test.

---

<div align="center">

**Built for wHACKiest 2026** · Online Ideathon, 26–27 Nov 2026 · Dept. of CSE, Ramaiah Institute of Technology

*Think before you click Allow. Or let LoginGuard think with you.* 🛡️

</div>
