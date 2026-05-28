# E-Waste Sorter Embedded Project & Dashboard

This repository contains the source code for an Embedded Final Project, featuring an **E-Waste Sorter** and a real-time web dashboard built with Vue.js, Vite, MQTT, and Firebase.

## 🚀 For Co-Developers: How to Clone the Repository

To start working on this project, clone the repository to your local machine using the HTTPS URL:

```bash
# Clone the repository
git clone https://github.com/Zendrii04/Embedded-Final-Project.git

# Navigate into the project folder
cd Embedded-Final-Project
```

## 🛠️ How to Set It Up Properly

The web dashboard is located in the `ewaste-dashboard` folder. You need Node.js (version 20.10.0 or compatible) to install the dependencies.

1. **Navigate to the dashboard directory:**
   ```bash
   cd ewaste-dashboard
   ```

2. **Install all required dependencies:**
   ```bash
   npm install
   ```

3. **Configure API Keys & Connections:**
   Before running the app, you need to add your own configuration details. Open `ewaste-dashboard/src/App.vue` and update:
   - Your **Firebase Config** (`apiKey`, `authDomain`, `databaseURL`, `projectId`).
   - Your **MQTT Broker credentials** (`wss://YOUR_HIVEMQ_CLUSTER_URL`, `username`, `password`).

## ▶️ How to Run It Properly

Once the setup is complete, you can start the local development server to view the dashboard in your browser.

1. **Ensure you are in the dashboard directory:**
   ```bash
   cd ewaste-dashboard
   ```

2. **Start the Vite development server:**
   ```bash
   npm run dev
   ```

3. **Open the Dashboard:**
   Open your browser and navigate to the local URL provided in your terminal (usually `http://localhost:5173`).

4. **Hardware Integration:**
   To see live data, ensure your embedded hardware (e.g., ESP32/Arduino) is powered on, connected to the internet, and publishing to the correct MQTT topic (`ewaste/sort/events`).
