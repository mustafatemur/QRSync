# QRSync - Real-Time Secure Communication 

QRSync is an open-source web application that allows you to instantly establish a real-time connection between devices (especially between a computer without a camera and a smartphone) using a QR code or a 6-digit security code, without requiring any installation or backend server infrastructure.

## Features

* **Two-Way Communication:** Mutual data and message transmission from both devices once the connection is established.
* **Easy Pairing:** Instant connection by scanning a QR Code or manual entry via a randomly generated 6-digit session code.
* **Secure Approval System:** The host is presented with a modern approval screen (Modal) for devices requesting to join the room. Unapproved devices cannot initiate communication.
* **Device Independent:** Works flawlessly in all scenarios: Phone-to-PC, Phone-to-Phone, or PC-to-PC. The "Copy Link" feature makes sharing incredibly easy when hosting a room from a mobile device.
* **XSS Protection:** All sent and received messages are stripped of HTML characters, protecting the application against malicious script (XSS) attacks.
* **100% Client-Side:** No database is used. Data is transmitted instantly and isolated between devices via the HiveMQ public MQTT broker.

## Technologies Used

* **HTML5 & JavaScript (ES6+)**
* **Tailwind CSS:** For a modern and responsive user interface design.
* **QRCode.js:** To generate dynamic QR codes directly in the browser.
* **MQTT.js:** To enable real-time data transfer (over WebSockets) between devices.

##  How to Use

### 1. Hosting a Room (Host)
1. Open the application and click the **"Create New Room"** button.
2. A unique QR Code and a 6-digit Connection Code will appear on your screen.
3. Wait for the other device to scan the QR code with their camera, or copy the connection link and send it to them.
4. When a connection request arrives from the guest device, click the **"Approve"** button on the confirmation modal that appears on your screen.

### 2. Joining a Room (Guest)
* **Via QR Code:** When you scan the QR code on the host's screen using your camera or a barcode scanner, the system will automatically request permission to connect to the room.
* **Via Code:** Open the app, enter the 6-digit Connection Code provided by the host into the input box at the bottom of the screen, and click the **"Join Room"** button. Wait for the host's approval.

##  Publishing on GitHub Pages

Since this application consists solely of static frontend files, you can host it for free on GitHub Pages in just a few seconds:

1. Create a new repository on your GitHub account or *fork* this repository.
2. Go to the **Settings** tab of your repository.
3. Click on **Pages** from the left sidebar menu.
4. Under the "Build and deployment" section, set the **Branch** to `main` (or `master`) and save.
5. Within a few minutes, your application will be live at `https://yourusername.github.io/qrsync/`.

##  License

This project is licensed under the MIT License. You are free to use, modify, and distribute it as you wish.
