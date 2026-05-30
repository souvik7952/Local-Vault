<div align="center">

<br/>

<img src="https://img.shields.io/badge/AES--256--GCM-Encrypted-00d4aa?style=for-the-badge&logo=shield&logoColor=white"/>
&nbsp;
<img src="https://img.shields.io/badge/PBKDF2-200k%20Iterations-00d4aa?style=for-the-badge&logoColor=white"/>
&nbsp;
<img src="https://img.shields.io/badge/Zero%20Server-100%25%20Local-00d4aa?style=for-the-badge&logoColor=white"/>

<br/><br/>

<h1>🔐 File Vault</h1>

<p><strong>A password-protected file encryption tool that runs entirely in your browser.<br/>No server. No uploads. No dependencies. Just open and use.</strong></p>

<br/>

<img src="https://img.shields.io/badge/HTML5-Only-E34F26?style=flat-square&logo=html5&logoColor=white"/>
&nbsp;
<img src="https://img.shields.io/badge/CSS3-Vanilla-1572B6?style=flat-square&logo=css3&logoColor=white"/>
&nbsp;
<img src="https://img.shields.io/badge/JavaScript-Vanilla-F7DF1E?style=flat-square&logo=javascript&logoColor=black"/>
&nbsp;
<img src="https://img.shields.io/badge/Web%20Crypto%20API-Browser%20Native-4dabf7?style=flat-square"/>
&nbsp;
<img src="https://img.shields.io/badge/Offline-Ready-00d4aa?style=flat-square"/>

<br/><br/>

</div>

---

## 📖 What is File Vault?

**File Vault** is a single, self-contained `HTML` file that lets you encrypt any file with a password directly inside your browser. The encrypted output is saved as a `.vault` file on your disk. To access the original file again, open the `.vault` file in the same tool, enter your password, and the file is decrypted in memory — never written to disk automatically.

It works like opening a password-protected ZIP or PDF: the file stays encrypted on disk at all times. Decryption only ever happens in browser memory, and only after the correct password is entered.

> **No installation. No backend. No data ever leaves your device.**

---

## ✨ Features

<table>
<tr>
<td width="50%" valign="top">

### 🔒 Encryption
- Encrypt **any file type** with a password
- Outputs a portable `.vault` file
- Random **256-bit Salt** generated per file
- Random **96-bit IV** generated per file
- Rename the vault file before saving
- Re-download with a custom filename anytime

</td>
<td width="50%" valign="top">

### 🔓 Decryption
- Open any `.vault` file
- Password-gated — wrong password reveals nothing
- Decryption happens **in memory only**
- Rename the original file before downloading
- Extension is locked — only the stem is editable

</td>
</tr>
<tr>
<td valign="top">

### 👁️ File Preview (in-browser)
- **Images** — rendered inline (JPG, PNG, GIF, WebP…)
- **PDF** — embedded iframe preview
- **Text files** — syntax-friendly plain-text view (TXT, JSON, CSV, XML, JS, Python, etc.)
- Open in new browser tab
- Preview never writes to disk

</td>
<td valign="top">

### 🎨 Interface
- **Dark & Light mode** with smooth transitions
- Theme preference saved to `localStorage`
- Drag-and-drop file upload
- Live password strength indicator (4 tiers)
- Password visibility toggle
- Progress indicators during crypto operations
- Fully responsive — works on mobile

</td>
</tr>
</table>

---

## 🛡️ Security

| Property | Detail |
|---|---|
| **Algorithm** | AES-256-GCM (authenticated encryption) |
| **Key Derivation** | PBKDF2 with SHA-256 |
| **Iterations** | 200,000 |
| **Salt** | 32 bytes (256-bit), random per file |
| **IV** | 12 bytes (96-bit), random per file |
| **Auth Tag** | 16 bytes (GCM built-in) — detects tampering & wrong passwords |
| **Password Storage** | Never stored anywhere |
| **Network Requests** | Zero — fully offline |
| **Crypto Engine** | Browser-native [Web Crypto API](https://developer.mozilla.org/en-US/docs/Web/API/Web_Crypto_API) |

### How wrong-password detection works

AES-GCM includes a cryptographic authentication tag. If the password is wrong, the derived key is wrong, the tag verification fails, and `crypto.subtle.decrypt()` throws — before a single byte of plaintext is produced. The app catches this and shows `"Invalid password or corrupted file."` No partial data is ever exposed.

---

## 📁 Vault File Format

The `.vault` file is a compact binary format:

```
┌──────────────────────────────────────────────────────┐
│  6 bytes   │  Magic header: "VAULT1"                 │
│  2 + N     │  Original filename  (length-prefixed)   │
│  2 + M     │  MIME type          (length-prefixed)   │
│  32 bytes  │  Salt  (random, 256-bit)                │
│  12 bytes  │  IV    (random, 96-bit)                 │
│  rest      │  AES-GCM ciphertext + 16-byte auth tag  │
└──────────────────────────────────────────────────────┘
```

The original filename and MIME type are stored **inside** the encrypted package so the app knows how to restore and preview the file correctly on decryption.

---

## 🚀 Getting Started

### Option 1 — Open directly

1. Download `vault.html`
2. Open it in any modern browser (Chrome, Firefox, Edge, Safari)
3. That's it

> ⚠️ Must be opened from `localhost` or `https://` — the Web Crypto API requires a [secure context](https://developer.mozilla.org/en-US/docs/Web/Security/Secure_Contexts). Double-clicking the file from your filesystem (`file://`) works in most browsers but may be blocked in some.

### Option 2 — Serve locally

```bash
# Python 3
python -m http.server 8080

# Node.js (npx)
npx serve .

# Then open: http://localhost:8080/vault.html
```

---

## 🔄 How to Use

### Encrypting a file

```
1. Open the "Encrypt File" tab
2. Drop any file onto the upload zone (or click to browse)
3. Enter a password and confirm it
4. Click "Encrypt & Save .vault File"
5. The .vault file is downloaded automatically
6. Optionally rename it in the result card and re-download
```

### Decrypting a file

```
1. Open the "Open Vault File" tab
2. Drop your .vault file onto the upload zone
3. Enter the password used during encryption
4. Click "Unlock Vault File"
5. On success: view, preview, or download the original file
6. Optionally rename the file before downloading
```

---

## 📂 Supported File Types

File Vault works with **any binary or text file**. Preview support varies:

| File Type | Encrypt | Decrypt | In-Browser Preview |
|---|:---:|:---:|:---:|
| Images (JPG, PNG, GIF, WebP…) | ✅ | ✅ | ✅ |
| PDF | ✅ | ✅ | ✅ |
| Text / Markdown / Log | ✅ | ✅ | ✅ |
| JSON / XML / CSV | ✅ | ✅ | ✅ |
| Code files (JS, TS, PY, etc.) | ✅ | ✅ | ✅ |
| Word (DOC / DOCX) | ✅ | ✅ | ⬜ Download only |
| Excel (XLS / XLSX) | ✅ | ✅ | ⬜ Download only |
| PowerPoint (PPT / PPTX) | ✅ | ✅ | ⬜ Download only |
| ZIP / archives | ✅ | ✅ | ⬜ Download only |
| Any other binary | ✅ | ✅ | ⬜ Download only |

---

## 🖥️ Browser Compatibility

| Browser | Supported |
|---|:---:|
| Chrome / Chromium 60+ | ✅ |
| Firefox 57+ | ✅ |
| Edge 79+ | ✅ |
| Safari 11+ | ✅ |
| Opera 47+ | ✅ |
| IE / Legacy Edge | ❌ |

Requires **Web Crypto API** support and a **secure context** (`https://` or `localhost`).

---

## 🏗️ Project Structure

```
vault.html          ← The entire application (single file)
README.md           ← This file
```

Everything — HTML, CSS, and JavaScript — lives in one self-contained file with no external dependencies (except optional Google Fonts for typography, which degrades gracefully offline).

---

## ⚙️ Technical Stack

```
Language        Vanilla HTML + CSS + JavaScript (ES2020+)
Crypto          Web Crypto API  (window.crypto.subtle)
Encryption      AES-256-GCM
Key Derivation  PBKDF2 · SHA-256 · 200,000 iterations
Packaging       Custom binary format with magic header
UI              Custom design system · CSS variables · Dark/Light theme
Storage         None — all data stays in memory
Network         None — fully offline capable
```

---

## 🔐 Security Notes

- **Passwords are never stored** — not in memory beyond the operation, not in `localStorage`, not anywhere
- **The decrypted file is never written to disk** unless the user explicitly clicks "Download Original"
- **Each encryption uses a fresh random salt and IV** — encrypting the same file twice produces different vault files
- **Tamper detection is built in** — AES-GCM's authentication tag means any modification to the vault file (even a single bit) will cause decryption to fail
- **The vault file is opaque** — without the correct password it reveals nothing about the original file's contents, name, or type (metadata is inside the ciphertext)
- **No telemetry, no analytics, no callbacks** — the source is fully auditable in a single file

---

## 📄 License

```
MIT License — free to use, modify, and distribute.
```

---

<div align="center">

<br/>

Built with the browser's native **Web Crypto API** — no libraries, no servers, no trust required.

<br/>

<img src="https://img.shields.io/badge/Made%20with-HTML%20only-E34F26?style=flat-square&logo=html5&logoColor=white"/>
&nbsp;
<img src="https://img.shields.io/badge/Zero%20Dependencies-✓-00d4aa?style=flat-square"/>
&nbsp;
<img src="https://img.shields.io/badge/No%20Data%20Leaves%20Your%20Device-✓-00d4aa?style=flat-square"/>

<br/><br/>

</div>
