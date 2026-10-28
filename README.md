# 🌐 Nexus - Secure P2P Terminal

Nexus is a military-grade, end-to-end encrypted (E2EE) peer-to-peer web terminal. It allows two devices to establish a highly secure, real-time communication channel without the need for any backend infrastructure, databases, or user registration. 

Whether you need to securely transfer barcode data from a mobile scanner to a desktop computer, or establish an untraceable ephemeral chat, Nexus handles it entirely within the browser.

![Nexus Interface Preview](https://via.placeholder.com/800x400.png?text=Nexus+-+Secure+P2P+Terminal)

## ✨ Key Features

* **🛡️ AES-256-GCM End-to-End Encryption:** Your messages, images, and barcode data are encrypted on the device using the Web Crypto API. The MQTT broker only routes unbreakable ciphertext.
* **📷 Built-in Hardware Scanner:** Native HTML5 QR/Barcode scanner integration. Read physical barcodes using your phone's camera and instantly beam them to the connected desktop terminal.
* **🖼️ Secure Media Transfer:** Send images directly. Images are rendered and compressed securely via the HTML5 `<canvas>` before transmission to prevent memory-exhaustion (DoS) attacks and malicious payloads.
* **🕵️‍♂️ Zero-Trust Architecture:** Strict Content-Security-Policy (CSP), payload size limits (max 2000 chars), and strict DOM text-injection (XSS immunity).
* **🔄 Multi-Broker Fallback Network:** Never lose connection. Nexus dynamically routes traffic through multiple public MQTT brokers (HiveMQ, EMQX, Mosquitto) based on network availability and port blocking (bypassing carrier-grade NATs).
* **📱 Native PWA Experience:** WhatsApp-style fullscreen UI, dark/light mode toggle, typing indicators, and hardware-generated audio feedback (Web Audio API) for notifications.
* **🧠 Session Recovery:** Accidentally refreshed the page? No problem. `sessionStorage` keeps your encryption keys and chat history alive until you close the tab.

## 🚀 How to Use

### Establishing a Node (Host)
1. Open Nexus on your primary device (e.g., Desktop).
2. Click **"Create Secure Node"**.
3. A unique QR code and a 7-character session code will be generated.
4. Wait for the guest device to scan the QR code or manually enter the code. 
5. Approve the incoming connection request from the inline permission area.

### Joining a Node (Guest)
1. Open Nexus on your secondary device (e.g., Mobile Phone).
2. Tap the **QR Scan** icon on the home screen and scan the host's QR code.
3. *Alternatively*, enter the 7-character session code manually and click **"Connect Device"**.
4. Once the host approves, the E2EE terminal will open.

## 🛠️ Technical Stack

* **Frontend:** HTML5, JavaScript (ES6+)
* **Styling:** Tailwind CSS (via CDN)
* **Cryptography:** Native Web Crypto API (PBKDF2 + AES-256-GCM)
* **Real-time Protocol:** MQTT over WebSockets (`mqtt.js`)
* **Hardware Integrations:** `html5-qrcode` (Camera), Web Audio API (Hardware beeps)

## 🌐 Deployment (GitHub Pages)

Nexus is a 100% client-side Single Page Application (SPA) contained within a single `index.html` file. 

To deploy your own secure instance for free:
1. Fork this repository.
2. Go to **Settings > Pages**.
3. Set the Source to the `main` branch.
4. Your secure terminal is now live on `https://your-username.github.io/repo-name/`.

---
*Disclaimer: While Nexus uses AES-256-GCM encryption, it relies on public MQTT brokers for signaling. Do not use for transmitting highly classified government data.*
