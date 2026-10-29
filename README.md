# Nexus – Browser Chat over Public MQTT Brokers

**[EN]** Nexus is a single-file web app that lets two devices chat in a temporary room. Messages are encrypted in the browser (AES-256-GCM) and relayed through public MQTT brokers. There is no backend of your own and no database. A built-in QR scanner lets a guest join by scanning the host's QR code.

**[TR]** Nexus, iki cihazın geçici bir odada sohbet etmesini sağlayan tek dosyalık bir web uygulamasıdır. Mesajlar tarayıcıda şifrelenir (AES-256-GCM) ve herkese açık MQTT broker'ları üzerinden iletilir. Kendi sunucunuz ve veritabanınız yoktur. Misafir, ev sahibinin QR kodunu okutarak odaya katılabilir.

> **Note:** "P2P" here means no server of your own. Traffic is **not** direct peer-to-peer (no WebRTC); it goes through third-party brokers. Please read [Security Model & Limitations](#-security-model--limitations) before trusting this with anything sensitive.

---

## Features

- **Temporary rooms:** the host creates a room and gets a 7-character code plus a QR code. The guest joins by scanning the QR code or typing the code, and the host approves or denies the request.
- **Text and image messages:** images are resized (max 800 px wide) and re-encoded as JPEG (quality 0.6) on a `<canvas>` before sending.
- **Client-side encryption:** AES-256-GCM via the Web Crypto API, with the key derived from the room code using PBKDF2-SHA256 (100,000 iterations).
- **Panic mode:** while a session is active, 3 quick clicks/taps (within 0.8 s) anywhere on the screen send a `leave` signal, clear the session from `sessionStorage` and the DOM, and remove `?session=` from the URL.
- **Broker fallback:** tries HiveMQ (`8443`), EMQX (`8084`) and Mosquitto test broker (`8081`) in order over secure WebSockets.
- **7 UI languages:** EN, TR, ES, FR, DE, RU, AR (with RTL layout for Arabic). Language and theme (light/dark) are stored in `localStorage`.
- **Sound notifications:** generated with the Web Audio API (oscillator), so no audio files are loaded.
- **XSS hygiene:** messages and usernames are rendered with `textContent`, not `innerHTML`.

## How to Use

### Host
1. Open Nexus on the first device and enter your name.
2. Click **Create Secure Node**. A QR code and a 7-character code appear.
3. When a guest asks to join, an approval prompt appears. Click **Accept** or **Deny**.

### Guest
1. Open Nexus on the second device and enter your name.
2. Tap the scanner icon and scan the host's QR code, or type the 7-character code.
3. Wait for the host to approve.

## 🔐 Security Model & Limitations

Nexus is a hobby / learning project. It has **not been audited**. Know what it does and does not protect:

- **The room code is both the encryption secret and the MQTT topic name.** The topics are `qsys/<CODE>` and `qchat/<CODE>`, and the AES key is derived from the same `<CODE>`. The broker, and anyone who can observe or subscribe to topics on a public broker, can see the code and derive the key. Encryption therefore does **not** protect message contents from the broker or from someone who learns the code. It only protects against someone who cannot see the topic names.
- **Low-entropy code.** The code is `H` plus 6 random characters from `A–Z0–9` (about 31 bits), generated with `Math.random()` (not a CSPRNG). The PBKDF2 salt is a fixed constant. An attacker who knows the code, or records traffic, can brute-force the key offline.
- **Metadata is not hidden.** Sender names, message timing, message sizes and your IP address are visible to the broker. Names are sent in plaintext outside the encrypted payload.
- **Control messages are unauthenticated.** `join_request`, `join_response`, `typing` and `leave` are plaintext JSON. Anyone who knows the code can send them (for example, to end a session or request to join). The host approval is based on a self-declared name only.
- **No incoming validation for images.** Compression happens on the sender's side only, so a modified client can send arbitrarily large payloads.
- **Chat history is stored unencrypted in `sessionStorage`** for the lifetime of the tab (so reloads keep the chat). Panic mode and "End Session" remove it from the app's storage and DOM, but this is not a forensic wipe and cannot guarantee that memory is cleared.
- **Public brokers offer no guarantees.** They can rate-limit, go down or log traffic. Messages use QoS 0 and are not retained, so an offline peer will miss them. Non-standard ports (`8081`, `8084`, `8443`) may be blocked on some corporate or carrier networks.
- **Third-party scripts.** Tailwind (CDN), qrcodejs and mqtt.js (cdnjs) and `html5-qrcode` (unpkg, unpinned) are loaded without Subresource Integrity. The CSP is a `<meta>` tag and allows `'unsafe-inline'` scripts, so it limits where resources can load from but is not a strong XSS barrier.
- **The room code appears in the join link** (`?session=<CODE>`), so it can end up in browser history or in chat apps where you share the link.

If you need a messenger you can rely on, use an audited one (Signal, Matrix with E2EE, etc.).

## Technical Stack

- **Frontend:** HTML5, vanilla JavaScript (ES6+), single `index.html`
- **Styling:** Tailwind CSS (CDN)
- **Crypto:** Web Crypto API (PBKDF2-SHA256, AES-256-GCM)
- **Transport:** MQTT over secure WebSockets (`mqtt.js` 4.3.7)
- **QR generation:** `qrcodejs` 1.0.0
- **QR scanning:** `html5-qrcode`

## Deployment (GitHub Pages)

Nexus is a static page, but it must be served over **HTTPS** (Web Crypto and the camera require a secure context).

1. Fork this repository.
2. Go to **Settings → Pages**.
3. Set the source to the `main` branch.
4. Open the published URL.

## Roadmap / Ideas

- Generate the room code with `crypto.getRandomValues` and make it longer.
- Use a random topic ID that is separate from the encryption secret, and add a per-room random salt.
- Authenticate control messages (e.g. HMAC with a key derived from the secret).
- Pin library versions and add Subresource Integrity hashes.
- Validate and size-limit incoming images.

## Disclaimer

**[EN]** Nexus is provided "as is", without warranty of any kind, for educational purposes and personal experimentation. The developer accepts no liability for misuse, illegal activity, data loss or any other damage. By using it you are responsible for complying with applicable laws.

**[TR]** Nexus, hiçbir garanti verilmeksizin, "olduğu gibi" ve eğitim / kişisel deneme amaçlı sunulmaktadır. Geliştirici; kötüye kullanım, yasa dışı faaliyet, veri kaybı veya doğabilecek diğer zararlardan sorumlu tutulamaz. Kullanıcı, yürürlükteki yasalara uymaktan kendisi sorumludur.
