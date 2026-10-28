# 🌐 Nexus - Secure P2P Terminal

**[ 🇬🇧 English ]** Nexus is a zero-trust, end-to-end encrypted (AES-256) P2P web terminal with a built-in barcode scanner. It operates 100% on the client-side without any database.

**[ 🇹🇷 Türkçe ]** Nexus, dahili barkod okuyucuya sahip, uçtan uca şifreli (AES-256) ve sıfır güven mimarili bir P2P web terminalidir. Veritabanı olmadan %100 istemci tarafında çalışır.

**[ 🇪🇸 Español ]** Nexus es un terminal web P2P con cifrado de extremo a extremo (AES-256) y un escáner de código de barras incorporado. Funciona 100% en el lado del cliente sin base de datos.

**[ 🇫🇷 Français ]** Nexus est un terminal web P2P chiffré de bout en bout (AES-256) avec un scanner de codes-barres intégré. Il fonctionne 100% côté client sans base de données.

**[ 🇩🇪 Deutsch ]** Nexus ist ein Ende-zu-Ende-verschlüsseltes (AES-256) P2P-Webterminal mit integriertem Barcode-Scanner. Es arbeitet zu 100 % clientseitig ohne Datenbank.

**[ 🇷🇺 Русский ]** Nexus — это P2P веб-терминал со сквозным шифрованием (AES-256) и встроенным сканером штрих-кодов. Работает на 100% на стороне клиента без базы данных.

**[ 🇸🇦 العربية ]** Nexus عبارة عن محطة ويب P2P مشفرة من البداية إلى النهاية (AES-256) مع ماسح ضوئي للرموز الشريطية مدمج. يعمل بنسبة 100% على جانب العميل بدون قاعدة بيانات.


---

## 🚀 Overview

Nexus is designed to establish a highly secure, real-time, and ephemeral communication channel between any two devices without the need for backend infrastructure, databases, or user registration. 

Whether you need to securely beam physical barcode data from a mobile scanner to a desktop, or establish an untraceable and isolated chat room, Nexus handles it entirely within the browser using advanced Web APIs.

## ✨ Core Features & Security Architecture

### 🛡️ Asymmetric & Symmetric Cryptography (E2EE)
*   **AES-256-GCM:** All text messages and images are encrypted locally on the device using the native `Web Crypto API` before transmission.
*   **PBKDF2 Key Derivation:** The 7-character session code is transformed into a highly secure encryption key via 100,000 iterations of PBKDF2. The public MQTT broker only routes unbreakable ciphertext.

### 🌍 Global Ready (Multi-Language & RTL)
*   Built-in **i18n** translation system with instant, zero-reload switching.
*   Supports **7 Languages:** English, Turkish, Spanish, French, German, Russian, and Arabic.
*   **Full RTL Support:** When Arabic is selected, the entire UI (including flex alignments, chat bubbles, and icons) perfectly mirrors to Right-to-Left orientation.

### 📸 Hardware Integrations
*   **Built-in Scanner:** Native HTML5 camera integration. Scan a host's QR code from the home screen to instantly and securely join a node without typing.
*   **Oscillator Audio:** Notification sounds (beeps) are generated purely by the device's hardware via the **Web Audio API** (sine/triangle waves), eliminating external network requests for audio files.

### 🖼️ Secure Media Transfer & Sanitization
*   Send images directly from your gallery or camera.
*   **Canvas Compression:** Images are drawn onto an HTML5 `<canvas>`, sanitized, and compressed (Base64 JPEG) *before* encryption. This prevents memory-exhaustion (DoS) attacks and strips malicious payloads embedded in image files.

### 📡 Resilient Multi-Broker Network
*   Carrier-grade NATs or strict firewalls blocking a port? Nexus bypasses them.
*   It utilizes a **Multi-Broker Fallback System**. It dynamically attempts connections across HiveMQ, EMQX, and Mosquitto public brokers on different ports (8081, 8084, 8443) to guarantee uptime and connection success.

### 🛡️ Zero-Trust UI & XSS Immunity
*   **Strict CSP:** Enforced Content-Security-Policy headers.
*   **DOM Injection Defense:** Usernames and incoming messages are rendered strictly as text nodes (`textContent`), making Cross-Site Scripting (XSS) impossible.
*   **Session Recovery:** Accidentally refreshed? Your encrypted state, session ID, and chat history are securely held in `sessionStorage` and instantly restored.

## 📱 User Experience (WhatsApp-Style UI)
*   **Custom Identities:** Choose your own display name upon entry.
*   **Compact UI:** Side-by-side username and timestamp rendering inside sleek chat bubbles.
*   **Dark/Light Mode:** Seamless theme toggling.
*   **Secure Image Modal:** Click on received images to view them in a blurred-background fullscreen modal.
*   **Typing Indicators:** Real-time visual feedback when the remote device is typing.

## 🚦 How to Use

### Establishing a Node (Host)
1. Open Nexus on your primary device (e.g., Desktop).
2. Enter your name and click **"Create Secure Node"**.
3. A QR code and a 7-character session code will appear.
4. When a guest requests to join, an inline approval prompt will appear. Click **Accept**.

### Joining a Node (Guest)
1. Open Nexus on your secondary device (e.g., Mobile Phone).
2. Enter your name.
3. Tap the **Scanner Icon** and scan the host's QR code, OR enter the 7-character code manually.
4. Wait for the host to approve your identity.

## 🛠️ Technical Stack
*   **Frontend:** HTML5, Vanilla JavaScript (ES6+), `sessionStorage`
*   **Styling:** Tailwind CSS (via CDN)
*   **Cryptography:** Web Crypto API
*   **Real-time Protocol:** MQTT over WebSockets (`mqtt.js`)
*   **Scanner:** `html5-qrcode`

## 🌐 Deployment (GitHub Pages)
Nexus is a 100% client-side application contained within a single `index.html` file.
1. Fork this repository.
2. Go to **Settings > Pages**.
3. Set the Source to the `main` branch.
4. Your secure terminal is live!

---
*Disclaimer: While Nexus uses AES-256-GCM encryption, it relies on public MQTT brokers for signaling. Do not use for transmitting highly classified government data.*
