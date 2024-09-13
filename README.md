# SPARTA - Target-X

This project consists of two main components:
1. **Logging tools** that run on Android, collecting data from the network interface (e.g., latency and bandwidth) and Android's mobile network state.
2. **A real-time dashboard** that displays the collected data.

## Logging Tools

The logging tools require ADB-level permissions to gather radio data via `logcat`. The recommended setup involves:

- Installing Termux, preferably from the [latest APK release](https://github.com/termux/termux-app/releases/latest).
- Installing [iperf3](https://iperf.fr/), [tmux](https://github.com/tmux/tmux/wiki), [jq](https://github.com/jqlang/jq), and [curl](https://github.com/curl/curl) on Termux with the command:  
  `pkg install iperf3 tmux jq curl`
- Installing [Shizuku](https://github.com/RikkaApps/Shizuku) and ensuring the [Shizuku server is running](https://shizuku.rikka.app/guide/setup/#start-shizuku).
- Verifying that [rish](https://github.com/RikkaApps/Shizuku-API/tree/master/rish) can run inside your Termux console. Follow this guide for setup: [Termux, Shizuku, and Rish configuration for Android 14](https://oddity.oddineers.co.uk/2024/01/14/termux-shizuku-and-rish-configuration-for-android-14/).
- Filling out the `.env` configuration for your logging tools:
   ```bash
   API_URL="http://backend_hostname"
   COLLECTION="default_collection_name"
   USERNAME="user"
   PASSWORD="password"
   ```
- Pushing the logging tools to your Android device:  
  `adb push log_tools /storage/emulated/0/Documents/targetx/`
- Starting data acquisition by running:  
  `start_logging.sh [collection_name]`

## Dashboard

The dashboard consists of a backend and a frontend:

- The **backend** is an API built with [Express](https://github.com/expressjs/express) that stores data in a [MongoDB](https://github.com/mongodb/mongo) database.
- The **frontend** is a lightweight [React](https://github.com/facebook/react) application. Real-time data acquisition is streamed to the dashboard via [WebSockets](https://developer.mozilla.org/en-US/docs/Web/API/WebSockets_API).

### Setup

- Fill in the `.env` file for the dashboard with the appropriate configuration:
   ```bash
   MONGO_USERNAME=mongouser
   MONGO_PASSWORD=mongopass
   MONGO_HOST=hostname.mongodb.net
   MONGO_DBNAME=mongodbname
   MONGO_OPTIONS=?retryWrites=true&w=majority&appName=appname
   JWT_SECRET=jwt_secret_token
   ADMIN_USERNAME=user
   ADMIN_PASSWORD=password
   MONGO_COLLECTION=mongocollection
   ```
- Run the application using Docker Compose:  
  `docker compose up` to build and start the backend and frontend.
